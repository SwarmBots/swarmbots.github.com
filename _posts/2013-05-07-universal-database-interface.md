---
layout: post
comments: true
title: Universal Database Interface
type: post
date: 2013-05-07 1:59
---

##Universal Database Interface

```
var get = function (collection, uid, callback){
  var args = Array.prototype.slice.call(arguments, 1);
  callback = typeof args[args.length - 1] == 'function' ? args.pop() : null;
  if (typeof uid != 'string'){
    collection.findOne({_id: uid}, callback);
  }else{
    collection.findOne({sid: uid}, callback);
  }
}

var getAll = function(collection, callback){
  collection.find().toArray(callback);
}

var update = function (collection, doc, callback){
  if (doc["_id"] != null){
    collection.update({"_id": doc["_id"]}, doc, {upsert: true}, callback);
  }else{
    collection.update({sid: doc.sid}, doc, {upsert: true}, callback);
  }
}
```

```
exports.updateTest = function (db, doc, callback){
  var collection = db.collection('test');
  update(collection, doc, callback);
}

exports.getTest = function (db, uid, callback){
  var collection = db.collection('test');
  get(collection, uid, callback);
}
```
