------
Extras
------

:mod:`extras` provides some extra functionalities for working with api.


Comparison functions
____________________

.. function:: dictdiff(wanted, present, split_keys=tuple(), split_char=',')

    Compare two dictionaries and return items from *wanted* not listed in *present*. If *split_keys* is provided, additional comparison is made based on provided keys.

    :param dict wanted: Desired items.
    :param dict present: Present items.
    :param tuple split_keys: Tuple with key names to split.
    :param str split_char: Character used to split key values by.

    >>> wanted = {'name': 'full', 'policy': 'api,winbox,read,write'}
    >>> present = {'name': 'full', 'policy': 'api,winbox,read,!write'}
    >>> dictdiff(wanted,present,split_keys=('policy',),split_char=',')
    {'policy': 'write'}

    Note how comparison is made without specifying *split_keys*.

    >>> dictdiff(wanted,present)
    {'policy': 'api,winbox,read,write'}

    .. versionadded:: 1.1


.. function:: strdiff(welem, pelem, splchr)

    Compare two strings and return items from *welem* not present in *pelem*. Items from *pelem*, *welem* are splitted by *splchr* and compared. Returns (unordered) string joined by *splchr*.

    :param str welem: Wanted element.
    :param str present: Present element.
    :param str splchr: Character to split *welem* and *pelem*.


    >>> wanted = 'api,winbox,read,write'
    >>> present = 'api,winbox,read,!write'
    >>> strdiff(wanted, present, ',')
    'write'

    .. versionadded:: 1.1


