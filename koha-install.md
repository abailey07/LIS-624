## 2026-04-09 - Week 12 - Installing Koha

**Goal:** Create a new VM for Koha installation. Install and configure Koha.  Embed Koha OPAC link on WordPress site and complete the ILS.

**Context:** This is our final step using open source software to create an ILS system.

**Steps:**

1. Create new VM using the parameters defined in our textbook.  
    1.  VM parameters from our [textbook](https://cseanburns.github.io/systems-librarianship/6c-install-koha.html).
        1. Creating the network tags at this step is crucial!
        2. After VM creation, run `sudo apt update/upgrade/autoremove/clean`.
    2. Firewall set up
        1. Open large menu (hamburger icon) > VPC Network > Firewall
        2. Create firewall rule for "koha-staff-8080":
            + Name: koha-staff-8080
            + Description: Open port 8080 for the Koha staff interface
            + Add target tag: koha-staff-8080
            + Source IPv4 range : 0.0.0.0/0
            + Select TCP protocol
            + Add 8080 to ports box
        2. Create firewall rule for "koha-opac-8081"
             + Name: koha-opac-8081
            + Description: Open port 8081 for the Koha OPAC interface
            + Add target tag: koha-opac-8081
            + Source IPv4 range : 0.0.0.0/0
            + Select TCP protocol
            + Add 8081 to ports box
2. Check system requirements and add Koha repository.
    1. Because this is a long installation, start with `tmux` before doing anything in case the connection to the VM drops.
    2. Add the Koha repository
        1. `sudo apt install apt-transport-https ca-certificates curl`
        2. `sudo mkdir -p --mode=0755 /etc/apt/keyrings`
        3. `sudo curl -fsSL https://debian.koha-community.org/koha/gpg.asc -o /etc/apt/keyrings/koha.asc`
        4. Switch to root user `sudo su` for the following command:
        ```
        tee /etc/apt/sources.list.d/koha.sources <<EOF
        Types: deb
        URIs: https://debian.koha-community.org/koha/
        Suites: 25.05
        Components: main
        Signed-By: /etc/apt/keyrings/koha.asc
        EOF
        ```
        5. Confirm this was entered correctly by checking and comparing with `cat /etc/apt/sources.list.d/koha.sources`
        6. `exit` to stop working as the root user.
3. Install MariaDB
    1. Check for additional system updates.
    2. `sudo install mariadb-server`
4. Install Koha on VM.
    1. Check for addtional system updates.
    2. (Optional) Read through the Koha package information: `apt show koha-common`
    3. Install Koha: `sudo apt install koha-common`
5. Open ports, setup and configure Apache, and create Koha instance on VM
    1. To open the ports, we edit the file `/etc/koha/koha-sites.conf`.  Create a copy of the default in case we need to start again: `sudo cp /etc/koha/koha-sites.conf /etc/koha/koha-sites.conf.backup`
    2. Add the following information:
        ```
        DOMAIN="34.45.82.164"
        INTRAPORT="8080"
        OPACPORT="8081"
        ```
        Nothing else is modified in this file.
    3. Set up and configure Apache
        1. Enable modules with `sudo a2enmod rewrite cgi headers proxy_http`.  Restart Apache.
        2. Modify Apache ports to listen for 8080 and 8081.  
            1. Create a backup of the configuration file: `sudo cp /etc/apache2/ports.conf /etc/apache2/ports.conf.backup`
            2. Open file in tilde: `sudo tilde /etc/apache2/ports.conf`
            3. Under "Listen 80", add `Listen 8080` and `Listen 8081`.
    4. Create the Koha instance and name it.
        1. `sudo koha-create --create-db valleyarchive`
            + The name of the system comes after `--create-db`.
        2. Restart Apache.
    5. Set additional Apache configurations:
        + sudo a2dissite 000-default
        + sudo a2enmod deflate
        + sudo a2ensite valleyarchive
6. Complete Koha staff side and public side installations in web browser.
    1. Set a username and password in the VM:
        1. `sudo koha-passwd valleyarchive`
        2. Establish username: Head_Librarian
        3. Establish password: XXXXXXXXXXXX
        4. Do press enter to clear the screen.
    2. Open web browser and navigate to `http://34.45.82.164:8080`.  Enter the newly established username and password to start the guided web installation.  For additional information, you can read the [Koha Web Installer Instructions](https://koha-community.org/manual/latest/en/html/installation.html#web-installer)
    3. After completing the installation, set the IP address for the OPAC on the staff staff side.
        1. Navigation bar "More" > Administration > System Preferences.
        2. Find the section on OPAC settings.  In the option "OPACBaseURL", add the IP address of the server: `http://34.45.82.164`
	
**Results:**
I successfully met the goals of this week's learning.  The work done last week in creating a new VM from a snapshot came in very handy after misunderstanding the instructions.  I took the time to catalog one item so that the OPAC can be searched.  I included a hint to search the term "library" to pull the item. 

**Verification:**
All address have been integrated into the WordPress site.  People other than myself have been able to access it and find the single book I cataloged. 

**Notes:**
After the struggles of last week, this final installation seemed like smooth sailing.  I did misunderstand the dirctions and didn't realized that we need 2 VM total to complete the process.  This error caused me to delete the MySQL database information holding WordPress and Omeka up.  I was lucky that I had a very recent snapshot to save everything!

**Current System:**
VM running : Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1012-gcp x86_64)
Last updated: 2026-04-20
Text editor: Tilde