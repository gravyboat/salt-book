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

Keep in mind we'll be building on the previous tasks we've completed, however
for the sake of avoiding swapping between pages the instructions on how to add
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

Configuring Our Salt Minion and Salt Master for connectivity
============================================================

Once you've installed your Salt Master, it's time to configure the minion to
connect to the master server. This process is quite straight forward, but
we'll need to make sure that ports 4505, and 4506 are open on the master. If
you aren't familiar with how to do this, there are instructions below (you may
need elevated privileges to run these commands):

iptables
---------

iptables -A INPUT -p tcp --dport 4505 -j ACCEPT
iptables -A INPUT -p tcp --dport 4506 -j ACCEPT

Confirm that you can telnet from the minion to the master on those ports, if
you are having issues you may need to ensure that the iptables filtering is
configured properly so that this chain is encountered prior to dropping all
packets.

We now need to modify the /etc/salt/minion file on the Salt minion. This will
allow us to point our minion at the master, so we can start the authentication
process.

The minion conf
---------------

Open the /etc/salt/minion conf with your preferred editor, and find the line
that says ``#master: salt``. You'll want to uncomment this line, and change
the value of master to either your master server's IP address, or the server's
name depending on how your network is configured. Once you've done this, save
the file, and restart the minion with ``service salt-minion restart``, again
this may require escalated privileges if you aren't the root user.

Ok, our minion now has a basic configuration, and should be able to talk to
our Salt master, if it seems easy that's because it is supposed to be.

Accepting Keys From the Salt Minion
===================================

Once you've completed configuring the Salt Minion, you'll need to tell your
Salt master that you want to allow this minion to connect. Log onto the salt
master, and as the root user, or a user with sudo run the following command:

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


Now if you run ``salt-key -L`` again, you'll see that the minion is now 
accepted. To test this out we'll run the simplest command we can from
the master to confirm connectivity:

.. code-block:: bash

    salt '*' test.ping

We're using the salt command here to confirm connectivity by targeting all
('*') machines, and running test.ping against them. This simply confirms
the system is available via a ZeroMQ connection.

Moving The Salt States and Top File
===================================

At this point we need to move our top file and states over to the master
server. These files will exist in the exact same location that they
currently exist on the minion, so scp the files over, and see if you can
apply them to the minion.


Introduction to Pillars
=======================

Pillars are a way which we store data within in Salt that we may need to keep
secure.

Introduction to Jinja2
======================


Chapter Challenge
=================

1. Create a second minion, and join it to the ``Salt Master``. Configure
this minion 