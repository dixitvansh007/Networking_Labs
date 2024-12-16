
# Networking Labs

### Networking Lab - 10

#### Duration: 3 - 4 hourse

#### Lab: Setting Up and Configuring a Proxy Server in a Network Environment
A proxy server acts as an intermediary between a client and the internet. It forwards requests from the client to the appropriate server and returns the server's response back to the client. Proxy servers are used for various purposes, including improving network performance, controlling internet access, and increasing security.
In this lab, we will set up a basic Squid Proxy Server on a Linux system and configure it for different use cases, including web filtering, caching, and controlling access.

#### Lab Overview:
•	Set up a Squid proxy server on a Linux-based system.

•	Configure the proxy server for web caching and access control.

•	Test the proxy server functionality with different clients (i.e., browsers or command-line tools).

•	Optionally, configure authentication for secure access.

#### Lab Environment:

•	OS: Linux (Ubuntu or CentOS)

•	Proxy Server: Squid

•	Client: A web browser or command-line tools like curl and wget
________________________________________
#### Lab 1: Setting Up a Squid Proxy Server

#### Objective:

•	Install and configure Squid proxy server.

•	Configure basic settings, such as port number and access control.
#### Tasks:

Step 1: Install Squid Proxy Server

    1.	Install Squid on Ubuntu/Debian: Open a terminal and run the following commands to install Squid.

bash

Copy code

sudo apt update

sudo apt install squid -y
    
    2.	install Squid on CentOS/RHEL:

bash

Copy code

sudo yum install squid -y

    3.	Verify Installation: After installation, check the status of the Squid service to ensure it’s running.

bash

Copy code

sudo systemctl status squid

Step 2: Configure Squid Proxy Server

    1.	Backup the Default Configuration File: Before making any changes, back up the default configuration.

bash

Copy code

sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.bak

    2.	Edit the Squid Configuration File: Open the Squid configuration file for editing:

bash

Copy code

sudo nano /etc/squid/squid.conf
     
     o	Set the Port Number: By default, Squid listens on port 3128. You can modify the port if needed.

yaml

Copy code

http_port 3128
     
     o	Access Control Lists (ACLs): You need to configure ACLs to control who can use the proxy. For example, allow local network IPs (assuming the local network is 192.168.1.0/24):

css

Copy code

acl localnet src 192.168.1.0/24

http_access allow localnet
     
     o	Deny All Access by Default: To block access from all other networks except for the local network, add the following line:

css

Copy code

http_access deny all

    3.	Save and Exit: After making the changes, save the file and exit the editor (Ctrl + O, Enter, Ctrl + X).

Step 3: Restart Squid Proxy Server

To apply the changes, restart the Squid service:

bash

Copy code

sudo systemctl restart squid

Verify that the proxy server is running:

bash

Copy code

sudo systemctl status squid

Step 4: Test Proxy Server

    1.	Configure a Client to Use the Proxy: On a client machine (could be a different system or the same system), configure the web browser or command-line tools to use the proxy.
For example, if using Firefox:

     o	Go to Preferences > Network Settings > Manual Proxy Configuration.
     
     o	Enter the IP address of the proxy server and the port (e.g., 192.168.1.100:3128).

    2.	Access the Web via Proxy: Open a web browser and try to access a website (e.g., http://www.google.com). If configured correctly, the browser should go through the Squid proxy.

    3.	Verify Proxy Logs: Check the Squid logs for details about the requests.

bash

Copy code

tail -f /var/log/squid/access.log
________________________________________
#### Lab 2: Configuring Squid for Web Caching and Filtering

#### Objective:
     •	Configure Squid to cache web content.
     •	Set up URL filtering to block certain websites.

#### Tasks:

Step 1: Enable Web Caching

    1.	Edit Squid Configuration: Open the Squid configuration file again:

bash

Copy code

sudo nano /etc/squid/squid.conf

    2.	Enable Caching: By default, Squid caches web content. You can adjust the cache size by modifying the cache_mem and maximum_object_size settings. For example:

Copy code

cache_mem 256 MB

maximum_object_size 128 MB

    3.	Save and Exit.
    
    4.	Restart Squid: Restart Squid to apply the changes:

bash

Copy code

sudo systemctl restart squid

Step 2: Configure URL Filtering

    1.	Block Specific Websites: Add an ACL to block specific URLs or websites. For example, to block example.com:

bash

Copy code

acl blocked_sites dstdomain .example.com

http_access deny blocked_sites

    2.	Allow or Deny Access Based on URL:

     o	To block multiple sites, add multiple acl entries:

bash

Copy code

acl blocked_sites dstdomain .example.com .test.com

http_access deny blocked_sites

     o	To allow access to certain websites only, modify the ACL to allow specific domains:

bash

Copy code

acl allowed_sites dstdomain .allowedsite.com

http_access allow allowed_sites
http_access deny all

    3.	Save and Exit: Save the file and exit.

    4.	Restart Squid: Apply the changes:

bash

Copy code

sudo systemctl restart squid

Step 3: Test URL Filtering

    1.	Try to Access Blocked Websites: On a client machine, try accessing a blocked website (e.g., http://example.com). The request should be denied, and the user should see a "Forbidden" message or the browser will be unable to load the page.

    2.	Access Allowed Websites: Try accessing allowed websites, and they should load as expected.
________________________________________
#### Lab 3: Configuring Proxy Authentication

#### Objective:

•	Configure authentication for the Squid proxy to restrict access.

#### Tasks:

Step 1: Install Authentication Helpers

    1.	Install apache2-utils (Ubuntu/Debian): This package provides the htpasswd utility for creating password files.

bash

Copy code

sudo apt install apache2-utils -y

    2.	Install httpd-tools (CentOS/RHEL):

bash

Copy code

sudo yum install httpd-tools -y

Step 2: Create Password File

    1.	Create Password File: Use the htpasswd command to create a password file:

bash

Copy code

sudo htpasswd -c /etc/squid/passwords user1

     o	You will be prompted to enter a password for the user.

     o	To add more users, use the following command (without the -c flag):

bash

Copy code

sudo htpasswd /etc/squid/passwords user2

Step 3: Configure Squid to Use Authentication

    1.	Edit Squid Configuration: Open the Squid configuration file and enable authentication:

bash

Copy code

sudo nano /etc/squid/squid.conf

    2.	Add Authentication Settings: Add the following lines to enable authentication:

bash

Copy code

auth_param basic program /usr/lib/squid/
basic_ncsa_auth /etc/squid/passwords

auth_param basic realm Squid Proxy Server

acl authenticated proxy_auth REQUIRED

http_access allow authenticated

    3.	Save and Exit.

    4.	Restart Squid: Restart Squid to apply the changes:

bash

Copy code

sudo systemctl restart squid

Step 4: Test Authentication

    1.	Configure a Client: On a client machine, try accessing the proxy server through the web browser. You should be prompted to enter a username and password.

    2.	Login: Enter the credentials you created (e.g., user1 and the associated password). If the credentials are correct, access should be granted.
________________________________________
#### Lab 4: Monitoring and Troubleshooting Squid Proxy

#### Objective:

•	Learn how to monitor the Squid proxy server and troubleshoot common issues.
#### Tasks:

Step 1: Check Squid Logs

    1.	View Access Logs: View the logs to monitor client requests:

bash

Copy code

tail -f /var/log/squid/access.log

    2.	View Cache Logs: Monitor cache-related activity:

bash

Copy code

tail -f /var/log/squid/cache.log

Step 2: Troubleshooting Squid

    1.	Check Squid Service Status: If the proxy server is not working, check the service status:

bash

Copy code

sudo systemctl status squid

    2.	Check Squid Configuration: Test the Squid configuration for syntax errors:

bash

Copy code

sudo squid -k parse
     
    3.	Clear the Cache: If the cache is corrupted, clear it using:

bash

Copy code

sudo squid -z

sudo systemctl restart squid
________________________________________
#### Conclusion:
By completing this lab, you have:

•	Set up a Squid Proxy Server.

•	Configured it for web caching and access control.

•	Implemented URL filtering to block certain sites.

•	Secured the proxy with authentication.

•	Learned how to monitor and troubleshoot the proxy server.

These tasks are fundamental for network administrators who need to manage internet access, improve network performance, and secure browsing within an organization.












