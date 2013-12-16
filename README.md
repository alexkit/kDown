# kDown.js 

A little engine to power your keyboard-centric applications.

## Install

No dependency, no third-party library required. Just load kDown, and you're done.

    <script scr="path/to/kDown.js"></script>

## Live examples

The documentation below is very straight to the point. You may want to check the [live examples](http://alexduloz.github.io/kDown/examples.html) to see the methods described below in action.

## API

### `.whenShortcut( keys, callback )`

Fires `callback` when `keys` are down. 

The `whenShortcut` method should be used only to listen to, well, shortcuts (zero, one or more modifiers + _one_ regular key).

#### Arguments

* `keys` can be one of the following:
    * a "+" delimited string ( "alt+a" )
    * an array of strings and/or key codes and/or arrays:
        * ["alt","shift","a"] 
        * ["alt","shift",65] 
        * [18,16,65] 
        * [18,"shift",65]
        * ["alt",["a","b","c"]] – which means "alt" + either "a" or "b" or "c"
    * a single key code ( 65 ) (works only if you're targeting one specific key)
* `callback` accepts one optional argument: the `event` object.

#### Note

The implementation of the Mac "command" key is quite unfriendly and makes it problematic to count how many keys are down. A combo with more than one "regular" key like "alt+b+c" will not work with the `whenShortcut` method. The `whenDown` method, on the other hand, can count keys and is ok with complex combos. You should avoid using the Mac <kbd>cmd</kbd> key with `whenDown`, though.

More on that subject: 
<http://bitspushedaround.com/on-a-few-things-you-may-not-know-about-the-hellish-command-key-and-javascript-events>
<http://codepen.io/alexduloz/pen/nteqG>
   
### `.whenDown( keys, callback )`

Fires `callback` when `keys` are down.

The `whenDown` method is preferred to the `whenShortcut` method when you need to react to combos that are not shortcuts (see the `whenShortcut` method for more info).

#### Arguments

* `keys` can be one of the following:
    * a "+" delimited string ( "a+b+c" )
    * an array of strings and/or key codes and/or arrays: 
        * ["a","b","c"] 
        * ["a","b",67]
        * [65,66,67]
        * [65,"b",67]
        * ["alt",["a","b","c"]] – which means "alt" + either "a" or "b" or "c"
    * a single key code ( 65 ) (works only if you're targeting one specific key)
* `callback` accepts one optional argument: the `event` object.

### `.whenXpress( keys, callback[ ,n[, ms[, strictOrLoose]]] )`

Fires `callback` when `keys` are pressed `n` times during `ms` milliseconds.

#### Arguments

* `keys` can be one of the following:
    * a "+" delimited string ( "a+b+c" )
    * an array of strings and/or key codes and/or arrays: 
        * ["a","b","c"] 
        * ["a","b",67]
        * [65,66,67]
        * [65,"b",67]
        * ["alt",["a","b","c"]] – which means "alt" + either "a" or "b" or "c"
    * a single key code ( 65 ) (works only if you're targeting one specific key)
* `callback` accepts one optional argument: the `event` object.
* __int__ `n` how many times `keys` should be pressed.
* __int__ `ms` the milliseconds during which `n` should occur to fire `callback`.
* __string__ `strictOrLoose` specifies if `callback` should be fired if `keys` are pressed *exactly* `n` times (that's "strict") or *at least* `n` times (that's "loose").
    * values: "strict" (default) or "loose"

### `.context( HTMLElement )`

Limits `callback` to a given context. Works with `whenDown`, `whenShortcut` and `whenXpress`.

### `.condition( func )`

Runs a function before calling `callback`, which is fired only if that function returns `true`. Works with `whenDown`, `whenShortcut` and `whenXpress`.

### `.key( key )`

Associates a key to a given method/combo. Works with `whenDown`, `whenShortcut` and `whenXpress`.

### `.shut( [method[, combo[ ,key]]] )`

Deletes callbacks in various ways.

### `.always( func )`

Fires a function every time a combo is triggered.

### `.isShortcut( keys[, event] )`

Behaves like `whenShortcut`, but inside an event handler, and without a callback.

kDown has an internal `event` object which is used by the `isShortcut` method. If you want to use your own `event` object, you can simply pass it as the second argument of the method.

### `.isDown( keys[, event] )`

Behaves like `whenDown`, but inside an event handler, and without a callback.

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


