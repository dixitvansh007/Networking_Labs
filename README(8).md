
# Networking Labs

### Networking Lab - 8 

#### OBJECTIVE: 
Configuring a web server firewall to block ICMP requests.
#### PRE-REQUISITES: 
Cisco packet tracer software installed.
#### DURATION: 
30 min

#### STEPS:

•	Start the packet tracer file included (Lab-8 Start), and have a look at the configuration.

•	Go to IPv4 firewall under Desktop in web server.

•	Update the information as following:

i.	Action: “Deny”

ii.	Protocol: ICMP

iii.	Remote IP: Enter the network address

iv.	Remote Wildcard Mask: Enter the inverse the Subnet Mask

•	Click on “ADD” to save the information.

•	Use the “ping” command from laptop in the topology to test connectivity in the network.

•	Turn the Firewall “ON” in the webserver.

•	Now test the connectivity again using “ping” command.

#### CONCLUSION:  
The above lab practice gives an opportunity to configure a web server and enable the firewall to block the ICMP requests, which are one of the sources of web attack (Denial of Service). Since “ping” command utilises ICMP, we can further check the firewall for our web server configurations.
