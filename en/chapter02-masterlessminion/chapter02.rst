The Masterless Minion
=====================

The simplest configuration to get started with is the ``masterless minion``.
This is simply a minion server where you call all of your commands as well
as configuration files locally. This server does not use key authentication,
since it's just the ``minion`` itself. What makes this a great starting place
for this book is that the ``masterless minion`` takes about 5 minutes to set
up. In this chapter we'll talk about how to set up our ``minion``, run some
commands, and write a few simple configuration files. By the time you're done
with this chapter you'll have a simple web server set up that actually shows
a static web page.


Setting Up The Salt Minion
==========================

There are multiple ways to set up the Salt minion. The quickest way to do so
is to use the salt bootstrap script:

.. code-block:: bash

    curl -L http://bootstrap.saltstack.org | sudo sh

This method allows you to quickly install the Salt minion onto a given machine.
In the event you wish to use a repository, examples have been provided below
which explain how to install the minion on RHEL and Debian distros.

RHEL Distros
------------

Unless you're using Fedora we'll need to install the EPEL repository. You'll
do this via running:

.. code-block:: bash

    yum --enablerepo=epel-testing install salt-minion


If for some reason that doesn't work, you'll have to set up the repo manually:

.. code-block:: bash

    rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/6/i386/epel-release-6-8.noarch.rpm
    yum clean all; yum install salt-minion

.. note::

    If you're on RHEL you'll need to enable the 'optional' repo, this is due
    to a naming issue of with Jinja2.

While installing the EPEL repo you may get a key error, if so, download the
latest key:

.. code-block:: bash

    install the key

Debian Distros
--------------

In ``/etc/apt/sources.list``, or a file within ``/etc/apt/sources.list.d`` if
you prefer, add the following line:

.. code-block:: bash
    
    deb http://debian.saltstack.com/debian wheezy-saltstack main

From there import the repository key:

.. code-block:: bash

    wget -q -O- "http://debian.saltstack.com/debian-salt-team-joehealy.gpg.key" | apt-key add -

Update the database, and install the minion:

.. code-block:: bash

    apt-get update
    apt-get install salt-minion

So now that our ``Salt Minion`` is installed, we need to start the service up.
It should have already started when you installed the package, but in the
event it has not, run the following command:

.. code-block:: bash

    service salt-minion start

Our setup is now functional, and we can start running commands!


Running Your First Local Commands Using Salt Modules
====================================================

When running Salt locally (without a master), we'll be using the ``salt-call``
command. This command is specifically used to run calls on the minion
itself instead of executing them from the master.

We'll begin with an easy example, with a simple package installation. To do
this run the following command:

.. code-block:: bash

    salt-call --local pkg.install vim-enhanced

Ok so let's break this down, ``salt-call`` was explained above, but when you
look at the ``--local`` option it seems as though this is a duplicate of
``salt-call``. The key item to remember with ``salt-call`` is that you're
executing FROM the minion, yet you can still rely on data from the master. The
``--local`` option is specifically to run ``salt-call`` locally, as if there
was no master running. This means that all the data and configuration will be
pulled from the minion itself. ``pkg.install`` does exactly what it sounds
like, it installs a pkg. Keep in mind that when you run something like this
from the command line, you're using the ``execution module``. From there we
simply provide the command with an option (in this case ``vim-enhanced``)
for what we want to install.

The difference between Salt States, and Salt Modules
====================================================

One of the most confusing parts of Salt for new users is the difference
between an ``execution module`` and a ``state module``. Think of an
``exeuction module`` as the underlying layer of actions to be performed, and
the ``state modules`` invoke them. These different types of modules are
commonly referred to as states, and modules (or execution modules). This can be
confusing as a state contains multiple ``state modules``. So this
brings about another question, why is everything that occurs in a module not
supported in a state (or vice versa)? The reasoning behind this is that some
things simply don't belong in states, or they wouldn't work correctly.

A vast majority of actions that Salt performs are completed in States, and that
is what 90% of what you're going to write will be. We aren't going
to focus too heavily on ``execution modules``. Modules are most often used for
one off commands. and troubleshooting which we'll cover later. The main take
away here is to make sure when you're looking at the Salt documentation that
you recognize that both Module and State documentation can
exist for something that seems similar, so there's the pkg module
documentation which looks like this: 

http://docs.saltstack.com/en/latest/ref/states/all/salt.states.pkg.html

And the pkg state module which looks like this:

http://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.pkg.html

Be aware of what you're looking at, otherwise you might try to use
functionality that doesn't exist in a state.

Writing Your First State Files and a YAML Intro
===============================================

Before we get too deep into state files, let's take a look at some YAML syntax
with a very simple state example:

.. code-block:: yaml

    nginx:
      pkg:
        - installed
      service:
        - running
        - enable: True

In this state we're simply installing a package, and starting the service. It
isn't very complex so that should make it easier to understand what is going
on within the state itself. As you can see above, we use the ``:`` to denote a
sub-section, or an associated value of some kind, everything is indented two
spaces for the sub-sections. Most text editors support some sort of YAML 
implementation which should make it easier to see what is going on.

We're specifying the item to be installed as nginx, from here, we want the
status of the package to be installed, and the service to be running, and
enabled. It's very simple to see what's going on. Keep in mind that states
can become very complex.

Since we want to use this nginx state, let's put it inside of
``/srv/salt/nginx/init.sls``. When we name something 'init' it means that Salt
will treat the directory it sits in as the name, so if I wanted to use this
init, I would simply reference nginx like so:

.. code-block:: yaml

    salt-call --local state.sls nginx

If we had placed this file within ``/srv/salt/nginx/package.sls``, we would
reference it like this:

.. code-block:: yaml

    salt-call --local state.sls nginx.package

Easy to understand right? We're simply replacing the directory (``/``) with a
dot.


Writing Your First Top File
===========================

The top file (top.sls) is quite simple in what it is, and how it works. This
is simply a file that says 'apply these states, to these machines'. It's also
formatted with YAML, and operates similarly to a state file. Add this example
to your server under ``/srv/salt/top.sls``:

.. code-block:: yaml

    base:
      '*':
        nginx

So let's look at what's going on here, we have our base environment(more on
environments later), and as part of that environment, we match ALL systems.
The star represents every server that Salt knows about. Since that is
currently only one system, this would represent one machine. The last section
(remember our indenting, and that ':' represents that an item has sub items),
indicates that we want to apply the nginx state to the servers in question.

Chapter Overview
================

So we've gotten into the basics regarding states, top files, and YAML. At this
point you're probably saying 'jeez this seems pretty easy', and that's because
what we've done so far is very easy. Work through the chapter challenge below
and ensure that it works before going forward, you may need to head online to
the Salt docs to take a look at how some of these work, if you get stuck feel
free to review the repository (REPO LINK HERE), as it includes the solution to
the chapter challenge.


Chapter Challenge
=================

1. Review the pkg module documentation
(http://docs.saltstack.com/ref/modules/all/salt.modules.pkg.html), and compare
it to the pkg state documentation
(http://docs.saltstack.com/ref/states/all/salt.states.pkg.html), note the
differences in both the documentation, and the functionality.

2. Review some of the example projects where Salt is used
(http://docs.saltstack.com/topics/salt_projects.html), and try to see what's
going on, make some notes regarding what you don't understand.

3. Configure the masterless minion to have a secondary HTML file, and ensure
that the Nginx service watches this file. What do you notice is problematic
about these service watch commands? Review
http://docs.saltstack.com/ref/states/requisites.html to see if there's a more
efficient way we could take advantage of watch, or it's alternatives.

4. Create an additional directory structure for Python, and create the
necessary states to install virtualenv and pip. Do these all belong in the
same state? Think carefully on what our directory structure should look like
to ensure these are as modular as possible so we can use them repeatedly.
