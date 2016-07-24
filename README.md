WebLogic Server password decryptor
==================================

Description
-----------
A simple script to decrypt stored passwords from Oracle WebLogic Server configuration files.

Features
--------
* Supports AES and 3DES encrypted passwords

Prerequisites
-----
### 1. Jython
`apt-get install jython` or download it [here](http://www.jython.org/downloads.html)

### 2. BouncyCastle cryptographic provider setup
Download the latest BouncyCastle provider jar file ([such as this one](https://www.bouncycastle.org/fr/download/bcprov-jdk15on-153.jar)) and place it into the following folder: `$JAVA_HOME/jre/lib/ext` (run `$ jython -v` to see which java instance you use)

### 3. `SerializedSystemIni.dat` file
The `SerializedSystemIni.dat` file related to your WebLogic domain, as this is a **per-domain file** containing encryption keys.  
It is usually stored into the following folder: `~/wls<VERSION>/user_projects/domains/<DOMAIN_NAME>/security/`  

### 4. An encrypted password or `config.xml` file  
I guess you might already have one if you came here.  
The `config.xml` file is usually stored into the following folder: `~/wls<VERSION>/user_projects/domains/<DOMAIN_NAME>/config/`  

Options
-------
```
$ jython weblogicpassworddecryptor.jy -h
Usage: weblogicpassworddecryptor.jy [options]
Version: 1.0

Options:
  -h, --help            show this help message and exit

  Mandatory parameters:
    -s SERIALIZEDSYSTEMINI_FILE, --serializedsystemini-file=SERIALIZEDSYSTEMINI_FILE
                        path to the SerializedSystemIni.dat file containing
                        the machine-unique salt to decrypt the master-key, and
                        keys to decrypt passwords. Ex. -s
                        ./SerializedSystemIni.dat
    -p ENCRYPTED_PASSWORD, --encrypted-password=ENCRYPTED_PASSWORD
                        an encrypted password, starting by either {AES} or
                        {3DES}. Ex. -p
                        {AES}6ZP0QttrG2K97NXbm9qz+KtaO4xGG8i6DP+2vopT2Cs=

  Optional parameters:
    -c CONFIG_XML_FILE, --config-xml-file=CONFIG_XML_FILE
                        path to a config.xml file containing encrypted
                        passwords. Ex: -c ./config.xml
    -v, --verbosity     verbosity level, repeat it to increase the level { -v
                        INFO, -vv DEBUG } (default verbosity ERROR)
```

Examples
--------
#### 3DES-encrypted password
```
$ jython weblogicpassworddecryptor.jy -s SerializedSystemIni_3DES.dat -p {3DES}vNxF1kIDgtydLoj5offYBQ==

[+] encrypted: {3DES}vNxF1kIDgtydLoj5offYBQ==
[+] decrypted: Password1
```

#### AES-encrypted passwords from a `config.xml` file
```
$ jython weblogicpassworddecryptor.jy -s SerializedSystemIni.dat -c config.xml  

[+] found encrypted value: <credential-encrypted>{AES}ZPtRzLpxmwaFjehjSPzjZfHoOu4DAq/ZKD4IBYmaCA/dm1SGHAgJTkhIP2SJzG4blKGYlkRSwOLxOpNsoOdaBrcZFnsKzN7KKPo+xyq7FhFf2BgvQwzGuykt8Wfb9aQb</credential-encrypted>
[+] encrypted: {AES}ZPtRzLpxmwaFjehjSPzjZfHoOu4DAq/ZKD4IBYmaCA/dm1SGHAgJTkhIP2SJzG4blKGYlkRSwOLxOpNsoOdaBrcZFnsKzN7KKPo+xyq7FhFf2BgvQwzGuykt8Wfb9aQb
[+] decrypted: 0x06a50109696b77f796217ac8435fb9309481c21e145c25925c8a3d5b79c4849c

[+] found encrypted value: <node-manager-password-encrypted>{AES}6ZP0QttrG2K97NXbm9qz+KtaO4xGG8i6DP+2vopT2Cs=</node-manager-password-encrypted>
[+] encrypted: {AES}6ZP0QttrG2K97NXbm9qz+KtaO4xGG8i6DP+2vopT2Cs=
[+] decrypted: weblogic2014

[+] found encrypted value: <credential-encrypted>{AES}QOujDfm62cZk3k32OAlcEzelVPK/zknVPyBivOuNXTvwq2hNLf0fIXJLdbZz12Kv</credential-encrypted>
[+] encrypted: {AES}QOujDfm62cZk3k32OAlcEzelVPK/zknVPyBivOuNXTvwq2hNLf0fIXJLdbZz12Kv
[+] decrypted: 0x157bb1759727a87eca6fb02cfe
```

Changelog
---------
* version 1.0 - 07/24/2016: Initial commit

Copyright and license
---------------------
weblogicpassworddecryptor is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

weblogicpassworddecryptor is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  

See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with weblogicpassworddecryptor. 
If not, see http://www.gnu.org/licenses/.

Greetings
---------
* Eric Gruber at NetSPI for its [cool post](https://blog.netspi.com/decrypting-weblogic-passwords/) and all previous researchers on this topic

Contact
-------
* Thomas Debize < tdebize at mail d0t com >