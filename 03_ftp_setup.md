# FTP Install and Set-Up

These instructions are guided by this source: https://www.digitalocean.com/community/tutorials/how-to-set-up-vsftpd-for-a-user-s-directory-on-ubuntu-16-04

## Install

1.  First we need to update package list:
    
    `sudo apt-get update`
    
    and then install vsftpd daemon
    
    `sudo apt-get install vsftpd`
    
3.  Once complete, we'll copy the configuration file so we can start with a blank configuration, saving the original as a backup.
    
    `sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig`
    
    Now that the configuration is in place, the next step is to configure the firewall.

## Set-Up

4. Firstly we shall check the status of the firewall to see if it is enabled. This step will identify whether FTP traffic is permitted.

    `sudo ufw status`
    
    Outputs:
    
    If the firewall is inactive 
    
    ```
    status: inactive
    ```
    
    If the firewall is active
    
    ```
    Status: active

    To Action  From
    -- ------  ----
    OpenSSH ALLOW   Anywhere
    OpenSSH (v6)   ALLOW   Anywhere (v6)
    ```
    
    There may be a variety of other rules in place.
    
    To see this information, but not in the terminal and with a GUI, you can install gfuw
    
    `sudo apt-get install gufw`
    
    
    
    
