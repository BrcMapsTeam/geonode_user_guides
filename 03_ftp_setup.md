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
    
 5. We'll need to open ports 20 and 21 for FTP, port 990 for later when we enable TLS, and ports 40000-50000 for the range of passive ports we plan to set in the configuration file:
 
    ```
    sudo ufw allow 20/tcp
    sudo ufw allow 21/tcp
    sudo ufw allow 990/tcp
    sudo ufw allow 40000:50000/tcp
    sudo ufw status
    ```
    The firewall rules should now be updated. 
    
    vsftpd is now installed and the neccassary ports are open.
    
6.  The next step is to prepare the user directory, which will have access to the FTP.

    Create a user
    
    `sudo adduser admin`
    
    You will be prompted to assign a password.
    
    Press ENTER through other prompts.
    
7.  Now create the `ftp` folder

    `mkdir /home/admin/ftp`
    
    set ownership
    
    `sudo chown nobody:nogroup /home/admin/ftp`
    
    remove all write permissions
    
    `sudo chmod a-w /home/admin/ftp`
    
8.  Now verify the permissions we have just set.

    `sudo ls -la /home/admin/ftp`
    
    ``` 
    Output
    total 8
    4 dr-xr-xr-x  2 nobody nogroup 4096 Aug 24 21:29 .
    4 drwxr-xr-x 3 sammy  sammy   4096 Aug 24 21:29 ..
    ```
    
9.  Now create the directory where files can be uploaded. Assign ownership to the uder

    `sudo mkdir /home/admin/ftp/files`
    
    `sudo chown admin:admin /home/admin/ftp/files`
    
10. Check the permissions again

    ` sudo ls -la /home/admin/ftp`
    
    ```
    Output
    total 12
    dr-xr-xr-x 3 nobody nogroup 4096 Aug 26 14:01 .
    drwxr-xr-x 3 admin  admin   4096 Aug 26 13:59 ..
    drwxr-xr-x 2 admin  admin   4096 Aug 26 14:01 files
    ```
11. We will now add a `test.txt` file to use when we test the FTP later on

    `echo "vsftpd test file" | sudo tee /home/sammy/ftp/files/test.txt`
    
    Now that we have the `ftp` directory and allowed user access to the `files` directory the next step is configuaration.
    
12. We're planning to allow a single user with a local shell account to connect with FTP. The two key settings for this are already set in `vsftpd.conf`. Start by opening the config file   

    `sudo gedit /etc/vsftpd.conf`
    
    I have used gedit, but you can also use nano
    
13. In `vsftpd.conf` check that the settings in your configuration match those below:

    
    ```
    . . .
    # Allow anonymous FTP? (Disabled by default).
    anonymous_enable=NO
    #
    # Uncomment this to allow local users to log in.
    local_enable=YES
    . . .
    ```

14. We will need to change/ uncomment some lines in the file. In order to allow the user to upload files, we’ll uncomment the `write_enable` setting so that we have:

    ```
    . . .
    write_enable=YES
    . . .
    ```
    
    Also uncomment the chroot to prevent the FTP-connected user from accessing any files or commands outside the directory tree.
    
   
    ```
    . . .
    chroot_local_user=YES
    . . .
    ```

15. At the bottom of the conf file add the following:
    
    - add a user_sub_token in order to insert the username in our local_root directory path so our configuration will work for this user and any future users that might be added.
    
    
    ```
    user_sub_token=$USER
    local_root=/home/$USER/ftp
    ```
    
    - Limit the range of ports that can be used for passive FTP to make sure enough connections are available:
    
   
    ```
    pasv_min_port=40000
    pasv_max_port=50000
    ```
    
    _Note: Earlier, we pre-opened the ports that we set here for the passive port range. If you change the values, be sure to update your firewall settings.
    
    - Also add a directive telling `vsftpd` to listen on a particular port for incoming FTP connections
    
   
    ```
    listen_port=45000
    ```

16. 

    
    
    
    
    
    
    
