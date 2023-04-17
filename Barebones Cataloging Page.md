# Creating a Bare Bones Cataloging Module
Follow these notes to create a bare bones cataloging module in a similar manner as the bare bones OPAC was created. This will provide a graphical user interface to add entries to the database rather than adding them directly to the database using MySQL queries in command line.

### Creating the HTML Page and a PHP Cataloging Page
Create a basic HTML page that contains a form for entering bibliographic data. This will be a simple version and not have all the details of a real world cataloging module. 

The form will need to have the same fields that are contained in the books table that was created in the barebones OPAC assignment. Therefore it will need to contain the following fields:

author
title
publisher
copyright

##### Create a new directory
```
cd /var/www/html
sudo mkdir cataloging
```

##### Create the HTML page

###### Use nano to open the file
```
cd cataloging
sudo nano index.html
```

###### Copy the following HTML code into the new file
```
<!DOCTYPE html>
<html>
<head>
    <title>Enter Records</title>
</head>
<body>
    <h1>OPAC Library Administration</h1>

    <p>This is the library administration page for entering records into the OPAC.</p>
    <p>Please do not use this page unless you are an authorized cataloger.</p>

    <form action="insert.php" method="post">
        <label for="author">Author:</label>
        <input type="text" name="author" id="author" required><br><br>

        <label for="title">Book Title:</label>
        <input type="text" name="title" id="title" required><br><br>

        <label for="publisher">Publisher:</label>
        <input type="text" name="publisher" id="publisher" required><br><br>

        <label for="copyright">Copyright:</label>
        <input type="date" id="copyright" name="copyright">

        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

##### Create the PHP Insert Script
The PHP script is needed to communicate and add the data from the form on the index.html page into the books table in the MySQL database.

###### Use nano to open the file
```
sudo nano login.php
```

###### Copy the following PHP code into the login.php file

__Change the IP addess in the last line of code to be my address (34.123.18.251)__

```
<?php

// Load MySQL credentials
require_once '../login.php';

// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");

// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");

// Prepare and bind SQL statement
$stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
$stmt->bind_param("ssss", $author, $title, $publisher, $copyright);

// Set parameters and execute statement
$author = $_POST["author"];
$title = $_POST["title"];
$publisher = $_POST["publisher"];
$copyright = $_POST["copyright"];

if ($stmt->execute() === TRUE) {
    echo "New record created successfully";
} else {
    echo "Error: " . $stmt->error;
}

// Close statement and connection
$stmt->close();
$conn->close();

echo "<p>Return to the cataloging page: <a href='http://11.111.111.111/cataloging/'>http://11.111.111.111/cataloging/</a></p>";
?>
```

#### Security
Put a simple authorization mechanism in place that is provided by the Apache2 server called htpasswd.  Security in a real world system would be much more complex.

##### Steps:
1. Create authentication file in the /etc/apache2 using the following code:
```
sudo htpasswd -c /etc/apache2/.htpasswd libcat
```

2. Open the apache2.conf file
```
sudo nano /etc/apache2/apache2.conf
```

3. Change AllowOverride to 'All' in the <Directory /var/www> section around line 173
```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride None
  Require all granted
</Directory>

to

<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride All
  Require all granted
</Directory>
```

4. Change to the cataloging directory and use nanon to create a file called '.htcaccess' (include the leading period)
```
cd /var/www/html/cataloging
sudo nano .htaccess
```

5. Add the following content to .htaccess:
```
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```

6. Check that the apache2 configuration is still okay
```
apachectl configtest
```

7. If the config test return OK, restart Apache2 and check the status
```
sudo systemctl restart apache2
systemctl status apache2
```

#### Visit the Cataloging Page

http://34.123.18.251/index.html

__Note: The page should prompt users for a username and password (which is the username and password created with htpasswd)__
