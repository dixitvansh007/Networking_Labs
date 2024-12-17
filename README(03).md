
# Networking Labs

### Networking Lab - 3

#### OBJECTIVE: 
To demonstrate the importance of protocols like FTP that utilise encryption.

#### PRE-REQUISITES: 
FileZilla server, FileZilla client, and Wireshark software installed.

#### DURATION: 
30 min

#### STEPS:
•	Setup and configure the server with a single user.

•	Goto Wireshark and start capturing the packets.

•	Connect to the FileZilla server using its ip address, username, password, and port number 21.

•	Once connected to the server, download a file.

•	Finally go to the Wireshark and stop capturing the packets.

•	Take a look at the requests made in the data packets flow for the username and password.

•	You can create a new directory in the server and download this again.

•	Take a closer look at the data packets while capturing the download requests.

#### CONCLUSION:  
This lab aims at bringing up the importance of encryption that is not utilised on some protocols. While looking at the packets data in Wireshark, it is observed that the data actually flows in plain text when the server is not utilising a secure protocol. Hence it is always recommended to use protocols that are secure.
