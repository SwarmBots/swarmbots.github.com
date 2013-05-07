---
layout: post
comments: true
title: Universal Database Interface
type: post
date: 2013-05-07 1:59
---

##Universal Database Interface

>In order to maintain a simple, consistent interface to our remote database, our web application and master control program use the same interface to perform identical commands.
>The following code represents the three main functions used in the operation of the SwarmBots, get, getAll, and update.
>
    // This function is used for retrieving the single document that has the matching uid.
    var get = function (collection, uid, callback){
      var args = Array.prototype.slice.call(arguments, 1);
      callback = typeof args[args.length - 1] == 'function' ? args.pop() : null;
      if (typeof uid != 'string'){
        collection.findOne({_id: uid}, callback);
      }else{
        collection.findOne({sid: uid}, callback);
      }
    }

    // This function is used for retrieving all of the documents in a given collection.
    var getAll = function(collection, callback){
      collection.find().toArray(callback);
    }

    // This function is used for updating a single document in the database.
    var update = function (collection, doc, callback){
      if (doc["_id"] != null){
        // The "upsert: true" means that a new document is created if one to update is not found.
        collection.update({"_id": doc["_id"]}, doc, {upsert: true}, callback);
      }else{
        collection.update({sid: doc.sid}, doc, {upsert: true}, callback);
      }
    }

> As an example of how these functions are implemented, see these test functions:

    // Called by client to update a document in the collection 'test'
    exports.updateTest = function (db, doc, callback){
      var collection = db.collection('test');
      update(collection, doc, callback);
    }

    // Called get a document from collection 'test' matching uid.
    exports.getTest = function (db, uid, callback){
      var collection = db.collection('test');
      get(collection, uid, callback);
    }


{% include disqus.html %}
