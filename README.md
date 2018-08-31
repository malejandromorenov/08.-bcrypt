# bcryptr
optional instalation
Project description
# bcrypt

Latest Version  https://travis-ci.org/pyca/bcrypt.svg?branch=master
Modern password hashing for your software and your servers

Installation
To install bcrypt, simply:

$ pip install bcrypt 

Note that bcrypt should build very easily on Linux provided you have a C compiler, headers for Python (if you’re not using pypy), and headers for the libffi libraries available on your system.

For Debian and Ubuntu, the following command will ensure that the required dependencies are installed:

$ sudo apt-get install build-essential libffi-dev python-dev
For Fedora and RHEL-derivatives, the following command will ensure that the required dependencies are installed:

$ sudo yum install gcc libffi-devel python-devel
Changelog
3.1.4
Fixed compilation with mingw and on illumos.
3.1.3
Fixed a compilation issue on Solaris.
Added a warning when using too few rounds with kdf.
3.1.2
Fixed a compile issue affecting big endian platforms.
Fixed invalid escape sequence warnings on Python 3.6.
Fixed building in non-UTF8 environments on Python 2.
3.1.1
Resolved a UserWarning when used with cffi 1.8.3.
3.1.0
Added support for checkpw, a convenience method for verifying a password.
Ensure that you get a $2y$ hash when you input a $2y$ salt.
Fixed a regression where $2a hashes were vulnerable to a wraparound bug.
Fixed compilation under Alpine Linux.
3.0.0
Switched the C backend to code obtained from the OpenBSD project rather than openwall.
Added support for bcrypt_pbkdf via the kdf function.
2.0.0
Added support for an adjustible prefix when calling gensalt.
Switched to CFFI 1.0+
Usage
Password Hashing
Hashing and then later checking that a password matches the previous hashed password is very simple:

>>> import bcrypt
>>> password = b"super secret password"
>>> # Hash a password for the first time, with a randomly-generated salt
>>> hashed = bcrypt.hashpw(password, bcrypt.gensalt())
>>> # Check that an unhashed password matches one that has previously been
>>> # hashed
>>> if bcrypt.checkpw(password, hashed):
...     print("It Matches!")
... else:
...     print("It Does not Match :(")
KDF
As of 3.0.0 bcrypt now offers a kdf function which does bcrypt_pbkdf. This KDF is used in OpenSSH’s newer encrypted private key format.

>>> import bcrypt
>>> key = bcrypt.kdf(
...     password=b'password',
...     salt=b'salt',
...     desired_key_bytes=32,
...     rounds=100)
Adjustable Work Factor
One of bcrypt’s features is an adjustable logarithmic work factor. To adjust the work factor merely pass the desired number of rounds to bcrypt.gensalt(rounds=12) which defaults to 12):

>>> import bcrypt
>>> password = b"super secret password"
>>> # Hash a password for the first time, with a certain number of rounds
>>> hashed = bcrypt.hashpw(password, bcrypt.gensalt(14))
>>> # Check that a unhashed password matches one that has previously been
>>> #   hashed
>>> if bcrypt.checkpw(password, hashed):
...     print("It Matches!")
... else:
...     print("It Does not Match :(")
Adjustable Prefix
Another one of bcrypt’s features is an adjustable prefix to let you define what libraries you’ll remain compatible with. To adjust this, pass either 2a or 2b (the default) to bcrypt.gensalt(prefix=b"2b") as a bytes object.

As of 3.0.0 the $2y$ prefix is still supported in hashpw but deprecated.

Maximum Password Length
The bcrypt algorithm only handles passwords up to 72 characters, any characters beyond that are ignored. To work around this, a common approach is to hash a password with a cryptographic hash (such as sha256) and then base64 encode it to prevent NULL byte problems before hashing the result with bcrypt:

>>> password = b"an incredibly long password" * 10
>>> hashed = bcrypt.hashpw(
...     base64.b64encode(hashlib.sha256(password).digest()),
...     bcrypt.gensalt()
... )
Compatibility
This library should be compatible with py-bcrypt and it will run on Python 2.6+, 3.3+, and PyPy 2.6+.

C Code
This library uses code from OpenBSD.

Security
bcrypt follows the same security policy as cryptography, if you identify a vulnerability, we ask you to contact us privately.
