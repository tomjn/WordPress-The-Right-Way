# JavaScript

## Enqueing a script

### For the widget form in the admin area

A quick note on how to do it, and a note on running the JS, so that it doesn't get ran on the html used to create new widget forms, only those in the sidebars on the right.

### For the frontend

How to enqueue a widgets scripts and styles, but only if the widget is on the page

##Events

Running code when:


### A New Widget is Added, or Re-ordered

 - Make use of the ajaxStop event to process javascript when a widget is added or re-ordered

```javascript
jQuery( document ).ready( function( $ ) {
    function doWidgetStuff() {
        var found = $( '#widgets-right .mywidgetelement' );
        found.each( function( index, value ) {
          // process elements
        } );
    }

    window.counter = 1;

    doWidgetStuff();

    $( document ).ajaxStop( function() {
        doWidgetStuff();
    } );
} );
```

### The widget form opens

 - Some js to show how to do things when the form opens and closes

## Further Reading

 - [Executing javascript when a widget is added ](http://wordpress.stackexchange.com/questions/130084/executing-javascript-when-a-widget-is-added-in-the-backend)
