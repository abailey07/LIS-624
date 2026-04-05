## 2026-04-04 - Week 11 - Installing Wordpress

**Goal:** To download and install Wordpress to our VM. Create a database and user in MySQL and set configuration for Wordpress to access and use it.

**Context:** This is our first step using open source software to create an ILS system.

**Steps:**

1. Update/upgrade Linux system and clean up unneeded packages.
    1. `sudo apt update`
    2. `sudo apt -y upgrade`
    3. `sudo apt autoremove`
    4. `sudo apt clean`
2. Find and install Wordpress in VM.
	1. Check system requirements for PHP and MySQL:
		1. `php --version`
		2. `mysql --version`
		3. Both meet the system requirements.
	2. Install additional PHP packages needed per our textbook:
		1. `sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl`
	3. Restart Apache and MySQL.
		1. `sudo systemctl restart apache2`
		2. `sudo systemctl restart mysql`
		3. Confirm both are running with `sudo systemclt status apache2` and `sudo systemctl status mysql`.
	4. Download and extract Wordpress
		1. Navigate to `/var/www/html`
		2. Download Wordpress: `sudo wget https://wordpress.org/latest.zip`
		3. Unzip package: `sudo unzip latest.zip`
			1. If `unzip` is not installed, install it with `sudo apt install unzip`.
		4. Wordpress has been unzipped at `/var/www/html/wordpress`			
3. Create a database and user for Wordpress in MySQL.
	1. Log into MySQL as the root user: `sudo mysql -u root`
	2. Create Wordpress user: `create user 'wordpress'@'localhost' identified by 'XXXXXXXXX';` where the Xs are the password.
	3. Create the database for Wordpress: `create database wordpress;`
	4. Set privilages in MySQL for this user: `grant all privileges on wordpress.* to 'wordpress'@'localhost';`
4. Set Wordpress configuration with MySQL and the new database and user.
	1. Navitage to Wordpress directory: `cd /var/www/html/wordpress`
	2. Copy and rename sample config file: `sudo cp wp-config-sample.php wp-config.php`
	3. Edit file: `sudo tilde wp-config.php`
	4. Under database settings, change the following settings established above:
		1. `( 'DB_NAME', 'database_name_here' )` to `( 'DB_NAME', 'wordpress' )`
		2. `( 'DB_USER', 'username_here' )` to `( 'DB_USER', 'wordpress' )`
		3. `( 'DB_PASSWORD', 'password_here' )` to `( 'DB_PASSWORD', 'XXXXXXXXXX' )` (whatever password was set
	5. At the bottom of the file, add the following to give Wordpress write file permissions: `define('FS_METHOD','direct');`.  This may change for other installs depending on ownership and security.
5. I made the choice to change the name of the Wordpress directory so that my URL would be different.  I chose to name it "Valley Archive" using the following: `sudo mv /var/www/html/wordpress /var/www/html/valley-archive`.
6. Perform the installation in a browser window.
	1. Navigate to http://34.45.197.17/valley-archive
	2. Follow the installation instructions on the screen.
		1. Library name: Valley Archive of the Crimson Soul
		2. Create username and password.
		3. We set our site to be discouraged from being crawled.
		4. Login was successful!

	
**Results:**
I successfully met the goals of this week's learning.  My VM instance had to be suspended and restarted twice so that the instance could load.  There was also significant lag on 4/4, which I haven't experienced before from the VM.  I did get a new external IP as a result of the stopping and restarting.  I did run into the issue of `unzip` needing to be installed before that was discovered in the video.  A simple Google search helped me accomplish that.  There was something wrong with the permissions that would allow me to install new templates in Wordpress.  This is an ongoing issue that is still being worked out in Teams.  There are also errors in uploading pictures and other media.

**Verification:**
I was able to log into Wordpress and do a few small changes to the site.  There seems to be a lot to learn about using a Wordpress site! 

**Notes:**
There will be shenanigans going forwards.  I will need to spend time learning how to use Wordpress and format the site to look more library like.  There are already things that I am thrilled with in terms of Wordpress's interface.  It took a while to figure out how to make a page the landing page and not the blog.  I don't recall that being a setting that we could pick at the beginning.  I would like to add some images as well.  Interesting fact: the linked library in our textbook is my first library.  I lived in Reading, MA until I was almost 7 years old.  

**Current System:**
VM running : Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1010-gcp x86_64)
Last updated: 2026-04-04
Text editor: Tilde