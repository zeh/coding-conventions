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

    var buttons:Vector.<PageButton> = getListOfButtons();
    var i:int;
    var button:PageButton;
    for (i = 0; i < buttons.length; i++) {
        button = buttons[i];
        button.doSomething();
    }


#### Booleans start with `is` or `has` (even if it's a getter)

Avoid other prefixes. `was` doesn't have a clear connotation; if something "was destroyed" but was brought back to life, `wasDestroyed` is technically true, but doesn't reflect its current state.

    isOld
    hasChildren
    hasLoggedLunch
    isDestroyed
    
TODO
----

Everything below here is temporary.
    
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
