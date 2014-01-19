Library usage
=============

First of all import basic function to make connection.
::

    from librouteros import connect
    conn = connect( '127.0.0.1' )

This function will try to connect to specified device. You can specify FQDN, ipv4/ipv6 address. If everything went fine it will return a connection class.

Logging in
----------

After connecting to a device you need to login to be able to issue commands. If you do not have any password just leave it as empty string.
::

    api = conn.login( 'username', 'password' )

Type casting
------------

Library automaticly casts values to/from python equivalents. Table below specifies how values are casted.

.. csv-table::
    :header: "Python", "Api"

    "False", "no/false"
    "True", "yes/true"
    "None", "''"
    "int",  "'int'"

Any type not specified in table above is converted to str.
Keys that starts with ``=.`` or ``.`` are converted to uppercase.

Printing elements
-----------------

::

    api.getall( '/system/logging/action' )

Result may contain 0 or more elements.
::

    ({'ID': '*0',
    'default': True,
    'memory-lines': 100,
    'memory-stop-on-full': False,
    'name': 'memory',
    'target': 'memory'},
    {'ID': '*1',
    'default': True,
    'disk-file-count': 2,
    'disk-file-name': 'log',
    'disk-lines-per-file': 100,
    'disk-stop-on-full': False,
    'name': 'disk',
    'target': 'disk'},
    {'ID': '*2',
    'default': True,
    'name': 'echo',
    'remember': True,
    'target': 'echo'},
    {'ID': '*3',
    'bsd-syslog': False,
    'default': True,
    'name': 'remote',
    'remote-port': 514,
    'src-address': '0.0.0.0',
    'syslog-facility': 'daemon',
    'syslog-severity': 'auto',
    'syslog-time-format': 'bsd-syslog',
    'target': 'remote'})

You may get additional information such as stats.
::

    api.getall( '/interface/ethernet', args={'stats':True} )

    (...... ,
    {'ID': '*1',
    'arp': 'enabled',
    'auto-negotiation': True,
    'bandwidth': 'unlimited/unlimited',
    'default-name': 'sfp1',
    'disabled': True,
    'driver-rx-byte': 0,
    'driver-rx-packet': 0,
    'driver-tx-byte': 0,
    'driver-tx-packet': 0,
    'full-duplex': True,
    'l2mtu': 1598,
    'mac-address': 'D4:CA:6D:86:ED:04',
    'master-port': 'none',
    'mtu': 1500,
    'name': 'sfp1',
    'orig-mac-address': 'D4:CA:6D:86:ED:04',
    'running': False,
    'rx-1024-1518': 0,
    'rx-128-255': 0,
    'rx-1519-max': 0,
    'rx-256-511': 0,
    'rx-512-1023': 0,
    'rx-64': 0,
    'rx-65-127': 0,
    'rx-align-error': 0,
    'rx-broadcast': 0,
    'rx-bytes': 0,
    'rx-fcs-error': 0,
    'rx-fragment': 0,
    'rx-multicast': 0,
    'rx-overflow': 0,
    'rx-pause': 0,
    'rx-too-long': 0,
    'rx-too-short': 0,
    'sfp-rate-select': True,
    'speed': '100Mbps',
    'switch': 'switch1',
    'tx-1024-1518': 0,
    'tx-128-255': 0,
    'tx-1519-max': 0,
    'tx-256-511': 0,
    'tx-512-1023': 0,
    'tx-64': 0,
    'tx-65-127': 0,
    'tx-broadcast': 0,
    'tx-bytes': 0,
    'tx-collision': 0,
    'tx-deferred': 0,
    'tx-excessive-collision': 0,
    'tx-excessive-deferred': 0,
    'tx-late-collision': 0,
    'tx-multicast': 0,
    'tx-multiple-collision': 0,
    'tx-pause': 0,
    'tx-single-collision': 0,
    'tx-too-long': 0,
    'tx-underrun': 0},
    ..... )

Adding element
--------------

When adding element api always returns newly created ID. You can reference element with this ID.
::

    data = { 'interface':'ether1', 'address':'172.31.31.1/24' }
    api.add( '/ip/address', data )
    '*23'

Removing element
----------------

You can remove multiple elements. Always refer to them with ID.
::

    idlist = ( '*12', '*1' )
    api.remove( '/ip/address', idlist )

Removing single element.
::

    api.remove( '/ip/address', '*33' )

Setting element
---------------

Some menu levels do not have ID. For example ``/system ntp client`` is a single element menu.
::

    data = { 'primary-ntp':'1.1.1.1', 'secondary-ntp':'2.2.2.2', 'enabled':True }
    api.set( '/system/ntp/client', data )

Referencing particular ID.
::

    data = { 'interface':'ether10', 'ID':'*33' }
    api.set( '/ip/address', data )
