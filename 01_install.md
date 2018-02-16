# 01 Installation

## General info
Reference: http://docs.geonode.org/en/latest/index.html

We will install Geonode 2.8 on Ubuntu 16.04 64bit

This install guide will be similar to the link above, but will be specific for our purpose.

## Install

1.  Log in to our server using the IP and open the terminal.

2.  Run the command. This ensures that all packages are up to date. (This should have been completed by IT already, but just to check)

    `sudo apt-get update; sudo apt-get upgrade`
    
    "Amongst other things, this will ensure that the software-properties-common package is installed, which is required to make the add-apt- repository command used below available."
    
    You will be prompted for a password. Enter the password to the server
    
    You will be promted to confirm if you want to continue, type Y and enter
    
    It will start to update
    
    This will take a few minutes

3.  The steps to install geonode and all dependencies in Ubuntu 16.04 are as follows:

    Set up the GeoNode PPA repository (you only need to do this once; the repository will still be available for upgrades later):
    
    This downloads the stable version of the package

    `sudo add-apt-repository ppa:geonode/stable`
    
    You will be asked to confirm, by pressing enter

4.  Install the geonode package and dependencies:
    You can do this one by bone or all at once

    `sudo apt-get update; sudo apt-get upgrade; sudo apt-get autoremove
    sudo apt-get install geonode`

    During our `apt-get autoremove` we removed snap-confine v2.29.4.2

    You will be asked to confirm

5.  It will now install

    You may get a couple of warnings about the Django version.
    The Django version which is installed is: 1.8.7

6. Create a superuser account

This will be used in the geonode web application. You will be prompted for a username, email and password:
    `geonode createsuperuser`

7. The next step is to det the correct ip address. Change 'localhost' to the IP
    `sudo geonode-updateip localhost` 
        
    6.1     To find out the name of the server   
            `tracepath IPADDRESS`

    -> We added the IP address as the command was updateip, rather than DN/HOSTNAME
    
    

