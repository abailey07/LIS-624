## 2026-03-24 - Week 10 - Create a Barebones OPAC and Cataloging Module

**Goal:** Gain deeper understaning of MySQL commands.  Create basic OPAC to search database "Books".  Create web-based basic cataloging module that is password protected to enter items into the database "Books".

**Context:** This is a basic, handmade version of an ILS.  This is providing foundational knowledge before we move to using open source software.

**Steps:**

1. Update and upgrade Linux.  There were two updates.  Performed clean up after.

2. Create database "DinnerDB" with tables Meals and Ingredients to experiment with SQL commands.
	1. Use SELECT in conjunction with WHERE, ORDER BY, AS to rename a column when displayed, GROUP BY, and JOIN.
	2. For additional help on MySQL commands and syntax, see [W3Schools MySQL Documentation](https://www.w3schools.com/sql/default.asp).
	3. Explorded database management including privilage granting and revoking, viewing user accounts, and deleting user accounts and databases.
	
3. Create HTML and PHP pages for barebones OPAC.
	1. In the document root `/var/www/html` create the two files.
		1. HTML file
			1. This is the file that the broswer will display.  It contains an HTML form that is a search box where users will enter their search information.
			2. File name is `mylibrary.html`.  We entered the HTML code given in our [textbook](https://cseanburns.github.io/systems-librarianship/5b-basic-opac.html).
			3. The form requires the Start Date and End Date fields to be filled.  This can be changed by removing "required" from the line of code ` <input type="date" name="start_date" id="start_date" required>` and `<input type="date" name="end_date" id="end_date" required>`.
		2. PHP file
			1. This is the file that performs the search in the database and returns to echo the findings in the browser.
			2. File name is `search.php`.  We entered the HTML/PHP code given in our [textbook](https://cseanburns.github.io/systems-librarianship/5b-basic-opac.html).
			3. This code has some embedded CSS to help display the results.  It includes the MySQL commands to peform a keyword search accross all of the columns with the copyright date as a constraint (which can be removed).  Results are displayed in a table.  It will also echo is there are no results found.
		
4. Create barebones cataloging module
	1. Created a new directory in the document room for cataloging: `/var/www/html/cataloging`.  Here is where we created and HTML and PHP files for a simple web interface that is user/password protected.
	2. Create HTML file
		1. In new directory `sudo tilde index.html` for the cataloging module page.
		2. We entered HTML form code from our [textbook](https://cseanburns.github.io/systems-librarianship/5c-basic-opac-admin.html).
	3. Creat PHP file
		1. In the new directory `sudo tilde insert.php` for the script to communicate with the database and insert the information entered on the form.
		2. We entered the HTML/PHP code given in our [textbook](https://cseanburns.github.io/systems-librarianship/5c-basic-opac-admin.html).
		3. This code connects to the database, performs MySQL INSERT command of the data, and disconnects from the database.  There is also a link after an insert to either go catalog another item or go to the OPAC page.
	4. Secure the cataloging module with a user name and password.
		1. Created a user on Apache server with `sudo htpasswd -c /etc/apache2/.htpasswd libdragon`, where "libdragon" is the user name I chose.
		2. Created a stanza in the `/etc/apache2/apache2.conf` file to prompt for authentication when accessing anything in the cataloging module. The small paragraph and details of that location were found in our textbook.
		3. Created file `.htaccess` in `/var/www/html/cataloging` to set the authentication connect with what was set in `apache2.conf` and `.htpasswd`.  The data for that was provided in our textbook.
	5. Check Apache configurations and restart Apache.
		1. `sudo apachectl configtest` . Syntax OK message confirms.
		2. `sudo systemctl restart apache2`
		3. `sudo systemctl status apache2` to confirm it is running.
		
5. Give Apache user account permissions and ownership to write data where needed
	1. Changed ownership of `/var/www/html` to Apache user `www-data` using `sudo chown :www-data /var/www/html`.
	2. Make sure new files and directories also have this using `sudo find /var/www/html -type d -exec chmod g+s {} +`

6. Test cataloging module
	1. Navigate to [cataloging module](http://34.68.187.184/cataloging/index.html).  When prompted, enter username and password.
	2. Enter data in the fields and submit.  New record successfully submitted.
	3. Navigate to [OPAC](http://34.68.187.184/mylibrary.html) and perform search for added book.
	4. In VM, also checked within the MySQL database for added materials.
		
**Results:**
I successfully completed all of the steps for week 10 and got the desired outcome.  There were significant issues in establishing the username and password, and not creating a server error.  It took three full tries of creating all of the documents and profiles before it worked.  There are two changes that could have made the attempt a success.  First, I did more indenting and clean up of the codes for the cataloging module.  There may have been an unintentional space or invisible character causing issues.  Second, the password I set previously had an "_" as part of it.  On the third attempt I removed this.

**Verification:**
Everything was tested at each point and I didn't move on until it proved successful.

**Notes:**
The ongoing overlap between this course and LIS 668 has been extremely helpful in understanding how to use MySQL commands.

**Current System:**
VM running : Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1008-gcp x86_64)
Last updated: 2026-03-24
Text editor: Tilde
