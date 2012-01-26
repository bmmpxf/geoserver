.. _sec_passwd:

Passwords
=========

Passwords are a central concept in any security system. 

.. _passwd_encryption:

Password encryption
-------------------

A GeoServer configuration stores two types of passwords:

#. Passwords for user accounts used to access GeoServer services and resources
#. Passwords used internally to access external services like relational databases and cascading OGC services

Since these passwords are typically stored on disk it is important that they not be stored in plain text. GeoServer
security provides a number of options for encrypting passwords:

* *Plain text* - No encryption, passwords are stored in plain text
* *Digest* - Digest encryption, using a SHA-256 method
* *PBE* - Password based encryption based on a user-generated passphrase

*Plain text* encryption has been the method historically used by GeoServer and for obvious reasons is not recommended as
it stores passwords non-obfuscated on the file system.

*Digest* encryption applies as `cryptographic hash function <http://en.wikipedia.org/wiki/Cryptographic_hash_function>`_ 
to passwords. This scheme is one way in that it is virtually impossible to reverse and obtain the original password from 
its hashed representation. More on this :ref:`below <passwd_reversible>`.

*PBE* or `password based encryption <http://www.javamex.com/tutorials/cryptography/password_based_encryption.shtml>`_ uses a user-supplied passphrase to generate an encryption key. This method 
utilizes an additional random value called a "salt" to generate the key in order to protect from dictionary attacks. 
GeoServer comes with support for two forms of PBE encryption: weak and strong. *Weak PBE* uses a basic encryption method
that is relatively easy to decipher. *Strong PBE* uses a much stronger method based on a AES 256 bit algorithm. Strong PBE 
is not natively available on all Java virtual machines. Such environments require the installation of some additional 
`JCE Unlimited Strength Jurisdiction <http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html>`_
policy files. 

* `Oracle JCE policy jars <http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html>`_
* `IBM JCE policy jars <https://www14.software.ibm.com/webapp/iwm/web/preLogin.do?source=jcesdk>`_

In cases where strong PBE is not available is **strongly** recommended that the additional policy files be installed. 

A password encryption is specified in two places. The first is a global setting that affects the encryption of passwords
used for external resources like databases which require a reversible method. The second is a setting per 
user/group service setting that can use any type of password encryption. Continue to the next section for more details.

.. _passwd_reversible:

Reversible encryption
^^^^^^^^^^^^^^^^^^^^^

Password encryption methods can be *reversible* meaning that from the encrypted password it is possible to obtain the 
original plain text version. For passwords used with database connections, or external OGC services like cascading WMS and 
WFS it is necessary to use a reversible method, since GeoServer must be able to decode the encrypted password and pass it 
to the external service unencrypted. The only method listed above that is non-reversible is *Digest* encryption. 

For user passwords either any reversible or non-reversible will work, although *Digest* is preferable since by definition 
it makes it impossible to reverse and provides the highest level of security.

.. _keystore:

Secret keys and the keystore
----------------------------

Encrypting and decrypting passwords involves the generation of a secret or private key. These keys must be stored somewhere
and typically in a Java system the notion of a *keystore* is used for this purpose. GeoServer uses its own keystore for 
this purpose named ``geoserver.jceks`` and is located in the ``security`` directory under the roof of the GeoServer data
directory. As its name implies the keystore is stored in "JCEKS" format rather than the default "JKS" format. More 
information about the difference is available `here <http://www.itworld.com/nl/java_sec/07202001>`_. 

The GeoServer keystore is password protected with the :ref:`master_passwd`. It is possible to access the contents of the 
keystore with external tools such as *keytool*. For example::

  keytools -list -keystore geoserver.jceks -storetype "JCEKS"
  
Would prompt for the master password and list the contents of the keystore.

.. _passwd_policy:

Password policies
-----------------

A *password policy* defines constraints on passwords such as password length, and mix of characters and case. Password
policies are specified when creating a user/group service and used to constrain passwords when creating new users. 

valid user passwords such as password length, mix of case, and special characters. Each user group service uses a password policy to enforce these rules. The default GeoServer password policy allows for the following constraints.

* Passwords must contain at least one digit
* Passwords must contain at least one upper case letter
* Passwords must contain at least one lower case letter
* Password minimum and maximum length


.. _master_passwd:

Master password
---------------

GeoServer security contains the notion of a *master password* that serves two purposes.

#. Protect the :ref:`keystore <keystore>`
#. Provide access to the GeoServer :ref:`sec_root`

TODO: setting the master password
