Zeh's Coding Conventions
========================

## Introduction

These are the coding style conventions I have adopted over time when writing code.

Obviously enough, these are my own, and may differ from other people's conventions. However, I'm documenting them here so it can make reading my code easier, and help myself formalize my reasoning behind some of the conventions adopted.

I've always had some coding conventions in my head. However, after reading [id Software's Doom 3 Coding Style document](https://docs.google.com/document/d/1YBSlKzCplo2YkRgewn6lsNPNCX4W9OAotJRsAJywtTE/edit), I've been more inspired to write my own document. That is what follows.

They will change over time. Not all of my code follow all of these conventions, because it may have been written when I was still using a different convention.

Input is of course welcome, but as any convention goes, much of what I have here is either highly subjective or conforming to my own thought processes. That is to say, your mileage may vary and that is OK.

### Language-specific files

The ideas here contained are also expressed as:

 * A [tslint.json](rules/tslint.json) file (for [TypeScript](http://www.typescriptlang.org/) and [tslint](https://github.com/palantir/tslint))

### Other references

* [Google's C++ coding conventions](http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml)
* [Google's JavaScript coding conventions](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [Microsoft's C# coding conventions](http://msdn.microsoft.com/en-us/library/ff926074.aspx)
* [Microsoft's C# capitalization conventions](http://msdn.microsoft.com/en-us/library/vstudio/ms229002(v=vs.100).aspx)


## General

* **Files use real tabs that equal 4 spaces.** It works on every configuration.
* **Empty lines have no trailing spaces.** Save those bytes for a more noble cause.
* **Use trailing braces on every block ([1TBS](https://en.wikipedia.org/wiki/Indent_style#1TBS)).** For compactness. Example:

    ```javascript
    if (x) {
        // ...
    }
    ```

* **The else statement starts on the same line as the last closing brace.** Also for compactness.

    ```javascript
    if (x) {
        // ...
    } else {
        // ...
    }
    ```

* **All blocks that can have braces, probably should.** For clarity and consistency, easiness of expanding/contracting a block without having to add/remove braces, and fewer  mistakes.

    ```javascript
    // OK:
    if (x) {
        something();
    } else {
        somethingElse();
    }

    // NOT OK:
    if (x)
        something();
    else
        somethingElse();

    // One exception: single-line ifs/whiles for short lines
    // OK:
    if (x) something();
    ```


## Variable naming

* **Variable names start with a lower case character, and then each successive word in the name starts with an upper case letter.**

    ```javascript
    x
    total
    oneVariable
    numRows
    tempVariable
    ```

* **Local private variables inside a class start with an underline.** This makes them easier to read.

    ```typescript
    public name:string;

    private _id:string;
    ```

* **Certain compound words are common words and shouldn't have the second word capitalized.** When in doubt, do a Google search and see how a word is commonly used (especially in Wikipedia). For example, `tooltip` and `username` should not normally be capitalized in the middle.

    The exception is when they mean something else; for example, `toolTip` for a tool's tip string, or `userName` for a user's name when there's `userAge`, `userType`, `userId`, etc. However, those should generally be avoided because they can create confusion.


* **Function/method parameters don't have a prefix (like `_`, `__`, `$`, etc).**

    ```javascript
    OK:
    function whatever(name) {
        this.name = name;
    }

    NOT OK:
    function whatever(_name) {
        name = _name;
    }
    ```

* **List/collection names are always plurals (ending with an "s").**

    ```java
    // Java
    ArrayList<ListButton> buttons = new ArrayList<ListButton>();
    ```

    ```typescript
    // TypeScript
    const boxes:Array<DisplayBox> = new Array<DisplayBox>();
    ```

    ```javascript
    // JavaScript
    const names = [];
    ```

* **When looping through a list, temporary names for the item at a given position (when used) are the singular of the list name.**

    ```typescript
    // Normal loop
    const buttons:Array<PageButton> = this.getListOfButtons();
    buttons.forEach(button => {
        button.doSomething();
    });
    ```

* **#### Booleans start with `is` or `has` (even if it's a getter).** Try to avoid other prefixes. `was` doesn't have a clear connotation; if something "was destroyed" but was brought back to life, `wasDestroyed` is technically true, but doesn't reflect its current state. Something like `wasDestroyedBefore`, on the other hand, works.

    ```javascript
    let isOld = false;
    let hasChildren = true;
    let hasLoggedLunch = false;
    let isDestroyed = true;
    ```

* **Temporary methods and variables used for testing should start with `debug_`**. Those should not make into production, or should only be used in debug states.

    ```javascript
    let debug_stats = [...];
    console.log(debug_totalMemory);
    debug_restart();
    debug_simulateUserEntry({...});
    ```


## Getter/setters

* **Getters/setters are a good thing if used wisely**. Getter/setters are good especially if you're tweening a value, since they can be used more transparently by tweening engines.

* **Getters/setters should only exist if both the getter and the setter exist. They should act invisibly, like a normal variable would**. This avoids confusion on whether a variable can be set or not, and what the alternative is.

* **Getters/setters should always refer to a private property, or some other private member. The private variable has the same name, but starts with an underline.** If the getter requires more calculation or is not based in a single property, it should be a clear getter function.

    ```typescript
    // OK, private
    public get maxWidth():number {
        return this._maxWidth;
    }
    public set maxWidth(value:number) {
        this._maxWidth = value;
    }

    // OK, calculated/proxied
    public get numItems():number  {
        return this._items.length;
    }
    public set numItems(value:number) {
        this._items.length = value;
    }

    // More complex, uses a clear function
    public getValidItems():Array<Item> {
        return this._items.filter(item => item.isValid);
    }
    ```

* **In languages that need it, the name of the parameter received by the setter method is always `value`.** This avoids conflicts and confusion with local naming of properties.

* **Getter/setters should have no [side-effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science%29)).** As often as possible, the getter/setter functions should not change any of its instance's state other than the obvious changes required by the setter.

    The important exception for this is when getter/setters are used for a value that's not directly tied to a variable, but is instead calculated on the fly as necessary and often cached.

    ```typescript
    public get numSelectedItems():number  {
        if (this._isNumSelectedItemsDirty) {
            // Recalculate number
            this._numSelectedItems = 0;
            this._items.forEach(item => {
                if (item.isSelected) _numSelectedItems++;
            });
            // Mark as calculated
            this._isNumSelectedItemsDirty = false;
        }
        return _numSelectedItems;
    }
    ```

    Often, however, such getter/setters should be used as methods/functions rather than native getter/setters.

* **If a property only has a getter or a setter, use normal methods/functions (Java style) instead of native getter/setters.** This makes it clear it is read-only.

    ```typescript
    public getNumItems():number  {
        return this._list.length;
    }
    ```

    The lone setter is more rare, because it would mean you're setting something that can't be read later (can't think of a solid reason for that behavior).

    ```typescript
    public setAlpha(value:Number) {
        this._alpha = value;
    }
    ```

## TODO

Everything below is temporary.

Function/method naming
----------------------

Same as variables.

In C#, public methods and properties and functions start with an uppercase letter.

Class
-----

Order:
 * public variables
 * protected variables
 * private variables
 * public methods
 * protected methods
 * private methods

Other
-----

float f = 1.0f; // explicit scope

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
