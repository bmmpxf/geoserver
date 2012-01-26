.. _usergroup_jdbc:

JDBC
^^^^

User/group service that persists the user/group database in an database. 

The jdbc user/group service represents the user database with multiple tables. The following shows the database schema::

    +--------------+
    | Table: users |
    +----------+--------------+------+-----+
    | Field    | Type         | Null | Key |
    +----------+--------------+------+-----+
    | name     | varchar(128) | NO   | PRI |
    | password | varchar(64)  | YES  |     |
    | enabled  | char(1)      | NO   |     |
    +----------+--------------+------+-----+
    
    +-------------------+
    | Table: user_props |
    +-----------+---------------+------+-----+
    | Field     | Type          | Null | Key |
    +-----------+---------------+------+-----+
    | username  | varchar(128)  | NO   | PRI |
    | propname  | varchar(64)   | NO   | PRI |
    | propvalue | varchar(2048) | YES  |     |
    +-----------+---------------+------+-----+
    
    +---------------+
    | Table: groups |
    +---------+--------------+------+-----+
    | Field   | Type         | Null | Key |
    +---------+--------------+------+-----+
    | name    | varchar(128) | NO   | PRI |
    | enabled | char(1)      | NO   |     |
    +---------+--------------+------+-----+

    +----------------------+
    | Table: group_members |
    +-----------+--------------+------+-----+
    | Field     | Type         | Null | Key |
    +-----------+--------------+------+-----+
    | groupname | varchar(128) | NO   | PRI |
    | username  | varchar(128) | NO   | PRI |
    +-----------+--------------+------+-----+

The *users* table is the primary table and contains the list of users with associated passwords. The *user_props* table is 
a mapping table that maps additional properties to a user. See :ref:`users_and_groups` for more details.

The *groups* table lists all available groups, with the *group_members* table containing the mapping of users to the groups
they are associated with.

The default GeoServer security configuration would be represented with the following database contents::

    > select * from users;
    +-------+-----------------+---------+
    | name  | password        | enabled |
    +-------+-----------------+---------+
    | admin | digest1:UTb.... | Y       |
    +-------+-----------------+---------+
    
    > select * from user_props;
    Empty
    
    > select * from groups;
    Empty
    
    > select * from group_members;
    Empty
    
Configuration
-------------

.. figure:: images/usergroup_jdbc1.jpg

*Password encryption* is the method to use to encrypt user passwords. See :ref:`passwd_encryption` for more details.

*Password policy* is the policy to use to enforce constraints on user passwords. See :ref:`passwd_policy` for more details.

*JNDI* when unset specifies a direct connection to the database. When set it specifies an existing connection located 
through JNDI. See more :ref:`below <jndi>`.

*Driver class name* is the JDBC driver to use for the database connection.

*Connection URL* specifies the JDBC url to use when creating the database connection.

*Username* is the database username to connect with.

*Password* is the password of the database user.

*Create database tables* specifies whether to create all the necessary tables in the underlying database. 

*Data Definition Language (DDL) file* is used to specify a custom DDL file to use for creating tables in the underlying 
database. If left unspecified internal defaults are used. This option would be used in cases where the default DDL 
statements fail on the given database.

*Data Manipulation Language (DML) file* is used to specify a custom DML file to use for accessing tables in the underlying 
database. If left unspecified internal defaults are used. This option would be used in cases where the default DML 
statements fail on the given database.

.. figure:: images/usergroup_jdbc2.jpg

The following parameters apply only when the *JNDI* flag is set. 

*JNDI resource name* is the JNDI name used to locate the database database connection. More :ref:`below <jndi>`.

.. _jndi:

JNDI
----

`Java Naming and Directory Interface <http://en.wikipedia.org/wiki/Java_Naming_and_Directory_Interface>`_ (JNDI) allows for
components in a Java system to look up other objects and data by a predefined name. A common use of JNDI is to use it to
store a JDBC data source globally in a container. This has a few benefits.

First it can lead to a much more efficient use of database resources. Database connections in java are very expensive objects, so usually they are pooled. If each component that requires a database connection is responsible for creating
their own connection pool, resources will add up fast. And often those resources are under utilized as a component may not
size it's connection pool accordingly. A more efficient method is to set up a global pool, at the servlet container level, and have every component that requires a database connection use it. 

Furthermore it consolidates database connection configuration as not every component that requires a database connection needs to know the details. They just need to know the JNDI name and that is it. This is very useful to administrators who
may have to change database parameters in a running system and allowing the change to occur in a single place.
