.. _log-module:

-------------------------------------------------------------------------------
                                   Module `log`
-------------------------------------------------------------------------------

.. module:: log

The Tarantool server puts all diagnostic messages in a log file specified by
the :ref:`log <cfg_logging-log>` configuration parameter. Diagnostic
messages may be either system-generated by the server's internal code, or
user-generated with the :samp:`log.{log_level}` function.

.. function:: error(message)
              warn(message)
              info(message)
              debug(message)

    Output a user-generated message to the :ref:`log file <cfg_logging-log>`,
    given log_level_function_name = ``error`` or ``warn`` or ``info`` or
    ``debug``.

    :param string message: The actual output will be a line containing the
                           current timestamp, a module name, 'E' or 'W' or
                           'I' or 'D' or 'R' depending on
                           ``log_level_function_name``, and ``message``.
                           Output will not occur if ``log_level_function_name``
                           is for a type greater than :ref:`log_level
                           <cfg_logging-log_level>`. Messages may contain
                           C-style format specifiers %d or %s, so
                           :samp:`log.error('...%d...%s', {x}, {y})`
                           will work if x is a number and y is a string.
    :return: nil

.. _log-logger_pid:

.. function:: logger_pid()

.. function:: rotate()

=================================================
                     Example
=================================================

.. code-block:: tarantoolsession

    $ ~/tarantool/src/tarantool
    tarantool> box.cfg{log_level=3, logger='tarantool.txt'}
    tarantool> log = require('log')
    tarantool> log.error('Error')
    tarantool> log.info('Info %s', box.info.version)
    tarantool> os.exit()
    $ less tarantool.txt

.. cssclass:: highlight
.. parsed-literal::

    2...0 [5257] main/101/interactive C> version 1.7.0-355-ga4f762d
    2...1 [5257] main/101/interactive C> log level 3
    2...1 [5261] main/101/spawner C> initialized
    2...0 [5257] main/101/interactive [C]:-1 E> Error

The 'Error' line is visible in tarantool.txt preceded by the letter E.

The 'Info' line is not present because the log_level is 3.
