============
Api analysis
============

This document aims to cover in depth analysis of routeros API. Lines that begin with ``--->`` represent data received from a device. Linet that start with ``<---`` represent data send to a device. End of sentence is marked with ``EOS``.

----------------
Succesfull login
----------------
.. code-block:: none

    <--- /login
    <--- EOS
    ---> !done
    ---> =ret=xxxxxxxxxxxxxxxxxxxxx
    ---> EOS
    <--- /login
    <--- =name=admin
    <--- =response=xxxxxxxxxxxxxxx
    <--- EOS
    ---> !done
    ---> EOS

--------------------
Failed login attempt
--------------------
.. code-block:: none

    <--- /login
    <--- EOS
    ---> !done
    ---> =ret=xxxxxxxxxxxxxxxxxxxxx
    ---> EOS
    <--- /login
    <--- =name=admin
    <--- =response=xxxxxxxxxxxxxxxxxxxxx
    <--- EOS
    ---> !trap
    ---> =message=cannot log in
    ---> EOS
    ---> !done
    ---> EOS

-----------
Logging off
-----------
.. code-block:: none

    <--- /quit
    <--- EOS
    ---> !fatal
    ---> session terminated on request
    ---> EOS

------------------------
Multiple empty responses
------------------------
.. code-block:: none

    <--- /ip/service/print
    <--- =.proplist=comment
    <--- EOS
    ---> !re
    ---> EOS
    ---> !re
    ---> EOS
    ---> !re
    ---> EOS
    ---> !re
    ---> EOS
    ---> !re
    ---> EOS
    ---> !re
    ---> EOS
    ---> !re
    ---> EOS
    ---> !done
    ---> EOS

--------------
Adding element
--------------
.. code-block:: none

    <--- /ip/address/add
    <--- =address=192.168.1.1/24
    <--- =interface=ether1
    <--- EOS
    ---> !done
    ---> =ret=*3
    ---> EOS

-----------------------
Multiple error messages
-----------------------
.. code-block:: none

    <--- '/ip/aasasa/p'
    <--- EOS
    ---> '!trap'
    ---> '=category=0'
    ---> '=message=no such command or directory (aasasa)'
    ---> EOS
    ---> '!trap'
    ---> '=message=no such command prefix'
    ---> EOS
    ---> '!done'
    ---> EOS

--------------
Print with tag
--------------
.. code-block:: none

    <--- /ip/address/print
    <--- .tag=2
    <--- EOS
    ---> !re
    ---> =.id=*1
    ---> =address=172.30.30.1/24
    ---> =network=172.30.30.0
    ---> =interface=bridge1
    ---> =actual-interface=bridge1
    ---> =invalid=false
    ---> =dynamic=false
    ---> =disabled=false
    ---> .tag=2
    ---> EOS
    ---> !re
    ---> =.id=*7
    ---> =address=10.10.10.1/24
    ---> =network=10.10.10.0
    ---> =interface=ether5
    ---> =actual-interface=ether5
    ---> =invalid=false
    ---> =dynamic=true
    ---> =disabled=false
    ---> .tag=2
    ---> EOS
    ---> !done
    ---> .tag=2
    ---> EOS

-------------
Query example
-------------

Print all interfaces that are of type ether OR vlan OR bridge AND not disabled AND not slave. Return only key ``name``

.. code-block:: none

    <--- '/interface/print'
    <--- '=.proplist=name'
    <--- '?-slave'
    <--- '?=disabled=no'
    <--- '?=type=bridge'
    <--- '?=type=ether'
    <--- '?=type=vlan'
    <--- '?#|'
    <--- '?#|'
    <--- '?#&'
    <--- '?#&'
    <--- EOS
    ---> '!re'
    ---> '=name=ether1'
    ---> EOS
    ---> '!re'
    ---> '=name=ether2'
    ---> EOS
    ---> '!done'
    ---> EOS


