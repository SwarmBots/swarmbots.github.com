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

> Next, we will use Rem.js to connect to twitter and open a stream.
> We want to configure the stream to filter out a keyword that we will listen for and intercept.
> For our purposes, we will be listening for the use of the word 'SwarmBots.'
> Everytime the stream finds a relevant tweet, it gets collected by our Carrier until the full text has been received.
> Then, we pass the tweet off into our control system.

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

> The first step in our message's journney is to be parsed for logistical information.
> Here we are just getting some basic user data so we can show who is using SwarmBots on our website.
> Then, se send the packaged information off to our command checker.
	    
	    var parseTweet = function(json){
	      //console.log(json.id_str, json.text, json.user);
	      if (json.user.name != "SwarmBots"){
	        user_info= {sid:json.id_str, name:json.user.name, location:{name:json.user.location}, type:'tw',picture:{data:{url:json.user.profile_image_url}} }
	        //autoRespond(json.user.screen_name);
	        checkValidCommand(json.text, user_info, json.user.screen_name);
	      }    
	    }

> He you can write your own custom code to look for any specific command and pass that off for submission.
> In this example, we will just pass commands straight through, and assign them to an arbitrary bot.

	    var checkValidCommand = function(text, user_info, screen_name){
	      text = text.toLowerCase();
	      var bot = "bot0";
	      if (isValid(text)){
	        submitCommand(bot, user_info)
	        acceptCommand(screen_name);
	      }
	    }

	    var isValid = function(command) {
	    	//write your own validation here.
	    }

> Here our submission function pushes the new command to the database.
> In this instance, we are using a shared Queue, but you could just as easily have a separate queue for each one of your bots.

	    var submitCommand = function(bot, json){
	      mongo.getSwarmBot(db, bot, function (err, sb){
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

> The following is just an example of some reciept messages you can send to users, and how to send them.
>
		// Create a queue of tweets to be sent out.
	    var tweetQueue =[];

	    var acceptCommand = function(screen_name){
	      sendReceipt(screen_name);
	    }

	    var sendReceipt = function(screen_name){
	      tweetQueue.push("@" + screen_name + " you have successfully moved our bots! Thanks!");
	    }

	    var declineDuplicate = function(screen_name){
	      tweetQueue.push("@" + screen_name + " sorry, you can only be in the queue once. Try again once your turn is up!");
	    }

	    var declineCommand = function(screen_name){
	      tweetQueue.push("@" + screen_name + " that is an invalid command.");
	    }

	    // How we actually send a tweet, using Rem.js.
	    var tweet = function(){
	      if (tweetQueue.length > 0){
	        user('statuses/update').post({
	          status: tweetQueue.shift()
	        }, function (err, json){
	          console.log("Tweet sent.");
	        });
	      }
	    }

	    // We need to rate limit our tweets to keep Twitter happy.
	    setInterval(tweet, 60000);

> Finally, we will get into sending and receiving messages with the master Arduino.
>

		// Used to hold outgoing messages.
	    var dispatchQueue = [];

	    // Open a serial connection with our Arduino.
	    serialPort.open(function () {
	      // Listen for any returning messages, send them to the parser.
	      serialPort.on('data', function(data) {
	        parseMessage(data);
	        console.log(data);
	      });
	    
	      var parseMessage = function(message){ 
	        if(message.indexOf('response:') > -1){
	          // If this is a response, get the message
	          // In this example, the message is encoded in the last 4 characters.
	          var resp = message.substring(message.length - 4, message.length)
	          if (resp != '0000'){
	          	// If the bot is ready, get the next move and dispatch it.
	          	getNextMove();
	          }
	        }
	      }

	      var getNextMove = function(){
	        mongo.getQueue(db, function (err, queue){
	          if (queue.length > 1){
	            dispatchQueue.push(queue.shift());
	            console.log(dispatchQueue);
	          }
	          mongo.updateQueue(db, queue, function (){
	            dispatch();
	          });
	        });
	      }

	      var sendNextMove = function(message){
	        dispatchQueue.push(message);
	      }
	    
	      // Write the next message out to serial.
	      var dispatch = function(){
	        if(dispatchQueue.length > 0){
	          console.log("Writing to serial...");
	          message = dispatchQueue.shift()
	          serialPort.write(message);
	        }
	      }

	    });
	  });


The full source code can be found on the [SwarmBots GitHub page][].


[SwarmBots GitHub page]: http://github.com/Swarmbots "SwarmBots GitHub"


{% include disqus.html %}
