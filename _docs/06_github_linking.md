List of file locations

Settings file:

'/etc/geonode/local_settings.py

MEDIA_ROOT = '/var/www/geonode/uploaded'

STATIC_ROOT = '/var/www/geonode/static/'

TEMPLATE_DIRS = '/etc/geonode/templates' (empty atm)

STATICFILES_DIRS = '/etc/geonode/media' (empty atm)

1. Installing git (you don't need to do this again if it is already installed
Adding git to watch folders we care about:

```
sudo apt-get install git
git config --global user.name "Zibethin"
git config --global user.email "emailaddress@email.com"
```

Note: you can change the commit name above ^ if anyone else works on this

2. Tracking the folder you need
Go to the folder you want to keep track of and check the size of the folder (right click - Properties) if a reasonable size (in MB) then you can add it to git.

Navigate to the folder above the folder you wish to track (for example here, we will track the folder called "static" which sits at 
/var/www/geonode/static

```
cd /var/www/geonode/
```
Create a git repository there
```
git init static
```
Go to the folder
```
cd static
```










