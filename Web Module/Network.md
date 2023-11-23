A. Network - Amazon VPC -</b></h3> 
In this lab, we'll create not only one Public Subnet and one Private Subnet in two Availability Zone(AZ-a, AZ-c) 
but also one NAT Gateway placed on the Public Subnet through VPC Wizard. After configuring these resources,
you will set up a routing table to define the network traffic flow. 
Through these tasks, you can complete a basic networking configuration to create a highly available and scalable web service environment. 
<h3><b>
Network Architecture - </b></h3>
<img width="552" alt="network architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/8d22459a-3bd4-4fea-8d05-9d0805903c47">
<h3><b>
1.) Create  VPC - </b></h3>Amazon Virtual Private Cloud (Amazon VPC) allows you to start AWS resources with a user-defined virtual network. 
This virtual network, along with the benefits of using AWS's scalable infrastructure, is very similar to 
the existing network operating in the customer's own data center. 
a. After logging in to the AWS console, select VPC from the service menu. 
Select VPC Dashboard and click Launch VPC Wizard to create your own VPC. 
To create a space to provision AWS resources used in this lab, we will create a VPC and Subnets. 
Select VPC, subnets, etc in Resource to create tab and change name tag to VPC-Lab. Leave the default setting for IPv4 CIDR block. 
To design high availability architecture, we create 2 subnet space and select 2a and 2c for Customize AZs.
And set the CIDR value of the public subnet that can communicate directly with the Internet as shown in the screen below. 
Set the CIDR value of the private subnet as shown in the screen. 
You can use a NAT gateway so that instances in your private subnets can connect to services outside your VPC, 
but external services cannot initiate direct connections to these instances. In this lab, 
we will create a NAT gateway in only one Availability Zone to save cost. Also, for DNS options, 
enable both DNS hostnames and DNS resolution. After confirming the setting value, click the Create VPC button. 
As the VPC is created, you can see the process of creating network-related resources as shown in the screen below. 
For NAT Gateway, provisioning may take longer compared to other resources. 
You can check the information of the created VPC. Check related information such as CIDR value, route table, network ACL, etc. 
Check that the values you just set are correct. 
<h3><b>
2.) Create VPC Endpoint - </b></h3>In this section, you create an endpoint for S3 to learn a VPC endpoint. 
In VPC Dashboard, select Endpoints. Click Create endpoint button. 
Type s3 endpoint for name and select AWS services in Service category tab. In the search bar below, type s3 and select the list at the top. 
For S3 VPC endpoints, there are gateway types and interface types. For this lab, select the gateway type. 
And for the deployment location, select the VPC-Lab-vpc created in this lab. 
Choose a route table to reflect the endpoint. Select the two private subnets as shown below. 
Additional routing information for using the endpoint is automatically added to the selected route table. 
NOTE:- VPC endpoints are communications within the AWS network and have the security and compliance 
advantage of being able to control traffic through the endpoints. You can also optimize the data processing 
cost if you transfer your data through a VPC endpoint rather than a NAT gateway. 
</p>