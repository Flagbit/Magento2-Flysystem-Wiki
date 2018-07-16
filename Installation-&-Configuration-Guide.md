# Requirements #
Before you start, make sure that your Magento2 Store has the needed requirements.

* PHP Version 7.0.x or higher
* Magento2 Version 2.2.x or higher

# Installation #

The module can be installed via composer. To do so follow these Steps.

* Check the Flysystem Repository for the current version tag https://bitbucket.org/flagbit/magento2-flysystem/commits/all
* Add the module with composer using this command
```
composer require flagbit/magento2-flysystem "^0.1.1"
```
* Update composer using this command
```
composer update --ignore-platform-reqs
```
* Get the current files on your magento system and run this commands in the Webroot for your Webshop
```
php bin/magento module:enable Flagbit_Flysystem
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento cache:flush
```

# Configuration #

After installation navigate in your Magento2 adminpanel to **Stores > Configuration > General / Flagbit Flysystem**. If it is not there after installation, something went wrong.

Here you can configure the Adapter which is used by default for the Flysystem modals. By default Flagbit Flysystem provides the functionality for 3 different Adapters:

* Local: read files from your Server, or whereever your Magento2 application is installed
    * Root Path: The path which the local adapter will use as the root path. Define this to something safe, since you are able to delete all folders and files with flysystem, if the folder permissions enable it
    * Lock during write: Should be self explainable. Don't touch this config if you don't know what it does
* Ftp: reads files from any FTP Server. (not SFTP, but FTP/FTPS works)
    * Host: The FTP Server IP or DNS name to connect to
    * Username: Username for the FTP connection
    * Password: Password for the user
    * Port: Port where to connect to the server. Don't define this with ":" in the host, because that won't work. Most FTP connection use port 21 is you are unsure which one is correct
    * Root Path: The root path which the adapter will use as root
    * Passive: define if the connection should be passive or active
    * SSL: use ssl for the ftp connection. The Server needs to support FTPS
    * Timeout: timeout at which it will stop try to create a ftp connection, if the ftp connection fails
* NullAdapter / Null: used for testing, it won't create a connection to files. If you are unsure if the Flagbit Flysystem module works, you can try to debug by using this adapter.

If you want to configure another Adapter which is not supported by default. See the section "Integrate a new Flysystem Adapter".

If you are unexperienced in configuring a filesystem Adapter. First try to configure the Local Adapter to see if everything is working, there are not many things which you could do wrong.

The configuration Flysystem Source defines which adapter will be used by default for the Flysystem Modals. You will need to define it that the moal will work.