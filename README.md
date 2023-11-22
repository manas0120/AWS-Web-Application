<h1> <b><pre># Highly-Available, Fault tolerant, Multi-Tier Web-Application</pre></b></h1>
 <h2><pre>
Specifically, we will walk through the following topics:
1. Network – Amazon VPC
2. Compute – Amazon EC2
3. Database – Amazon Aurora
4. Storage – Amazon S3
5. Clean up resource.
</pre>
</h2>

<pre>
<h3> <b>
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
</pre>

<pre>
<h3><b>
B. Compute - Amazon EC2 </b></h3>
   Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. 
 It is designed to make web-scale cloud computing easier for developers. Amazon EC2’s simple web service interface allows you 
 to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources 
 and lets you run on Amazon’s proven computing environment.

<h3><b>EC2 Architecture - </b></h3>Auto Scaling Group to deploy web service instances to private subnets in VPC that we 
 created earlier in this network lab. This configures the highly available web services so that external users can access the Sample Web Page through the web.

   <img width="554" alt="EC2 Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/05c12a41-1634-4b3c-a2c8-577a1306d72e">

<h5><b>The following hands-on are to be done -:
Launch web server instances and execute user data 
Set up a security group 
Create a custom Amazon Machine Image (AMI) 
Launch an Application Load Balancer (ALB) 
Configure a Launch Template 
Configure an Auto Scaling Group 
Test auto scaling and change manual settings</b></h5>

<h4><b>
a. Launch instance and connect to web service - </b></h4>

In the AWS console search bar, type EC2  and select it. Then click EC2 Dashboard at the top of the left menu. 
Press the Launch instance button and select Launch instance from the menu.  

In Name, put the value Web server for custom AMI. And check the default setting in Amazon Machine Image below. 
Select t2.micro in Instance Type.  

For Key pair, choose Proceed without a key pair.  

Click the Edit button in Network settings to set the space where EC2 will be located. 
And choose the VPC-Lab-vpc created in the previous lab, and for the subnet, choose public subnet. Auto-assign public IP is set to Enable. 

<h4><b>b. Create Security groups - </b></h4>To act as a network firewall. Security groups will specify the protocols and addresses you want to allow in your firewall policy. 

For the security group you are currently creating, this is the rule that applies to the EC2 that will be created. 
After entering Multi-Tier in Security group name and Description, select Add Security group rule and set HTTP to Type.
Also allow TCP/80 for Web Service by specifying it. Select My IP in the source. 
All other values accept the default values, expand by clicking on the Advanced Details tab at the bottom of the screen. 
Click the Meta Data version dropdown and select V2 only (token required) 
Wait for the instance's Instance state result to be Running. Open a new web browser tab and enter the Public DNS or IPv4 Public IP of your EC2 instance in the URL address field. If the page is displayed as shown below, the web server instance is configured normally. 

<h4><b>c. Create a custom AMI - </b></h4>

In the EC2 console, select the instance that we made earlier in this lab, and click Actions > Image and templates > Create Image. 
 
In the Create Image console, type as shown below and press Create image to create the custom image.   

Verify in the console that the image creation request in completed.  

In the left navigation panel, Click the AMIs button located under IMAGES. You can see that the Status of the AMI that you just created. 
It will show either Pending or Available. 

<h4><b>d. Terminate the instance </b></h4>

Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created. 
Therefore, the EC2 instance currently running is no longer needed, so let's try to terminate it. ( In Deploy auto scaling web service, 
we will use custom AMI to create a new web server.) 
  

NOTE - Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created. Therefore, the EC2 instance currently running is no longer needed, so let's try to terminate it. ( In Deploy auto scaling web service, we will use custom AMI to create a new web server.)

<h3>Reference to Golden AMI - https://medium.com/@chiragdarji/aws-golden-ami-e85a24f15f9c</h3>

Architecture Configured till now - 

<img width="626" alt="Ec2 Architecture with IG" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/2db11c8c-50d6-48bf-8aad-52f7601783c1">

<h3>e. Lauch Application Load Balancer </h3>

Using the network infrastructure created in the Network- Amazon VPC lab, we will deploy a web service that can 
automatically scale out/in under load and ensure high availability. We use the web server AMI created in 
the previous chapter and the network infrastructure named VPC-Lab. 

<h3>f. Configure Application Load Balancer </h3>

AWS Elastic Load Balancer supports three types of load balancers: Application Load Balancer, Network Load Balancer, and Classic Load Balancer. 

In this lab, you will configure and set up the Application Load Balancer to handle load balancing HTTP requests. 
From the EC2 Management Console in the left navigation panel, click Load Balancers under Load Balancing. 
Then click Create Load Balancer. In the Select load balancer type, click the Create button under Application Load Balancer.
Name the load balancer. In this case, name Name as Web-ALB. Leave the other settings at their default values. 

 Scrolling down a little bit, there is a section for selecting availability zones. 
 First, Select the VPC-Lab-vpc created previously. For Availability Zones select the 2 public subnets that were created previously. 
 This should be Public Subnet for ap-northeast-2a and Public Subnet C for us-east-1c. 

 In the Security groups section, click the Create new security group hyperlink. Enter web-ALB-SG as the security group name 
 and check the VPC information. Click the Add rule button and select HTTP as the Type and Anywhere-IPv4 as the Source. 
 And create a security group. 

 Return to the load balancer page again, click the refresh button, and select the web-ALB-SG you just created. 
 Remove the default security group. 
In Listeners and routing column, click Create target group. Put Web-TG for Target group name and check all settings 
same with the screen below. After that click Next button. 
 
This is where we would register our instances. However, as we mentioned earlier, there are not instances to register at this moment. 
Click Create target group. 
Again, move into the Load balancers page, click refresh button and select Web-TG. And then Click Create load balancer. 

<h3>g. Configure Launch Template </h3>

Now that ALB has been created, it's time to place the instances behind the load balancer. 
To configure an Amazon EC2 instance to start with Auto Scaling Group, you can use Launch Template, Launch Configuration, or EC2 Instance. 
In this workshop, we will use the Launch Template to create an Auto Scaling group. 

The launch template configures all parameters within a resource at once, reducing the number of steps required to create an instance. 
Launch templates make it easier to implement best practices with support for Auto Scaling and spot fleets, as well as spot and on-demand instances. 
This helps you manage costs more conveniently, improve security, and minimize the risk of deployment errors. 

The launch template contains information that Amazon EC2 needs to start an instance, such as AMI and instance type. 
The Auto Scaling group refers to this and adds new instances when a scaling out event occurs. 
If you need to change the configuration of the EC2 instance to start in the Auto Scaling group, 
you can create a new version of the launch template and assign it to the Auto Scaling group. 
You can also select a specific version of the launch template that you use to start an EC2 instance in the Auto Scaling group, if necessary. 
You can change this setting at any time. 

<h3>h. Create Security Groups</h3>
Select Security Groups under the Network & Security heading and click Create Security Group in the upper right corner.
modify the Inbound rules. First, select the Add rule button to add the Inbound rules, and select HTTP in the Type. 
For Source, type ALB in the search bar to search for the security group created earlier Web-ALB-SG. 
This will configure the security group to only receive HTTP traffic coming from ALB.
Leave outbound rules' default settings and click Create Security Group to create a new security group. 
 This creates a security group that allows traffic only for HTTP connections (TCP 80) that enter the instance via ALB from the Internet.

<h3>i. Create Launch Template</h3>
In the EC2 console, select Launch Templates from the left navigation panel. Then click Create Launch Template.

First, set Launch template name and Template version description as shown below, and 
select Checkbox for Provide guidance in Auto Scaling guidance. 
Select this checkbox to enable the template you create to be utilized by Amazon EC2 Auto Scaling.

In Amazon Machine Image(AMI), set the AMI to Web Server v1, which was created in the previous EC2 lab. 
You can find it by typing Web Server v1 in the search section, or you can scroll down to find it in the My AMI section. 
Next, select t2.micro for the instance type. We are not going to configure SSH access because this is only for Web service server. 
Therefore, we do not use key pairs.
Leave the other parts as default. Let's take a look at the Network Settings section. 
First, in Networking platform select Virtual Private Cloud(VPC). 
In security group section, find and apply ASG-Web-Inst-SG created before.
Follow the Storage's default values without any additional change. Go down and define the Instance tags. 
Click Add tag and Name for Key and Web Instance for Value. Select Resource types as Instances and Volumes.
Finally, in the Advanced details tab, set the IAM instance profile to SSMInstanceProfile. 
 Leave all other settings as default, and click the Create launch template button at the bottom right to create a launch template.
<h3>j. Set Auto Scaling Group</h3>
Enter the EC2 console and select Auto Scaling Groups at the bottom of the left navigation panel. 
Then click the Create Auto Scaling group button to create an Auto Scaling Group.
In [Step 1: Choose launch template or configuration], specify the name of the Auto Scaling group. 
In this workshop, we will designate it as Web-ASG. Then select the launch template that you just created named Web. 
The default settings for the launch template will be displayed. Confirm and click the lower right Next button.
Set the network configuration with the Purging options and instance types as default. Choose VPC-Lab-vpc for VPC, 
 select Private subnet 1 and Private subnet 2 for Subnets. When the setup is completed, click the Next button.
Next, proceed to set up load balancing. First, select Attach to an existing load balancer. 
Then in Choose a target group for your load balancer, select Web-TG created during in ALB creation.
At the Monitoring, select Check box for Enable group metrics collection within CloudWatch. 
This allows CloudWatch to see the group metrics that can determine the status of Auto Scaling groups. Click the Next button at the bottom right.
In the step of Configure group size and scaling policies, set scaling policy for Auto Scaling Group. 
In the Group size column, specify Desired capacity and Minimum capacity as 2 and Maximum capacity as 4.
Keep the number of the instances to 2 as usual, and allow scaling of at least 2 and up to 4 depending on the policy.
In the Scaling policies section, select Target tracking scaling policy and type 30 in Target value.
This is a scaling policy for adjusting the number of instances based on the CPU average utilization remaining at 30% overall.
Leave all other settings as default and click the Next button in the lower right corner.
Click the Next button to move to the next step. In the Add tags step, we will simply assign name tag. 
Click Add tag, type Name in Key, ASG-Web-Instance in Value, and then click Next.
Now we are in the final stage of review. After checking the all settings, click the Create Auto Scaling Group
button at the bottom right.
Auto Scaling group has been created.
High available and automatically scales Load Architecture
 <img width="626" alt="HA Load Balancing arch" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/5681516e-194f-49a1-abed-a9f06ba56ec2">
<h3> CHECK WEB SERVICE AND TEST </h3>
   <p>First, let's check whether you can access the website normally and whether the load balancer works,
    and then load the web server to see if Auto Scaling works.
 i. Check Web Service and load balancer
 To access through the Application Load Balancer configured for the web service, click the Load Balancers 
    menu in the EC2 console and select the Web-ALB you created earlier. Copy DNS name from the basic configuration.
 ii. Open a new tab in your web browser and paste the copied DNS name. You can see that web service 
    is working as shown below. For the figure below, you can see that the web instance placed in 
    us-east-1a is running this web page.
 iii. If you click the refresh button here, you can see that the host serving the web page has been 
    replaced with an instance of another availability zone area (us-east-1c) as shown below. 
    This is because routing algorithms in ALB target groups behave Round Robin by default.
 iv. Currently, in the the Auto Scaling group, scaling policy's baseline has been set to 30% CPU utilization for each instance.
- If the average CPU utilization of an instance is less than 30%, Reduce the number of instances.
- If the average CPU utilization of an instance is over 30%, Additional instances will be deployed, 
    load will be distributed, and adjusted to ensure that the average CPU utilization of the instances is 30%.
 v. Now, let's test load to see whether Auto Scaling works well. On the web page above, click the 
    LOAD TEST menu. The web page changes and the applied load is visible. Click on the logo at the 
    top left of the page to see that each instance is under load.
  <b> NOTE-: The principle that causes CPU load is that when the CPU Idle value is over 50, the PHP code 
 operates every five seconds to create, compress, and decompress arbitrary files. Traffic is distributed 
 and operated by the ALB, so the load is applied to other instances continuously.</b>
vi. Enter Auto Scaling Groups from the left side menu of the EC2 console and click the Monitoring tab. 
    Under Enabled metrics, click EC2 and set the right time frame to 1 hour. If you wait for a few seconds, 
    you'll see the CPU Utilization (Percent) graph changes.
vii. Wait for about 5 minutes (300 seconds) and click the Activity tab to see the additional EC2 instances deployed according to the scaling policy.
viii. When you click on the Instance management tab, you can see that two additional instances have sprung up and a total of four are up and running.
ix. If you use the ALB DNS that you copied earlier to access and refresh the web page, you can see that it is
    hosting the web page in two instances that were not there before. The current CPU load is 0% because it is
    a new instance. It can also be seen that each of them was created in a different availability zone. 
    If it's not 0%, it can look more than 100% because it's a constant load situation.
</pre>
<pre>
<h3>C. DATABASE - AMAZON AURORA</h3>
 Among the many database options available on AWS, Amazon RDS(Relational Database Service) is a cloud-based 
 database service that is easy to configure and operate and easy to scale. Amazon RDS is cost-effective, easy
 to adjust capacity, and reduces time-consuming management tasks, 
 allowing users to focus more on their business and applications.
 In this database lab, we will deploy RDS Aurora instance in VPC-Lab and configure the web service(Apache + PHP) 
 in Auto Scaling Group to use RDS Aurora(MySQL). When the connection to the database is completed, create a new 
 version of the existing custom AMI and update the Auto Scaling Group to use the new version of AMI. In addtion,
 we conduct a test to add/modify/delete contacts in a simple address book stored in RDS' DB through the web browser.

 <h3>Architecture - </h3>
 <img width="347" alt="Aurora Db Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/8264335c-badc-443f-8362-44575560b6e4">
 <h4>
Following Hands-on for Database Section - 
1. Create VPC security group
2. Create RDS instance
3. Connect RDS with Web App server
4. Access RDS from EC2
5. (option) RDS Management Features
6. (option) Connect RDS Aurora
 </h4>
 <h4>1. Create VPC Security group
 NOTE- The RDS service uses the same security model as EC2. The most common usage format is to provide data as 
  a database server to an EC2 instance operating as an applicatiojn server within the same VPC, or to configure
  it to be accessible to the DB Application client outside of the VPC. The VPC Security Group must be applied 
  for proper access control.</h4>
  <h4>The RDS service uses the same security model as EC2. The most common usage format is to provide data as a
  database server to an EC2 instance operating as an applicatiojn server within the same VPC, or to configure
  it to be accessible to the DB Application client outside of the VPC. The VPC Security Group must be applied
  for proper access control.</h4>
 <ol>
<li> On the left side of the VPC dashboard, select Security Groups and then select Create Security Group.</li>
<li>Enter Security group name and Description as shown below. Choose the VPC that was created in the first lab. It should be named VPC-Lab.</li>
<li>Scroll down to the Inbound rules column. Click Add rule to create a security group policy that allows access to RDS from the EC2 Web servers that you previously created through the Auto Scaling Group. Under Type, select MySQL/Aurora The port range should default to 3306. The protocol and port ranges are automatically specified. The Source type entry can specify the IP band (CIDR) that you want to allow acces to, or other security groups that the EC2 instances to access are already using. Select the security group(named ASG-Web-Inst-SG ) that is applied to the web instances of the Auto Scaling group in the Compute - Amazon EC2</li>
<li>When settings are completed, click Create Security Group at the bottom of the list to create this security group.</li>
</ol>
 <h3>2. Create RDS Instance </h3>
        <h4>Create an instance of RDS Aurora (MySQL compatible).</h4>
<ol style="list-style-type:lower-alpha">
<li>In the AWS Management console, go to the RDS (Relational Database Service)</li>
<li>Select Create Database in dashboard to start creating a RDS instance.</li>
<li>Select the RDS instances' database engine. In Amazon RDS, you can select the 
 database engine based on open source or commercial database engine. 
 In this lab, we will use Amazon Aurora with MySQL-compliant database engine. 
 Select Standard Create in the choose a database creation method section. 
 Set Engine type to Amazon Aurora, Set Edition to Amazon Aurora with MySQL compatibility, 
 Set Capacity type to Provisioned and Version to Aurora (MySQL 5.7) 2.10.2.></li>
<li>Select Production in Template. Under Settings, we want to specify administrator 
 information for identifying the RDS instances. Enter the information as it appears below.</li>
<li>Under DB instance size select Memory Optimized class. Under Availability & durability 
 select Create an Aurora Replica or reader node in a different AZ. Select db.r5.large for instance type.</li>
<li>Set up network and security on the Connectivity page. Select the VPC-Lab that 
 you created earlier in the Virtual private cloud (VPC) and specify the subnet that 
 the RDS instance will be placed in, public access, and security groups. 
 Enter the information as it appears below.</li>
<li>Scroll down and click Additional configuration. Set database options as shown below. 
 Be aware of the uppercase and lowercase letters of Initial database name.</li>
<li>Subsequent items such as Backup, Entry, Backtrack, Monitoring, and Log exports all 
 accept the default values, and press Create database to create a database.</li>
<li>A new RDS instance is now creating. This may take more than 5 minutes. 
 You can use an RDS instance when the DB instance's status changed to</li>
</ol>
<h4><b>Services Configured so far </b></h4>
<img width="521" alt="DB arch configured" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/d279ffda-dc6d-43ce-a33f-b7758bc85ace">
 <h3>Connect RDS with Web App Server</h3>
 <h4>NOTE: The Web Server instance that you created in the previous computer 
  lab contains code that generates a simple address book to RDS. 
  The Endpoint URL of the RDS must be verified first in order to 
  use the RDS on the EC2 Web Server.</h4>

<h3>Storing RDS Credentials in AWS Secrets Manager</h3>
<h4>
The web server we built includes sample code for our address book. 
In this lab, you specify which database to use in the sample code and how to connect it. 
We will store that information in AWS Secrets Manager.</h4>
<h4>
We will create a secret containing data connection information. Later, we will give the web server the appropriate permission to retrieve the secret.
<ol style="list-style-type:lower-alpha">
<li>In the console window, open AWS Secrets Manager (https://console.aws.amazon.com/secretsmanager/ ) and
click the Store a new secret button.</li>
<li>Under Secret Type, choose Credentials for Amazon RDS database. 
Write down the user name and password you entered when creating the database. And under Database select the database you just created. 
Then click the Next button.</li>
<li>Name your secret, mysecret. The sample code is written to ask for the secret by this specific name. Click Next.</li>
<li>Leave Secret rotation at default values. Click Next.</li>
<li>Review your choices. Click Store.</li>
<li>You can check the list of secret values with the name mysecret as shown below.</li>
<li>Click mysecret hyperlink and find Secret value tab. And click Retrieve secret value button.</li>
<li>Click Edit button, and check whether there is dbname
and immersionday in key/value section. If they were not, 
click Add button, fill out the value and click save button.</li>
</ol>
</h4>
<h3>Access RDS from EC2</h3>
 <h4>Note: Now that you have created a secret, you must give 
 your web server permission to use it. To do this, we will 
 create a Policy that allows the web server to read a secret. 
 We will add this policy to the Role you previously assigned 
 to the web server.</h4>
<h3> Allow the web service to access the Internet</h3>
<ol>
 <li>Sign in to the AWS Management Console and open the IAM console. 
  In the navigation pane, choose Policies, and then choose Create Policy.</li>
<li>Choose the service. Type Secret Manager into the search box. Click Secret manager.</li>
<li>Under Access Level, Click on the carat next to read anf then check the box by GetSecretValue</li>
<li>Click on the carat next to the resource. For this Lab, Sselect all Resources. Click Next: Tags.
(NOTE: For the lab, we're allowing EC2 to access all secrets. 
With a real workload, you should consider allowing access to specific secrets.)</li>
<li>Click Next: Review. On the Review Policy screen, give your 
 new policy the name ReadSecrets. Click Create policy. </li>
<li>In the navigation pane, choose Roles and type SSMInstanceProfile 
into the search box. This is the role you created previously 
in Connect to your Linux instance using Session Manager. 
Click SSMInstanceProfile.</li>
<li>Under Permission Policies, Attach Policies</li>
<li>Search for the policy you created called ReadSecrets. 
Check the box and click Attach policy.</li>
<li>Under Permissions policies, verify that AmazonSSMManagedInstanceCore
and ReadSecrets are both listed.</li>
</ol>
 <h3> Try Address Book</h3>
 <ol>
 <li>Access the [EC2 Console] (https://console.aws.amazon.com/ec2/v2/home?instanceState=running ) 
 window and click load balancer. After copying the DNS name of the load balancer 
 created in the compute lab, open a new tab in your browser and paste it.</li>
<li>After connecting to the web server, go to the RDS tab.</li>
  <li>Now you can check the data in the database you created.</li>
 </ol>
 <h3>RDS Managment Features</h3>
 <h4>NOTE:- In multiple AZ deployments, Amazon RDS automatically provisions and maintains 
  synchronous spare replicas in different availability zone. The default DB instance is 
  synchronized from the availability zone to the spare replica to provide data redundancy.
</h4>
<h3> RDS FAILOVER Tests</h3>
<h4>NOTE:- When multiple AZs are enabled, Amazon RDS automatically switches to a spare replica 
 in another availability zone if the DB instance has a planned or unplanned outage. The amount of time 
 that failover takes to complete depends on the database activity and other conditions when the default
 DB instance becomes unavailable. The time required for failover is typically 60-120 seconds. However, 
 if the transaction is large or the recovery process is complex, the time required for failover can be increased. 
 When failover is complete, the RDS console UI takes additional time to reflect in the new availability zone.
</h4>
<ol>
 <li>From the RDS management console, select Databases, select the instance that you want to proceed with the failover,
  and click Failover in the task menu</li>
 <li>A message asking whether you're going to failover the rdscluster. Press the Failover button.</li>
 <li>The refresh button changes the status of rdscluster in the DB identifier to Failing-over.
  In a few minutes, press the Refresh button to see Reader and Writer roles changed. The failover is complete.</li>
</ol> 
<h4> Create RDS Snapshot - Snapshot can be created at any frequency for backup to 
 database instances, and the database can be restored at any time based on the snapshots created.</h4>
<ol>
 <li>From the RDS management console, select Databases, and Select the instance on which you want to perform 
  the snapshot operation. Select Actions > Take snapshot in the upper right corner.</li>
 <li>Type the name you want to use for the snapshot as immersionday-snapshot. 
  Press the Take Snapshot button to complete the creation.</li>
 <li>From the left RDS menu, select Snapshots and check the creation status of the snapshot.
  The state of the snapshot is the first creating state, and you can use that snapshot to restore 
  the database when state become available. To restore, select the snapshot and select Actions to 
  see what you can do with that snapshot. Restore Snapshot allows you to create RDS instances with
  the same data based on snapshots taken. This lab will not perform a restore.</li>
</ol>
 <h3> Change RDS INSTANCE type</h3>
 <h4>Scale-Up/Scale-Down of RDS instances can be done very simply 
  through the RDS Management Console.
</h4>
 <ol>
  <li>Let's change the specification of the RDS instance by selecting the instance you want 
   to change and clicking Modify. </li>
  <li>You can select the specification of the instance that you want to change by selecting 
   the list box of instance classes. Let's choose db.r6g.large here.</li>
  <li>Scroll to the bottom and select Continue to go to the page 
   where you check the instance's current value and new value 
   and select when to apply.</li>
  <li>Select Apply immediately. In this case, RDS changes its instance immediately after perform a back up task. 
   Then click Modify DB Instance. Depending on the type of instance and the amount of data to back up, 
   it can take several minutes. Therefore, you should expect a certain amount of downtime for RDS services
   (Redundant configuration minimizes downtime).</li>
  <h4>NOTE:- In case of selecting Apply during the next scheduled maintenance window, make the change in the 
   user's Maintenance Window, which is specified on a weekly basis.</h4>
  <li>You can see that the status of the instance has changed to Modifying.</li>
  <li>When you click refresh button again, you can see that the Writer instance has changed. 
   This is because the instance you selected earlier for the size change was the Writer instance.
   RDS minimizes downtime through failover before resizing. If you wait a moment, you will see that the 
   change to Available status has been completed as shown below.</li>
   </ol>
 <h4>RDS can change the size of the instance at any time. However, the size of the database does not support shrink after scaling up.
</h4>
<h3>Coneect RDS Aurora</h3>
 <h5>
1. Create an EC2 instance with the AMI created in Public Subnet within the VPC-Lab. The networking option should allow Public IP.
2. Changes the security group settings for RDS Aurora. Configure the newly created EC2 instance to accept security groups as sources.
3. Log in to the EC2 instance you just created with SSH, and connect to RDS Aurora through the MySQL Client. 
   The EC2 web server already has MySQL client installed during EC2 deployment.</h5>
 <h4>Once the setup is successful, you can connect to the CLI environment and perform mysql commands as shown below.</h4>
 <pre>
  $ ssh -i AWS-ImmersionDay-Lab.pem ec2-user@”EC2 Host FQDN or IP”
Last login: Sun Feb 18 14:41:59 2018 from 112.148.83.236

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2017.09-release-notes/


$ mysql -u awsuser -pawspassword -h awsdb.ccjlcjlrtga1.ap-northeast-2.rds.amazonaws.com

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 34
Server version: 5.6.10 MySQL Community Server (GPL)


Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| immersionday       |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.01 sec)

mysql> use immersionday;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+------------------------+
| Tables_in_immersionday |
+------------------------+
| address                |
+------------------------+
1 row in set (0.01 sec)

mysql> select * from address;
+----+-------+--------------+---------------------+
| id | name  | phone        | email               |
+----+-------+--------------+---------------------+
|  1 | Bob   | 630-555-1254 | bob@fakeaddress.com |
|  2 | Alice | 571-555-4875 | alice@address2.us   |
+----+-------+--------------+---------------------+
2 rows in set (0.00 sec)

mysql>

 </pre>
</pre>

<pre>
 <h3>D. Storage - Amazon S3</h3>
 <h4>Amazon Simple Storage Service (S3) provides a web service-based interface that simplifies data processing anytime, anywhere.
Outline - This lab is designed to help users learn how to save, check, move, and delete data using S3. 
You can also see the ability to host simple static web pages using S3's static website hosting functionality.
</h4>
<h3>Architecture</h3>
<img width="522" alt="S3 Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e67849bb-4ea3-4c75-8c67-a336830fdbd1">

 <h4>Create Bucket on S3</h4>
 <ol>
  <li>From the AWS Management Console, connect to S3 . Press Create bucket to create a bucket.</li>
 <li>Enter a unique bucket name in the Bucket name field. For this lab, type immersion-day-user_name, 
  substituiting user-name with your name. All bucket names in Amazon S3 have to be unique and cannot be duplicated. 
  In the Region drop-down box, specify the region to create the bucket. In this lab, select the region closest to you. 
  The images will show the Asia Pacific (Seoul) region. Object Ownership change to ACLs enabled. 
  Bucket settings for Block Public Access use default values, and select Create bucket in the lower right corner.</li>

<h4>Note - Bucket names must comply with these rules:
Can contain lowercase letters, numbers, dots (.), and dashes (-).
Must start with a number or letter.
Can be specified from a minimum of 3 to a maximum of 255 characters in length.
Cannot be specified in the format like the IP address (e.g., 265.255.5.4).</h4>
<li>A bucket has been created on Amazon S3.</li>
</ol>

NOTE - There are no costs incurred for creating bucket. You pay for storing objects in your S3 buckets. 
 The rate you’re charged depends on the region you are using, your objects' size, how long you stored the 
 objects during the month, and the storage class. There are also per-request fees. Click for more information 

 <h4>Adding Objects to Bucket</h4>
This lab hosts static websites through S3. The static website serves as a redirect to an instance 
created by the VPC Lab when you click on a particular image. Therefore, prepare one image file, 
one HTML file, and an ALB DNS name.
 <ol>
  <li>Download the image file <a href="https://static.us-east-1.prod.workshops.aws/public/dd38a0a0-ae47-43f1-9065-f0bbcb15f684/static/common/s3_advanced_lab/aws.png">aws.png</a>and save it as aws.png.</li>
 <li>Write index.html using the source code below.</li>

  <img width="602" alt="S3 index file" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/1bf194fd-180d-4224-a7da-964d992376af">
 
 <li>Upload the aws.png file to S3. Click S3 Bucket that you just created</li>
 <li>Click the Upload button. Then click the Add files button. Select the pre-downloaded aws.png 
  file through File Explorer. Alternatively, place the file in Drag and Drop to the screen.</li>
  <li>Check the file information named aws.png to upload, 
   then click the Upload button at the bottom.</li>
   <li>Check the URL information to fill in the image URL in index.html file. 
    Select the uploaded aws.png file and copy the Object URL information from the details on the right.</li>
   <li>Paste Object URL into the image URL part of the index.html. 
     Then specify the ALB DNS Name of the load balancer created by Deploy auto scaling web service 
     to redirect to ALB when you click on the image.</li>
   <li>Upload the index.html file to S3 following the same instructions as you did to upload the image.</li> 
 </ol>

 NOTE - By default, all objects in the S3 bucket are owner-only(Private). To determine the object through 
 a URL of the same format as https://{Bucket}.s3.{region}.amazonaws.com/{Object}, you must grant Read permission for external users to read it. Alternatively, you can create a signature-based Signed URL that contains credentials for that object, allowing unauthorized users to access it temporarily.

<ol>
 VIEW OBJECTS
 <li>select the Permissions tab in the bucket. To modify the application of Block public access (bucket settings), press the right Edit button.</li>
 <li>Uncheck box and press the Save changes button</li>
 <li>Enter confirm in the bucket's Edit Block public access pop up window and press the Confirm button.</li>
 <li>Click the Objects tab, select the uploaded files, click the Action drop-down button, and press the Make public button to set them to public.</li>
 <li>When the confirmation window pops up, press the Make public button again to confirm.</li>
 <li>Return to the bucket page, select index.html, and click the Object URL link in the Show Details entry.</li>
 <li>When you access the HTML object file object URL, the following screen is printed.</li>
</ol>
<img width="464" alt="AWS EC2 S3 image" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/cb630570-6c7d-475e-bfea-7d43c9d66304">

</pre>
