*****************************
Library objects and functions
*****************************

Functions
---------

.. py:function:: connect( host, user, pw, **kwargs )

    Connect and login to routeros device. Upon success return an :py:class:`Api` object.

    :param str host: Hostname to connecto to. May be ipv4,ipv6,FQDN.
    :param str user: Username to login with.
    :param str pw: Password to login with.
    :param float timeout: Socket timeout. Defaults to 10.
    :param int port: Destination port to be used. Defaults to 8728.
    :param logger: Logger to be used. Defaults to an logging instance with `NullHandler <https://docs.python.org/3/library/logging.handlers.html#logging.NullHandler>`_.
    :type logger: `Logging instance <https://docs.python.org/3/library/logging.html#logging.Logger>`_
    :param str saddr: Source address to bind to. Defaults to ''.
    :raises LoginError: If login attempt fails.


Classes
-------

.. py:class:: Api


    .. py:method:: run(cmd, args=dict())

    Run any `non interactive` command. This method is not suited to run `interactive` commands. Those commands do not stop on their own. For example `/tool/torch`.

    :param str cmd: Command to run.
    :param dict args: Dictionary with key value pairs.
    :return: Parsed command output.
    :rtype: tuple with dictionaries.
    :raises CmdError: if execution fails for one or more reasons.
    :raises ConnError: if any connection realated error occurs.

    .. py:attribute:: timeout

    Connection timeout property. May be set to > 0.

    .. py:method:: close()

    Send `/quit` and close the connection.



Exceptions
----------

Exceptions with their hierarchy.

.. py:exception:: LibError(Exception)

    This is a base exception for all other.

    .. py:exception:: LoginError

        All login attempt errors.

    .. py:exception:: CmdError

        Command execution errors.

    .. py:exception:: ConnError

        Connection related errors. Timeouts, broken socket connection.
