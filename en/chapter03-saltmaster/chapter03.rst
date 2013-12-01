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

Debian Distros
--------------


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