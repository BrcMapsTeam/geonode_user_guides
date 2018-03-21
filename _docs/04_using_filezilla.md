# Using Filezilla

- So far we have installed GeoNode and setup an FTP.
- Images and guide sourced from: https://www.digitalocean.com/community/tutorials/how-to-set-up-vsftpd-for-a-user-s-directory-on-ubuntu-16-04

1.  Open Filezilla and select the 'Site Manager'

    ![1](http://assets.digitalocean.com/articles/vsftp-user/site-manager.png)

2.  A new window will open. Add a new site.

    ![2](http://assets.digitalocean.com/articles/vsftp-user/new-site.png)

3.  Fill out the host name and port (remember you may has specified a port in setting up your ftp, if not, leave bank or put 21)

    Set the encription to "Require explicit ftp over TLS" and logon type to "Ask for password"
    
    Finally enter the user. In the previous section, setting up the ftp, we used 'admin'
    
    ![3](http://assets.digitalocean.com/articles/vsftp-user/site-config2.png)
    
4.  Click connect. You will be asked for a password. This is the same login details as entered when the ftp was set up.

5.  You should connect. You will see the certificate, accept this.

6.  You should now have the entire tree, as set up in the previous section, or a set folder. You should also be able to upload and download files. 
    
    
