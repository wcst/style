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
