# WordPress Install

### Useful Links:

- https://wordpress.org/
- https://wordpress.org/about/security/
- https://wordpress.org/documentation/article/updating-wordpress/
- https://wordpress.org/documentation/article/how-to-install-wordpress/
- https://wordpress.org/about/requirements/

### Installation:
So far I have shown you how to install software using two methods:

1. Check Requirements
```
php --version
mysql --version
```

2. Install additional php modules then restart apache server and mysql
```
sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl
sudo systemctl restart apache2
sudo systemctl restart mysql
```

3. Download and extract wordpress
```
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
```

Note: /var/www/html/wordpress will be the installation directory

4. Create the database and setup user
```
sudo su
mysql -u root
create user 'wordpress'@'localhost' identified by 'opac2023';
create database wordpress;
grant all privileges on wordpress.* to 'wordpress'@'localhost';
show databases;
\q
```

5. Set up wp-config.php
    1. Change to the wordpress directory
    ```
    cd /var/www/html/wordpress
    ```
    2. Copy and rename the wp-config-sample.php file to wp-config.php.
    ```
    sudo cp wp-config-sample.php wp-config.php
    ```
    3.Edit the file and add your WordPress database name, user name, and password in the fields for DB_NAME, DB_USER, and DB_PASSWORD.
    ```
    sudo nano wp-config.php
    ```
    4. Edit fields for database name, user and password
    5. Disable FTP uploads by adding the line below to the file
        - define('FS_METHOD','direct');
    6. Optional: rename default path from wordpress to something else
    ```
    cd /var/www/html
    sudo mv wordpress blog
    ```
    7. Change File Ownership
    ```
    sudo chown -R www-data:www-data *
    ```
    8. Run the Install Script
        - http://34.121.182.91//blog/wp-admin/install.php


#### Finishing installation
Follow wordpress documentation from this point forward if needed
