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

