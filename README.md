
# Backbone.Model basics
This example is an expansion of the [Backbone.Model example at backbonejs.org](http://backbonejs.org/#Model).

We are creating a sidebar object in javascript that has an associated #sidebar div and we want to assign properties to that sidebar object so that when properties are assigned an event is triggered which gives us the ability to act on that data change in data.  

In backbone models, instead of setting a property directly like...

```js
sidebar.color = "white"
```

We use a set function that allows us to hook in and build events off of that assignment.

```js
  var Sidebar = Backbone.Model.extend({ });

  // Explicitly set the sidebar object in the global 'window' scope
  window.sidebar = new Sidebar;

  // This will trigger the change:color event and cause the #sidebar element's background 
  // property to change to white.
  sidebar.set({color: 'white'});
```

This then allows us to define an event for when set function is used to save the color attribute.  We'll use the ["change:[attribute]" event](http://backbonejs.org/#Events-catalog) but we could also use the "change" event and run an if statement to see if the color attribute is changing.

```js
  // Define tell that object what it should do when its color property changes.
  sidebar.on('change:color', function(model, color) {
    $('#sidebar').css({background: color});
  });
```

Now lets attach some functionality to the sidebar object. Lets give it the ability to prompt the user for a color that it should change its color property to.  When this code runs, it will set the color property to white and then immediately prompt the user for a new color.


```js
  var Sidebar = Backbone.Model.extend({
    promptColor: function() {
      var cssColor = prompt("Please enter a CSS color:");
      this.set({color: cssColor});
    }
  });

  window.sidebar = new Sidebar;

  sidebar.on('change:color', function(model, color) {
    $('#sidebar').css({color: color});
  });

  sidebar.set({color: 'white'});

  sidebar.promptColor();
```

# Getting an instance of a model as JSON
This is listed under the model.toJSON() documentation because .toJSON() is not actually used in the example. I think they are just pointing out that when you JSON.stringify(someModelInstance) you get the JSON of the attributes as opposed to the actual model instance.

```js
var artist = new Backbone.Model({
  firstName: "Wassily",
  lastName: "Kandinsky"
});

artist.set({birthday: "December 16, 1866"});

// alerts: {"firstName":"Wassily","lastName":"Kandinsky","birthday":"December 16, 1866"}
alert(JSON.stringify(artist));  
```

# Interacting with a server

Before going over the methods for pulling and pushing data, it's important to understand that Backbone assumes there is a unique URL for every single one of your model instances that it can perform REST operations on (HTTP POST=Create, HTTP GET=Read, HTTP UPDATE=Update, HTTP DELETE=Delete).  

Lets say you had a RESTful endpoint at http://127.0.0.1/artists that you could use to perform CRUD operations on objects at urls like http://127.0.0.1/artists/kadinsky and http://127.0.0.1/artists/fleming ... When you define your artist model you would define it like so...

```js
  var artist = Backbone.Model.extend({
    url: 'http://127.0.0.1/artists'
  });
```
This makes the following methods possible...

## model.sync()
Set the model instance to save automatically to the server using someModelInstance.sync()
I think that's what .sync() is used for...

## model.fetch()
Get the current version of the object from the server.

## model.save()
Save what is stored locally to the remote database.


# Controllers in Backbone.js are labeled as "Views"
> Spine controllers and Backbone views serve the same purpose and bridge the gap between the HTML markup and the model layer. In a conventional server-side MVC framework this layer is most often referred to as controllers and in my mind that name makes more sense than calling it views because the markup represents the view.
> http://hjortureh.tumblr.com/post/22117245794/spine-js-vs-backbone-js

```js
var ContactView = Backbone.View.extend({

  events: { 
    "click li": "openContact" 
  },

  initialize: function() {
    contacts.bind( "reset", reset, this );
    contacts.fetch();
  },
  
  reset: function() {  
      this.contactList = this.$("#contact-list");

      // Do stuff
    }

});

// Usage
new ContactView({ el: $("#view").get(0) });
```
