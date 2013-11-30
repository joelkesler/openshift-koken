# Koken on Openshift (alpha)

A hackity way to install and run [Koken](http://koken.me/) on Redhat's [Openshift](https://www.openshift.com/) Paas.

It has been configured to run on the **Zend Server** Gear with the **MySQL** cartridge.

## How does it work?

I have placed a modified Koken installer in the .openshift folder.  The deploy shell script in the .openshift/action_hooks/ directory instigates the installation when pushed to the server.

## What does the Deploy script do?

The script checks to see if there is a Koken folder in the data directory.  If none is found, it copies the koken folder containing the installer from the .openshift directory to the data directory.
In order for Koken to be seen on the front end, a symbolic link created in the php/ directory.

## What has changed in the Koken installer?

I have added the environmental variables for the MySQL cartridge (ip address, username, password, db name) to be filled in automatically.

**This is still very much a work in progress.  Use at your own risk**




# Original Openshift Readme

**Repo layout**
php/ - Externally exposed php code goes here
libs/ - Additional libraries
misc/ - For not-externally exposed php code
../data - For persistent data (full path in environment var: OPENSHIFT_DATA_DIR)
deplist.txt - list of pears to install
.openshift/action_hooks/pre_build - Script that gets run every git push before the build
.openshift/action_hooks/build - Script that gets run every git push as part of the build process (on the CI system if available)
.openshift/action_hooks/deploy - Script that gets run every git push after build but before the app is restarted
.openshift/action_hooks/post_deploy - Script that gets run every git push after the app is restarted


###### Notes about layout
Please leave php, libs and data directories but feel free to create additional
directories if needed.

Note: Every time you push, everything in your remote repo dir gets recreated
please store long term items (like an sqlite database) in ../data which will
persist between pushes of your repo.


###### Environment Variables

OpenShift provides several environment variables to reference for ease
of use.  The following list are some common variables but far from exhaustive:

    $_ENV['OPENSHIFT_APP_NAME']   - Application name
    $_ENV['OPENSHIFT_DATA_DIR']   - For persistent storage (between pushes)
    $_ENV['OPENSHIFT_TMP_DIR']    - Temp storage (unmodified files deleted after 10 days)

When embedding a database using 'rhc cartridge add', you can reference environment
variables for username, host and password:

    $_ENV['OPENSHIFT_MYSQL_DB_HOST']      - DB host
    $_ENV['OPENSHIFT_MYSQL_DB_PORT']      - DB Port
    $_ENV['OPENSHIFT_MYSQL_DB_USERNAME']  - DB Username
    $_ENV['OPENSHIFT_MYSQL_DB_PASSWORD']  - DB Password

To get a full list of environment variables, simply add a line in your
.openshift/action_hooks/build script that says "export" and push.


###### deplist.txt
A list of pears to install, line by line on the server.  This will happen when
the user git pushes.


###### Additional information
To access the Zend Management console go to http://app-domain.rhcloud.com/ZendServer
