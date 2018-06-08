List of file locations

Settings file:

'/etc/geonode/local_settings.py

MEDIA_ROOT = '/var/www/geonode/uploaded'

STATIC_ROOT = '/var/www/geonode/static/'

TEMPLATE_DIRS = '/etc/geonode/templates' (empty atm)

STATICFILES_DIRS = '/etc/geonode/media' (empty atm)

Adding git to watch folders we care about:

```
sudo apt-get install git
git config --global user.name "Zibethin"
```

Note: you can change the commit name above ^ if anyone else works on this










