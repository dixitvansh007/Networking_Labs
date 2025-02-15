
# Networking Labs

### Networking Lab - 6 

#### OBJECTIVE: 
    1. Setup DNS on a server.
	2. Update DNS settings on systems.
    3. Setup HTTP web-server on server.
	4. Test website.

#### PRE-REQUISITES: 
Cisco packet tracer software installed.

#### DURATION: 
30 min

#### STEPS:

•	Start the packet tracer file included (PKT-3 Start), and have a look at the configuration.

•	Firstly, change the DNS server to the name of the server in the IP Configuration of the server.

•	Go to DHCP in services, update the DNS to the same, and Starting IP address.

•	Now go to each device in their command prompt, and run “ipconfig/renew”.

•	Make an entry for DNS in the server, and turn ON the HTTP server.

•	Verify by going to web-browser in PC#1 and hit the domain name for the DNS server.

•	Alternate method is to ping the server in command prompt using ip address or the domain name.

#### CONCLUSION:  
This lab is based on the theory of DNS servers and their applications in a network. It demonstrates how we can setup a DNS server in a network, make entries for the ip addresses known as DNS Records, and finally verify the connectivity.
