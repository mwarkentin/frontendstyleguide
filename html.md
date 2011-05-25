# Frontend Style Guide

## HTML

* Properly indent nested elements, which helps readability, adding new elements, and in catching missing closing tags.

    **No**

        <div id="Foo">
            <span class="bar">Something</span></div>
            <div id="Asdf">
                Something
        </div>

    **Yes**

        <div id="Foo">
            <span class="bar">Something</span>
        </div>

        <div id="Asdf">
            Something
        </div>

* Properly indent inside `if` statements and other template control blocks (eg `with`, `for`, etc), without worrying about maintaining indentation of the surrounding HTML structure. This makes the unique sections of the template's flow more obvious to parse visually (although it can make a changeset more cumbersome to read).

    **No**

        <ul id="Something">
            <li>Foo</li>
            {% if baz %}
            <li>Baz</li>
            {% else %}
            <li>Bar</li>
            {% endif %}
            <li>Quux</li>
        </ul>

    **Yes**

        <ul id="Something">
            <li>Foo</li>
            {% if baz %}
                <li>Baz</li>
            {% else %}
                <li>Bar</li>
            {% endif %}
            <li>Quux</li>
        </ul>

* Naming convention 1: name ids with upper camelcase, and classes with lowercase separated by dashes. This helps with CSS legibility (see CSS doc). +++

* Naming convention 2:

        form ==> FooForm
        list ==> FooList or FooNav
        div ==> FooContainer
        etc

* We care about code validity not from a purist perspective but only in as much as it helps catch obvious bugs.

* Try to avoid adding elements unnecessarily, especially as hooks for styling or scripting purposes only.

* Be aware of when using an id to address elements is more concise.

    **No**

        <ul>
            <li class="something">...</li>
            <li class="something">...</li>
            <li class="something">...</li>
            <li class="something">...</li>
            <li class="something">...</li>
        </ul>

        li.something {
            ...
        }

    **Yes**

        <ul id="Something">
            <li>...</li>
            <li>...</li>
            <li>...</li>
            <li>...</li>
            <li>...</li>
        </ul>

        #Something li {
            ...
        }

* Be aware of the opposite case, when adding class names can enhance CSS reuse (ie., perhaps you don't need to write any new CSS, and classes can be combined to give you the desired effect):

    **No**

        <div id="MyCustomOneOffBox">...</div>

    **Yes**

        <div class="box round alert message whatever">...</div>

* Properly quote element attributes if they are being substituted somehow with untrusted user-generated content. This can be a potential security issue.

    **No**

        <img src={{ image.url }} alt={{ image.alt_text }} />
        // if alt_text is `"" onclick="...malicious js..."`, you're hosed

    **Yes**

        <img src="{{ image.url }}" alt="{{ image.alt_text }}" />
