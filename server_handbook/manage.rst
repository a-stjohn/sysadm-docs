.. _management:

Managing |sysadm|
*****************

|sysadm| comes with a standard configuration file located in
:file:`/usr/local/etc/sysadm.conf.dist`.

It is possible to edit this file for a custom configuration, but the
result will need to be saved as :file:`sysadm.conf`. Here are the
current default settings for |sysadm|:

.. code-block:: none

  #Sample Configuration file for the SysAdm™ server

  ### Server Port Number ###
  # - Websocket Server (standard)
  PORT=12150
  # - REST Server (started with the "-rest" CLI flag)
  PORT_REST=12151

This default configuration also has blacklist options:

.. code-block:: none

  ### Blacklist options ###
  # - Number of minutes that an IP remains on the blacklist
  BLACKLIST_BLOCK_MINUTES=60
  # - Number of authorization failures before an IP is placed on the
      blacklist
  BLACKLIST_AUTH_FAIL_LIMIT=5
  # - Number of minutes of no activity from an IP before  resetting the
      failure counter
  #   (Note: A successful authorization will always reset the fail
      counter)
  BLACKLIST_AUTH_FAIL_RESET_MINUTES=10

.. note:: These default options are subject to change as the |sysadm|
   utility is developed.
