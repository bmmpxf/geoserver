.. _sec_root::

Root Account
============

The GeoServer security subsystem contains the notion of a *root account*. The purpose of this account is to provide a
"super user"" account that is always active regardless of the state of the security configuration. The highly configurable
nature of security in GeoServer makes it very possible to achieve a situation in whichin which a misconfiguration causes
normal authentication to cease functioning, essentially disabling all accounts, including administrative accounts. The root
account is meant to protect from this situation and provide a last resort in order to be able to log in and fix the misconfiguration.

The username for the root account is "root". The password for the root account is the :ref:`master_passwd`. Out of the box
this password is the historical "geoserver" password. Naturally it is **highly** recommended that the master password be 
changed immediately after any GeoServer install.
