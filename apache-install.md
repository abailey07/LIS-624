## 2026-02-23 - Week 7 - Installing Apache Web Server

**Goal:** To install an Apache web server in the Ubutu VM.  To install a text based web browser.  To create a very basic web page in the document root to test that Apache is working.

**Context:** This is our first step in creating a LAMP server, a critical component of creating our ILS system.

**Steps:**

1. Find and install Apache in VM.
	1. Update/upgrad Ubuntu if needed (I did).
	2. Used `apt search apache2 | head` to find the correct Apache package titled `apache2`.  Double check package is correct using `apt show apache2`. Used `sudo apt install apache2` to install the package.
	3. Confirmed that Apache was running using `systemctl status apache2`, which stated that was both enabled and running.
	
2. Connect with Apache using a text based web browser. I installed the browser `elinks` and confirmed that my server was running.  It did not want to load using `localhost` but loaded with the IP address `127.0.0.1`.  I also tested the server using the External IP address for my VM in Chrome, which was also successful.

3. Created a web page in the document root.
	1. The document root for the server is `/var/www/html`.  We created a file titled `index.html`.
	2. I copied the HTML given in our textbook, and changed it a little.
	
**Results:**
I successfully met the goals of this week's learning and did not run into any issues along the way.  I'm looking forward to more work on the webpages, as I am more familiar with HTML at work.  I would like to incorporate some CSS as well.

**Verification:**
I confirmed Apache was running and enabled, was able to view the server through both text and graphical browsers, and changed `index.html` in the document root of the server. 

**Notes:**
It was noted that when we are working outside of our home directory (like when working on `index.html`) that we must use `sudo` before each command.  Previously, this has just been used with installations or updating things.  This was the first instance where we needed to use it for a text file.  It's important to remember that it doesn't matter how simple of a file you are working with.  It has to do with what directory you are working in, and when using `sudo` we must be careful what we do.