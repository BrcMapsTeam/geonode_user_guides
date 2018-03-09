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
    __For our purpose, we will only have one user, admin. Therefore keep the below commented out. The admin should have access to the entire tree.__
   
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
    
    - As we are only planning to allow FTP access on a case-by-case basis, set up the configuration so that access is given to a user only when they are explicitly added to a list rather than by default:

    ```
    userlist_enable=YES
    userlist_file=/etc/vsftpd.userlist
    userlist_deny=NO
    ```

    `userlist_deny` toggles the logic. When it is set to "YES", users on the list are denied FTP access. When it is set to "NO", only users on the list are allowed access. 
    
    Now, save and exit the file.

16. Create and add a user to the file. Use the `-a` flag to append to file

    `echo "admin" | sudo tee -a /etc/vsftpd.userlist`
    
    Double check it is as expected
    
    ` cat /etc/vsftpd.userlist`
    
    ```
    Output
    admin
    ```
     
17. Restart the daemon to load the configuration changes:

    `sudo systemctl restart vsftpd`

## Testing

18. We have configured the server to allow only the user `admin` to connect via FTP. Let's make sure that's the case.

    __Anonymous users should fail to connect.__ We disabled anonymous access. Here we'll test that by trying to connect anonymously. If we've done it properly, anonymous users should be denied permission:
    
    `ftp -p IP/HOSTNAME 45000`
    
    _Note: The port number is the same as we put in the conf file. If the listen_port was not included, then the 45000 is not required and the ftp will listen to the default port.__ 
    
    
    ```
    Output
    Connected to IP/HOSTNAME
    220 (vsFTPd 3.0.3)
    Name (203.0.113.0:default): anonymous
    530 Permission denied.
    ftp: Login failed.
    ftp>
    ```
    
    To close the connection
    
    ```
    ftp> bye
    ```
    
    __Users other than admin should fail to connect__
    
    `ftp -p IP/HOSTNAME 45000`
    
     ```
    Output
    Connected to IP/HOSTNAME
    220 (vsFTPd 3.0.3)
    Name (203.0.113.0:default): admin
    331 Please specify the password.
    Password: your_user's_password
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp>
    ```
    
    We'll change into the `files` directory, then use the get command to transfer the test file we created earlier to our local machine:
    
    ```
    ftp> cd files
    ftp> get test.txt
    ```
    
    ```
    Output
    227 Entering Passive Mode (203,0,113,0,169,12).
    150 Opening BINARY mode data connection for test.txt (16 bytes).
    226 Transfer complete.
    16 bytes received in 0.0101 seconds (1588 bytes/s)
    ftp>
    ```
    
    We'll turn right back around and try to upload the file with a new name to test write permissions:
    
    `ftp> put test.txt upload.txt`
    
    ```
    Output
    227 Entering Passive Mode (203,0,113,0,164,71).
    150 Ok to send data.
    226 Transfer complete.
    16 bytes sent in 0.000894 seconds (17897 bytes/s)
    ```
    
    Close the connection:
    
    `ftp> bye`
    
    Now that we've tested our configuration, we'll take steps to further secure our server.

19. Securing Transactions
    
    Since FTP does not encrypt any data in transit, including user credentials, we'll enable TTL/SSL to provide that encryption. The first step is to create the SSL certificates for use with vsftpd.

    We'll use openssl to create a new certificate and use the -days flag to make it valid for one year. In the same command, we'll add a private 2048-bit RSA key. Then by setting both the -keyout and -out flags to the same value, the private key and the certificate will be located in the same file.

    We'll do this with the following command:
    
    `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem`
    
    There will be a prompt to provide address information for your certificate. Substitute your own information for the questions below:
    
    ```
    Output
    Generating a 2048 bit RSA private key
    ............................................................................+++
    ...........+++
    writing new private key to '/etc/ssl/private/vsftpd.pem'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:**
    State or Province Name (full name) [Some-State]:******
    Locality Name (eg, city) []:******
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:******* *** *****
    Organizational Unit Name (eg, section) []:***
    Common Name (e.g. server FQDN or YOUR name) []: ***
    Email Address []:****@********.***.**
    ```
    
20. Once the certificate has been created, open the vsftpd configuration file

    `sudo gedit /etc/vsftpd.conf`
    
    _we have used gedit, you can also use nano_
    
    Toward the bottom of the file, there are two lines that begin with rsa_. Comment them out:

    ```
    # rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
    # rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
    ```
    
    Below them, add the following lines which point to the certificate and private key just created:

    ```
    rsa_cert_file=/etc/ssl/private/vsftpd.pem
    rsa_private_key_file=/etc/ssl/private/vsftpd.pem
    ```
    
21. After that, we will force the use of SSL, which will prevent clients that can't deal with TLS from connecting. This is necessary in order to ensure all traffic is encrypted but may force your FTP user to change clients. Change ssl_enable to YES:
    
    ```
    ssl_enable=YES
    ```
    
    Add the following lines to explicitly deny anonymous connections over SSL and to require SSL for both data transfer and logins:
    
    ```
    allow_anon_ssl=NO
    force_local_data_ssl=YES
    force_local_logins_ssl=YES
    ```
    
    Configure the server to use TLS, the preferred successor to SSL by adding the following lines:
        
    ```
    ssl_tlsv1=YES
    ssl_sslv2=NO
    ssl_sslv3=NO
    ```
    
    Finally, add two more options. First, we will not require SSL reuse because it can break many FTP clients. We will require "high" encryption cipher suites, which currently means key lengths equal to or greater than 128 bits:
   
    ```
    require_ssl_reuse=NO
    ssl_ciphers=HIGH
    ```
   
    Save and close the file
   
22. Now, restart the server for the changes to take effect

    `sudo systemctl restart vsftpd`
    
    At this point, we will no longer be able to connect with an insecure command-line client. If we tried, we'd see something like:
    
    ```
    ftp -p 203.0.113.0
    Connected to 203.0.113.0.
    220 (vsFTPd 3.0.3)
    Name (203.0.113.0:default): admin
    530 Non-anonymous sessions must use encryption.
    ftp: Login failed.
    421 Service not available, remote server has closed connection
    ftp>
    ```
    
### The FTP has now been set up. To test and/or use the FTP, we use filezilla. Instructions are found in the next guide section.
