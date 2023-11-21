<h1> <b></b># Highly-Availabl, Fault toilerant, Multi-Tier Web-Application
Specifically, we will walk through the following topics`:
1. Network – Amazon VPC
2. Compute – Amazon EC2
3. Database – Amazon Aurora
4. Storage – Amazon S3
5. Clean up resource.
</h1>

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

e. Lauch Application Load Balancer 

Using the network infrastructure created in the Network- Amazon VPC lab, we will deploy a web service that can automatically scale out/in under load and ensure high availability. We use the web server AMI created in the previous chapter and the network infrastructure named VPC-Lab. 

f. Configure Application Load Balancer 

AWS Elastic Load Balancer supports three types of load balancers: Application Load Balancer, Network Load Balancer, and Classic Load Balancer. In this lab, you will configure and set up the Application Load Balancer to handle load balancing HTTP requests. 
From the EC2 Management Console in the left navigation panel, click Load Balancers under Load Balancing. Then click Create Load Balancer. In the Select load balancer type, click the Create button under Application Load Balancer.
Name the load balancer. In this case, name Name as Web-ALB. Leave the other settings at their default values. 
Scrolling down a little bit, there is a section for selecting availability zones. First, Select the VPC-Lab-vpc created previously. For Availability Zones select the 2 public subnets that were created previously. This should be Public Subnet for ap-northeast-2a and Public Subnet C for us-east-1c. 
In the Security groups section, click the Create new security group hyperlink. Enter web-ALB-SG as the security group name and check the VPC information. Click the Add rule button and select HTTP as the Type and Anywhere-IPv4 as the Source. And create a security group. 
Return to the load balancer page again, click the refresh button, and select the web-ALB-SG you just created. Remove the default security group. 
In Listeners and routing column, click Create target group. Put Web-TG for Target group name and check all settings same with the screen below. After that click Next button. 
This is where we would register our instances. However, as we mentioned earlier, there are not instances to register at this moment. Click Create target group. 
Again, move into the Load balancers page, click refresh button and select Web-TG. And then Click Create load balancer. 

g. Configure Launch Template 

Now that ALB has been created, it's time to place the instances behind the load balancer. To configure an Amazon EC2 instance to start with Auto Scaling Group, you can use Launch Template, Launch Configuration, or EC2 Instance. In this workshop, we will use the Launch Template to create an Auto Scaling group. 

The launch template configures all parameters within a resource at once, reducing the number of steps required to create an instance. Launch templates make it easier to implement best practices with support for Auto Scaling and spot fleets, as well as spot and on-demand instances. This helps you manage costs more conveniently, improve security, and minimize the risk of deployment errors. 

The launch template contains information that Amazon EC2 needs to start an instance, such as AMI and instance type. The Auto Scaling group refers to this and adds new instances when a scaling out event occurs. If you need to change the configuration of the EC2 instance to start in the Auto Scaling group, you can create a new version of the launch template and assign it to the Auto Scaling group. You can also select a specific version of the launch template that you use to start an EC2 instance in the Auto Scaling group, if necessary. You can change this setting at any time. 

h. Create Security Groups
Select Security Groups under the Network & Security heading and click Create Security Group in the upper right corner.
modify the Inbound rules. First, select the Add rule button to add the Inbound rules, and select HTTP in the Type. For Source, type ALB in the search bar to search for the security group created earlier Web-ALB-SG. This will configure the security group to only receive HTTP traffic coming from ALB.
Leave outbound rules' default settings and click Create Security Group to create a new security group. This creates a security group that allows traffic only for HTTP connections (TCP 80) that enter the instance via ALB from the Internet.

i. Create Launch Template
In the EC2 console, select Launch Templates from the left navigation panel. Then click Create Launch Template.

First, set Launch template name and Template version description as shown below, and select Checkbox for Provide guidance in Auto Scaling guidance. Select this checkbox to enable the template you create to be utilized by Amazon EC2 Auto Scaling.

In Amazon Machine Image(AMI), set the AMI to Web Server v1, which was created in the previous EC2 lab. You can find it by typing Web Server v1 in the search section, or you can scroll down to find it in the My AMI section. Next, select t2.micro for the instance type. We are not going to configure SSH access because this is only for Web service server. Therefore, we do not use key pairs.

Leave the other parts as default. Let's take a look at the Network Settings section. First, in Networking platform select Virtual Private Cloud(VPC). In security group section, find and apply ASG-Web-Inst-SG created before.

Follow the Storage's default values without any additional change. Go down and define the Instance tags. Click Add tag and Name for Key and Web Instance for Value. Select Resource types as Instances and Volumes.

Finally, in the Advanced details tab, set the IAM instance profile to SSMInstanceProfile. Leave all other settings as default, and click the Create launch template button at the bottom right to create a launch template.

j. Set Auto Scaling Group.
Enter the EC2 console and select Auto Scaling Groups at the bottom of the left navigation panel. Then click the Create Auto Scaling group button to create an Auto Scaling Group.
In [Step 1: Choose launch template or configuration], specify the name of the Auto Scaling group. In this workshop, we will designate it as Web-ASG. Then select the launch template that you just created named Web. The default settings for the launch template will be displayed. Confirm and click the lower right Next button.
Set the network configuration with the Purging options and instance types as default. Choose VPC-Lab-vpc for VPC, select Private subnet 1 and Private subnet 2 for Subnets. When the setup is completed, click the Next button.

Next, proceed to set up load balancing. First, select Attach to an existing load balancer. Then in Choose a target group for your load balancer, select Web-TG created during in ALB creation. At the Monitoring, select Check box for Enable group metrics collection within CloudWatch. This allows CloudWatch to see the group metrics that can determine the status of Auto Scaling groups. Click the Next button at the bottom right.

In the step of Configure group size and scaling policies, set scaling policy for Auto Scaling Group. In the Group size column, specify Desired capacity and Minimum capacity as 2 and Maximum capacity as 4. Keep the number of the instances to 2 as usual, and allow scaling of at least 2 and up to 4 depending on the policy.
In the Scaling policies section, select Target tracking scaling policy and type 30 in Target value. This is a scaling policy for adjusting the number of instances based on the CPU average utilization remaining at 30% overall. Leave all other settings as default and click the Next button in the lower right corner.

Click the Next button to move to the next step. In the Add tags step, we will simply assign name tag. Click Add tag, type Name in Key, ASG-Web-Instance in Value, and then click Next.
Now we are in the final stage of review. After checking the all settings, click the Create Auto Scaling Group button at the bottom right.
Auto Scaling group has been created.

 High available and automatically scales Load Architecture

 <img width="626" alt="HA Load Balancing arch" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/5681516e-194f-49a1-abed-a9f06ba56ec2">

 
<h1> CHECK WEB SERVICE AND TEST </h1>

