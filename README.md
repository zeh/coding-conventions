Zeh's Coding Conventions
========================

Introduction
------------

These are the coding conventions I have adopted over time when writing code.

Obviously enough, these are my own, and may differ from other people's conventions. However, I'm documenting them here so it can make reading my code easier, and help myself formalize my reasoning behind some of the conventions adopted.

I've always had some coding conventions in my head. However, after reading [id Software's Doom 3 Coding Style document](https://docs.google.com/document/d/1YBSlKzCplo2YkRgewn6lsNPNCX4W9OAotJRsAJywtTE/edit), I've been more inspired to write my own document (this document also reuses large chunks of id Software's one). That is what follows.

They will change over time. Input is of course welcome, but as any convention goes, much of what I have here is either highly subjective or conforming to my own thought processes. That is to say, your mileage may vary and that is OK.


General
-------

#### Files use real tabs that equal 4 spaces.

#### Empty lines have no trailing spaces.

#### Use trailing braces everywhere (if, else, functions, etc).

Reason: compactness.

    if ( x ) {
    }


#### The else statement starts on the same line as the last closing brace.

    if ( x ) {
    } else {
    }


#### All blocks that can have braces, should.

Reason: clarity and easiness of expanding/contracting a block without having to add/remove braces.

    if ( x ) {
        something();
    } else {
        somethingElse();
    }

Instead of

    if ( x )
        something();
    else
        somethingElse();

One exception is single-line IFs for short lines. This is ok:

    if ( x ) something();


Variable naming
---------------

#### Variable names start with lowercase characters and then use uppercase letters on new words.

Examples:

    oneVariable
    userName
    tempVariable

#### Certain compound words are common words and shouldn't have the second word capitalized.

When in doubt, do a Google search and see how a word is commonly used (especially in Wikipedia).

Examples of common compound words that shouldn't be capitalized in the middle:

    tooltip
    username

In certain cases, similar words should be capitalized because they mean something else (but should generally be avoided  because they can create confusion).

    toolTip // A tool's tip string
    userName // The user's name

#### Function parameters start with __ (double underline)

Function and method parameters start with a double underline to differentiate them from other variables or members of an object.

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
