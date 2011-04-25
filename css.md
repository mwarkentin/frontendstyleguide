# Frontend Style Guide

## CSS

* Top boilerplate commenting: You can make an English-->hex colour quickref if helps for copying and pasting (obviously assuming you're not using some kind of CSS preprocessor). Don't worry about overly-complicated tables of content; Firebug/Inspector will tell you what line to get to faster. Organizing CSS, either within a file or across multiple, is beyond the scope of this document.

* Standard CSS definitions: selector and opening brace on one line, new indented lines for each style declaration, closing brace on new line. Some people prefer writing CSS all on one line, but this makes reading changesets difficult.

**No**

    .something
    {
        font-size: 10px;
    }

    .otherthing { font-size: 10px; color: #090; line-height: 24px; font-weight: bold; ... }

    .lastthing{
        font-style:italic;
    padding: 5px; }

**Yes**

    .something {
        font-size: 10px;
    }

    .otherthing {
        font-size: 10px;
        color: #090;
        line-height: 24px;
        font-weight: bold;
        ...
    } /* skip the line break if you want to visually "attach" related groups together */
    .lastthing {
        font-style:italic;
        padding: 5px;
    }

* CSS definitions with more than one selector: each on one line.

**No**

    .something, .otherthing, .lastthing {
        font-weight: bold;
    }

**Yes**

    .something,
    .otherthing,
    .lastthing {
        font-weight: bold;
    }

* A single line declaration is ok in limited cases, especially for overrides. Preferably only one style definition.

**Yes**

    p {
        font-size: 12px;
        font-weight: normal;
        line-height: 24px;
        color: #333;
        padding-bottom: 12px;
    }
    p.crazy { color: #00decaf; }

* Avoid unnecessary selectors, unless it clearly helps clarify.

**No**

    body.account table#MyDiv tr td.foo { ...

**Yes**

    .account #MyDiv .foo { ...

* Use upper camelcase for ids, and lowercase separated by dashes for classes. Even though we have `#` vs `.` to make this distinction, the naming convention makes things much more obvious. Compare:

    .default-box {
        color: #333;
    }
    #some-crazy-box {
        padding: 20px;
        border: 1px solid green;
        font-weight: bold;
    }
    .common-other-box {
        background: #ccc;
        padding: 8px;
        margin-bottom: 20px;
    }

versus

    .default-box {
        color: #333;
    }
    #SomeCrazyBox {
        padding: 20px;
        border: 1px solid green;
        font-weight: bold;
    }
    .common-other-box {
        background: #ccc;
        padding: 8px;
        margin-bottom: 20px;
    }

* Spent a lot of time in Photoshop preparing an image? Please add that raw file to the repository, someone may come along behind you and need to make a change.

* It should not be necessary to have CSS either inline in an element (add an id or class and write CSS as appropriate; use the predefined "hidden" CSS class or equivalent, instead of `style="display: none;"`, etc) or in the `<head>` section of individual pages (exception: something that needs to be dynamically rendered).

* Specify font-sizing, margin, padding, etc in pixels (`px`) rather than ems (`em`). It makes math much easier to do, is easier to quickly understand (where the dimensions of `0.5em` is dependent on context) and modern browsers can scale the page appropriately.



* Use shorthand for margin, padding, border, background, font, etc declarations.

**No**

    .some-div {
        margin-top: 10px;
        margin-right: 20px;
        margin-bottom: 15px;
        margin-left: 30px;
    }

**Yes**

    .some-div {
        margin: 10px 20px 15px 30px;
    }

* Where shorthand does *not* appear, it's a strong indicator that you are defining overrides or updating only one portion of a declaration:

    #SomeDiv {
        margin: 10px 20px 15px 13px;
    }
    .foo-page #SomeDiv {
        margin-top: 5px;
        margin-bottom: 5px;
    }

    a.weird {
        background: #900 url(../images/sprite-whatever.png) no-repeat left top;
    }
    a.weird:hover {
        background-position: right bottom;
    }

* Use relative path names in background URL definitions -- not full URLs starting with `http://` (unless for some reason the asset is truly offsite) and not absolute paths starting with `/` (which means you are likely hardcoding the `MEDIA_URL`)
