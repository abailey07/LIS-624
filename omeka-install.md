## 2026-04-09 - Week 12 - Installing Omeka

**Goal:** To download and install Omeka to our VM. Create a database and user in MySQL and set configuration for Omeka to access and use it.

**Context:** This is our second step using open source software to create an ILS system.

**Steps:**

1. Update/upgrade Linux system and clean up unneeded packages.
    1. `sudo apt update`
    2. `sudo apt -y upgrade`
    3. `sudo apt autoremove`
    4. `sudo apt clean`
2. Check system requirements and install additional programs and plugins.
	1. Check system requirements for PHP and MySQL:
		1. `php --version`
		2. `mysql --version`
		3. Both meet the system requirements.
	2. Check for PHP extensions that are needed per Omeka (`mysqli` and `exif`):
		1. `php -m`
        2. `mysqli` and `exif` are present.
	3. Install ImageMagick for Omeka to manipulate photos.
		1. `sudo apt install imagemagick`
	4. Enable Apache `mod_rewrite` so that Omeka can make friendly URLs, and restart Apache.
		1. `sudo a2enmod rewrite`
		2. `sudo systemctl restart apache2`
		3. Unzip package: `sudo unzip latest.zip`
3. Download and install Omeka and set permissions
    1. Create a new database in MySQL for the Omeka installation.
    	1. Log into MySQL as the root user: `sudo mysql -u root`
	    2. Create Omeka user: `create user 'omeka'@'localhost' identified by 'XXXXXXXXX';` where the Xs are the password.
	    3. Create the database for Omeka: `create database omeka;`
	    4. Set privilages in MySQL for this user: `grant all privileges on omeka.* to 'omeka'@'localhost';`
    2. Download and install Omeka.
	    1. Navigate to `/var/www/html`
        2. Download Omeka: `sudo wget https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip`
        3. Unzip: `sudo unzip omeka-3.2.zip`
        4. Rename Omeka directory: `sudo mv omeka-3.2 omeka-VA`
    3. Add database credentials in `db.ini`
        1. Open file: `sudo tilde db.ini`
        2. Fill in the XXXXs with the appropriate information:
            1. host = `localhost`
            2. username = `omeka`
            3. password = `XXXXXXX`
            4. dbname = `omeka`
            5. All other fields are unaltered.
    4. Set Apache access permissions
            1. Be in `/var/www/html/omeka-VA`
            2. Set permissions: `sudo chmod -R g+w *`
            3. Navigate to `/etc/apache2`
            4. Open `apache2.conf`: `sudo tilde apache2.conf`
            5. Change permissions under `<Directory /var/www/>` to `AllowOverride All`.
    5. Restart Apache : `sudo systemctl restart apache2`
4. Perform the installation in a browser window.
	1. Navigate to http://34.59.146.43/omeka-VA
	2. Follow the installation instructions on the screen.
		1. Select a username and set password.
		2. Select Sit Title and Description.
        3. Set admin email.
        4. ImageMagick directory path is `/usr/bin`, which is the default.
        5. Complete installation and start working on digital archive.
	
**Results:**
I successfully met the goals of this week's learning.  After much back and forth about the size of our VMs and issues relating to that, I created a new instance with more disk space (20GB instead of 10GB) and more memory (micro machine with 1GB memory, now small machine with 2GB memory) using a snapshot of the original.  I am working to update all of the URLs throughout this Git to reflect that change.

**Verification:**
I was able to log into Omeka and look at the settings.  I also created a link to Omeka from my WordPress site. 

**Notes:**
Candace from class did significant work to help figure out some of the permission error that we all seemed to have.  Her investigating and work also led to the fixes for the issues with WordPress from last week.  I will be updating that Git post to reflect those additional permissions.  At this point, the installation process is becoming highly collaborative through our discussion board and Teams.  

**Current System:**
VM running : Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1010-gcp x86_64)
Last updated: 2026-04-08
Text editor: Tilde