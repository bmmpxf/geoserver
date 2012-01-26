.. _role_jdbc:

JDBC
====

Role service that persists the role database in an database. 

The jdbc role service represents the role database with multiple tables. The following shows the database schema::

    +--------------+
    | Table: roles |
    +--------+-------------+------+-----+
    | Field  | Type        | Null | Key |
    +--------+-------------+------+-----+
    | name   | varchar(64) | NO   | PRI |
    | parent | varchar(64) | YES  |     |
    +--------+-------------+------+-----+

    +-------------------+
    | Table: role_props |
    +-----------+---------------+------+-----+
    | Field     | Type          | Null | Key |
    +-----------+---------------+------+-----+
    | rolename  | varchar(64)   | NO   | PRI |
    | propname  | varchar(64)   | NO   | PRI |
    | propvalue | varchar(2048) | YES  |     |
    +-----------+---------------+------+-----+
    
    +-------------------+
    | Table: user_roles |
    +----------+--------------+------+-----+
    | Field    | Type         | Null | Key |
    +----------+--------------+------+-----+
    | username | varchar(128) | NO   | PRI |
    | rolename | varchar(64)  | NO   | PRI |
    +----------+--------------+------+-----+
    
    +--------------------+
    | Table: group_roles |
    +-----------+--------------+------+-----+
    | Field     | Type         | Null | Key |
    +-----------+--------------+------+-----+
    | groupname | varchar(128) | NO   | PRI |
    | rolename  | varchar(64)  | NO   | PRI |
    +-----------+--------------+------+-----+

The *roles* table is the primary table and contains the list of roles. Roles in GeoServer support inheritance so a role may
optionally have a link to a parent role. The *role_props* table is  a mapping table that maps additional properties to a role. See :ref:`roles` for more details.

The *user_roles* table maps users to the roles they are assigned. Similarly the "group_roles" tables does the same but for 
groups rather than users. 

The default GeoServer security configuration would be represented with the following database contents::

    > select * from roles;
    +--------------------+--------+
    | name               | parent |
    +--------------------+--------+
    | ROLE_ADMINISTRATOR | NULL   |
    +--------------------+--------+

    > select * from role_props;
    Empty

    > select * from user_roles;
    +----------+--------------------+
    | username | rolename           |
    +----------+--------------------+
    | admin    | ROLE_ADMINISTRATOR |
    +----------+--------------------+

    > select * from group_roles;
    Empty
    
Configuration
-------------

.. figure:: images/role_jdbc.jpg

*JNDI* when unset specifies a direct connection to the database. When set it specifies an existing connection located 
through JNDI with name specified by the *JNDI resource name* parameter. See the :ref:`jndi` section of the jdbc user/group
documentation for more details.

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