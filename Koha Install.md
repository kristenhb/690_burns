# Koha ILS Install

### Useful Links:
- https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/
- https://koha-community.org/
- https://wiki.koha-community.org/wiki/Koha_on_Debian
- https://www.youtube.com/watch?v=mzUop9R4sKc

### Google Cloud Setup:
#### Create a new virtual machine instance and configure the Google firewall to allow HTTP traffic to our Koha install.

1. New Virtual Instance
    - Use E2 Series
    - Set Machine Type 2 vCPU
    - 4 GB Memory
    - Ubuntu 20.04

2. Google Cloud firewall - Create rule to allow traffic on port 8080
    1. Go to the Google Cloud Console:
    2. Click on the hamburger ☰ icon at the top left.
    3. Click on VPN Network
    4. Click on Firewall
    5. At the top of the page, choose Create a firewall rule (do not choose Create a firewall policy)
    6. Add name: koha
    7. Add description: open port 8080
    8. Next to Targets, click on All instances in the network
    9. In the Source IPv4 ranges, add 0.0.0.0/0
    10. Click on Specified protocols and ports
    11. Click on TCP
    12. Add 8080 in the Ports box
    13. Click on Create

### Install Koha:

1. Server setup
    1. Update local repositories
    ```
    sudo apt update
    ```

    2. Upgrade Server
    ```
    sudo apt upgrade
    ```

    3. Space Saving Commands
    ```
    sudo apt autoremove -y && sudo apt clean
    ```

    4. Install gnupg2 - used to create digital signatures, ecrypt data, and aid in secure communication
    ```
    sudo apt install gnupg2
    ```

    5. Reboot Server
    ```
    sudo reboot now
    ```

2. Add Koha Repository
```
sudo su
echo 'deb http://debian.koha-community.org/koha stable main' | sudo tee /etc/apt/sources.list.d/koha.list
wget -q -O- https://debian.koha-community.org/koha/gpg.asc | sudo apt-key add -
```

3. Install Koha
    1. Update repository
    ```
    apt update
    ```

    2. View the package information for Koha:
    ```
    apt show koha-common
    ```

    3.Install package
    ```
    apt install koha-common
    ```
    **Note: The above command will download and install a lot of additional software and process will take     several minutes.**

4. Configure Koha
    1. Edit koh-sites.conf
    ```
    nano /etc/koha/koha-sites.conf
    ```
    2. INTRAPORT="80"  -->  INTRAPORT="8080"
    
5. Install and setup mysql-server:
    1. Install MySQL server
    ```
    apt install mysql-server
    ```

    2. Set MySQL root password
    ```
    mysqladmin -u root password bibliolib1
    ```
    > MySQL Admin Password: syslib2023SQLAdmin

6. Enable URL rewriting and CGI functionality.
```
a2enmod rewrite
a2enmod cgi 
```

7. Restart Apache2
```
systemctl restart apache2
```

8. Create a database for Koha
```
koha-create --create-db bibliolib
```

9. Configure Apache
    1. Tell Apache2 to listen on port 8080:
        1. Open the ports.conf file
        ```
        nano /etc/apache2/ports.conf 
        ```        
        2. Add the following to the file
        ```
        Listen 8080
        ```
    
    2. Verify Apache configuration changes are valid
    ```
    apachectl configtest
    ```

    3. Restart Apache
    ```
    systemctl restart apache2
    ```
    
    4. Disable the default Apache2 setup, enable traffic compression using deflate, enable the bibliolib site
    ```
    a2dissite 000-default
    a2enmod deflate
    a2ensite bibliolib
    ```
    
    5. Reload Apache2's configuration and restart Apache
    ```
    systemctl reload apache2
    systemctl restart apache2
    ```
    
10. Koha Web Installer
    1. Get Koha username and password in the following file
    ```
    nano /etc/koha/sites/bibliolib/koha-conf.xml
    ```
        1. Look for the <config> stanza (line number 252) and the line beginning with <user> (line number 257). The password is on the line after (line number 258).
        
        > Koha Admin User: koha_bibliolib
        
        > Koha Admin Password: Qs,iG,q31-?B(0fR

    2. Go to the web installer at the following URL
    ```
    http://34.125.99.189:8080
    ```

11. Change Settings after web installer steps are complete    
    1. Click on More in the top drop down box
    2. Click on Administration
    3. Click on Global System Preferences
    4. Click on OPAC in the left hand side bar
    5. Scroll down to the OPACBaseURL line.
    6. Enter the IP address of your server: http://34.125.99.189/
    7. Click on Save all OPAC Preferences
    
**Note: Once these preferences are save, visit the public facing OPAC at the server IP address.**

### Users and Patrons 
#### Koha Furry Patrons Administrator Identity:
- Surname: Bolt
- First Name: Kristen
- Card Number: 12291981
- Library: Centerville
- Patron Category: Furry Patrons

#### Koha Furry Patrons Administrator Login
- Username: kbolt 
- Password: kboltAdmin123

#### Patrons:
##### Toast Paulus
- Username: toast
- Password: ToastyCat18

##### Riley Bolt
- Username: riley
- Password: RileyKitty22

References
Helpful documentation and demos:

Koha ILS documentation.
Koha on Debian
Install Koha on Google Cloud Platform
