Advanced Top Files and Pillars
==============================

In this chapter we'll start to understand what Pillars are and how we can use
them to our advantage. We'll also review the basics of Jinja2 templating as
it will be required to craft more complext states and functionality. Top files
will also be revisited where we'll put some of what we've learned into practice
to extend and diversify the functionality of our top file.

Introduction to Pillars
=======================

Pillars are a way which we store data within Salt that we may need to keep
secure. Pillars should not be used for 

Introduction to Jinja2
======================

Jinja2 (referred from this point on simply as Jinja) is a templating
language for Python. We'll review some basic Jinja syntax to get you started.
Additional syntax can be reviewed at http://jinja.pocoo.org/docs/dev/ . Jinja's
use inside of Salt is to apply and assign variables, as well as performing
logic. The documentation provided above is very thorough, but we will review
uses in Salt that will get you started.

The Basics
----------

Let's start by making our index.html file a little more interesting. Open
``/srv/salt/nginx/files/index.html`` on the ``master`` and let's learn about
Jinja. We need to start by understanding the types of delimiters that exist
inside of Jinja. The main two types that we will be using for Salt are:

Statements:

.. code-block:: jinja

    {% %}

and Expressions:

.. code-block:: jinja

    {{ }}

If those names don't make sense to you think of statements as logic to perform
and expressions as variable references.

Let's look at ``/srv/salt/nginx/files/index.html`` as it currently exists:

.. code-block:: html

    Hello world!

As we already saw this is extremely simple, let's make it a little more
interesting. Let's start by using a statement to assign a variable a name:

.. code-block:: jinja

    {% set name = 'Jake' %}

    Hello world!

So what we've done above is set a variable called name to 'Jake'. We can now
reference this variable in our html file:

.. code-block:: jinja

    {% set name = 'Jake' %}

    Hello {{ name }}!

Now save this file, and apply it to our minion with:

``salt 'minion_name' state.sls nginx.site``

Now when you visit the web page you should see:

.. code-block:: html

    Hello Jake!

Keep in mind this assignment is only valid within this file. We'll discuss
ways to push this data into other states and files later.

Loops
-----

More Salt Examples
------------------




Top Files
=========

Environments
------------

Grain Matching
--------------

Pillar Matching
---------------

Compound Matching
-----------------

