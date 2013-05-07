---
layout: post
comments: true
title: Master Control Program
type: post
date: 2013-05-07 1:58
---

##Master Control Program

>In order to control our SwarmBots, we need a way to process incoming user messages from Twitter, parse those messages,
> and then send the appropriate commands to the bots. This guide will go over the higher level concepts and control flow, 
> but leave the implementation of some aspects, such as which commands to look for, to the developer.
>
> <br/>
> Before we get started, we need to make sure our development environment is set up correctly. The first thing you should do
> is sign up as a developer on Twitter. Once you have signed up you can create a new Twitter application. Be sure to call it
> MySwarmBots or something else that clearly distinguishes it. Next you'll need to save the aplication key and secret to your
> bash profile (if using Linux). Next you should sign up for a free instance from MongoLab. We will use this to store information
> that can be used to communicate with a web application. If you do not want to have a web application, and just want to be able to 
> tweet at your SwarmBots, consider that database optional. Finally, be sure you have Node.js installed, as we will be writing our 
> application in JavaScript.
>
>
> Below is the list of dependencies that you will need. They can be installed through npm.


	// Library for making authorized API requests.
	var rem = require('rem');
	// Used for processing data streams, in our case Twitter.
	var carrier = require('carrier');
	// Used for connecting to our remote database.
	var MongoClient = require('mongodb').MongoClient;
	// Used for connecting to our master Arduino over serial.
	var serialport = require('serialport')
	var SerialPort = serialport.SerialPort
	var serialPort = new SerialPort("/dev/ttyACM0", {
	  baudrate: 9600,
	  parser: serialport.parsers.readline("\n") 
	}, false);
	// Our custom interface to the database explained in the next post.
	var mongo = require('./dbhelper');

> Once we have everything installed and set up, we can go ahead and get started designing our control flow.
> The first thing we will do is connect to our remote database. 
> The rest of our program will run inside the callback to this function, so it will have access to the database.

	// Start connection to MongoDB. Make sure your environment variables have been configured.
	MongoClient.connect(process.env.SWARMBOTS_MONGO_URI, function (err, db){

	  // Create Twitter API, prompting for key/secret.
	  var tw = rem.connect('twitter.com', 1).configure({
	    'key': process.env.TW_SWARMBOTS_KEY,
	    'secret':  process.env.TW_SWARMBOTS_SECRET
	  }).promptAuthentication(function (err, user){
	    
	    user.stream('statuses/filter').get({'track': 'swarmbots'},function (err, stream) {
	      carrier.carry(stream, function (line){
	        if (line){
	          var line = JSON.parse(line);
	          parseTweet(line);
	        }
	      });
	    });
	    
	    var parseTweet = function(json){
	      //console.log(json.id_str, json.text, json.user);
	      if (json.user.name != "SwarmBots"){
	        user_info= {sid:json.id_str, name:json.user.name, location:{name:json.user.location}, type:'tw',picture:{data:{url:json.user.profile_image_url}} }
	        //autoRespond(json.user.screen_name);
	        checkValidCommand(json.text, user_info, json.user.screen_name);
	      }    
	    }

	    var autoRespond = function(screen_name){
	      user('statuses/update').post({
	        status: "@" + screen_name + " thanks for the interest, but we aren't accepting commands just yet."
	      }, function (err, json){
	        console.log("Posted");
	      });
	    }

	    var checkValidCommand = function(text, user_info, screen_name){
	      text = text.toLowerCase();
	      if (text.indexOf("@IntroduceMeTo") > -1){
	        sayHello(screen_name);
	      }else{
	        if (text.indexOf("blue") > -1){
	          submitCommand("blue", user_info)
	          acceptCommand(screen_name);
	        }else if (text.indexOf("green") > -1){
	          submitCommand("green", user_info)
	          acceptCommand(screen_name);
	        }else if (text.indexOf("red") > -1){
	          submitCommand("red", user_info)
	          acceptCommand(screen_name);
	        }else if (text.indexOf("pink") > -1){
	          submitCommand("pink", user_info)
	          acceptCommand(screen_name);
	        }else if (text.indexOf("yellow") > -1){
	          submitCommand("yellow", user_info)
	          acceptCommand(screen_name);
	        }else{
	          submitCommand("blue", user_info)
	          acceptCommand(screen_name);
	        }
	      }
	    }

	    var submitCommand = function(bot, json){
	      mongo.getSwarmBot(db, bot, function (err, sb){
	        /*if (!sb.queue){
	          sb.queue = [];
	        }*/
	        mongo.getQueue(db, function (err, queue){
	          if(queue.people.indexOf(json.sid) > -1){
	            declineDuplicate(json.user.screen_name);
	          }else{
	            queue.meta.push({name: json.name, photo: json.picture.data.url, location: json.location.name, sid:json.sid});
	            queue.people.push(json.sid);
	            //mongo.updateSwarmBot(db, sb, function (){
	            mongo.updateQueue(db, queue, function (){
	            });
	            //});
	          }
	        });
	      });
	    } 

	    var tweetQueue =[];

	    var acceptCommand = function(screen_name){
	      sendReceipt(screen_name);
	    }

	    var sendReceipt = function(screen_name){
	      tweetQueue.push("@" + screen_name + " you have successfully moved our bots! Thanks!");
	    }

	    var sayHello = function(screen_name){
	      tweetQueue.push("@" + screen_name + " no need to go through them. Hi!");
	    }

	    var declineDuplicate = function(screen_name){
	      tweetQueue.push("@" + screen_name + " sorry, you can only be in the queue once. Try again once your turn is up!");
	    }

	    var declineCommand = function(screen_name){
	      tweetQueue.push("@" + screen_name + " that is an invalid command.");
	    }

	    var tweet = function(){
	      if (tweetQueue.length > 0){
	        user('statuses/update').post({
	          status: tweetQueue.shift()
	        }, function (err, json){
	          console.log("Tweet sent.");
	        });
	      }
	    }

	    setInterval(tweet, 60000);


	    var dispatchQueue = [];



	    serialPort.open(function () {
	      serialPort.on('data', function(data) {
	        //parseMessage(data);
	        console.log(data);
	      });
	    
	      var parseMessage = function(message){ 
	        if(message.indexOf('response:') > -1){
	          var resp = message.substring(message.length - 4, message.length)
	          getNextMove(resp.split(""));
	        }
	      }

	      var getNextMove = function(data){
	        mongo.getQueue(db, function (err, queue){
	          if (queue.people.length > 1){
	            dispatchQueue.push(queue.people.shift());
	            console.log(dispatchQueue);
	          }
	          mongo.updateQueue(db, queue, function (){
	            dispatch();
	          });
	        });
	      }

	      var packageNewMessage = function(json){
	        var message = json.client + json.bot + json.x.toString() + json.y.toString();
	        sendNextMove(message);
	      }

	      var sendNextMove = function(message){
	        dispatchQueue.push(message);
	      }
	    
	      var dispatch = function(){
	        if(dispatchQueue.length > 0){
	          console.log("Writing to serial...");
	          dispatchQueue.shift()
	          serialPort.write("1234");
	        }
	      }

	      var testMessage = function(){
	        console.log("Writing test message...");
	        serialPort.write("1234");
	      }

	      //setInterval(testMessage, 5000);
	      setInterval(getNextMove, 1000);

	    });
	    
	    

	    
	  });
	});