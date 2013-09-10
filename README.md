Zeh's Coding Conventions
========================

Introduction
------------

These are the coding style conventions I have adopted over time when writing code.

Obviously enough, these are my own, and may differ from other people's conventions. However, I'm documenting them here so it can make reading my code easier, and help myself formalize my reasoning behind some of the conventions adopted.

I've always had some coding conventions in my head. However, after reading [id Software's Doom 3 Coding Style document](https://docs.google.com/document/d/1YBSlKzCplo2YkRgewn6lsNPNCX4W9OAotJRsAJywtTE/edit), I've been more inspired to write my own document (this document also reuses large chunks of id Software's one). That is what follows.

They will change over time. Not all of my code follow all of these conventions, because it may have been written when I was still using a different convention.

Input is of course welcome, but as any convention goes, much of what I have here is either highly subjective or conforming to my own thought processes. That is to say, your mileage may vary and that is OK.

#### Other references

* [Google's C++ coding conventions](http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml) (thanks to Zero|DPX on ShackNews)
* [Google's JavaScript coding conventions](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)


General
-------

#### Files use real tabs that equal 4 spaces.

It just does.


#### Empty lines have no trailing spaces.

Save those bytes for a more noble cause.


#### Use trailing braces everywhere (if, else, functions, etc).

Reason: compactness.

    if (x) {
        // ...
    }


#### The else statement starts on the same line as the last closing brace.

    if (x) {
        // ...
    } else {
        // ...
    }


#### All blocks that can have braces, should.

Reason: clarity and easiness of expanding/contracting a block without having to add/remove braces.

    if (x) {
        something();
    } else {
        somethingElse();
    }

Instead of

    if (x)
        something();
    else
        somethingElse();

One exception is single-line IFs for short lines. This is ok:

    if (x) something();


Variable naming
---------------

#### Variable names start with a lower case characters, and then each successive word in the name starts with an upper case letter.

Examples:

    x
    total
    oneVariable
    numRows
    tempVariable


#### Certain compound words are common words and shouldn't have the second word capitalized.

When in doubt, do a Google search and see how a word is commonly used (especially in Wikipedia).

Examples of common compound words that shouldn't be capitalized in the middle:

    tooltip
    username

In certain cases, similar words should be capitalized because they mean something else (but should generally be avoided  because they can create confusion).

    toolTip // A tool's tip string
    userName // The user's name (e.g. if there's userAge, userType, userId, etc)


#### Function/method parameters start with __ (double underline)

Function and method parameters start with a double underline to easily differentiate them from other variables or members of an object.

In generic functions or methods:

    function doSomething(__subject) {
        log("Doing something to " + __subject);
    }
    
    protected destroyActor(Actor __actor) {
        __actor.destroy();
    }
    
In the constructor of a class, it prevents forcing the use of `this.` and creating reference inconsistencies when reading the code:

    public ProductButton(String __caption) {
        caption = __caption;
    }


#### List/collection names are always plurals (ending with an "s")

    // Java
    ArrayList<ListButton> buttons = new ArrayList<ListButton>();
    
    // ActionScript
    var boxes:Vector.<DisplayBox> = new Vector.<DisplayBox>();
    
    // JavaScript
    var names = [];


#### When looping through a list, temporary names for the item at a given position (when used) are the singular of the list name

Normal loop:

    var buttons:Vector.<PageButton> = getListOfButtons();
    var i:int;
    var button:PageButton;
    for (i = 0; i < buttons.length; i++) {
        button = buttons[i];
        button.doSomething();
    }

Simpler loop, when possible:

    var buttons:Vector.<PageButton> = getListOfButtons();
    for each (button:PageButton in buttons) {
        button.doSomething();
    }

#### Booleans start with `is` or `has` (even if it's a getter)

Avoid other prefixes. `was` doesn't have a clear connotation; if something "was destroyed" but was brought back to life, `wasDestroyed` is technically true, but doesn't reflect its current state.

    isOld
    hasChildren
    hasLoggedLunch
    isDestroyed


#### Temporary methods and variables used for testing should start with `debug_`.

Those should not be made into production, or should only be used in debug states.

    debug_stats
    debug_totalMemory
    debug_restart()
    debug_simulateUserEntry()

Getter/setters
--------------

#### Getter/setters are a good thing if used wisely.

Getter/setters are good especially if you're tweening a value, since they can be used more transparently by tweening engines.

#### Getters/setters should only exist if both the getter and the setter exist. They should act invisibly, like a normal variable would.

This avoids confusion on whether a variable can be set or not, and what's the alternative.

#### Getters/setters should always refer to a private property. The private variable has the same name, but starts with an underline.

    public function get maxWidth():Number {
        return _maxWidth;
    }
    public function set maxWidth(__value:Number):void {
        _maxWidth = __value;
    }

The exception is when the returned value must be calculated from something else, e.g.:

    public function get numItems():int  {
        return list.length;
    }
    public function set numItems(__value:int):void {
        list.length = __value;
    }

#### In ActionScript and others, the name of the parameter received by the setter method is always `__value`.

This avoids conflicts and confusion with local naming of properties.

#### Native getter/setters should have no [side-effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science%29).

As often as possible, the getter/setter functions should not change any of its instance's state (other than the obvious changes required by the setter).

The important exception for this is when getter/setters are used for a value that's not directly tied to a variable, but is instead calculated on the fly as necessary and often cached.

    public function numSelectedItems():int  {
        if (isNumSelectedItemsDirty) {
            // Recalculate number
            _numSelectedItems = 0;
            for (var i:int = 0; i < items.length; i++) {
                if (items[i].selected) _numSelectedItems++;
            }

            // Mark as calculated
            isNumSelectedItemsDirty = false;
        }
        return _numSelectedItems;
    }

Often, however, such getter/setters should be used as methods/functions rather than native getter/setters.

#### If a property only has a getter or a setter, use normal methods/functions (Java style) instead of native getter/setters.

This makes it clear it is read-only.

    public function getNumItems():int  {
        return list.length;
    }

The lone setter is more rare, because it would mean you're setting something that can't be read later (can't think of a solid reason for that behavior).

    public function setAlpha(__value:Number):void  {
        _alpha = __value;
    }


TODO
----

Everything below is temporary.
    
Function/method naming
----------------------

Scoping
-------

Where to declare local variables? i in loops use inside scope?

When to use composition and inheritance? When to use interfaces?

Standard headers/comments block?

Order?

Indent variable declarations with tabs

Always make declarations final when possible

How to mark pure functions?

Avoid function overloading by type alone - use specific named functions? Types are not reliable and different parameters may have redundant types

Order in classes - constructor, private, public, getter/setters?
Get/setter syntax?
