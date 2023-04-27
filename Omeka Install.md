# Omeka Install

### Prerequisites:
1. Install ImageMagick: this is a suite of utilities to work with photo files. It's used by Omeka to create thumbnail images of any photo uploaded to the digital library. 
```
sudo apt install imagemagick
```
2. Enable Apache mod_rewrite. This is an Apache module used to rewrite URLs. Omeka uses this to create appropriate URLs for items and collections in its digital libraries.
```
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### Useful Links:
- **Omeka:** https://omeka.org/
- **Omeka Classic:** https://omeka.org/classic/
- **Omeka Classic User Manual:** https://omeka.org/classic/docs/

### Installation:

1. Create a new user and a new database in MySQL for the Omeka installation
```
sudo su
mysql -u root
create user 'omeka'@'localhost' identified by 'Omeka2023_burns';
create database omeka;
grant all privileges on omeka.* to 'omeka'@'localhost';
show databases;
\q
```

2. Use wget to download Omeka Classic as a Zip file and extract it in /var/www/html:
```
cd /var/www/html
sudo wget https://github.com/omeka/Omeka/releases/download/v3.1/omeka-3.1.zip
sudo unzip omeka-3.1.zip
```
**Note: Use Omeka Classic and not Omeka S**

3. Add database credentials to the db.ini file. Replace all values containing XXXXXX, with the appropriate information. 
```
sudo nano db.ini
```

4. Change ownership of the files in the directory.
```
sudo chown -R www-data:www-data *
```

5. Restart Apache server and MySQL
```
sudo systemctl restart apache2
sudo systemctl restart mysql
```

6. In web browser, go to http://34.121.182.91//omeka/ to complete the setup via the web form


