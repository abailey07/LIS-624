# Module 4: LAMP Documentation and Reflection


## LAMP Server and Setup

For the University of Kentucky's System Librarianship class (LIS 624), we are building an open source integrated library system (ILS) over the course of this semester.  The following is a summary and documentation of module 4 where we installed and configured a LAMP stack using Google Cloud virtual machines.

A LAMP stack (Linux, Apache, MySQL, and PHP) is an open-source bundle of software components that work together to allow the building and sharing of web sites and web applications.  Each of the software components is free to use and is not difficult to use.  For our class, this is an ideal setup to have a hands-on experience in building an ILS from the ground up.


## Installation

Below are the installation steps for the Apache, PHP, and MySQL components.  More details on each step with comments are linked in each step.

### Installation and configuration of Apache : [Apache Installation Notes](https://github.com/abailey07/LIS-624/blob/main/apache-install.md)

1. Update/upgrade Linux system and clean up unneeded packages.
    1. `sudo apt update`
    2. `sudo apt -y upgrade`
    3. `sudo apt autoremove`
    4. `sudo apt clean`
2. Find and install the Apache package called "Apache 2".
    1.`apt search apache 2 | head` to find it at the top of the list.
    2.`apt show apache2` to confirm package is correct.
    3. `sudo apt install apache2` to install.
    4. Confirm Apache is running and enabled : `systemctl status apache2`
3. Confirm server is running
    1. Using a browser, navigate to your server's IP address and see if it's running.
    2. In this project, I installed a text browser on my virtual Linux machine and tested this.  I was also able to confirm using the VM's external IP address in a Chrome browser.
        1. Installed elinks
            1. `apt search elinks`
            2. `sudo apt install elinks`
        2. Navigate to Local Host in elinks
            1. Open and navigate to local host : `elinks localhost` or `elinks 127.0.0.1`
            2. Visually confirmed Apache was running
        3. Confirmation in Chrome
            1. Navigated to VM external IP : (http://34.68.187.184)  
            2. Same confirmation that Apache was running.  **Note that more installations and configurations have occured since this point and this is no longer the default page to be viewed.**
4. Create a simple web page
    1. Document root of the server is `/var/www/html`.  This was given in the default `index.html` file that we saw when navigated to the local host previously.  Using Tilde, I replaced the default file `index.html` saved as `index.original.html` and filled it with the given text from the course textbook.  Save and close.
    2. Confirmed Apache was running with new `index.html` by navigating to server's IP address again.


### Installation and configuration of PHP [PHP Installation Notes](https://github.com/abailey07/LIS-624/blob/main/php-install.md)

1. Update/upgrade Linux system and clean up unneeded packages.
    1. `sudo apt update`
    2. `sudo apt -y upgrade`
    3. `sudo apt autoremove`
    4. `sudo apt clean`
2. Find and install PHP and libapache2-mod-php to connect PHP and Apache.  This can be done in one command or two.  For class, we did it in one command, but here I will break it into two.
    1. PHP installation
        1. `apt search php` to find the PHP software
        2. `apt show php` to confirm the correct package
        3. `sudo apt install php`
        4. Perform clean up steps `sudo apt autoremove` and `sudo apt clean`
        5. Confirm version of PHP : `php -v`
    2. libapache2-mod-php installation
        1. `apt search libapache2-mod-php`
        2. `apt show libapache2-mod-php` to confirm correct package
        3. `sudo apt install libapache2-mod-php`        
        4. Perform clean up steps `sudo apt autoremove` and `sudo apt clean`
3.	Confirm PHP is working on the server
    1. Create file info.php in the document root `/var/www/html` with the following PHP code:
       ```
       <?php
        phpinfo();
        ?>
        ```
    2. Save and close.
    3. Using a browser, navigate to the server's public IP and the new PHP file to confirm PHP is working: http://34.68.187.184/info.php
    4. Remove `info.php` as it is a security risk for it to be public : `sudo rm /var/www/html/info.php`
4. Configure the server to load PHP first followed by HTML.  The order of these two are critical to make sure that future web pages load correctly.
    1. Find dir.conf file that controls the file load order for the server.  For us, this is located in /etc/apache2/mods-available.
    2. Make a copy of the original: `sudo cp dir.conf dir.conf.bak` in case a backup is needed.
    3. Edit the `dir.conf` in Tilde using `sudo tilde dir.conf`.  Change the file load order so that `index.php` comes before `index.html`.  Save and close.
    4. Check the configuration change: `apachectl configtest`.  If it is okay, it will say Syntax Ok.
    5. Reload and check Apache status
        1. `sudo systemctl reload apache2`
        2. `systemctl status apache2`
5. Create `index.php` file.  For this part, we were given a bit of code for a browser and OS detector that included HTML and PHP.
    1. In the document root `/var/www/html`, create the file `index.php`
        1. `sudo tilde index.php`
    2. Insert the following HTML and PHP code for the browser and OS detector:
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Browser Detector</title>
    </head>
    <body>
    <h1>Browser & OS Detection</h1>
    <p>You are using the following browser to view this site:</p>
    <?php
    $user_agent = $_SERVER['HTTP_USER_AGENT'];
    // Browser Detection
    if (stripos($user_agent, 'Edg') !== false || stripos($user_agent, 'Edge') !== false) {
        $browser = 'Microsoft Edge';
    } elseif (stripos($user_agent, 'Firefox') !== false) {
        $browser = 'Mozilla Firefox';
    } elseif (stripos($user_agent, 'Chrome') !== false && stripos($user_agent, 'Chromium') === false) {
        $browser = 'Google Chrome';
    } elseif (stripos($user_agent, 'Chromium') !== false) {
        $browser = 'Chromium';
    } elseif (stripos($user_agent, 'Opera Mini') !== false) {
        $browser = 'Opera Mini';
    } elseif (stripos($user_agent, 'Opera') !== false || stripos($user_agent, 'OPR') !== false) {
        $browser = 'Opera';
    } elseif (stripos($user_agent, 'Safari') !== false && stripos($user_agent, 'Chrome') === false) {
        $browser = 'Safari';
    } else {
        $browser = 'Unknown Browser';
    }
    // OS Detection
    if (stripos($user_agent, 'Windows') !== false) {
        $os = 'Windows';
    } elseif (stripos($user_agent, 'iOS') !== false || stripos($user_agent, 'iPhone') !== false || stripos($user_agent, 'iPad') !== false) {
        $os = 'iOS';
    } elseif (stripos($user_agent, 'Android') !== false) {
        $os = 'Android';
    } elseif (stripos($user_agent, 'Mac') !== false || stripos($user_agent, 'Macintosh') !== false) {
        $os = 'Mac';
    } elseif (stripos($user_agent, 'Linux') !== false) {
        $os = 'Linux';
    } else {
        $os = 'Unknown OS';
    }
    // Output Result
    echo "<p>Your browser is <strong>$browser</strong> and your operating system is <strong>$os</strong>.</p>";
    ?>
    </body>
    </html>
    ```

6. Navigate to VM [external IP](http://34.68.187.184) to confirm that the new file `index.php` is working.



### Installation and configuration of MySQL : [MySQL Installation Notes](https://github.com/abailey07/LIS-624/blob/main/mysql-install.md)

1. 1. Update/upgrade Linux system and clean up unneeded packages.
    1. `sudo apt update`
    2. `sudo apt -y upgrade`
    3. `sudo apt autoremove`
    4. `sudo apt clean`
2. Find and install `mysql-server`
    1. `apt search mysql-server`
    2. `apt show mysql-server` to confirm package is correct.
    3. `sudo apt install mysql-server`
    4. Confirm version with `mysql --version`
3. Confirm that MySQL is running with `systemctl status mysql`
4. Perform security checks and establish secure configuration.
    1. Run `sudo mysql_secure_installation`
    2. Use the following options for a low level security policy.
        1. Validate passwords - Yes
        2. Password validation policy - 0, which is the LOW setting
        3. Remove anonymous users - Yes
        4. Disallow root loging remotely - Yes
        5. Remove test database and access to it - Yes
        6. Reload privilege tables now - Yes
    2. Log in to MySQL root user to create regular user account
        1. Log in with `sudo mysql -u root`. Here the command line changes to `mysql>  `.  This is how you know you are working in MySQL.
        2. Enter the command `create user 'opacuser'@'localhost' identified by 'XXXXXXXX';`
            1. `opacuser` is the user name.  It is on the localhost server.
            2. `XXXXXXX` is a fill in for the password for the account.
            3. If an error occurs and password must be reset, you can use the SQL query `alter user 'opacuser'@'localhost' identified by 'ZZZZZZZZ';` where 'ZZZZZZZZ' is the new password.
5. Create a practice database and configure opacuser so that this profile can manipulate the database.
    1. In MySQL, the command `creat database opacdb defaul character set utf8mb4 collate utf8mb4_0900_ai_ci;`.  This establishes:
        1. The name of the database is "opacdb".
        2. The character encoding is UTF-8.
        3. `collate` establishes rules for comparing and sorting strings of text.
        4. `0900` refers to Unicode 9.0.
        5. `ai` allows for accent insensitivity in searches and sorting.
        6. `ci` allows for case insensitivity in searches and sorting.
    2. Grant use privilages to opacuser with the command `grant all privileges on opacdb.* to 'opacuser'@'localhost';`.
    3. For visibility in MySQL, open the bash file using `tilde ~/.bashrc` and add at the bottom of the file `export MYSQL_PS1="[\d]> "`.  Save and exit.  Then source the file using `source ~/.bashrc` so that the change takes effect.  Log back into MySQL using `mysql -u opacuser -p`.  Enter the password and hit "Enter".


### Verification of Work and Challenges Overcome

Embedded throughout this documentation are the commands to verify and check along the way that each step is successful, as well as the commands to configuring different settings.  In my own installation process, I encountered two challenges.  First, when checking on Apache using `elinks localhost`, I was shown an error that it was `Unable to retrieve https://localhost/`.  This many have to do with we are only using http for our VM's.  

My second challenge was with setting the password for opacuser.  When I first typed in the password, I typed slowly and could see blank characters marking each characters.  When I went to log in for the first time, where characters do not appear, I again types slowly what I had written down but it didn't accept the password.  I tried several times and some slightly different iterations of my password, but nothing worked.  After some research, I found I could use the ALTER command.  I was able to reset the password, which now works.  I included this helpful hint as part of my documentation.