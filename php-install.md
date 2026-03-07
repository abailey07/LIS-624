## 2026-03-06 - Week 8 - Installing and Configuring PHP on server

**Goal:** To install PHP and other modules on the Apache server.  Configure these to work with our server and the upcoming MySQL installation.

**Context:** This is our second step in creating our LAMP server.

**Steps:**

1. Update and upgrade Linux.  There were two updates.  Performed clean up after.

2. Find and install `php` and `libapache2-mod-php` in VM.
	1. Used `apt show php` and `apt show libapache2-mod-php` to confirm the packages matched what was demonstrated in the lecture video.  They were the same, so both were installed using `sudo apt install php libapache2-mod-php`.
	2. Performed clean up using `sudo apt clean`.
	3. Confirmed PHP version installed with `php -v`.  It matched what I saw previously.  It is version 8.3.6.
	
3. Check the PHP installation and that it's working on the server.  For this, I created `info.php` with the given code in `/var/www/html` (which is where the document root is).  It displayed the correct information.  I then removed the file using `suod rm /var/www/html/info.php` so that it wouldn't be a vulnerability.

4. Complete configurations.
	1. We want the server to look for `index.php` first, but it currently looks for `index.html` first.  We edited the `dir.conf` file that controls this.  This file is found in `/etc/apache2/mods-available/`.  First, we made a copy of this file in `dir.conf.bak`.
	2. Using `sudo tilde dir.conf`, I edited the file so that `index.php` now loads before `index.html` by moving it to first in the line of text.  I saved and quit tilde.
	3. I ran `apachectl configtest` and confirmed the syntax was okay and worked (it did).
	4. I reloaded Apache and confirmed everything was running and enabled. 
	
5. Create `index.php` file and text in browsers.
	1. In `/var/www/html`, I created the file `index.php` and inserted the code from our text.  I saved and exited.
	2. I loaded my VM's IP address and successfully got the new page, which is a browser and OS detector.
	3. Just for fun, I tested this on my text browser ELinks in the VM.  It worked!  Unknown browser and Linux OS.
	
**Results:**
I successfully met the goals of this week.  I still don't fully understand PHP, but I know that it is critical for the dynamic function of databases, which is exactly what an ILS is.

**Verification:**
I confirmed PHP and extra mods were installed, and that Apache was restarted and running as it should.  I changed the server load from starting with `index.html` to `index.php` and inserted the code for this PHP file.  The change was confirmed in both text and graphical browsers.

**Notes:**
I didn't entirely understand what PHP does and why we would need to use it in this instance.  I read a helpful article on [CodeAcadamy](https://www.codecademy.com/resources/blog/what-is-php-used-for) that helped me wrap my head around it.

**Current System:**
VM running : Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1008-gcp x86_64)
Last updated: 2026-03-06
Text editor: Tilde
