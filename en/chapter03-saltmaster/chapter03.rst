The Salt Master
===============

In this chapter we'll be setting up our ``Salt master`` and hooking up our
existing ``minion`` to the ``master``. This means we can start running
commands, and treating that server as though it's part of an actual 
infrastructure. Our ``master`` server will be very similar spec wise to the
``minion`` that was previously created, as we won't need a lot of resources
for what we'll be doing. When you finish this chapter you'll have a ``master``
server, and a single ``minion`` that you can control from the master.


Setting Up the Salt Master
==========================

Keep in mind we'll be building on the previous things we've done, however for
the sake of avoiding swapping between pages the instructions on how to add
repositories is also included here.

RHEL Distros
------------

Unless you're using Fedora we'll need to install the EPEL repository. You'll
either do this via running:

.. code-block:: bash

    yum --enablerepo=epel-testing install salt-master


If for some reason that doesn't work, you'll have to set up the repo:

.. code-block:: bash

    rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/6/i386/epel-release-6-8.noarch.rpm
    yum clean all; yum install salt-master

.. note::

    If you're on RHEL you'll need to enable the 'optional' repo, this is due
    to a naming issue of with Jinja2.

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

Configuring Our Salt Minion To Recognize the Salt Master
========================================================

Once you've installed your Salt Master, it's time to configure the minion to
connect to the master server. This process is quite straight forward, but
we'll need to make sure that ports 4505, and 4506 are open on the master. If
you aren't familiar with how to do this, there are instructions below.

RHEL Distros
------------

Debian Distros
--------------


We now need to modify the /etc/salt/minion file on the Salt minion. This will
allow us to point our minion at the master, so we can start the authentication
process.

Accepting Keys From the Salt Minion
===================================

Once you've completed configuring the Salt Minion, you need to access the Salt
Master. Once there, as the root user run the following command:

.. code-block:: bash

    salt-key -L


When you run this command you should see something similar to the following:

.. code-block:: bash

    return data from salt-key command


When we use a capital ``L`` for the salt-key command, this says to list all
keys, both those accepted, and those waiting to be accept. We now need to 
accept the key, use the following command to do so:

.. code-block:: bash

    salt-key -a serverName


Now if you run ``salt-key -L`` again, you'll see that the minion is now an
accepted server. To test this out we'll run the simplest command we can from
the master to test connectivity:

.. code-block:: bash

    salt '*' test.ping

This simply has the minion return information via ZeroMQ that it is indeed
connected to the master

Moving The Salt States and Top File
===================================


Introduction to Pillars
=======================


Introduction to Jinja2
======================


Chapter Challenge
=================

1. Create a second minion, and join it to the ``Salt Master``. Configure
this minion 