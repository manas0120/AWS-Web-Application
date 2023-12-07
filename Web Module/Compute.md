B. Compute - Amazon EC2 </b></h3>
Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. 
It is designed to make web-scale cloud computing easier for developers. Amazon EC2’s simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon’s proven computing environment.
<h3><b>EC2 Architecture - </b></h3>Auto Scaling Group to deploy web service instances to private subnets in VPC that we created earlier in this network lab. This configures the highly available web services so that external users can access the Sample Web Page through the web.
<br>
<img width="554" alt="EC2 Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/05c12a41-1634-4b3c-a2c8-577a1306d72e">
<br>
<h5><b>The following hands-on are to be done -:
<ol type="a">
<li>Launch web server instances and execute user data.</li>
<li>Set up a security group.</li> 
<li>Create a custom Amazon Machine Image (AMI).</li>
<li>Launch an Application Load Balancer (ALB).</li>
<li>Configure a Launch Template.</li>
<li>Configure an Auto Scaling Group.</li>
<li>Test auto scaling and change manual settings.</li>
</ol></b></h5>
<h4><b>
a. Launch instance and connect to web service - </b></h4>
<ol>
<li>In the AWS console search bar, type EC2  and select it. Then click EC2 Dashboard at the top of the left menu. Press the Launch instance button and select Launch instance from the menu.</li>
<li>In Name, put the value Web server for custom AMI. And check the default setting in Amazon Machine Image below.</li>
  
<img width="427" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/0d4cda27-93e9-463e-8fd3-692fb71079f9">

<li>Select t2.micro in Instance Type. For Key pair, choose Proceed without a key pair.  
Click the Edit button in Network settings to set the space where EC2 will be located. 
And choose the VPC-Lab-vpc created in the previous lab, and for the subnet, choose public subnet.</li>

<img width="409" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/26fc4866-29e7-407b-b17d-109d4ee3e299">

<li>Auto-assign public IP is set to Enable.</li> 
<h4><b>b. Create Security groups - </b></h4>
<li>To act as a network firewall. Security groups will specify the protocols and addresses you want to allow in your firewall policy. 
For the security group you are currently creating, this is the rule that applies to the EC2 that will be created. 
After entering Multi-Tier in Security group name and Description, select Add Security group rule and set HTTP to Type.
Also allow TCP/80 for Web Service by specifying it. Select My IP in the source.</li>

<img width="326" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/53a781ea-2e84-4592-8d47-0cd088d5d9f0">

<li>All other values accept the default values, expand by clicking on the Advanced Details tab at the bottom of the screen. </li>

<img width="391" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/014952f7-5517-45f8-b567-a52e64f04708">

<li>Click the Meta Data version dropdown and select V2 only (token required)
Information indicating that the instance creation is in progress is displayed on the screen.</li>

<img width="320" alt="5" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/092ce1ed-b70d-4ee5-ae43-135788c50ba6">

<li> You can view the list of EC2 instances by selecting View Instances in the lower right corner.</li>
After the instance configuration is complete, you can check the Availability Zone in which the instance is running, and externally accessible IP and DNS information.</li>

<img width="819" alt="6" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/297cc731-e07d-4ca2-a33e-8de99de54828">

<li>Wait for the instance's Instance state result to be Running. Open a new web browser tab and enter the Public DNS or IPv4 Public IP of your EC2 instance in the URL address field. If the page is displayed as shown below, the web server instance is configured normally. </li>

<img width="552" alt="7" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/db25fe4f-e3f0-473a-99cc-1dc390f40769">

<h2> IMPORTANT NOTE - In the database lab to be followed, connect to RDS using the IAM role granted to the web server - https://catalog.workshops.aws/general-immersionday/ko-KR/basic-modules/10-ec2/ec2-linux/3-ec2-1</h2>
</ol>

<h4><b>c. Create a custom AMI - </b></h4>
<ol>
<li>In the EC2 console, select the instance that we made earlier in this lab, and click Actions > Image and templates > Create Image.</li> 
<li>In the Create Image console, type as shown below and press Create image to create the custom image.</li>

<img width="707" alt="8" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/054095fa-b54f-4d9f-9135-70c593ae59c5">

<li>Verify in the console that the image creation request in completed.</li> 
<li>In the left navigation panel, Click the AMIs button located under IMAGES. You can see that the Status of the AMI that you just created.
It will show either Pending or Available. </li>

<img width="796" alt="9" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/45e42788-35ff-4159-a965-abbae013998b">
</ol>
<h4><b>d. Terminate the instance </b></h4>
Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created. Therefore, the EC2 instance currently running is no longer needed, so let's try to terminate it. ( In Deploy auto scaling web service, we will use custom AMI to create a new web server.) 
NOTE - Custom AMI (Golden Image) creation has been completed for the auto scaling by using the EC2 instance you just created. Therefore, the EC2 instance currently running is no longer needed, so let's try to terminate it. ( In Deploy auto scaling web service, we will use custom AMI to create a new web server.)
<h3>Reference to Golden AMI - https://medium.com/@chiragdarji/aws-golden-ami-e85a24f15f9c</h3>

<img width="470" alt="10" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/c99a839d-e72e-41ef-9468-d841565b6040">

Architecture Configured till now - 
<img width="626" alt="Ec2 Architecture with IG" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/2db11c8c-50d6-48bf-8aad-52f7601783c1">
<h3>e. Lauch Application Load Balancer </h3>
Using the network infrastructure created in the Network- Amazon VPC lab, we will deploy a web service that can automatically scale out/in under load and ensure high availability. We use the web server AMI created in 
the previous chapter and the network infrastructure named VPC-Lab. 
<h3>f. Configure Application Load Balancer </h3>
AWS Elastic Load Balancer supports three types of load balancers: Application Load Balancer, Network Load Balancer, and Classic Load Balancer. 
In this lab, you will configure and set up the Application Load Balancer to handle load balancing HTTP requests. 
<ol>
<li>From the EC2 Management Console in the left navigation panel, click Load Balancers under Load Balancing. Then click Create Load Balancer. In the Select load balancer type, click the Create button under Application Load Balancer.</li>

<img width="317" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/ec1533ee-cbaf-4358-be84-e19fb4addf23">

<li>Name the load balancer. In this case, name Name as Web-ALB. Leave the other settings at their default values. </li>

<img width="374" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/4848dd25-0c43-41ca-8c31-4511c0a7ed9f">

<li>Scrolling down a little bit, there is a section for selecting availability zones. First, Select the VPC-Lab-vpc created previously. For Availability Zones select the 2 public subnets that were created previously. 
This should be Public Subnet for us-east-1a and Public Subnet C for us-east-1c. </li>

<img width="378" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/c93aa10a-b745-44b2-b6be-7750d80410ca">

<li>In the Security groups section, click the Create new security group hyperlink. Enter web-ALB-SG as the security group name, and check the VPC information. Click the Add rule button and select HTTP as the Type and Anywhere-IPv4 as the Source and create a security group.</li>

<img width="598" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/b30d4220-92c0-46dc-9834-1d8fa99deb45">

<li>Return to the load balancer page again, click the refresh button, and select the web-ALB-SG you just created. Remove the default security group.</li>

<img width="598" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/57295480-3355-4bb3-89f1-ae822df7a84f">

<li>In Listeners and routing column, click Create target group. Put Web-TG for Target group name and check all settings same with the screen below. After that click Next button. </li>

<img width="416" alt="5 a" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/00562ca7-3371-410f-aefa-2c145aaf7fcb">

<img width="402" alt="5 b" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/1a704e3d-9fd5-4e66-b7c4-184c39b8816d">

<li>This is where we would register our instances. However, as we mentioned earlier, there are not instances to register at this moment. 
Click Create target group. Again, move into the Load balancers page, click refresh button and select Web-TG. And then Click Create load balancer. </li>

<img width="390" alt="6" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/47443a5c-edee-4315-9ab8-a978279089d5">

<img width="604" alt="7" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/568733e7-8157-42d3-bb28-6975d463e10e">
</ol>
<h3>g. Configure Launch Template </h3>
Now that ALB has been created, it's time to place the instances behind the load balancer. To configure an Amazon EC2 instance to start with Auto Scaling Group, you can use Launch Template, Launch Configuration, or EC2 Instance. 
In this workshop, we will use the Launch Template to create an Auto Scaling group. 

The launch template configures all parameters within a resource at once, reducing the number of steps required to create an instance. 
Launch templates make it easier to implement best practices with support for Auto Scaling and spot fleets, as well as spot and on-demand instances. 
This helps you manage costs more conveniently, improve security, and minimize the risk of deployment errors.

The launch template contains information that Amazon EC2 needs to start an instance, such as AMI and instance type. The Auto Scaling group refers to this and adds new instances when a scaling out event occurs. If you need to change the configuration of the EC2 instance to start in the Auto Scaling group, 
you can create a new version of the launch template and assign it to the Auto Scaling group. You can also select a specific version of the launch template that you use to start an EC2 instance in the Auto Scaling group, if necessary. 
You can change this setting at any time. 

<h3>h. Create Security Groups</h3>
<ol>
<li>Select Security Groups under the Network & Security heading and click Create Security Group in the upper right corner.</li>

<img width="928" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/1776600d-7c76-42e6-9f2d-0b57054fba5f">

<li>Modify the Inbound rules. First, select the Add rule button to add the Inbound rules, and select HTTP in the Type. For Source, type ALB in the search bar to search for the security group created earlier Web-ALB-SG. 
This will configure the security group to only receive HTTP traffic coming from ALB.</li>

<img width="400" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/cd59ddf5-68e1-4579-9598-b8539c0db392">

<li>Leave outbound rules' default settings and click Create Security Group to create a new security group. This creates a security group that allows traffic only for HTTP connections (TCP 80) that enter the instance via ALB from the Internet.</li>
</ol>

<h3>i. Create Launch Template</h3>
<ol>
<li>In the EC2 console, select Launch Templates from the left navigation panel. Then click Create Launch Template.</li>
<li>First, set Launch template name and Template version description as shown below, and select Checkbox for Provide guidance in Auto Scaling guidance. 
Select this checkbox to enable the template you create to be utilized by Amazon EC2 Auto Scaling.</li>

<img width="418" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/b264ca4b-3a4e-4b88-bd61-03389295ad50">
  
<li>In Amazon Machine Image(AMI), set the AMI to Web Server v1, which was created in the previous EC2 lab. You can find it by typing Web Server v1 in the search section, or you can scroll down to find it in the My AMI section. 
Next, select t2.micro for the instance type. We are not going to configure SSH access because this is only for Web service server. 
Therefore, we do not use key pairs.</li>

<img width="380" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/a3424d0f-56ef-48fd-bfd0-b68951cac58d">

<li>Leave the other parts as default. Let's take a look at the Network Settings section. First, in Networking platform select Virtual Private Cloud(VPC). 
In security group section, find and apply ASG-Web-Inst-SG created before.</li>

<img width="410" alt="5" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/bffcecdf-9dea-41de-a769-b5b58d8500d1">

<li>Follow the Storage's default values without any additional change. Go down and define the Instance tags. Click Add tag and Name for Key and Web Instance for Value. Select Resource types as Instances and Volumes.</li>

<img width="387" alt="6" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/8db3e94b-fa46-49cf-bed9-45fe8e450fd0">

<li>Finally, in the Advanced details tab, set the IAM instance profile to SSMInstanceProfile. Leave all other settings as default, and click the Create launch template button at the bottom right to create a launch template.</li>

<img width="390" alt="7" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/13db6c40-615f-4e41-b62f-5acb04ca1731">

<img width="388" alt="8" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/6187ef66-1e00-452e-bab0-2d6603987437">
</ol>

<h3>j. Set Auto Scaling Group</h3>
<ol>
<li>Enter the EC2 console and select Auto Scaling Groups at the bottom of the left navigation panel. Then click the Create Auto Scaling group button to create an Auto Scaling Group.</li>
<li></li>In [Step 1: Choose launch template or configuration], specify the name of the Auto Scaling group. In this workshop, we will designate it as Web-ASG. Then select the launch template that you just created named Web. 
The default settings for the launch template will be displayed. Confirm and click the lower right Next button.</li>

<img width="305" alt="9" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/3bfca7ba-3ba2-4c75-a9e4-d20d19e9e6e3">

<li>Set the network configuration with the Purging options and instance types as default. Choose VPC-Lab-vpc for VPC, select Private subnet 1 and Private subnet 2 for Subnets. When the setup is completed, click the Next button.</li>

<img width="404" alt="10" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/72107da5-5e5b-4e15-b0f2-5a22403a5277">

<li>Next, proceed to set up load balancing. First, select Attach to an existing load balancer. Then in Choose a target group for your load balancer, select Web-TG created during in ALB creation. At the Monitoring, select Check box for Enable group metrics collection within CloudWatch. This allows CloudWatch to see the group metrics that can determine the status of Auto Scaling groups. Click the Next button at the bottom right.</li>

<img width="416" alt="11" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e8bc108d-9d82-4bd2-9960-d6c355703fd1">

<img width="257" alt="12" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/f4394af2-a9a1-4c39-853f-3a05c968cb3b">

<li>In the step of Configure group size and scaling policies, set scaling policy for Auto Scaling Group. In the Group size column, specify Desired capacity and Minimum capacity as 2 and Maximum capacity as 4.
Keep the number of the instances to 2 as usual, and allow scaling of at least 2 and up to 4 depending on the policy. In the Scaling policies section, select Target tracking scaling policy and type 30 in Target value.
This is a scaling policy for adjusting the number of instances based on the CPU average utilization remaining at 30% overall. Leave all other settings as default and click the Next button in the lower right corner.</li>

<li>Click the Next button to move to the next step. In the Add tags step, we will simply assign name tag. Click Add tag, type Name in Key, ASG-Web-Instance in Value, and then click Next.</li>

<img width="409" alt="13" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/ef8624b3-6ebf-4e2f-aa39-0fdc220859dd">

<li>Now we are in the final stage of review. After checking the all settings, click the Create Auto Scaling Group. Button at the bottom right.
Auto Scaling group has been created.</li>
High available and automatically scales Load Architecture
<img width="626" alt="HA Load Balancing arch" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/5681516e-194f-49a1-abed-a9f06ba56ec2">
</ol>
<h3> CHECK WEB SERVICE AND TEST </h3>
<ol>
<p>First, let's check whether you can access the website normally and whether the load balancer works, and then load the web server to see if Auto Scaling works.
 <li>
Check Web Service and load balancer, To access through the Application Load Balancer configured for the web service, click the Load Balancers, menu in the EC2 console and select the Web-ALB you created earlier. Copy DNS name from the basic configuration.</li>

  <img width="955" alt="14" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/39719971-95d9-470d-9ff2-be5e99dbf6e0">
  
<li>Open a new tab in your web browser and paste the copied DNS name. You can see that web service, is working as shown below. For the figure below, you can see that the web instance placed in us-east-1a is running this web page.

<img width="488" alt="15 a" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/991da35a-f751-415c-9d2b-fb7ecfb7b7ba">

  
<li>If you click the refresh button here, you can see that the host serving the web page has been replaced with an instance of another availability zone area (us-east-1c) as shown below. This is because routing algorithms in ALB target groups behave Round Robin by default.</li>

<img width="507" alt="15 b" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/20785fc0-e323-4313-8b3f-dff3ea37c0b6">

<li>Currently, in the the Auto Scaling group, scaling policy's baseline has been set to 30% CPU utilization for each instance.</li>
<ul>
<li>If the average CPU utilization of an instance is less than 30%, Reduce the number of instances.</li>
<li>If the average CPU utilization of an instance is over 30%, Additional instances will be deployed, load will be distributed, and adjusted to ensure that the average CPU utilization of the instances is 30%.</li>
</ul>
<li>Now, let's test load to see whether Auto Scaling works well. On the web page above, click the LOAD TEST menu. The web page changes and the applied load is visible. Click on the logo at the top left of the page to see that each instance is under load.</li>
</ol>

<b> NOTE-: The principle that causes CPU load is that when the CPU Idle value is over 50, the PHP code operates every five seconds to create, compress, and decompress arbitrary files. Traffic is distributed and operated by the ALB, so the load is applied to other instances continuously.</b>
<ul>
<li>Enter Auto Scaling Groups from the left side menu of the EC2 console and click the Monitoring tab. Under Enabled metrics, click EC2 and set the right time frame to 1 hour. If you wait for a few seconds, you'll see the CPU Utilization (Percent) graph changes.</li>
<li>Wait for about 5 minutes (300 seconds) and click the Activity tab to see the additional EC2 instances deployed according to the scaling policy.</li>
<li>When you click on the Instance management tab, you can see that two additional instances have sprung up and a total of four are up and running.</li>
<li>If you use the ALB DNS that you copied earlier to access and refresh the web page, you can see that it is hosting the web page in two instances that were not there before. The current CPU load is 0% because it is a new instance. It can also be seen that each of them was created in a different availability zone. If it's not 0%, it can look more than 100% because it's a constant load situation.</li>
</ul>
