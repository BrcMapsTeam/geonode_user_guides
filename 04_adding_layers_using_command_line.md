# Adding layers (shapefiles) to Geonode using the command line

1.  Open VNC and access the terminal in Ubuntu
    You can navigate to this or by pressing Ctrl + Alt + T

2.  Navigate to the files folder:

    `cd /home/admin/ftp/files`

    **CHECK IF THIS FILE PATH CORRECT**
    
3.  Import the files
    
    `sudo geonode importlayers <foldername> -u  <your geonode username>`
    
    **Please ask which user to use as the Geonode username**
    
    It may ask for a password

    See parameters for importlayers at: http://docs.geonode.org/en/master/tutorials/admin/loading_data_into_geonode/importlayers.html

    This command will then trigger the upload, the progress of which can be seen in the command line in the Ubuntu session.

4.  Change the owner of the uploaded files 

    (This makes the files accessible to the webserver user ‘www-data’)

    Using the command line, navigate to the uploaded layers folder:

    `cd /var/www/geonode/uploaded/layers`

    Then change the owner using the following commands for each of the file types in the shapefile
    For example:

    ```
    sudo chown www-data:www-data rofrs_v201704_extract_high.dbf
    sudo chown www-data:www-data rofrs_v201704_extract_high.prj
    sudo chown www-data:www-data rofrs_v201704_extract_high.shp
    sudo chown www-data:www-data rofrs_v201704_extract_high.shx
    ```


5.  You may need to get the admin user to change ownership of the layer before you can edit it using the geonode website interface.
