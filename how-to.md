
# How to use Backbone with CouchDB without a Backbone connector

## Working with Models

### Fetch a document
```js
var ClassroomModel = Backbone.Model.extend({"urlRoot":"/someDatabaseUrl","idAttribute":"_id"})
var someClassroom = new ClassroomModel({_id:"someClassroomId"})
someClassroom.fetch({success: function (model, response, options) {
    console.log(Classroom.get('teacher'))
}})
```

### Save a document
```js
var ClassroomModel = Backbone.couch.Model.extend({_db:Backbone.couch.db('someDatabaseId')})
var someClass = new ClassroomModel({_id:"someClassroomId"})
someClass.fetch({success: function (model, response, options) {
  someClass.set("teacher", "Opra")
  someClass.save()
}})
```

## Working with Views
> Well I noticied views always responded with a {"total_rows":4,"offset":0,"rows":[{...}, {...}]} etc...and backbone was expecting the data as an array [{...},{...}] so I wrote a list that just responded with the array.
[link to comment](https://github.com/pyronicide/backbone.couchdb.js/issues/7#issuecomment-15276651)

```js
ddoc.lists= {
    asArray:function(head,req){
        //The provides function is a handy helper that determines what is returned 
        //based on the content type.
        provides('json', function(){
           var r = [];
           var row;
           while(row = getRow()){
               r.push(row.value);
               //You have a key and a value object in a row, essentially one of the rows returned from 
               //the view. This essentially gives you the opportunity to morph what you want returned.
           }
           return JSON.stringify(r);
           //A variation would be to call the send(...) function in the while loop
           //This would allow you to send back chuncks of data. Not sure if this is 
           //ideal for backbone because it may rely on the whole list getting there 
           //first so it may not make a difference.
        });
    }
};
```


# How to use node.js as the middleman between couch and backbone
- http://www.codeproject.com/Articles/507936/NodeplusApplicationplusServerpluswithplusCouchDB
-- same? -> http://brettjonesdev.com/node-application-server-with-couchdb/ 
- piping! http://japhr.blogspot.com/2011/08/deleting-things-in-backbonejs.html

A simple example...

from http://stackoverflow.com/questions/12850216/connecting-backbone-js-with-couchdb-when-hosting-in-apache-or-iis

```js
var couchdb = require('felix-couchdb'),
    client = couchdb.createClient(config.port, config.host),
    myDb = client.db('my_db');

// (... express boilerplate)
app.get('/resource/:resource_id', function (req, res) {
  myDb.getDoc(req.params.resource_id, function (err, doc) {
    if(err)
      return res.send(err);

    res.send(doc);
  });
});
```