logio-initscript-Debian
================

Log.io server and harvester init scripts for Debian


Installation
------------

## Log.io ##

Add a new user for log.io. Run the following commands as root.

    useradd -d /home/logio logio
    usermod -Glogio,adm logio
    usermod logio -s /bin/bash
    mkdir /home/logio
    chown logio /home/logio

Install nodejs and npm, if not already installed

    sudo apt-get install git-core curl build-essential openssl libssl-dev
    git clone https://github.com/joyent/node.git
    cd node
    # now go to nodejs website and find out which version is latest Stable.
    git checkout v0.10.25
Install node.js: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
  
Login as user logio and install log.io

    su - logio
    npm install log.io
	
   exit # to go back to root shell
	
## Init Script ##

Create the directories for the log and pid files (Run as root).

    mkdir -p /var/log/logio/
    mkdir -p /var/run/logio/
    
Change the ownership of the newly created directories to the user running Log.io. (Change logio:logio, with your username and group).

    chown logio:logio /var/log/logio
    chown logio:logio /var/run/logio

Edit the wrapper scripts (available in `wrapper-scripts` directory) and update the `COMMAND` variable to point to the log.io scripts. 
If you followed the above steps for installing log.io, then this step is not required.

Copy the files to `/usr/local/bin`
    
    cp wrapper-scripts/log.io-harvester /usr/local/bin/
    
Edit the init scripts (available under `init.d` directory) and update the `WRAPPERSCRIPT` variable 
to point to the location of the corresponding wrapper script. If you copied the files to /usr/local/bin, then this step is not required.

Copy the init scripts to `/etc/init.d`

Your configuration files will be under `~log.io/.log.io/` directory. Update the harvester.conf file to add a new harvester.

To start the server, run

    service logio-server start
    
To start the harvester, run

    service logio-harvester start
