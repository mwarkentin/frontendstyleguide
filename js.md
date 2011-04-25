# Frontend Style Guide

## JavaScript

There are a bazillion blog posts and books which cover the intricacies of JavaScript gotchas, best practices, performance improvements, etc. which are we will obviously not rewrite here. Instead, what follows are some conventions and preliminary stuff. (Also note that as responsible frontends building web *sites*, we would address progressive enhancement and ensure our site is usable to users without JavaScript. But we are working on a web *application* where JavaScript is considered required, so we will not delve into that for the moment.)

* It should not be necessary to have JavaScript either inline in an element (definitely no `onclick` handlers, etc) or in the `<head>` section of individual pages (exception: something that needs to be dynamically rendered).

* Naming conventions `+++`

        MyConstructor() // upper camel case for constructor
        myFunction()    // lower camel case for regular function
        _myPrivate()    // prefix with underscore to denote private functions (jslint complains unless nomen = false)
        var first_name  // snake case for variable names 
                        // -- or -- 
        var firstName   // lower camel case for variable names
        var TAX_RATE    // uppercase for constant or known global

* Combine `var` declarations for conciseness.

    **No**

        var foo = 'a';
        var bar = 'b';
        var what = {};

    **Yes**

        var foo = 'a',
            bar = 'b',
            what = {};
        // let's not talk about leading commas just yet

* Use correct indenting and brackets surrounding blocks. Explicitly add semi-colons. Etc. Basically, unless we want to [completely switch to something else](https://github.com/jashkenas/coffee-script), we should strive for vanilla JavaScript, not JavaScript-that-looks-more-like-Python, JavaScript-except-the-parts-I-don't-like, etc.

    **No**

        if (something)
            doThing();
        else
            doOtherThing();

        for (var listName in listItems) if (listItems.hasOwnProperty(listName)) {
            // do stuff
        }

    **Yes**

        if (something) {
            doThing();
        } else {
            doOtherThing();
        }

        for (var listName in listItems) {
            if (listItems.hasOwnProperty(listName)) {
                // do stuff
            }
        }

* An exception to the above can be made for a very fast `if (something) return false;` type of bailout in an otherwise busy function.

* Indenting: 2 spaces. New lines are indented to next level, not to align with opening braces. Ensure closing braces match indentation level. `+++`

    **No**

        element.find('.actions').animate({marginLeft:'-20px',
                                          width: '1px',
                                          paddingTop: '10px'},
                                          100, function(){
                                            $(this)
                                                .css('position','static')
                                                .hide();
                                          });
        });

    **Yes**

        element
            .find('.actions')
            .animate({
                marginLeft:'-20px',
                width: '1px',
                paddingTop: '10px'
            }, 100, function(){
                $(this)
                    .css('position','static')
                    .hide();
            });

* Don't bother aligning around `=` in variable declarations. Makes adding new lines more annoying, doesn't really help with readability.

    **No**

        var a                        = "",
            bar                      = 4,
            myReallyLongVariableName = "foo",
            comeOn                   = null;

    **Yes**

        var a = "",
            bar = 4,
            myReallyLongVariableName = "foo",
            comeOn = null;

* Spacing around function definitions.

    **No**

        var a = function(foo,bar){ ...

        var b = function (foo,bar) { ...

        var c = function (foo,bar){ ...

    **Yes**

        var d = function(foo, bar) { ...

* Spaces around operators (`a = 2`, not `a=2`), after colons (`{a: 1, b: 2}`, not `{a:1,b:2}`), after comma in lists of things (`(a, b, c)`, not `(a,b,c)`).

* Spacing for `for` loops.

    **No**

        for (var i=0,i<10;i++) {
            ...
        }

    **Yes**

        for (var i = 0, i < 10; i++) {
            ...
        }

* Cache the length of your array for `for` loops.

    **No**

        for (var i = 0; i < myarray.length; i++) {
            // myarray is accessed each time through the loop. If it's a DOM collection, this is very expensive.
        };

    **Yes**

        for (var i = 0, max = myarray.length; i < max; i++) {
            // use myarray[i]
        };

* `typeof` is not very helpful:

        typeof function () {}   // "function"
        typeof [1, 2, 3]        // "object"
        typeof {}               // "object"
        typeof null             // "object"

        // use either `instanceof`:
        var f = function(){}; f instanceof Function    // true
        [] instanceof Array                            // true
        var o = {}; o instanceof Object                // true

        // or access the object's constructor:
        [].constructor
        "".constructor
        // note you can't access the constructor for null and undefined

* Check whether your object has a direct property defined (versus a sneaky one further up the prototype chain).

    **Yes**

        for (var listName in listItems) {
            if (listItems.hasOwnProperty(listName)) {
                // do stuff
            }
        }

* Instantiate objects, arrays, etc directly, instead of via constructors.

    **No**

        var a = new Object();
        var myArr = new Array();

    **Yes**

        var a = {};
        var myArr = [];

* jQuery: Refer to the event object as `e` (instead of `e`, `event`, `evt`...).

* jQuery: Be aware of the difference between `e.preventDefault();` and `return false;` in functions to cancel default browser behaviour. You probably want the former.

* jQuery: If you make a local variable of a jQuery collection, prepend the variable name with a `$` to make it more obvious what it is. `+++`

    **No**

        var items = $("#Foo li"),
            bar = 1,
            baz = "a";
        bar += 1;
        items.addClass('foo').show();
        baz = "b";

    **Yes**

        var $items = $("#Foo li"),
            bar = 1,
            baz = "a";
        bar += 1;
        $items.addClass('foo').show();
        baz = "b";

* jQuery: Cache or chain selections.

    **No**

        // you are looking up .data each time!
        $(".data").addClass('foo');
        $(".data").width(400);
        $(".data").fadeOut();

    **Yes**

        $(".data")
            .addClass('foo');
            .width(400);
            .fadeOut();
            
        // also cache selections in a regular var:
        var $listItems = $("#foo li");
        // ... do something expensive with $listItems

* jQuery: Assemble new DOM elements first and attach them once.

    **No**

        for (var i = 0; i < 100; i++) {
            $("#elem").append(....
        }

    **Yes**

        for (var i = 0; i < 100; i++) {
            // assemble yourString
        }
        $("#elem").append(yourString)

* jQuery: Detach elements while working on them.

    **No**

        var $table = $("#table");
        $table.addLotsOfRows();

    **Yes**

        var $table = $("#table"),
            $parent = $table.parent();
          
        $table.detach(); // takes out of DOM, but leaves jQuery data intact (requires 1.4)
        $table.addLotsOfRows();
        $parent.append($table);

* jQuery: Don't pass selector as context.

    **No**

        var foo = $(".bar", "#baz");

    **Yes**

        var foo = $("#baz").find(".bar");

* jQuery: Selector optimization.

        // selectors with Sizzle are read right-to-left, so put specific on right, light on left -- tag.class on right, just tag or just .class on left
        $(".data td.someone");

        // descending from id is best
        $("#foo td.someone");

        // don't be needlessly specific
        $(".data table.people td.someone"); // don't need table.people

        // don't use universal selector
        $(".buttons > *");        // no
        $(".buttons").children(); // yes

        $(":password");      // no: has to check all DOM elements to see if they are of type password
        $("input:password"); // yes: narrows search set down considerably

* jQuery: Storing data.

        // normal
        $("#elem").data(key, value);

        // faster
        $.data(elem, key, value);
