# Lab - Build Your VPC and Launch a Web Server

OBJECTIVES
After completing this lab, you should be able to:

Create a virtual private cloud (VPC)
Create subnets
Configure a security group
Launch an Amazon Elastic Compute Cloud (Amazon EC2) instance into a VPC
DURATION
This lab takes approximately 45 minutes to complete.

SCENARIO
In this lab, you use Amazon Virtual Private Cloud (VPC) to create your own VPC and add additional components to produce a customized network for a Fortune 100 customer. You also create security groups for your EC2 instance. You then configure and customize an EC2 instance to run a web server and launch it into the VPC that looks like the following customer diagram:

CUSTOMER DIAGRAM
A picture of what the customer is requesting: a VPC, subnets (public and private), security group, and an EC2 instance on which to run a web server

Figure: The customer is requesting the build of this architecture to launch their web server successfully.

AWS SERVICE RESTRICTIONS
In this lab environment, access to AWS services and service actions might be restricted to the ones that you need to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that this lab describes.

Start lab
To launch the lab, at the top of the page, choose Start lab.
 You must wait for the provisioned AWS services to be ready before you can continue.

To open the lab, choose Open Console.
You are automatically signed in to the AWS Management Console in a new web browser tab.

 Do not change the Region unless instructed.

COMMON SIGN-IN ERRORS
Error: You must first sign out


If you see the message, You must first log out before logging into a different AWS account:

Choose the click here link.
Close your Amazon Web Services Sign In web browser tab and return to your initial lab page.
Choose Open Console again.
Error: Choosing Start Lab has no effect
In some cases, certain pop-up or script blocker web browser extensions might prevent the Start Lab button from working as intended. If you experience an issue starting the lab:

Add the lab domain name to your pop-up or script blocker’s allow list or turn it off.
Refresh the page and try again.
Task 1: Create your VPC
In this task, you use the VPC Wizard to create a VPC, an internet gateway, and two subnets in a single Availability Zone. An internet gateway is a VPC component that allows communication between instances in your VPC and the internet.

After creating a VPC, you can add subnets. Each subnet resides entirely within one Availability Zone and cannot span zones. If a subnet’s traffic is routed to an internet gateway, the subnet is known as a public subnet. If a subnet does not have a route to the internet gateway, the subnet is known as a private subnet.

The wizard also creates a NAT gateway, which is used to provide internet connectivity to EC2 instances in private subnets.

On the AWS Management Console, in the Search bar, enter and choose 

VPC
 to go to the VPC Dashboard.

Choose Create VPC and configure the following options:

Resources to create: Choose VPC and more

Name tag auto-generation: UnCheck the box Auto-generate

IPv4 CIDR: Enter 

10.0.0.0/16

IPv6 CIDR block: Choose No IPv6 CIDR block.

Tenancy: Choose Default.

Number of Availability Zones (AZs): 1

Number of public subnets: 1

Number of private subnets: 1

Expand Customize subnets CIDR blocks

Public subnet CIDR block in region: 

10.0.0.0/24

Private subnet CIDR block in region: 

10.0.1.0/24

NAT gateways: Choose In 1 AZ

VPC endpoints: Choose None

On the Preview pane, name the resources as follows:
VPC: 

Lab VPC

Subnets (2)

First box, Public subnet one without name tag: 

Public Subnet 1

Second box, Private subnet one without name tag: 

Private Subnet 1

Route tables (2)

First box, Public route table without name tag: 

Public Route Table

Second box, Private route table without name tag: 

Private Route Table

Choose Create VPC.

Choose View VPC.

Lab-vpc details are displayed as per configuration.

Task 1 shows the creation of the VPC, public and private route tables, and their CIDRs.

Figure: The VPCs, CIDRs, and the creation of public and private route tables. You created these options using the VPC Wizard.

The public subnet has a Classless Inter-Domain Routing (CIDR) of 10.0.0.0/24, which means that it contains all IP addresses starting with 10.0.0.x.

The private subnet has a CIDR of 10.0.1.0/24, which means that it contains all IP addresses starting with 10.0.1.x.

Task 2: Create additional subnets
In this task, you create two additional subnets in a second Availability Zone. This option is useful for creating resources in multiple Availability Zones to provide high availability.

In the left navigation pane, choose Subnets.

To configure the second public subnet, choose Create subnet and configure the following options:

VPC ID: From the dropdown list, choose Lab VPC.
Subnet name: Enter 

Public Subnet 2
Availability Zone: From the dropdown list, choose the second Availability Zone.
IPv4 CIDR block: Enter 

10.0.2.0/24
Choose Create subnet.
The subnet will have all IP addresses starting with 10.0.2.x.

To configure the second private subnet, choose Create subnet and configure the following options:
VPC ID: From the dropdown list, choose Lab VPC.
Subnet name: Enter 

Private Subnet 2
Availability Zone: From the dropdown list, choose the second Availability Zone.
IPv4 CIDR block: Enter 

10.0.3.0/24
Choose Create subnet.
The subnet will have all IP addresses starting with 10.0.3.x.

Task 3: Associate the subnets and add routes
In the left navigation pane, choose Route Tables.

Choose Public route table

In the lower pane, choose the Subnet associations tab.

Under Subnets without explicit associations, choose Edit subnet associations.

Select the check box for Public Subnet 2.

Choose Save associations.

You now configure the route table that is used by the private subnets.

Choose Private route table

In the lower pane, choose the Subnet associations tab.

Under Subnets without explicit associations, choose Edit subnet associations.

Select the check box for Private Subnet 2.

Choose Save associations.

Your VPC now has public and private subnets configured in two Availability Zones:

The creation of the networking components and routing components

Figure: The creation of the networking resources and routing components and attachment of these resources that make the VPC functional as a network.

Task 4: Create a VPC security group
In this task, you create a VPC security group, which acts as a virtual firewall for your instance. When you launch an instance, you associate one or more security groups with the instance. You can add rules to each security group that allow traffic to or from its associated instances.

In the left navigation pane, choose Security Groups.

Choose Create security group.

Configure the security group with the following options:

Security group name: Enter 

Web Security Group
Description: Enter 

Enable HTTP access
VPC: Choose Lab VPC.
Under Inbound rules, choose Add rule.

Configure the following options:

Type: Choose HTTP.
Source: Choose Anywhere IPv4.
Description: Enter 

Permit web requests
Choose Create security group.
You use this security group in the next task when launching an EC2 instance.

Task 5: Launch a web server instance
In this task, you launch an EC2 instance into the new VPC. You configure the instance to act as a web server.

On the AWS Management Console, in the Search bar, enter and choose 

EC2
 to go to the EC2 Management Console.

In the left navigation pane, choose Instances.

Choose Launch instances and configure the following options:

In the Name and tags section, Name: 

Web Server 1
.

In the Application and OS Images (Amazon Machine Image) section, configure the following options:

Quick Start: Choose Amazon Linux.

Amazon Machine Image (AMI): From dropdown, Choose Amazon Linux 2023 AMI.

In the Instance type section, choose t3.micro.

In the Key pair (login) section, choose AWSLabsKeyPair-<random string - stack Id>.

In the Network settings section, choose Edit and configure the following options:
VPC - required: Choose Lab VPC.
Subnet: Choose Public Subnet 2.
Auto-assign public IP: Choose Enable.
Firewall (security groups): Choose Select existing security group.
Choose Web Security Group.
Expand Advanced details

Under User data, copy and paste the following code


#!/bin/bash
#Install Apache Web Server and PHP
dnf install -y httpd mariadb105-server php
#Download Lab files
wget https://us-east-1-tcprod.s3.amazonaws.com/courses/CUR-TF-100-RSNETK/v3.0.0.prod-ea58589a/267-lab-NF-build-vpc-web-server/scripts/lab-app.zip
unzip lab-app.zip -d /var/www/html/
#Turn on web server
systemctl enable httpd
systemctl start httpd
Choose Launch instance.

To display the launched instance, choose View all instances.

Wait until the Web Server 1 shows 2/2 checks passed in the Status check column.

 This may take a few minutes. To update the page, choose refresh  at the top of the page.

You now connect to the web server running on the EC2 instance.

Select the check box for the instance, and choose the Details tab.

Copy the Public IPv4 DNS value.

Open a new web browser tab, paste the Public IPv4 DNS value, and press Enter.

When successful, the page should look like the following:

The success page of when the web server is launched

Figure: The success page when the web server is launched.

The following is the complete architecture that you deployed:

A picture of the end product, which is what the customer requested: a fully functional VPC with public and private route tables, its resources, and a web server

Figure: A picture of the end product, which is the delivery of the exact customer request: a fully functional VPC with its resources (network and security) and a web server.

https://awsrestart.labs.awsevents.com/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FCUR-TF-100-RSNETK-3%2F267-lab-NF-build-vpc-web-server%3A3.0.0-0d254a0e/en-US/46a424b2-440d-424c-b01f-97df9bc7474a::dsu7cQfmPCkLJi5Dr66o2R