
# Networking Labs

### Networking Lab - 1 

#### OBJECTIVE: 
To dynamically assign IP addresses to different computers on a LAN using DHCP server.

#### PRE-REQUISITES: 
Cisco packet tracer software installed.

### DURATION: 
30 min

#### STEPS:

•	Start the packet tracer file included (Lab-5 Start), and have a look at the configuration.

•	Add a server and connect it to the switch using straight-through cable.

•	Configure the server and assign it a class C static ip address.

•	Verify by using “ipconfig” in command prompt.

•	Go to PC#1 in Desktop and IP Configuration, turn off Static IP, and turn on DHCP server to pull an IP address automatically.

•	Now go to the server, select Services and then DHCP.

•	Provide the required details known as DHCP Scope, and turn ON the DHCP server.

•	Go to command prompt of the PC which you want to update with new IP address.

•	Release the previous ip address using “ipconfig release”, and then use “ipconfig renew”.

•	The PC will have the ip address in the DHCP defined range of ip addresses.

#### CONCLUSION:  
This lab is based on the theory of DHCP servers and their applications in a network. It demonstrates how a device with static ip address can be allotted with a dynamic ip address, once a DHCP server has been introduced in the network.
