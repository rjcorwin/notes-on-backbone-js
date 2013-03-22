
# node.js as the middleman between couch and backbone
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