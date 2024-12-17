
# Networking Labs

### Networking Lab - 11

### Duration: 3 - 4 hourse

### Lab: Implementing SSL/TLS in Networking
SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are cryptographic protocols designed to provide secure communication over a computer network. TLS is the successor to SSL and is more secure, although the term "SSL" is still commonly used. These protocols use encryption to secure the data transmission between clients and servers, ensuring confidentiality, integrity, and authenticity.

In this lab, we will set up an SSL/TLS secured connection for a web server using Apache and NGINX, and then test it using a web browser and tools like openssl.

#### Lab Overview:
•	Set up SSL/TLS on a web server (Apache or NGINX).

•	Create and install a self-signed certificate.

•	Test the secure connection using OpenSSL and a browser.

•	Configure the server to use strong SSL/TLS ciphers.

•	Implement SSL/TLS best practices.
________________________________________
#### Lab 1: Setting Up SSL/TLS with Apache Web Server

#### Objective:
•	Install and configure Apache to support SSL/TLS.

•	Generate a self-signed SSL certificate.

•	Configure Apache to use SSL/TLS for secure communication.

#### Tasks:

Step 1: Install Apache Web Server
      
    1.	Install Apache on Ubuntu/Debian:

bash

Copy code

sudo apt update

sudo apt install apache2 -y

    2.	Install Apache on CentOS/RHEL:

bash

Copy code

sudo yum install httpd -y

    3.	Start and Enable Apache Service:

bash

Copy code

sudo systemctl start apache2   # Ubuntu/Debian

sudo systemctl enable apache2  # Ubuntu/Debian

Or for CentOS/RHEL:

bash

Copy code

sudo systemctl start httpd

sudo systemctl enable httpd

Step 2: Enable SSL Module and Install SSL Package

    1.	Enable SSL Module: On Ubuntu/Debian systems, Apache comes with the SSL module, but you may need to enable it:

bash

Copy code

sudo a2enmod ssl

    2.	Install SSL Certificate Packages (Ubuntu/Debian):

bash

Copy code

sudo apt install openssl -y

sudo apt install ssl-cert -y

Step 3: Create a Self-Signed SSL Certificate

    1.	Generate a Private Key and Self-Signed Certificate: Run the following commands to generate a private key and a certificate signing request (CSR). Then, use the CSR to generate the certificate.

bash

Copy code

sudo mkdir /etc/ssl/private

sudo mkdir /etc/ssl/certs

#### Generate a private key
sudo openssl genpkey -algorithm RSA -out /etc/ssl/private/server.key -aes256

#### Generate a self-signed certificate
sudo openssl req -new -x509 -key /etc/ssl/private/server.key -out /etc/ssl/certs/server.crt -days 365

    2.	Set Proper Permissions: Make sure the certificate and key files have appropriate permissions:

bash

Copy code

sudo chmod 600 /etc/ssl/private/server.key

sudo chmod 644 /etc/ssl/certs/server.crt

Step 4: Configure Apache for SSL

    1.	Edit Apache SSL Configuration: Edit the Apache SSL configuration file to enable the SSL virtual host.

bash

Copy code

sudo nano /etc/apache2/sites-available/default-ssl.conf

     o	Set the following configuration:

bash

Copy code

<VirtualHost *:443>

    ServerAdmin webmaster@localhost

    DocumentRoot /var/www/html

    SSLEngine on

    SSLCertificateFile /etc/ssl/certs/server.crt

    SSLCertificateKeyFile /etc/ssl/private/server.key

</VirtualHost>

    2.	Enable SSL Site: Enable the SSL site and reload Apache to apply changes:

bash

Copy code

sudo a2ensite default-ssl.conf

sudo systemctl reload apache2

    3.	Verify SSL Configuration: Verify that SSL is enabled by visiting your server’s IP address or domain name using https:// in a web browser (e.g., https://<server-ip>).

Step 5: Test SSL/TLS Connection
    
    1.	Verify SSL/TLS Connection Using Browser:

     o	Open a web browser and navigate to https://<your-server-ip> (or the domain name).

     o	You may see a warning about the self-signed certificate. This is normal, as self-signed certificates are not trusted by default. You can bypass the warning to access the page.

2.	Verify SSL/TLS Using OpenSSL: You can also verify the connection using the openssl command:

bash

Copy code

openssl s_client -connect <your-server-ip>:443

The output will show you details about the SSL connection, including the certificate chain, ciphers, and more.
________________________________________
#### Lab 2: Setting Up SSL/TLS with NGINX

#### Objective:
•	Install and configure NGINX for SSL/TLS.

•	Create and install a self-signed certificate for NGINX.

#### Tasks:

Step 1: Install NGINX Web Server

    1.	Install NGINX on Ubuntu/Debian:

bash

Copy code

sudo apt update

sudo apt install nginx -y

    2.	Install NGINX on CentOS/RHEL:

bash

Copy code

sudo yum install nginx -y

    3.	Start and Enable NGINX Service:

bash

Copy code

sudo systemctl start nginx

sudo systemctl enable nginx

Step 2: Generate SSL Certificates

    1.	Create a Self-Signed SSL Certificate: Similar to the Apache setup, generate a self-signed certificate for NGINX:

bash

Copy code

sudo mkdir -p /etc/nginx/ssl

sudo openssl genpkey -algorithm RSA -out /etc/nginx/ssl/nginx.key -aes256

sudo openssl req -new -x509 -key /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt -days 365

    2.	Set Proper Permissions:

bash

Copy code

sudo chmod 600 /etc/nginx/ssl/nginx.key

sudo chmod 644 /etc/nginx/ssl/nginx.crt

Step 3: Configure NGINX for SSL
    
    1.	Edit NGINX Configuration for SSL: Open the default configuration file to configure SSL:

bash

Copy code

sudo nano /etc/nginx/sites-available/default

Add the following server block to configure SSL:

bash

Copy code

server {

    listen 443 ssl;
    
    server_name your_domain_or_ip;

    ssl_certificate /etc/nginx/ssl/nginx.crt;
    
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    root /var/www/html;
    
    index index.html;

}

    2.	Redirect HTTP to HTTPS: To ensure users are redirected to HTTPS, add a server block for HTTP (port 80) that redirects traffic to HTTPS:

bash

Copy code

server {

    listen 80;

    server_name your_domain_or_ip;

    return 301 https://$host$request_uri;
}

    3.	Test NGINX Configuration: Check the configuration for syntax errors:

bash

Copy code

sudo nginx -t

    4.	Restart NGINX: Restart NGINX to apply the changes:

bash

Copy code

sudo systemctl restart nginx

Step 4: Test SSL/TLS Connection

    1.	Verify SSL/TLS Connection Using Browser: Navigate to https://<your-server-ip> or https://<your-domain-name> in your browser to check if SSL is working.

    2.	Verify SSL/TLS Using OpenSSL: Similar to Apache, use the OpenSSL command to verify the SSL/TLS connection:

bash

Copy code

openssl s_client -connect <your-server-ip>:443
________________________________________
#### Lab 3: Hardening SSL/TLS Configuration

#### Objective:

•	Configure SSL/TLS with strong security settings and disable weak ciphers.

#### Tasks:

Step 1: Disable SSL and Weak Ciphers

    1.	Disable SSL 2.0/3.0 and Weak Ciphers in Apache: In the ssl.conf file (Apache’s SSL configuration file), add the following lines to disable weak ciphers:

bash

Copy code

SSLEngine on

SSLProtocol all -SSLv2 -SSLv3

SSLCipherSuite HIGH:!aNULL:!MD5:!3DES

SSLHonorCipherOrder on

    2.	Disable SSL and Weak Ciphers in NGINX: In the nginx.conf file, add the following lines:

bash

Copy code

ssl_protocols TLSv1.2 TLSv1.3;

ssl_ciphers 'ECDHE+AESGCM:ECDHE+AES256:ECDHE

+AES128:!SHA:!3DES:!RC4:!MD5';

ssl_prefer_server_ciphers on;

    3.	Restart Servers: After making changes to the SSL/TLS configuration, restart both Apache and NGINX:

bash

Copy code

sudo systemctl restart apache2   # For Apache

sudo systemctl restart nginx     # For NGINX

Step 2: Verify Secure Configuration

    1.	Use SSL Labs' SSL Test: Go to SSL Labs SSL Test and test your server's SSL/TLS configuration. 

It will provide a grade (A+, A, etc.) and give feedback on how to further improve security.

    2.	Use OpenSSL to Test Supported Protocols: You can also check the supported protocols using OpenSSL:

bash

Copy code

openssl s_client -connect <your-server-ip>:443 
-tls1_2

openssl s_client -connect <your-server-ip>:443 
-tls1_3
________________________________________
#### Conclusion:

In this lab, you've learned how to:

    1.	Set up and configure SSL/TLS on Apache and NGINX.

    2.	Generate and install self-signed certificates.

    3.	Secure your server with strong SSL/TLS ciphers and protocols.
    
    4.	Test the SSL/TLS connection using a web browser and OpenSSL.
    
    5.	Harden your SSL/TLS configuration for improved security.

These steps are crucial for securing communication on web servers and ensuring the confidentiality and integrity of data in transit.



