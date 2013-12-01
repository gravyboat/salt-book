The Masterless Minion
=====================

The simplest configuration to get started with is the ``masterless minion``.
This is simply a minion server, where you call all of your commands, as well
as configuration files locally. This server does not use key authentication,
since it's just the ``minion`` itself. What makes this a great starting place
for this book is that the ``masterless minion`` takes about 5 minutes to set
up. In this chapter we'll talk about how to set up our ``minion``, run some
commands, and write a few simple configuration files. By the time you're done
with this chapter you'll have a simple web server set up that actually shows
a static web page.


Setting Up The Salt Minion
==========================

RHEL Distros
------------

Unless you're using Fedora we'll need to install the EPEL repository. You'll
either do this via running:

.. code-block:: bash

    yum --enablerepo=epel-testing install salt-minion


If for some reason that doesn't work, you'll have to set up the repo:

.. code-block:: bash

    rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/6/i386/epel-release-6-8.noarch.rpm


While installing the EPEL repo you may get a key error, if so, download the
latest key:

.. code-block:: bash

    install the key

Debian Distros
--------------


So now that our ``Salt Minion`` is installed, we need to start the service up.
It should have already started when you installed the package, but in the
event it has not, run the following command:

.. code-block:: bash

    service salt-minion start


Running Your First Local Commands Using Salt Modules
====================================================

When running Salt locally (without a master), we'll be using the ``salt-call``
command. This command is specifically used to run calls on the minion
itself instead of executing them from the master.


The difference between Salt States, and Salt Modules
====================================================

One of the most confusing parts of Salt for new users is the difference
between a ``module`` and a ``state``? Think of a module as the underlying
layer of actions to be performed, and the states invoke them. So this brings
about another question, why is everything that occurs in a module not
supported in a state? The reasoning behind this is that some things simply
don't belong in states, or they wouldn't work correctly. 

A vast majority of actions that Salt performs are completed in States, and that
is what 90% of what you're going to write commands of will be. We aren't going
to focus too heavily on modules as they aren't used that often, modules are
most often used for one off commands, and troubleshooting, which we'll cover
later. The main take away here is to make sure when you're looking at the Salt
documentation that you recognize that both Module and State documentation can
exist for something that seems similar, so there's the pkg module, and the pkg
state. Be aware of what you're looking at, otherwise you might use
functionality that doesn't exist in a state!

Writing Your First State File and a YAML Intro
==============================================

Before we get too deep into state files, let's take a look at some YAML syntax
with a very simple state example:


In this state we're simply installing a package, it isn't very complex so that
should make it easier to understand what is going on within the state itself.
As you can see above, we use the ``:`` to denote a sub-section, or an
associated value of some kind, everything is indented two spaces for the
sub-sections. Most text editors support some sort of YAML implementation which
should make it easier to see, 


Writing Your First Top File
===========================


Chapter Challenge
=================

1. Configure the masterless minion to have a secondary HTML file, and ensure
that the Nginx service watches this file. What do you notice is problematic
about these service watch commands? Review
http://docs.saltstack.com/ref/states/requisites.html to see if there's a more
efficient way we could take advantage of watch, or it's alternatives.

2. Create an additional directory structure for Python, and create the
necessary states to install virtualenv and pip. Do these all belong in the
same state? Think carefully on what our directory structure should look like
to ensure these are as modular as possible.