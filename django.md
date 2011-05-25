# Frontend Style Guide

## Django

* Templates should go in the main `templates` directory (`settings.TEMPLATE_DIRS`) organized by app name, not inside a `templates` folder inside each individual app. Even though the latter will work (usually because `settings.TEMPLATE_LOADERS` is set to look in both places), ideally they should all be in one place.

* Template inheritance:

        base.html       // sitewide elements (<head>, <body>, major containers, etc)
        app1
            base.html   // app1 specific stuff
            foo.html    // actual page contents
            bar.html
        app2
            base.html
            what.html
            yeah.html

* Use a wrapper template block to be able to turn off inclusion of particular elements (named after what it is). Define an internal content block to put stuff in (named `foo_content`).

        {% block sidebar %}
            <div class="whatever foo">
                {% block sidebar_content %}
                {% endblock %}
            </div>
        {% endblock %}

        // subtemplates can now turn off the sidebar:
        {% block sidebar %}{% endblock %}
        // or put content in
        {% block sidebar_content %}
            ... stuff ...
        {% endblock %}

* Add a template block for insertion of a CSS class to define what page you're on. This is useful for doing app-level things like turning a nav highlight on, and for page level overrides (this also makes overrides more obvious in CSS, and if they pile up it's a good indication that something needs refactoring). If you do this elsewhere, name the block `foo_class`.

        // in base.html
        <body class="foo bar {% block body_class %}{% endblock %}">...

        // in app1/base.html
        {% block body_class %}{{ block.super }} app1-section{% endblock %}

        // in app1/foo.html
        {% block body_class %}{{ block.super }} foo-page{% endblock %}

* Include the name in the `endblock` tag, unless it's a oneliner:

    **No**

        {% block something %}
            tons
            of
            lines
            of
            {% block nested %}
                holy
                cats
            {% endblock %}
            code
            seriously
        {% endblock %}

    **Yes**

        {% block something %}
            tons
            of
            lines
            of
            {% block nested %}
                holy
                cats
            {% endblock nested %}
            code
            seriously
        {% endblock something %}

        {% block quickie %}foo{% endblock %}

* Top load statements go on one line each. Sort alphabetically for bonus points.

    **No**

        {% load foo bar i18n whatever %}

    **Yes**

        {% load bar %}
        {% load foo %}
        {% load i18n %}
        {% load whatever %}

* In subtemplates, separate major content blocks with two lines, and put short oneliner blocks near the top.
