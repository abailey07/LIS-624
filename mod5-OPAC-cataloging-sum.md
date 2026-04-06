# Module 5 - OPAC and Cataloging Module Documentiation and Reflection

## Library OPACs and Relational Databases

For the University of Kentucky's System Librarianship class (LIS 624), we are building an open source integrated library system (ILS) over the course of this semester.  The following is a summary and documentation of module 5 where we created a basic OPAC (Online Public Access Catalog) and use MySQL to create a relational database to hold the informations for "library" that the OPAC searches.

An OPAC (Online Public Access Catalog) is the current way that library users search for items that the library provides access to.  For most library OPACs, this includes both physical items and digital access items.  Before computers were a regular piece of equipment, libraries used card catalogs, where each item was typed on a card and kept in organized drawers for searching.  Depending on the library, these drawers might not have been accessible to the general user.  With most OPACs open for anyone to use, the ability to search a library's collection is open to all.

The underlying functionality of an OPAC is done through relational databases.  A relational database is a database where the data is stored in fixed rows and columns, and because of this structure, relationships can be determined within the table and between tables.  This is what allows an OPAC to execute simple to complex searches using SQL queries.  The OPAC searches are programmed to construct SQL queries on behalf of the user, submit the query to the database, receive the response, and display the response in a meaningful way to the user.  This module had us constructing these two items from the ground up, rather than relying on software.

## Setup and Construction OPAC and Cataloging Modules

Below are the step taken to setup and construct a relational database in MySQL, an OPAC on my LAMP stack, and a cataloging module that will insert information into the database through a webpage.  In the portion where we worked with a relational database, some that process was practice for learning MySQL syntax for creating tables, and entering and manipulating data.  This practice is not part of building the OPAC.  This practice can be found in detail in our class [textbook](https://cseanburns.github.io/systems-librarianship/5a-introduction-to-relational-databases.html).  The intial set up of this database was done as part of Module 4.  I am including those steps again here to give a complete picture of this specific work.

### Setting up and Constructing a Relational Database for OPAC

1. Create a database in MySQl and give the user profile permissions to needed.
    1. `sudo mysql -u root`.  This logs us in as the root user of MySQL who has full access.
    2. `mysql> create database opacdb;`
    3. `mysql> grant all privileges on opacdb.* to 'opacuser'@'localhost';`
    4. We can double check what permissions are granted using `mysql> show grants for 'opacuser'@'localhost';`
2. Create table "Books" and enter data.
    1. Log into MySQL user opacuser with password.  This was established in [Module 4](https://github.com/abailey07/LIS-624/blob/main/module4-LAMPdocumentation.md).
    2. Create a table for books: 
         ```
        mysql> create table books (
        id int unsigned not null auto_increment,
        author varchar(150) not null,
        title varchar(150) not null,
        publisher varcahr(75), 
        copyright date not null,
        primary key (id)
        );
        ```    
    3. The following titles were inserted:
        ```
        mysql> insert into books (author, title, copyright) values
        ('Jennifer Egan', 'The Candy House', '2022-01-01'),
        ('Imbolo Mbue', 'How Beautiful We Were', '2021-01-01'),
        ('Lydia Millet', 'A Children's Bible', '2020-01-01'),
        ('Julia Phillips', 'Disappearing Earth', '2019-01-01'),
        ('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010-01-01'),
        ('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-01');
        ```
	
### Create OPAC Webpage

1. Navigate to `/var/www/html`, the document root for the webserver.
2. Create file for OPAC: `sudo tilde mylibrary.html`
3. Insert the following code for OPAC search webpage, then save and exit.
    ```
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>MySQL Server Example</title>
    </head>
    <body>
    <h1>A Basic OPAC</h1>
    <p>In the form below, <b>optionally</b> enter text in the search field.
    Your search query will search by author, title, or publisher.
    Capitalization is usually not necessary on default case-insensitive MySQL collations.
    It's okay to enter partial information, like part of an author's, title's, or publisher's name.</p>
    <p>You can leave the search field empty and only enter dates.
    Regardless, both start and end dates are required for all searches.
    You can use the date fields to limit results, too.
    I added some extra records, which you can view to know what you can query:</p>
    <p><a href="opac.php">OPAC</a></p>
    <p>This is very much a toy, stripped down
    <a href="https://en.wikipedia.org/wiki/Online_public_access_catalog">OPAC</a>.
    The records are basic.
    Not only do they not conform to <a href="https://www.loc.gov/marc/">MARC</a>,
    they don't even conform to something as simple as <a href="https://www.dublincore.org/">Dublin Core</a>.</p>
    <p>I also don't provide options to select different fields, like author, title, or publisher fields.
    Instead the search field below searches key bibliographic fields (author, title, publisher) in our <b>books</b> table.</p>
    <p>The key idea is to get a sense of how an OPAC works, though.</p>
    <h2>My Basic Library OPAC</h2>
    <form method="post" action="search.php">
        <label for="search">Search Terms (optional):</label>
        <input type="text" name="search" id="search">        
        <br>        
        <label for="start_date">Start Date:</label>
        <input type="date" name="start_date" id="start_date" required>        
        <br>        
        <label for="end_date">End Date:</label>
        <input type="date" name="end_date" id="end_date" required>        
        <br>        
        <input type="submit" value="Search">
    </form>
    </body>
    </html>
    ```
    Notice in this HTML that we currently have it set that both start and end dates are required for a query submission.  This can be removed by deleting the word from each spot.  One could also make it a requirement to for something to be added the box "Search Terms" by adding the same word in the input tag and removing "optional" from the label tag for clarity.
4. Create file for the PHP that will connect to the database and perform the search: `sudo tilde search.php`.
5. Insert the following code for `search.php`, then save and exit.
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Search Results</title>
    <style>
    table {
        border-collapse: collapse;
        width: 100%;
    }
    th, td {
        border: 1px solid black;
        padding: 8px;
        text-align: left;
    }
    </style>
    </head>
    <body>
    <h1>Search Results</h1>
    <?php
    // Load MySQL credentials
    require_once '/var/www/login.php';
    // Enable MySQL error reporting
    mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
    // Establish connection
    $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        $search = trim($_POST['search']);
        $start_date = $_POST['start_date'];
        $end_date = $_POST['end_date'];
        // Prepared statement to prevent SQL injection
        $stmt = $conn->prepare("SELECT id, author, title, publisher, copyright FROM books 
                                WHERE (author LIKE ? OR title LIKE ? OR publisher LIKE ?) 
                                AND copyright BETWEEN ? AND ?");
        // Use wildcard search
        $search_param = "%$search%";
        $stmt->bind_param("sssss", $search_param, $search_param, $search_param, $start_date, $end_date);
        $stmt->execute();
        $result = $stmt->get_result();
        if ($result->num_rows > 0) {
            echo "<table>";
            echo "<tr><th>ID</th><th>Author</th><th>Title</th><th>Publisher</th><th>Copyright</th></tr>";
            while ($row = $result->fetch_assoc()) {
                echo "<tr>";
                echo "<td>" . htmlspecialchars($row["id"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["author"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["title"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["publisher"]) . "</td>";
                echo "<td>" . htmlspecialchars($row["copyright"]) . "</td>";
                echo "</tr>";
            }
            echo "</table>";
        } else {
            echo "<p>No results found.</p>";
        }
        $stmt->close();
    }
    $conn->close();
    ?>
    <p><a href="mylibrary.html">Return to search page</a></p>
    </body>
    </html>
    ```
### Create Cataloging Module, Webpage Interface, and Set Password Protection 

1. Create a cataloging directory off of root directory.
    1. Navigate to `/var/www/html`.
    2. `sudo mkdir cataloging`
2. Create cataloging webpage in cataloging directory.
    1. `cd cataloging`
    2. `sudo tilde index.html`
    3. Insert the following code into `index.html`, then save and exit.
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
                <input type="date" name="copyright" id="copyright" required>
                <input type="submit" value="Submit">
            </form>
        </body>
        </html>
        ```
        Within this code, you can see this is an HTML form that when used is pushed through to `insert.php`, which does the work of inserting this information into the database using PHP.  Note also that each of the value fields (author, title, publisher, copyright) are required fields.  If one of these fields is left blank, the data will not be inserted.
3. Create PHP file and insert code that will take data from the webpage and insert it into the database.
    1. Navigate to `/var/www/html/cataloging`.
    2. Create the file `sudo tilde insert.php` for the code that will take the information entered in the web page fields and insert it into the table "books" in the database.
    3. Insert the following code in `insert.php`, then save and exit.
    ```
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Cataloging: Data Entry</title>
    </head>
    <body>
        <h1>Cataloging: Data Entry</h1>
        <?php
        // Load MySQL credentials
        require_once '/var/www/login.php';
        // Enable MySQL error reporting
        mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
        // Establish connection
        $conn = new mysqli($db_hostname, $db_username, $db_password, $db_database);
        if ($conn->connect_error) {
            die("Connection failed: " . $conn->connect_error);
        }
        if ($_SERVER["REQUEST_METHOD"] === "POST") {
            $author = trim($_POST["author"] ?? "");
            $title = trim($_POST["title"] ?? "");
            $publisher = trim($_POST["publisher"] ?? "");
            $copyright = $_POST["copyright"] ?? "";
        if ($author === "" || $title === "" || $publisher === "" || $copyright === "") {
        echo "All fields are required.";
        } elseif (!preg_match('/^\d{4}-\d{2}-\d{2}$/', $copyright)) {
            echo "Copyright date must use YYYY-MM-DD format.";
        } else {
        // Prepare and bind SQL statement
        $stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
        $stmt->bind_param("ssss", $author, $title, $publisher, $copyright);
        if ($stmt->execute() === TRUE) {
            echo "New record created successfully";
        } else {
            echo "Error: " . $stmt->error;
        }
        $stmt->close();
        }
        } else {
            echo "Please submit records using the cataloging form.";
        }
        // Close connection
        $conn->close();
        ?>
        <p><a href='index.html'>Return to Cataloging Page</a></p>
        <p><a href='../mylibrary.html'>Return to Library Home Page</a></p>
    </body>
    </html>
    ```
    Notice that the PHP is nested inside HTML.  This allows for the echo of the results and the link back to the main OPAC page.  Nested inside the PHP are the rules for connecting to the database and what permissions it needed to give to insert the data.  There are rules for what is echoed if there are errors.  Most importantly, inside the PHP is the SQL statement that performs the insertion of the data values into the correct columns.
4. Set security for the cataloging module with a user name and password
    1. Since the cataloging module is a web page, we need to secure it so that only those who we want entering data are able to.  We use the Apache authentication mechanism `htpasswd`.  Be careful on each step!  If something is mistyped, it could lead to the cataloging module becomeing inaccessible.  
    2. Create the authentication file: `sudo htpasswd -c /etc/apache2/.htpasswd xxxxxxxx`.  The "xxxxxxxx" is a user name of the creator's choosing.  The authentication file is being stored in Apache's configuration files space.  Of note, `-c` is used when this file is created for the first time.  It isn't needed for additional profiles.  Also, `.htpasswd` becomes a hidden file.  To view hidden files, use `ls -a`.
    3. Edit the Apache configureation file so that is knows to use `.htpasswd` to control access to the cataloging module.  Open this in a text editor: `sudo tilde /etc/apache2/apache2.conf`
    4. Create a new stanza that will apply to the cataloging module.  We will place this under the stanza blcok for `<Directory /var/www/>`.  Insert the following code, then save and exit.
    ```
    <Directory /var/www/html/cataloging/>
         Options Indexes FollowSymLinks
        AllowOverride AuthConfig
        Require all granted
    </Directory>
    ```
    5. Create file `.htaccess` in `/var/www/html/cataloging` to ensure that all web requests to the cataloging directory are authenticated.
        1. `sudo tilde .htaccess`
        2. Insert the following, then save and close.
        ```
        AuthType Basic
        AuthName "Authorization Required"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
        ```
5. Verify that the new Apache configuration is okay with `sudo apachectl configtest`.  A sucessful test will say "Syntax OK".
6. Restart and confirm Apache is running again:
    1. `sudo systemctl restart apache2`
    2. `sudo systemctl status apache2`
7. Give the Apache server the permission to make read/write access to the database.  The Apache user account is `www-data`.
    1. Grant group ownership to the Apache server user: `sudo chown :www-data /var/www/html`.
    2. Modify the SetGID bit for `/var/www/html` so that anything added to this directory in the future will inherit the same read/write permissions.
        1. `sudo find /var/www/html -type d -exec chmod g+s {} +`
8. We can confirm that `htpasswd` is working by using `curl -I http://34.45.197.17/cataloging/index.html`.  If a 401 Unauthorized error is returned, we know that it is working.
        
### Verification of Work and Challenges Overcome

The biggest issue was setting up the authentication.  It took 3 tries before it was functioning as intended.  In deleting my work to start from scratch, I learned that the `.` in front of the `.htpasswd` and `.htaccess` means they are hidden files.  In order to see them and to confirm if I had deleted them or not, I learned that I could use `ls -a`.  The issue could have stemmed from a `_` used in the password or the formating of the `/cataloging/index.html` or `/cataloging/insert.php`.  Unfortuantely, I made 2 changes on the same and thus can't verify where the issue was.    
	

