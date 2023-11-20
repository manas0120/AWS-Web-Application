# Highly-Availabl, Fault toilerant, Multi-Tier Web-Application
Specifically, we will walk through the following topics`:
1. Network – Amazon VPC
2. Compute – Amazon EC2
3. Database – Amazon Aurora
4. Storage – Amazon S3
5. Clean up resource.

A. Network - Amazon VPC -
   In this lab, we'll create not only one Public Subnet and one Private Subnet in two Availability Zone(AZ-a, AZ-c) but also one NAT Gateway placed on the Public Subnet through VPC Wizard. After configuring these resources, you will set up a routing table to define the network traffic flow. Through these tasks, you can complete a basic networking configuration to create a highly available and scalable web service environment. 

Network Architecture - 
   
<img width="552" alt="network architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/8d22459a-3bd4-4fea-8d05-9d0805903c47">

1.) Create  VPC - Amazon Virtual Private Cloud (Amazon VPC) allows you to start AWS resources with a user-defined virtual network. This virtual network, along with the benefits of using AWS's scalable infrastructure, is very similar to the existing network operating in the customer's own data center. 

a. After logging in to the AWS console, select VPC from the service menu. 

Select VPC Dashboard and click Launch VPC Wizard to create your own VPC. 

To create a space to provision AWS resources used in this lab, we will create a VPC and Subnets. Select VPC, subnets, etc in Resource to create tab and change name tag to VPC-Lab. Leave the default setting for IPv4 CIDR block. 

To design high availability architecture, we create 2 subnet space and select 2a and 2c for Customize AZs. And set the CIDR value of the public subnet that can communicate directly with the Internet as shown in the screen below. Set the CIDR value of the private subnet as shown in the screen. 

You can use a NAT gateway so that instances in your private subnets can connect to services outside your VPC, but external services cannot initiate direct connections to these instances. In this lab, we will create a NAT gateway in only one Availability Zone to save cost. Also, for DNS options, enable both DNS hostnames and DNS resolution. After confirming the setting value, click the Create VPC button. 

As the VPC is created, you can see the process of creating network-related resources as shown in the screen below. For NAT Gateway, provisioning may take longer compared to other resources. 

You can check the information of the created VPC. Check related information such as CIDR value, route table, network ACL, etc. Check that the values you just set are correct. 

2.) Create VPC Endpoint - In this section, you create an endpoint for S3 to learn a VPC endpoint. 

In VPC Dashboard, select Endpoints. Click Create endpoint button. 

Type s3 endpoint for name and select AWS services in Service category tab. In the search bar below, type s3 and select the list at the top. 

For S3 VPC endpoints, there are gateway types and interface types. For this lab, select the gateway type. And for the deployment location, select the VPC-Lab-vpc created in this lab. 

Choose a route table to reflect the endpoint. Select the two private subnets as shown below. Additional routing information for using the endpoint is automatically added to the selected route table. 

  
NOTE:- VPC endpoints are communications within the AWS network and have the security and compliance advantage of being able to control traffic through the endpoints. You can also optimize the data processing cost if you transfer your data through a VPC endpoint rather than a NAT gateway. 


B. Compute - Amazon EC2
   Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. Amazon EC2’s simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon’s proven computing environment.


EC2 Architecture - Auto Scaling Group to deploy web service instances to private subnets in VPC that we created earlier in this network lab. This configures the highly available web services so that external users can access the Sample Web Page through the web.

   <img width="554" alt="EC2 Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/05c12a41-1634-4b3c-a2c8-577a1306d72e">

The following hands-on are to be done -:
Launch web server instances and execute user data 
Set up a security group 
Create a custom Amazon Machine Image (AMI) 
Launch an Application Load Balancer (ALB) 
Configure a Launch Template 
Configure an Auto Scaling Group 
Test auto scaling and change manual settings

a. Launch instance and connect to web service -

In the AWS console search bar, type EC2  and select it. Then click EC2 Dashboard at the top of the left menu. Press the Launch instance button and select Launch instance from the menu.  
In Name, put the value Web server for custom AMI. And check the default setting in Amazon Machine Image below. 
Select t2.micro in Instance Type.  
For Key pair, choose Proceed without a key pair.  
Click the Edit button in Network settings to set the space where EC2 will be located. 
And choose the VPC-Lab-vpc created in the previous lab, and for the subnet, choose public subnet. Auto-assign public IP is set to Enable. 

b. Create Security groups - to act as a network firewall. Security groups will specify the protocols and addresses you want to allow in your firewall policy. 

For the security group you are currently creating, this is the rule that applies to the EC2 that will be created. After entering Multi-Tier in Security group name and Description, select Add Security group rule and set HTTP to Type. Also allow TCP/80 for Web Service by specifying it. Select My IP in the source. 
All other values accept the default values, expand by clicking on the Advanced Details tab at the bottom of the screen. 
Click the Meta Data version dropdown and select V2 only (token required) 
Wait for the instance's Instance state result to be Running. Open a new web browser tab and enter the Public DNS or IPv4 Public IP of your EC2 instance in the URL address field. If the page is displayed as shown below, the web server instance is configured normally. 

c. Create a custom AMI - 

In the EC2 console, select the instance that we made earlier in this lab, and click Actions > Image and templates > Create Image. 
In the Create Image console, type as shown below and press Create image to create the custom image.   
Verify in the console that the image creation request in completed.  
In the left navigation panel, Click the AMIs button located under IMAGES. You can see that the Status of the AMI that you just created. It will show either Pending or Available. 

d. Terminate the instance 

Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created. Therefore, the EC2 instance currently running is no longer needed, so let's try to terminate it. ( In Deploy auto scaling web service, we will use custom AMI to create a new web server.) 
  

NOTE - Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created. Therefore, the EC2 instance currently running is no longer needed, so let's try to terminate it. ( In Deploy auto scaling web service, we will use custom AMI to create a new web server.)

Architecture Configured till now - 

<img width="626" alt="Ec2 Architecture with IG" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/2db11c8c-50d6-48bf-8aad-52f7601783c1">
