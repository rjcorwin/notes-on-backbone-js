

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

This then allows us to define an event for when the color attribute is set.

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

