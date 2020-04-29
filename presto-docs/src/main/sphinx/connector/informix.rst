====================
Informix Server Connector
====================

The Informix Server connector allows querying and creating tables in an
external Informix Server database. This can be used to join data between
different systems like Informix Server and Hive, or between two different
Informix Server instances.

Configuration
-------------

To configure the Informix Server connector, create a catalog properties file
in ``etc/catalog`` named, for example, ``informix.properties``, to
mount the Informix Server connector as the ``informix`` catalog.
Create the file with the following contents, replacing the
connection properties as appropriate for your setup:

.. code-block:: none

    connector.name=informix
    connection-url=jdbc:informix-sqli://[Hostname][:portNumber]]
    connection-user=informix
    connection-password=secret

Multiple Informix Server Databases or Servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Informix Server connector can only access a single database within
a Informix Server server. Thus, if you have multiple Informix Server databases,
or want to connect to multiple instances of the Informix Server, you must configure
multiple catalogs, one for each instance.

To add another catalog, simply add another properties file to ``etc/catalog``
with a different name (making sure it ends in ``.properties``). For example,
if you name the property file ``sales.properties``, Presto will create a
catalog named ``sales`` using the configured connector.

Querying Informix Server
-------------------

The Informix Server connector provides access to all schemas/owner visible to the specified user in the configured database.
For the following examples, assume the Informix Server catalog is ``informix``.

You can see the available schemas by running ``SHOW SCHEMAS``::

    SHOW SCHEMAS FROM informix;

If you have a schema/owner named ``informix``, you can view the tables
in this schema by running ``SHOW TABLES``::

    SHOW TABLES FROM informix.informix;

You can see a list of the columns in the ``clicks`` table in the ``informix`` schema/owner
using either of the following::

    DESCRIBE informix.informix.clicks;
    SHOW COLUMNS FROM informix.informix.clicks;

Finally, you can query the ``clicks`` table in the ``informix`` schema/owner::

    SELECT * FROM informix.informix.clicks;

If you used a different name for your catalog properties file, use
that catalog name instead of ``informix`` in the above examples.

Informix Server Connector Limitations
--------------------------------


Presto supports the following Informix Server data types.
The following table shows the mappings between Informix Server and Presto data types.

============================= ============================
Informix Server Type               Presto Type
============================= ============================
``bigint``                    ``bigint``
``smallint``                  ``smallint``
``int``                       ``integer``
``float``                     ``double``
``char(n)``                   ``char(n)``
``varchar(n)``                ``varchar(n)``
``date``                      ``date``
============================= ============================

Complete list of `Informix Server data types
<https://informix.hcldoc.com/14.10/help/index.jsp>`_.

The following Informix statements are not yet supported:

* :doc:`/sql/delete`
* :doc:`/sql/update`
* :doc:`/sql/alter-table`
* :doc:`/sql/grant`
* :doc:`/sql/revoke`
