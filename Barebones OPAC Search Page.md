# Barebones OPAC Search Page

### Creating the HTML Page and a PHP Search Page

Create a basic HTML page that contains a form for entering queries. The name of the HTML page with the search form will be named opacbb.php. When a user clicks on the submit button in the form, the form will activate a PHP script called search.php. That search.php script will establish a connection to the OPAC database. The PHP script will contain a MySQL query that will search all the fields in the books table. The query will iterate through each row of the books table and return results that match the search citeria. Two date fields will be added to the form to limit results by publication date, which will be labeled as copyright in the MySQL books table.

__Note: Editing text files in nano may need to use 'sudo'__

##### HTML Form

Copy the HTML code below into opacbb.php
```
<html>
<head>
<title>MySQL Server Example</title>
</head>
<body>

<h1>A Basic OPAC</h1>

<p>In the form below,
<b>optionally</b> enter text in the search field.
You can search by author, title, or publisher.
Capitalization is not necessary.
It's okay to enter partial information,
like part of an author's, title's, or publisher's name.</p>

<p>The date fields are <b>required</b>.
You can use the date fields to limit results.
I added some extra records,
which you can view to know what you can query:</p>

<p><a href="http://11.111.222.222/opac.php">http://11.111.222.222/opac.php</a></p>

<p>This is very much a toy, stripped down OPAC.
The records are basic.
Not only do they not conform to MARC,
but they don't even conform to something
as simple as Dublin Core.
I also don't provide options
to select different fields,
like author, title, or publisher fields.
Instead the search field below searches
all the fields in our <b>books</b> table.
The key idea is to get a sense,
an intuition, of how an OPAC works, though.</p>

<h2>My Basic Library OPAC</h2>
<form method="post" action="search.php">
    <label for="search">Search:</label>
    <input type="text" name="search" id="search">
    <br>
    <label for="start_date">Start Date:</label>
    <input type="date" name="start_date" id="start_date">
    <br>
    <label for="end_date">End Date:</label>
    <input type="date" name="end_date" id="end_date">
    <br>
    <input type="submit" value="Search">
</form>


</body>
</html>
```

##### PHP Search Script

Copy the code below into the search.php file. 
__Change the IP addess in the last line of code to be my address (34.123.18.251)__

```
<?php
// Load MySQL credentials
require_once 'login.php';

// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");

// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");

// Check if search query was submitted
if (isset($_POST['search'])) {
    // Sanitize user input to prevent SQL injection attacks
    $search = mysqli_real_escape_string($conn, $_POST['search']);

    // Get the start and end dates for the date range
    $start_date = mysqli_real_escape_string($conn, $_POST['start_date']);
    $end_date = mysqli_real_escape_string($conn, $_POST['end_date']);

    // Build the MySQL query with a WHERE
    // clause that includes the date range filter
    $query = "SELECT * FROM books WHERE
        (author LIKE '%$search%' OR
        title LIKE '%$search%' OR
        publisher LIKE '%$search%') AND
        copyright BETWEEN '$start_date' AND '$end_date'";

    // Execute the query
    $result = mysqli_query($conn, $query);

    // Check if any results were returned
    if (mysqli_num_rows($result) > 0) {
        // Loop through the results and output them
        while ($row = mysqli_fetch_assoc($result)) {
            echo "ID: " . $row["id"] . "<br>";
            echo "Author: " . $row["author"] . "<br>";
            echo "Title: " . $row["title"] . "<br>";
            echo "Publisher: " . $row["publisher"] . "<br>";
            echo "Copyright: " . $row["copyright"] . "<br><br>";
        }
    } else {
        echo "No results found.";
    }

    // Free up memory by closing the MySQL result set
    mysqli_free_result($result);
}

// Close the MySQL connection
mysqli_close($conn);

echo "<p>Return to search page: <a href='http://11.111.222.222/opacbb.php'>http://11.111.222.222/opacbb.php</a></p>";

?>
```

##### MySQL Insert 
###### Login into MySQL
```
mysql -u opacuser -p opac2023
```

###### Run the insert command with the data for the new records:
```
insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown \& Company', '2010-08-06'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-27');
```

##### Load the page
http://34.123.18.251/opacbb.php
