# kDown.js 

A little engine to power your keyboard-centric applications.

No dependency, no third-party library required. Just load kDown, and you're done.

    <script scr="path/to/kDown.js"></script>

[View examples](http://alexduloz.github.io/kDown/examples.html)

## API

### `.whenShortcut( keys, callback[, context] )`

Fires `callback` when targeted `keys` are down. 

The `whenShortcut` method should be used only to listen to, well, shortcuts (zero, one or more modifiers + _one_ regular key).

#### Arguments

* `keys` can be one of the following:
    * a "+" delimited string ( "alt+a" )
    * an array of strings and/or key codes and/or arrays ( ["alt","shift","a"], ["alt","shift",65], [18,16,65], [18,"shift",65], [ "alt",["a","b","c"]] (which means "alt" + either "a" or "b" or "c") )
    * a single key code ( 65 ) (works only if you're targeting one specific key)
* `callback` accepts one argument: the `event` object.
* `context` is an HTML Object. Useful when you want to fire `callback` only in specific contexts.

#### Examples

    kDown.whenShortcut("alt+a", function(e){
        console.log("alt + a");
    });

    kDown.whenShortcut("alt+shift+a", function(e){
        console.log("alt + shift + a");
    });

    kDown.whenShortcut("shift+3", function(e){
        console.log("shift + 3");
    });

It is possible to embed an array within an array. In this case, the embedded array mimics the "or" operator.

    kDown.whenShortcut(["alt",["a","b","c"]], function(e){
        console.log("alt + [ a or b or c ]");
    });

    kDown.whenShortcut([16, ["c","d",69]], function(e){
        console.log("shift + [ c or d or e ]");
    });

You can limit the firing of `callback` to a specifc DOM context using the option `context` argument:

    kDown.whenShortcut("alt+esc",function(){
        console.log("works only in textareas");
    },document.getElementsByTagName("textarea"));
    
#### Note

The implementation of the Mac "command" key is quite unfriendly and makes it problematic to count how many keys are down. A combo with more than one "regular" key like "alt+b+c" will not work with the `whenShortcut` method. The `whenDown` method, on the other hand, can count keys and is ok with complex combos. You should avoid using the Mac <kbd>cmd</kbd> key with `whenDown`, though.

More on that subject: 
<http://bitspushedaround.com/on-a-few-things-you-may-not-know-about-the-hellish-command-key-and-javascript-events>
<http://codepen.io/alexduloz/pen/nteqG>
   
### `.whenDown( keys, callback[, context] )`

Fires `callback` when targeted `keys` are down.

The `whenDown` method is preferred to the `whenShortcut` method when you need to react to combos that are not shortcuts (see the `whenShortcut` method for more info).

#### Arguments

* `keys` can be one of the following:
    * a "+" delimited string ( "a+b+c" )
    * an array of strings and/or key codes and/or arrays ( ["a","b","c"], ["a","b",67], [65,66,67], [65,"b",67], ["alt",["a","b","c"]] (which means "alt" + either "a" or "b" or "c") )
    * a single key code ( 65 ) (works only if you're targeting one specific key)
* `callback` accepts one argument: the `event` object.
* `context` is an HTML Object. Useful when you want to fire `callback` only in specific contexts.

#### Examples

    kDown.whenDown("a+b+c", function(e){
        console.log("a + b + c");
    });

    kDown.whenDown("esc", function(e){
        console.log("esc");
    });

    kDown.whenDown("alt+3", function(e){
        console.log("alt + 3");
    });

It is possible to embed an array within an array. In this case, the embedded array mimics the "or" operator.

    kDown.whenDown(["z",["a","b","c"]], function(e){
        console.log("z + [ a or b or c ]");
    });

    kDown.whenDown(["a", 66, ["c","d",69]], function(e){
        console.log("a + b + [ c or d or e ]");
    });

You can limit the firing of `callback` to a specifc DOM context using the option `context` argument:

    kDown.whenDown("enter",function(){
        console.log("'enter' was pressed inside textareas");
    },document.getElementsByTagName("textarea"));


### `.isShortcut( keys[, event] )`

Behaves like `whenShortcut`, but inside an event handler, and without a callback.

#### Example

    document.addEventListener("keydown",function(event){
        if(kDown.isShortcut([["e","f","g"]])) {
            console.log("is shortcut: e|f|g");
        }
    },true);

kDown has an internal `event` object which is used by the `isShortcut` method. If you want to use your own `event` object, you can simply pass it as the second argument of the method.

### `.isDown( keys[, event] )`

Behaves like `whenDown`, but inside an event handler, and without a callback.

#### Example

    document.addEventListener("keydown",function(event){
        if(kDown.isDown("a+b+c")) {
            console.log("down: a + b + c ");
        }
    },true);

kDown has an internal `event` object which is used by the `isDown` method. If you want to use your own `event` object, you can simply pass it as the second argument of the method.

### `.countKeys()`

Returns the number of keys that are down.

#### Example

    document.addEventListener("keydown",function(event){
        console.log(kDown.countKeys());
    },true);

### `.getKeys()`

Returns an array of key codes of the keys that are down.

#### Example

    document.addEventListener("keydown",function(event){
        console.log(kDown.getKeys());
    },true);


### `.ignoreInput( yesOrNope )`

Tells kDown not to discard combos fired in input fields and textareas. 

`yesOrNope`: boolean (default is `true`).


### `.cmdOrCtrl( yesOrNope )`

Tells kDown to consider equals the cmd and ctrl keys. 

`yesOrNope`: boolean (default is `true`).


### `.logKeyCodes( yesOrNope )`

Tells kDown to log key codes for each key pressed. 

`yesOrNope`: boolean (default is `false`).

### `.preventDefault( yesOrNope )`

Tells kDown whether to prevent default behavior of a given combo or not

`yesOrNope`: boolean (default is `true`).

### `.config( userConfig )`

A shortcut to access kDown's config methods

`userConfig`: obejct.

#### Example

    kDown.config({
        ignoreInput: true,
        cmdOrCtrl: true,
        logKeyCodes: false,
        preventDefault: true 	
    });


## Strings

Here's a list of the strings recognized by kDown:

* <kbd>a</kbd>-<kbd>z</kbd>
* <kbd>0</kbd>-<kbd>9</kbd>
* <kbd>f1</kbd>-<kbd>f12</kbd>
* <kbd>left</kbd> or <kbd> arrayleft </kbd> 
* <kbd>up</kbd> or <kbd>arrayup</kbd> 
* <kbd>right</kbd> or <kbd>arrayright</kbd> 
* <kbd>down</kbd> or <kbd>arraydown</kbd>
* <kbd>alt</kbd> or <kbd>altkey</kbd> 
* <kbd>ctrl</kbd> or <kbd>ctrlkey</kbd>
* <kbd>shift</kbd> or <kbd>shiftkey</kbd>
* <kbd>meta</kbd> or <kbd>metakey</kbd>
* <kbd>cmd</kbd> or <kbd>cmdkey</kbd>
* <kbd>backspace</kbd> or <kbd>del</kbd> 
* <kbd>delete</kbd> 
* <kbd>end</kbd>
* <kbd>enter</kbd>
* <kbd>escape</kbd> or <kbd>esc</kbd> 
* <kbd>home</kbd>
* <kbd>insert</kbd>
* <kbd>pageup</kbd>
* <kbd>pagedown</kbd>
* <kbd>tab</kbd>
* <kbd>space</kbd>

If you're targeting a key that is not represented in the above list, you'll have to use its key code.


