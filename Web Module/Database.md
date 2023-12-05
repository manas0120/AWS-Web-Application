<h3>C. DATABASE - AMAZON AURORA</h3>
Among the many database options available on AWS, Amazon RDS(Relational Database Service) is a cloud-based database service that is easy to configure and operate and easy to scale. Amazon RDS is cost-effective, easy
to adjust capacity, and reduces time-consuming management tasks, 
allowing users to focus more on their business and applications.
<br>
In this database lab, we will deploy RDS Aurora instance in VPC-Lab and configure the web service(Apache + PHP), in Auto Scaling Group to use RDS Aurora(MySQL). When the connection to the database is completed, create a new 
version of the existing custom AMI and update the Auto Scaling Group to use the new version of AMI. In addtion, we conduct a test to add/modify/delete contacts in a simple address book stored in RDS' DB through the web browser.

<br><h3>Architecture - </h3>
<br>
<img width="347" alt="Aurora Db Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/8264335c-badc-443f-8362-44575560b6e4">
<br>
<h4>
<ol>
Following Hands-on for Database Section - 
<li>Create VPC security group</li>
<li>Create RDS instance</li>
<li>Connect RDS with Web App server</li>
<li>Access RDS from EC2</li>
<li>RDS Management Features</li>
<li>(option) Connect RDS Aurora</li>
</ol>
</h4>
<br>
<h4>1. Create VPC Security group
NOTE- The RDS service uses the same security model as EC2. The most common usage format is to provide data as a database server to an EC2 instance operating as an applicatiojn server within the same VPC, or to configure
it to be accessible to the DB Application client outside of the VPC. The VPC Security Group must be applied for proper access control.</h4>
<br>
<h4>In the previous Compute - Amazon EC2 lab, we created web server EC2 instances using Launch Template and Auto Scaling Group. These instances use Launch Template to apply the security group AutoScaling-Web-Inst-SG . Using this information, we will create a security group so that only web server instances within the Auto Scaling Group can access RDS instances.</h4>

<ol>
<li> On the left side of the VPC dashboard, select Security Groups and then select Create Security Group.</li>

<li>Enter Security group name and Description as shown below. Choose the VPC that was created in the first lab. It should be named VPC-Lab.</li>

<img width="654" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e3a79a51-c0d4-4dc3-9d5f-aa1965e73c0c">

<li>Scroll down to the Inbound rules column. Click Add rule to create a security group policy that allows access to RDS from the EC2 Web servers that you previously created through the Auto Scaling Group. Under Type, select MySQL/Aurora The port range should default to 3306. The protocol and port ranges are automatically specified. The Source type entry can specify the IP band (CIDR) that you want to allow acces to, or other security groups that the EC2 instances to access are already using. Select the security group(named AutoScaling-Web-Inst-SG ) that is applied to the web instances of the Auto Scaling group in the Compute - Amazon EC2</li>

<img width="880" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/da92b2f9-bcc5-469b-93c2-a03ab8cab7bd">

<li>When settings are completed, click Create Security Group at the bottom of the list to create this security group.</li>

<img width="897" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/93f5e6e0-7177-401b-aa4b-5f12695da910">
</ol>
<br>
<h3>2. Create RDS Instance </h3>
<h4>Create an instance of RDS Aurora (MySQL compatible).</h4>
<ol style="list-style-type:lower-alpha">
<li>In the AWS Management console, go to the RDS (Relational Database Service)</li>
<li>Select Create Database in dashboard to start creating a RDS instance.</li>
<li>Select the RDS instances' database engine. In Amazon RDS, you can select the database engine based on open source or commercial database engine. 
In this lab, we will use Amazon Aurora with MySQL-compliant database engine. 
Select Standard Create in the choose a database creation method section. 
Set Engine type to Amazon Aurora, Set Edition to Amazon Aurora with MySQL compatibility, Set Capacity type to Provisioned and Version to Aurora (MySQL 5.7) 2.10.2.></li>
 
<img width="550" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/dcd9f331-11ae-40c5-8f75-a91e7e56cae0">
<br>
<img width="460" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/5837c37b-c8b3-4c27-969a-714638a6310c">


<li>Select Production in Template. Under Settings, we want to specify administrator information for identifying the RDS instances. Enter the information as it appears below.</li>

<img width="381" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/769bb2e6-e2a0-45ce-af22-aef5da5ec191">

<li>Under DB instance size select Memory Optimized class. Under Availability & durability select Create an Aurora Replica or reader node in a different AZ. Select db.r5.large for instance type.</li>

<img width="438" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/aaaf5309-9a20-45d4-8c1e-321ec69f2606">

<li>Set up network and security on the Connectivity page. Select the VPC-Lab that you created earlier in the Virtual private cloud (VPC) and specify the subnet that the RDS instance will be placed in, public access, and security groups. Enter the information as it appears below.</li>

<img width="430" alt="5 a" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/c1861ef5-4a0e-41b8-bc0d-efc1ed6e9194">
<br>
<img width="429" alt="5 b" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/7e4f12fd-d8bd-493d-804a-ca788eb0d0b0">

<li>Scroll down and click Additional configuration. Set database options as shown below. Be aware of the uppercase and lowercase letters of Initial database name.</li>

<img width="431" alt="6" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/61812d90-6c79-44b1-b543-5b0bd055474b">

<li>Subsequent items such as Backup, Entry, Backtrack, Monitoring, and Log exports all accept the default values, and press Create database to create a database.</li>
<li>A new RDS instance is now creating. This may take more than 5 minutes. 
 You can use an RDS instance when the DB instance's status changed to Available</li>

<img width="726" alt="7" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/ea7ca7e7-9646-4f0d-a789-d5b18429387c"> 
</ol>

<h4><b>Services Configured so far </b></h4>
<img width="521" alt="DB arch configured" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/d279ffda-dc6d-43ce-a33f-b7758bc85ace">
<br>

<h3>Connect RDS with Web App Server</h3>
<br>
<h4>NOTE: The Web Server instance that you created in the previous computer 
lab contains code that generates a simple address book to RDS. The Endpoint URL of the RDS must be verified first in order to use the RDS on the EC2 Web Server.</h4>
<br>
<h3>Storing RDS Credentials in AWS Secrets Manager</h3>
<br>
<h4>
The web server we built includes sample code for our address book. 
In this lab, you specify which database to use in the sample code and how to connect it. We will store that information in AWS Secrets Manager.
We will create a secret containing data connection information. Later, we will give the web server the appropriate permission to retrieve the secret.</h4>

<ol style="list-style-type:lower-alpha">
<li>In the console window, open AWS Secrets Manager https://console.aws.amazon.com/secretsmanager/ ) and
click the Store a new secret button.</li>

<li>Under Secret Type, choose Credentials for Amazon RDS database. 
Write down the user name and password you entered when creating the database. And under Database select the database you just created. Then click the Next button.</li>

<img width="421" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e8d09f51-354a-4928-949c-6a0adacfaa5b">

<li>Name your secret, mysecret. The sample code is written to ask for the secret by this specific name. Click Next.</li>

<img width="723" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/3752ca0c-30a0-4d99-a2ca-5448b0e0f8fd">

<li>Leave Secret rotation at default values. Click Next.</li>

<img width="413" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/d99c3c3f-b244-4d7a-9f43-c45dff423d7a">

<li>Review your choices. Click Store.</li>

<img width="484" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/f2ed8248-0f1e-4e16-a4f7-4c6aac64860f">

<li>You can check the list of secret values with the name mysecret as shown below.</li>

<img width="917" alt="5" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/7ebe91dc-7dcf-426b-abfe-82fd61755eea">

<li>Click mysecret hyperlink and find Secret value tab. And click Retrieve secret value button.</li>


<li>Click Edit button, and check whether there is dbname and MultiTierWB in key/value section. If they were not, click Add button, fill out the value and click save button.</li>

<img width="410" alt="6" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/24fe328a-f9ec-4799-9ab3-da12c032a195">
</ol>
</h4>
<h3>Access RDS from EC2</h3>
 <h4>Note: Now that you have created a secret, you must give 
 your web server permission to use it. To do this, we will 
 create a Policy that allows the web server to read a secret. 
 We will add this policy to the Role you previously assigned 
 to the web server.</h4>
 <br>
<h3> Allow the web service to access the secret</h3>
<ol>
 <li>Sign in to the AWS Management Console and open the IAM console. 
  In the navigation pane, choose Policies, and then choose Create Policy.</li>
<li>Choose the service. Type Secret Manager into the search box. Click Secret manager.</li>
<li>Under Access Level, Click on the carat next to read anf then check the box by GetSecretValue</li>

 <img width="698" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/df3cde6e-bebe-41a5-a3b3-43d54027c45a">

<li>Click on the carat next to the resource. For this Lab, Sselect all Resources. Click Next: Tags.
(NOTE: For the lab, we're allowing EC2 to access all secrets. 
With a real workload, you should consider allowing access to specific secrets.)</li>

<img width="685" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/dded8d34-1545-4eff-a59e-78f8c2c103d5">

<li>Click Next: Review. On the Review Policy screen, give your 
 new policy the name ReadSecrets. Click Create policy. </li>

<img width="713" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/5fa97116-f761-4ef4-8941-cdf38a0c4dec">

<li>In the navigation pane, choose Roles and type SSMInstanceProfile 
into the search box. This is the role you created previously 
in Connect to your Linux instance using Session Manager. 
Click SSMInstanceProfile.</li>

<img width="761" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/eab9027b-c0bf-4ef3-8627-ec545053786f">

<li>Under Permission Policies, Attach Policies</li>

<img width="656" alt="5" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/01525414-0c0d-447f-98fc-1c20e489cd6e">

<li>Search for the policy you created called ReadSecrets. 
Check the box and click Attach policy.</li>

<img width="877" alt="6" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e1c48487-c03a-406b-96f5-762ddd847336">

<li>Under Permissions policies, verify that AmazonSSMManagedInstanceCore
and ReadSecrets are both listed.</li>

<img width="752" alt="7" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/3b211238-3b3e-431a-9f9d-e0ce9316cc4a">

</ol>
 <h3> Try Address Book</h3>
 <ol>
 <li>Access the [EC2 Console] (https://console.aws.amazon.com/ec2/v2/home?instanceState=running ) window and click load balancer. After copying the DNS name of the load balancer. Created in the compute lab, open a new tab in your browser and paste it.</li>

<li>After connecting to the web server, go to the RDS tab.</li>

<li>Now you can check the data in the database you created.</li>

<img width="321" alt="8" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/df9347c0-42ba-4e01-a960-c419afce95a5">
</ol>

<h3>Architect Configured - </h3>

<img width="347" alt="Aurora Db Architecture" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/6bc8925a-f020-4e6c-b3bd-0c50a79bdc3f">

<br>

<h3>RDS Managment Features</h3>
<br><h4>NOTE:- In multiple AZ deployments, Amazon RDS automatically provisions and maintains synchronous spare replicas in different availability zone. The default DB instance is synchronized from the availability zone to the spare replica to provide data redundancy.</h4>
<br><h3> RDS FAILOVER Tests</h3>
<h4>NOTE:- When multiple AZs are enabled, Amazon RDS automatically switches to a spare replica in another availability zone if the DB instance has a planned or unplanned outage. The amount of time that failover takes to complete depends on the database activity and other conditions when the default
DB instance becomes unavailable. The time required for failover is typically 60-120 seconds. However, if the transaction is large or the recovery process is complex, the time required for failover can be increased. 
When failover is complete, the RDS console UI takes additional time to reflect in the new availability zone.
</h4><br>
<ol>
<li>From the RDS management console, select Databases, select the instance that you want to proceed with the failover, and click Failover in the task menu</li>
<img width="736" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/156cd37f-f820-4811-a548-c70b6483cbf9">

<li>A message asking whether you're going to failover the rdscluster. Press the Failover button.</li>

<li>The refresh button changes the status of rdscluster in the DB identifier to Failing-over.   In a few minutes, press the Refresh button to see Reader and Writer roles changed. The failover is complete.</li>

<img width="737" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/52b84d3b-a45d-48ea-999c-73d6fad5e38d">

</ol> 
<h4> Create RDS Snapshot - Snapshot can be created at any frequency for backup to database instances, and the database can be restored at any time based on the snapshots created.</h4>
<ol>
 <li>From the RDS management console, select Databases, and Select the instance on which you want to perform the snapshot operation. Select Actions > Take snapshot in the upper right corner.</li>

 <img width="736" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e2eacce4-ec8e-4fe4-b649-10dded0af12c">

<li>Type the name you want to use for the snapshot as MultiTierWB-Snapshot 
Press the Take Snapshot button to complete the creation.</li>
<li>From the left RDS menu, select Snapshots and check the creation status of the snapshot. The state of the snapshot is the first creating state, and you can use that snapshot to restore the database when state become available. To restore, select the snapshot and select Actions to see what you can do with that snapshot. Restore Snapshot allows you to create RDS instances with
  the same data based on snapshots taken. This lab will not perform a restore.</li>
</ol>
 
<h4>NOTE -: RDS can change the size of the instance at any time. However, the size of the database does not support shrink after scaling up.
</h4>

<h3>Coneect RDS Aurora</h3>
<h5>
<ol>
<li>Create an EC2 instance with the AMI created in Public Subnet within the VPC-Lab. The networking option should allow Public IP.</li>
Changes the security group settings for RDS Aurora. Configure the newly created EC2 instance to accept security groups as sources.
3. Log in to the EC2 instance you just created with SSH, and connect to RDS Aurora through the MySQL Client. The EC2 web server already has MySQL client installed during EC2 deployment.
</ol>
</h5>
 <h4>Once the setup is successful, you can connect to the CLI environment and perform mysql commands as shown below.</h4>
 <pre>
 $ ssh -i AWS-MultiTierWeb![Uploading image.png…]()
-Lab.pem ec2-user@”EC2 Host FQDN or IP”
Last login: Sun Feb 18 14:41:59 2018 from 112.148.83.236

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2017.09-release-notes/


$ mysql -u awsuser -pawspassword -h awsdb.ccjlcjlrtga1.us-east-1.rds.amazonaws.com

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 34
Server version: 5.6.10 MySQL Community Server (GPL)


Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| MultiTierWeb       |
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
| Tables_in_MultiTierWeb |
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
