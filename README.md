# WCST Style Guide & Patterns

* Work in progress *

## Baseline
  - Javascript: [Airbnb JS Style Guide](https://github.com/airbnb/javascript)
  - PHP: [Fig Standards](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)

***

## Project Settings

  The project scaffolding will vary depending on the type of application and its scope. The client-side code should however, remain consistent.
  
  The following demonstrates a basic WCST project structure:

``` unicode
.
├── public
│   ├── css
│   │   ├── app
│   │   │   └── app.css
# If using SCSS/SASS :
│   │   └── scss
│   │       ├── _settings.scss
│   │       └── app.scss
# Or, if using LESS :
│   │   └── less
│   │       ├── app.less
│   ├── img
│   │   └── ui
│   ├── index.html
│   ├── js
│   │   ├── lib
│   │   └── templates
│   └── templates

```

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
  
### Preprocessors & Frameworks
  While a majority of our projects use [LESS](http://lesscss.org/) for CSS pre-compilation, [SCSS/SASS](http://sass-lang.com) has been introduced into the workflow. WCST generally uses Foundation(http://foundation.zurb.com) and in some instances [Twitter bootstrap](http://twitter.github.io/bootstrap) when intiating a new project or application.
  
#### Variable Naming
  
```scss
  
  /**
  * Colors (LESS or SCSS)
  * Use generic, yet descriptive color declarations
  * Do not use standard CSS color names (http://www.w3schools.com/cssref/css_colornames.asp)
  * Use camelCase for color names to ensure differentiation for CSS defaults
  * **Note** this snippet uses the `$` to denote a SCSS variable, LESS uses `@`
  **/
  
  /* Bad */
  $white: white;
  $white: #FFF;
  
  $coolblue: blue;
  $coolblue: #0066FF;
  
  /* Good */
  $baseWhite: #FEFEFE;
  $coolBlue: #0066FF;
  
  /* -------- */
  
  /**
  * Measurements
  * Use specific and decriptive names for measurements
  * Use camelCase
  **/
  
  /* Bad */
  $fHeight: 6em;
  $scrollersize: 50%;
  
  /* Good */
  $footerHeight: 6em;
  $heroScrollwerWidth: 50%;

```

#### File setup
Whether using a collection of individual files and compiling via `@import()` of `$include`, keep similar rules and styles grouped.  
  Define each segment of styles with `/* -- <name> -- */` and insert at least 5 lines above
```scss
/* -- Globals -- */
html, body {
  height: 100%;
}

h1 {
  font-weight: 200;
}




/* -- Nav View (#nav_view) -- */
#nav_view {
  #nav_view_slider {
    width: 50%;
    a {
      text-decoration: none;
    }
  }
}




/* -- Footer (footer) -- */
footer {
  /* footer styles here */
}

```

#### Patterns

  When using LESS, serve @2x sprites for retina displays. Create a global class of `.sprite` and define rules at the introduction of your document:
  
```css
@sprite: '/path/to/sprite.png';
@sprite2x: '/path/to/sprite@2x.png';

/* Declare .sprite class */
.sprite{
  display: block;
  background: url(@sprite) no-repeat;
  text-indent: -9999em;
}

/* Retina display media query */
@media only screen and (-webkit-min-device-pixel-ratio: 1.5),
      only screen and (min--moz-device-pixel-ratio: 1.5),
      only screen and (min-resolution: 240dpi){

  .sprite{
    background-image: url(@sprite2x);
    background-size: 440px 280px; /* 1/2 the size of the original */
  }
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
  
  **Utility**  
    - [LoDash](http://lodash.com) – Drop in replacement for Underscore.js  
    - [Underscore.js](http://underscorejs.org)
    
  **Architecture/Routing/Events**  
    - [Backbone.js](http://backbonejs.org)  
    - [RequireJS](http://requirejs.org)
    
  **UI**  
    - [jQuery.transit](http://ricostacruz.com/jquery.transit)
    
  **Templating**  
    - [Handlebars.js](http://handlebarsjs.com)
    
  **Scaffolding & Build**  
    - [Grunt](http://gruntjs.com)  
    - [Yeoman](http://yeoman.io)
  
  **Node.js**  
    - Typically, our Node.js applications use [Express](http://expressjs.com) to fire up HTTP servers
