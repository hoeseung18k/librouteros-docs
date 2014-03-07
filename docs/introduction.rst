Introduction
============


Features
--------

* Python type casting
* Key and values stored in dictionary
* Source address, port specification
* Logging support

Limitations
-----------

* No support for sentence tagging.
* No asynchronous support for reading/writing
* No TLS/SSL socket encryption
* No Diffie Hellman negotiation

Requirements
------------

* At least Python 3.2 or newer.
* Mock 1.0.1 ( for runing unit tests ) or higher.
* nosetests3 for runing unit tests.

Unit tests
----------

This library comes with unit tests included. To run them:
::

    nosetests3 unit_tests/*
