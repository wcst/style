# WCST Style Guide & Patterns

* Work in progress *

## Baseline
  - Javascript: [Airbnb JS Style Guide](https://github.com/airbnb/javascript)
  - PHP: [Fig Standards](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)

***

## HTML Markup
  Selectors that are intened for consumption via JS only shoule be prefixed with an `_` underscore
```html
<!-- Nope -->
<div id="selectorName"></div>
<div id="selector_name"></div>

<!-- Yep -->
<div id="_selector_name"></div>
```

* * *

  Presentational ids should be `_` underscore separated
```html
<!-- Nein -->
<div id="_redbox"></div>
<div id="_redBox"></div>
<div id="redBox"></div>
<div id="redbox"></div>

<!-- Ja -->
<div id="red_box"></div>
```

* * *

  Presentational classes should be camelCase
```html
<!-- Nein -->
<div class="_the_redbox"></div>
<div class="_theredBox"></div>
<div class="theredbox"></div>
<div class="the_red_box"></div>

<!-- Ja -->
<div class="theRedBox"></div>
```
* * *

## CSS
  
  [OOCSS](http://oocss.org/) principles should be adhered to as musch as is possible.
  
* * *
  
  When defining styles of a selector, separate layout/positioning, presentation/type, and functionality within tags
```css
  #charlie_conway_bio {
    position: relative;
    margin: 10% auto;
    width: 25%;
    height: 2em;
    float: left;
    
    font-size: 1.4em;
    font-weight: 200;
    text-transform: uppercasel
    text-align: center;
    
    opacity: 0.5;
    
    transform: translate3d(0, 2%, 0);
    transition: transform 122ms ease-out;
  }
  
  #charlie_conway_bio:hover {
    opacity: 1;
    
    transform: translate3d(0, 4%, 0);
  }
```

* * *

## Javascript
  Modularity and separation of concerns is key!
  
  * * *
  
### Patterns
  
  Avoid pollution of the global namespace as much as possible! If using AMD, consider tying application level-events and configurations to a re-useable `app` object which can then be exported and passed around as needed.
  
``` javascript

  // app.js
  define(['someDependency'], function () {
    
    var app = {
      api: {
        urls: {
          'people': {url:'/people'},
          'places': {url: '/places'},
          'things': {url: '/things'}
        }
      },
      
      getApiUrl: function (req) {
        return this.api.urls[req].url;
      }
    };
    
    // Export the app object
    return app;
  });
  
  
  // Other modules throughout your app can then use app
  define(['app'], function(app) {
      app.someFunction();
      
      var apiURL = app.getApiUrl('people');
  });

```

  If not using AMD, attach a single namespace reference to the window object for app-level access. Our example above could be re-written as:

``` javascript

  // app.js
  (function (window) {
    
    var app = window.app || {};
    
    app.api = {
      urls: {
        'people': {url:'/people'},
        'places': {url: '/places'},
        'things': {url: '/things'}
      }
    };
    
    app.getApiUrl = function (req) {
      return this.api.urls[req];
    };
    
  })(window, undefined);
  
  // Other JS files (added *after* app.js) can then access the app object
  (function (window) {
    console.log( app.getApiUrl('things') ); // '/things'  
  })(window, undefined);
  

```  
  * * *
  
### Frameworks & Libraries
  Depending on the scope of the project and the depth of requirements, the set of libraries and frameworks will change. The followin list represents libraries most commonly used in WCST project and applications.
  
  **DOM access & manipulation (default to vanilla JS first)**  
    - [jQuery](http://jquery.com)  
    - [Zepto](http://zeptojs.com)
  
  **For utility**  
    - [LoDash](http://lodash.com) – Drop in replacement for Underscore.js  
    - [Underscore.js](http://underscorejs.org)
    
  **Architecture, routing, and/or Events**  
    - [Backbone.js](http://backbonejs.org)  
    - [RequireJS](http://requirejs.org)
    
  **Templating**  
    - [Handlebars.js](http://handlebarsjs.com)
    
  **Scaffolding & Build**  
    - [Grunt](http://gruntjs.com)  
    - [Yeoman](http://yeoman.io)
  
  **Node.js**  
    - Typically, our Node.js applications use [Express](http://expressjs.com) to fire up HTTP servers
