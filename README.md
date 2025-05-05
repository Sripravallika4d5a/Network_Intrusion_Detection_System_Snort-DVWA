# Network_Intrusion_Detection_System_Snort-DVWA
Network Intrusion Detection System using Snort and DVWA and simulating the real world attack in the kali virtual machine
Step-by-Step Guide Based on Provided Commands
Below is a corrected and clarified guide based on the provided document. I have fixed typos (e.g., Update.rc-d → update-rc.d, A2endmod → a2enmod), removed redundant steps (e.g., starting Apache multiple times), and added explanations for clarity. Commands assume a Debian/Ubuntu-based system with sudo privileges.
**Step 1: Install and Configure the LAMP Stack

 # 1.Update the System
      sudo apt update && sudo apt upgrade -y
# 2.Install Apache:
        sudo apt install apache2  -y
# 3.Start and Enable Apache:
      sudo systemctl start apache2
      sudo update-rc.d apache2 defaults.
# 4. Verify Apache: 
      Open a browser and navigate http://localhost. 
      You should see the default Apache page.
# 5.Install PHP and Apache PHP Module:
    sudo apt install libapache2-mod-php8.4 -y
# 6.Enable PHP Module: 
    sudo a2enmod php8.4
# 7.Create a PHP Test File: 
    sudo nano /var/www/html/index.php
    •	Add the following content: 
      <?php
    php phpinfo ();
    ?> 
    •	Save and exit (Ctrl+O, Enter, Ctrl+X).
# 8. Restart Apache:
      sudo systemctl restart apache2

# 9. Verify PHP: 
      Open a browser and navigate to http://localhost/index.php.
       You should see the PHP info page.

# 10.Install MariaDB: 
      sudo apt install mariadb-server mariadb-client-compat -y

# 11. SecureMariaDB Installation: 
        sudo mysql_secure_installation
# 12.Follow prompts: 
•	Set a root password.
•	Remove anonymous users (recommended: Yes).
•	Disallow root login remotely (recommended: Yes).
•	Remove test database (recommended: Yes).
•	Reload privilege tables (recommended: Yes)
# 13. Install and configure phpMyAdmin:
        sudo apt install phpMyAdmin -y
         · During installation, select Apache when prompted to configure the web server.
· Set up the root password and an application username/password for phpMyadmin


# 14. Enable phpMyAdmin configuration:
      Sudo ln -s /etc/phpMyAdmin/apache.conf /etc/apache2/conf-available/phpMyAdmin.conf sudo a2enconf .phpMyAdmin

# 15.Restart Services 
# 16. Access phpMyAdmin:
      •	Open a browser and navigate to http://localhost/phpMyAdmin.
      •	Log in with the MariaDB root username and password.
# 17.Install and Configure DVWA
# •	Clone DVWA Repository: 
    cd /var/www/html 
    sudo git clone https://github.com/digininja/DVWA.git
  # 18..Set Permissions
    sudo chmod -R 777 DVWA

# 19. Configure DVWA
      cd DVWA/config
      cp config.inc.php.dist config.inc.php
      sudo nano config.inc.php
# EDIT THE FOLLOWING:

      $_DVWA[‘db_user’] = ‘admin’;
      $_DVWA[‘db_password’] = ‘password’;
      $_DVWA[‘db_database’] = ‘dvwa’;
      •	Save and exit. 
# 20. Set Up MariaDB Database for DVWA: 
     mysql -u root -p
    Enter the root password, then run:
    CREATE DATABASE dvwa;
    CREATE USER ‘admin’@’127.0.0.1’ IDENTIFIED BY ‘password’;
    GRANT ALL PRIVILEGES ON dvwa.* TO ‘admin’@’127.0.0.1’;
  # 21. Modify PHP Settings: 
        cd /etc/php/8.4/apache2 sudo nano php.ini
      Find and set: 
      allow_url_fopen = On
       allow_url_include = On
       Save and exit.
· Note: Enabling allow_url_include is insecure and should only be done for testing environments like DVWA

# 22. Restart Apache: 
    sudo systemctl restart apache2

# 23. Access DVWA: 
    Open a browser and navigate to http://localhost/DVWA.
    Log in with admin/password.
    Follow the setup page to initialize the database
# 24. Install and Configure Snort
      sudo apt install snort -y
      snort -V
      ip a
      Note the interface name (e.g., eth0 for external communication, lo for localhost).
# 25. Create Snort Rules: 
      sudo nano /etc/snort/rules/local.rules
      XSS - alert tcp any any -> any 80 (msg:"Possible XSS"; content:"<script"; nocase; sid:1000001;)
      SQL INJECTION RULE - alert tcp any any -> any 80 (msg:"Possible SQL Injection"; content:"union select"; nocase;             sid:1000002;)
      Save and exit.
  # 26. Configure Snort:
      sudo nano /etc/snort/snort.lua
Ensure the local.rules file is included in the configuration. Add or verify:
ips = {
   rules = '/etc/snort/rules/local.rules',
}

# 27. Run snort:
      sudo snort -c /etc/snort/snort.lua -i eth0 -R /etc/snort/rules/local.rules -A alert_fast

Monitors traffic on eth0 and logs alerts to the terminal.

# 28. Test snort:
      Ping google.com
Check the Snort terminal for captured packets or alerts.

# 	Apache: http://localhost
# 	PHP Info Page: http://localhost/index.php
# 	phpMyAdmin: http://localhost/phpMyAdmin (login with MariaDB root credentials)
# DVWA: http://localhost/DVWA (login with admin/password)
# Snort: No web interface; view alerts in the terminal or configured log files.
 

















 


          


      
