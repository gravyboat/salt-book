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

    install the repo via RPM

While installing the EPEL repo you may get a key error, if so, download the
latest key:

.. code-block:: bash

    install the key

Debian Distros
--------------


Running Your First Local Commands Using Salt Modules
====================================================


Writing Your First State File and a YAML Intro
==============================================


Writing Your First Top File
===========================


Chapter Challenge
=================