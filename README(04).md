
# Networking Labs

### Networking Lab - 4

#### Duration: 2 - 3 hourse

#### Lab: Creating and Managing Virtual Networks in a Cloud Environment

Cloud environments like AWS, Azure, and Google Cloud Platform (GCP) provide powerful features for creating and managing virtual networks that can be used for various purposes like web hosting, application deployment, and connecting cloud services. In this lab, we will cover the process of creating, configuring, and managing virtual networks within a cloud environment.

#### Lab Overview:
•	You will learn how to create Virtual Private Networks (VPCs), subnets, configure IP addressing, manage network access controls, and securely connect cloud resources.

•	You can perform this lab in any of the three major cloud environments (AWS, Azure, or GCP). The examples here will be based on AWS, but the concepts can be adapted to other cloud providers.

#### Lab 1: Setting Up a Virtual Private Cloud (VPC) in AWS

#### Objective:
•	Create a Virtual Private Cloud (VPC) in AWS.

•	Create public and private subnets within the VPC.

•	Set up route tables and network access control lists (NACLs).

•	Launch an EC2 instance in the public subnet and one in the private subnet.
________________________________________
#### Tasks:
Step 1: Create a Virtual Private Cloud (VPC)

    1.	Log into the AWS Console:
       
       o	Go to the AWS Management Console, and log in using your credentials.
    
    2.	Create a New VPC:
       
       o	In the AWS Management Console, navigate to VPC under the "Networking & Content Delivery" section.
       
       o	Click Create VPC.
       
       o	Provide the following details:
       
       *	VPC Name: MyVPC
       
       *	IPv4 CIDR block: 10.0.0.0/16
       
       *	IPv6 CIDR block: Optional (leave empty if you don’t need IPv6)
       
       *	Tenancy: Default
       
       o	Click Create VPC.
    
    3.	Check VPC Creation:
       
       o	Go to VPC Dashboard and confirm that the VPC was created successfully.

Step 2: Create Subnets
    
    1.	Create a Public Subnet:
       o	In the VPC dashboard, navigate to Subnets and click Create subnet.
       o	Provide the following details:
       
       *	Name: PublicSubnet
       
       *	VPC: Select the VPC you created.
       
       *	Availability Zone: Select any available AZ in your region (e.g., us-east-1a).
       
       *	CIDR block: 10.0.1.0/24
       
       o	Click Create.
    2.	Create a Private Subnet:
       
       o	Repeat the above steps, but this time use:

       *	Name: PrivateSubnet

       *	CIDR block: 10.0.2.0/24

       o	Click Create.

Step 3: Set Up Route Tables

    1.	Create Route Table for the Public Subnet:
       
       o	In the VPC dashboard, go to Route Tables and click Create route table.

       o	Name the route table PublicRouteTable, and select the VPC you created.

       o	Click Create.
    
    2.	Configure Route Table for Public Access:
       
       o	Select the PublicRouteTable and go to the Routes tab.

       o	Click Edit routes, then add a new route with the following details:
       *	Destination: 0.0.0.0/0 (allowing all outbound traffic)

       *	Target: Select Internet Gateway (you will need to create an Internet Gateway for this).

       o	Click Save routes.

    3.	Associate Route Table with Public Subnet:

       o	Under Subnet associations, click Edit subnet associations.

       o	Select the PublicSubnet and click Save.

Step 4: Create and Attach an Internet Gateway

    1.	Create an Internet Gateway:
       
       o	Navigate to Internet Gateways and click Create internet gateway.
       
       o	Name it MyInternetGateway and click Create.
    
    2.	Attach Internet Gateway to the VPC:

       o	Select the newly created Internet Gateway.
       
       o	Click Actions > Attach to VPC and select the VPC you created. Click Attach.

Step 5: Launch EC2 Instances
    
    1.	Launch an EC2 Instance in the Public Subnet:

       o	Go to EC2 Dashboard and click Launch Instance.
        
       o	Choose an AMI (Amazon Machine Image), such as Amazon Linux 2 AMI.

       o	Select t2.micro instance type for a low-cost option.
       
       o	Under Network, select the VPC you created, and select the PublicSubnet for placement.

       o	Configure security group (add rules for HTTP/HTTPS and SSH).

       o	Launch the instance and create a new key pair (download the key pair for SSH access).
       
       o	Once the instance is running, retrieve the public IP address to SSH into the instance.
    2.	Launch an EC2 Instance in the Private Subnet:
       
       o	Repeat the same steps as above but choose the PrivateSubnet for placement.
       
       o	Since this instance is in a private subnet, it won’t have direct internet access.

       o	You will need to use a bastion host (instance in the public subnet) to SSH into the private instance.

Step 6: Verify Connectivity

    1.	SSH into the EC2 Instance in the Public Subnet:
     
       o	Use the SSH command to access the EC2 instance in the public subnet:
bash
Copy code
ssh -i /path/to/keypair.pem ec2-user@<public-ip>
    
    2.	Access the Private Subnet Instance:

       o	Use the instance in the public subnet as a bastion host to SSH into the private instance:

bash
Copy code
ssh -i /path/to/keypair.pem ec2-user@<public-ip-of-bastion-host>
ssh ec2-user@<private-ip-of-private-instance>


#### Lab 2: Configuring Security Groups and Network ACLs

#### Objective:
•	Learn to configure security groups and network access control lists (NACLs) to control traffic in and out of your VPC.
________________________________________
#### Tasks:
Step 1: Create a Security Group for Public Access
       
    1.	Create Security Group:

      o	In the VPC Dashboard, navigate to Security Groups and click Create Security Group.

      o	Name it PublicSecurityGroup, and select the VPC you created.
      
      o	Add the following inbound rules:
      
      *	Type: SSH, Port Range: 22, Source: 0.0.0.0/0 (to allow SSH access from any IP).

      *	Type: HTTP, Port Range: 80, Source: 0.0.0.0/0 (to allow web traffic).

      o	Click Create.

    2.	Assign Security Group to EC2 Instances:

      o	Edit your EC2 instance in the public subnet to use the PublicSecurityGroup for security.

Step 2: Create and Configure Network Access Control Lists (NACLs)
    
    1.	Create a Network ACL:
    
      o	Navigate to Network ACLs in the VPC dashboard and click Create network ACL.

      o	Name it PublicNACL, select the VPC, and click Create.

    2.	Add Inbound and Outbound Rules:

      o	For Inbound Rules:

      *	Allow inbound SSH and HTTP traffic:

      *	Rule #: 100, Type: SSH, Port Range: 22, 
      Source: 0.0.0.0/0, Allow.

      *	Rule #: 200, Type: HTTP, Port Range: 80, 
      Source: 0.0.0.0/0, Allow.

      o	For Outbound Rules:

      *	Allow all outbound traffic:

      *	Rule #: 100, Type: All Traffic, Port Range: All, Destination: 0.0.0.0/0, Allow.

    3.	Associate NACL with Subnet:
      
      o	Go to Subnet associations, click Edit subnet associations, and associate the NACL with the PublicSubnet.
    
    4.	Test Security and NACL Configuration:
      
      o	Test SSH and HTTP connectivity to ensure your security and NACL rules are working as expected.
________________________________________
#### Conclusion:
In this lab, you've learned how to:
1.	Create and configure a Virtual Private Cloud (VPC) in AWS.

2.	Set up public and private subnets.

3.	Set up routing and internet access for a public subnet.

4.	Launch EC2 instances in public and private subnets.

5.	Configure security groups and network ACLs for controlling traffic.
By following this lab, you’ve gained hands-on experience with essential network management tasks in a cloud environment, which are critical for securely deploying applications and managing resources in the cloud.
