## 2026-03-12 - Week 9 - Installing and Configurig MySQL on server

**Goal:** To install MySQL on the Apache server.  Configure the settings.  Create a database and populate it.  Test the database.  Connect PHP and MySQL.  Create login file and OPAC.php.

**Context:** This is our last step in creating our LAMP server.

**Steps:**

1. Update and upgrade Linux.  There were ten updates.  Performed clean up after.

2. Find and install `mysql-server` in VM.
	1. Installed using `sudo apt install mysql-server` and confirmed verion using `mysql --version` after installation (8.0.45).  `systemctl status mysql` showed that MySQL was up and running correctly.
	2. Ran `sudo mysql_secure_installation` and set up some security check.  Note for future configurations to investigate more about annonymous users for church setup.  This may need to be turned off.
	3. Performed clean up using `sudo apt autoremove` and `sudo apt clean`.
	
3. Open MySQL, create, and set up a regular user account.
	1. To create a user besides the root user, I logged into the database as the root user with the following: `sudo mysql -u root`.  The `-u root` allows us to log in as that root user (the admin user).
	2. Used the following to create the new user in MySQL: `create user 'opacuser'@'localhost' identified by 'xxxxxxxxxxx';`  The password is written down.

4. Create practice database
	1. Created new database called opacdb.  We also set up the character sets that are being used and if the searches are accent and case sensitive (they are not).  This took one long command: `create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;`.
		1. Code notes: `collate` referes to the rules used to compare and sort strings of text. `character set utf8mb4` is the character set that supports the full range of Unicode.  `0900` is Unicode 9.0. `ai` is accent insensititve.  `ci` is case insensitive.
	2. Gave user `opacuser` all permissions (privileges) to the database opacdb.  When setting up for church, this will need to be more narrow to keep database integrity.
	3. Modified the MySQL command line by editing `.bashrc`.  I added to the bottom of the file `export MYSQL_PS1="[\d]> "` and then applied it using `source ~/.bashrc`.
	
5. Creating tables in MySQL
	1. Log into opacuser.  Password problem.  Performed password reset using `alter user 'opacuser'@'localhost' identified by 'xxxxxxxxxxx';`.  That was rewarding.
	2. Created table `books` as shown in textbook.  It didn't like me copying and pasting the data, so I had to type it by hand.
	3. Added data to the table using SQL commands.  This made a lot of sense having done this in LIS 668. 
	4. I did the practice testing of the table using the commands `SELECT`, `ALTER`, `UPDATE`, and `DELETE`.  So cool!!  This hit perfectly with where we are in LIS 668.
	
6. Complete connection between PHP and MySQL
	1. Installed PHP support for MySQL using `sudo apt install php-mysql`.  Restarted both Apache and MySQL using `sudo systemctl restart apache2` and `sudo systemctl restart mysql`.
	2. Created and modified `login.php` for the database using PHP from textbook. Created `opac.php` in `/var/www/html` on server and inserted HTML/PHP code to do a specific retrieval on the opacdb. Confirmed the synatx using `sudo php -f /var/www/login.php` and `sudo php -f /var/www/html/opac.php` respectively.
	3.  I could clearly spot the HTML, SQL commands, and PHP.  The code in both files worked and I was able to see what I needed to see.
**Results:**
I successfully completed all of the steps for week 9 and got the desired outcome.

**Verification:**
Everything worked as it should, and when it didn't I was able to correct it.

**Notes:**
This week is the most confident I have been in the LAMP setup process.  I am taking LIS 668 and working with SQL commands regularly, so I was quite confident in my setups.  One note to remember when working in the SQL command line is that everything needs to end with a `;`.  Unlike Access, it doesn't insert it for you!

**Current System:**
VM running : Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1008-gcp x86_64)
Last updated: 2026-03-12
Text editor: Tilde
