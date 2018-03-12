====================================
Secure Multiparty Computation (SMC)
====================================

Sharemind MPC Application Server
================================

Requirements
------------

* **Minimal hardware requirements:** 8 GB of RAM, 2 CPU cores. Optimal requirements depend on planned workloads.
* **Operating system:** Debian (Stretch), Ubuntu (16.04 or newer) or other APT-based Linux distribution.

Installation
------------

Sharemind MPC is available from Cybernetica's private APT repository. To access it, add the repository location to your APT configuration and trust the Sharemind package signing GPG key.

.. code:: bash

   gpg --keyserver subkeys.pgp.net --search sharemind-packaging@cyber.ee # Note the downloaded key ID
   gpg -a --export <key ID> | sudo apt-key add - # Supply the key ID here

   echo "deb https://repo.cyber.ee/sharemind/apt/debian stretch main" | sudo tee /etc/apt/sources.list.d/sharemind-mpc.list
   sudo apt-get update

Install Sharemind Application Server with the *shared3p* SMC module and HDF5 embedded storage backend:

.. code:: bash

   sudo apt-get install sharemind-server sharemind-mod_shared3p sharemind-mod_tabledb-hdf5

Key Generation and Exchange
---------------------------

Sharemind MPC uses Transport Layer Security (TLS) technology for secure, mutually authenticated and encrypted communication channels between computation nodes as well as between computation nodes and client applications like CSV Importer. Therefore, each Sharemind component requires a personal asymmetric key pair for authentication and encryption.

As Sharemind MPC uses RSA keys in standard X.509 certificate format, already familiar tools like OpenSSL can be used.
The process described below generates a new RSA key pair, where the private key is in file ``my-private-key`` and public part in ``my-public-key``. The ``-days`` value should be at least the expected duration of the deployment.

.. code:: bash

   openssl req -x509 -days 300 -nodes -newkey rsa:2048 -keyout my-private-key -out my-public-key -outform der

.. code:: bash

   Generating a 2048 bit RSA private key
   .....................................................................+++
   ...+++
   writing new private key to 'my-private-key'
   -----
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [AU]: **(Required)**
   State or Province Name (full name) [Some-State]: **(Required)**
   Locality Name (eg, city) []: **(Required)**
   Organization Name (eg, company) [Internet Widgits Pty Ltd]: **(Required)**
   Organizational Unit Name (eg, section) []: 
   Common Name (e.g. server FQDN or YOUR name) []: **(Required)**
   Email Address []:

This ``openssl`` tool generates the private key by default in PEM format. However, for Sharemind MPC, it must be converted to DER format:

.. code:: bash

   openssl rsa -in my-private-key -out my-private-key -outform der

The public key must be sent to each other participant in the SMC deployment that communicates directly with the Sharemind Application Server. This includes other (two) Sharemind Application Servers and client or proxy applications. The public key should be sent in a way that allows sender authentication, e.g. by digitally signing it.

Configuration
-------------

In every deployment, all three Sharemind Application Server hosts have to agree on unique server names and their order. Before continuing, make sure you have the following information information about the deployment and other hosts:

* Deployment name
* For each Sharemind Application Server:
   * Node number (1, 2 or 3)
   * Name
   * Hostname or IP
   * Port number
   * Public key file

Sharemind Application Servers search for their main configuration file from the following locations (in order):

- Filename given by the ``--conf`` command line argument
- System-wide configuration file in ``/etc/xdg/sharemind/server.conf`` (XDG Basedir search path)
- System-wide configuration file in ``/etc/sharemind/server.conf``

The configuration file is an INI-formatted file, where section names are between square brackets (``[Section]``) and configuration values are given with ``key=value`` pairs. A commented example server configuration file is available in ``/usr/share/doc/sharemind/examples/server.conf``.

At minimum, the following changes to the example configuration are necessary:

* The value of ``UuidNamespace`` in section ``[Server]`` must be set to your deployment name.
* The value of ``Name`` in section ``[Server]`` must be set to your server's unique name.
* The value of ``ListenInterfaces`` in section ``[Network]`` must be set to an IP address and port number, where the Sharemind Application Server listens for incoming connections. If the server should listen only on a single network interface, insert it's IP address. Otherwise, specify ``0.0.0.0``. The port number can be chosen according to personal preference, keeping in mind that listening on low port numbers (up to 1023) requires root access.
* The values of ``PublicKeyFile`` and ``PrivateKeyFile`` in section ``[Network]`` should be the file names of your public and private keys, respectively. Following the example key generation procedure given above, these are ``my-public-key`` and ``my-private-key``. File location can be given relative to the current configuration file with ``%{CurrentFileDirectory}``, e.g. ``%{CurrentFileDirectory}/keys/my-public-key``.
* Information about the other two servers is in sections ``[Server <name>]``, where ``<name>`` is the unique agreed upon name of the respective server. For both servers, the following values should be changed: ``Address``: server's IP address, ``Port``: server's port number and ``PublicIdentity``: file name of the corresponding server's public key file. File location can be given relative to the current configuration file with ``%{CurrentFileDirectory}``.

Additionally the file ``shared3p.conf`` should be changed so that server names are assigned to the correct identifiers (node numbers). This configuration has to be identical for all three servers so they know their communication order in the secure multi-party protocols. The system-wide copy of this file is in ``/etc/sharemind/shared3p.conf``.

Other parts of the configuration files should remain unchanged, as network and security parameters must be consistent for all servers and client applications.

Each client application or proxy owner also generates a key pair and sends its public key to each server host. The Sharemind Application Server only allows incoming connections from client applications whose public key is registered in its access control list, referenced by the ``WhiteListFile`` from the main configuration file (by default ``%{CurrentFileDirectory}/server-whitelist.conf``). The format of this whitelist file is as follows:

.. code:: ini

   # Format:
   #   path/to/public-key-filename: script-filename1[, script-filename2, ...] # Ignored comment
   # Example:
   #   key1: script1, script2 # Allow running only 'script1' and 'script2' with public key 'key1'.
   #   key2: *                # Allow 'key2' to run any script. NB! Should not be used in production!
   client-public-key: secrec-program.sb

Compiling SecreC Code
---------------------

Each Sharemind Application Server host must audit and deploy necessary SecreC programs separately. SecreC code is compiled into bytecode with the ``scc`` program and by default, Sharemind Application Server looks for the bytecode from ``/var/lib/sharemind/scripts/`` folder:

.. code:: bash

   scc -o /var/lib/sharemind/scripts/program.sb /path/to/src/program.sc

Starting Sharemind Application Server
-------------------------------------

A system-wide installation of the Sharemind Application Server is controlled by the systemd unit file:

.. code:: bash

   sudo systemctl start sharemind-server
   sudo systemctl stop sharemind-server


Sharemind Web Application Gateway
=================================

Sharemind platform components communicate using a binary protocol. To support web-based client applications, a proxy service has to be deployed in front of each Sharemind Application Server that translates between HTTP and Sharemind's binary protocol. Such proxy applications can be built with Sharemind Web Application Gateway add-on -- a NodeJS library that must be installed separately on the server:

Installation
------------

.. code:: bash

   sudo apt-get install sharemind-web-gateway

Proxy applications written in NodeJS should then add "`sharemind-web-gateway`" as a dependency in their `package.json` file. Sharemind Web Application Gateway requires NodeJS version 6 or newer. If a suitable version is not provided directly by the Linux distribution, it can be obtained from `NodeJS site <https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions>`_.

Configuration
-------------

A proxy application using Sharemind Web Application Gateway acts as a client application for the Sharemind Application Server. Thus, it also needs a key pair for TLS as described in `Key Generation and Exchange`_.

An example client application configuration, also suitable for proxy applications, can be found in ``/usr/share/doc/sharemind/examples/client.conf``. At minimum, the following changes to the example configuration are necessary:

* The value of ``UuidNamespace`` in section ``[Controller]`` must be set to your deployment name.
* The values of ``PublicKeyFile`` and ``PrivateKeyFile`` in section ``[Network]`` should be the file names of your public and private keys, respectively. Following the example key generation procedure given at `Key Generation and Exchange`_, these are ``my-public-key`` and ``my-private-key``. File location can be given relative to the current configuration file with ``%{CurrentFileDirectory}``, e.g. ``%{CurrentFileDirectory}/keys/my-public-key``.
* Proxy applications only connect to one Sharemind Application Server and thus have only one ``[Server <name>]`` section, where ``<name>`` is the unique agreed upon name of the respective server. The following values should be changed: ``Address``: server's IP address, ``Port``: server's port number and ``PublicIdentity``: file name of the corresponding server's public key file. File location can be given relative to the current configuration file with ``%{CurrentFileDirectory}``.
