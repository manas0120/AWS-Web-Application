<h3>Clean up Resources</h3>

<h4>Delecting Objects and Buckets</h4>
<ol>
<li>In the Amazon S3 Console, select the Bucket that you want to delete. Then click Delete. 
A dialog box appears for deletion.</li>
<li>There is a warning that buckets cannot be deleted because they are not empty. 
Select empty bucket configuration to empty buckets.</li>
<li>Empty bucket performs a one-time deletion of all objects in the bucket. 
Confirm by typing permanently delete in the box. Then click the Empty button.</li>
<li>Now the bucket is empty. Perform task 1 again. Enter a bucket name and press the Delete bucket button.</li>
</ol>

<h4>NOTE-  If Completed this workshop with your own account, it is strongly recommend to delete the resources and avoid incurring costs</h4>

<h3> Delete Database</h3>
 <p>
  <ol>
  1.After accessing to the Amazon RDS console, select DB Instances.
  2.By default, an Amazon RDS cluster has delete protection enabled to prevent accidental deletions. 
  To disable it, select the Cluster and click the Modify button.
  3.Uncheck the Enable deletion protection button and click the Continue button. 
  4.For immediate deletion, select Apply immediately and click the Modify cluster button.
  5.In order to delete a DB Cluster, you must first delete the DB instances included in the cluster. 
  They can be deleted in any order, but we will delete the Writer instance first.
  Select the Writer instance, and click the Delete button on the Actions menu.
  6.Type delete me in the blank and click the Delete button.
  7.This time, we will delete the Reader instance. Select the Reader instance and
  click the Delete button on the Actions menu.
  8.Type delete me in the blank and click the Delete button.
  9.Lastly, we will delete the DB Cluster. Click the Delete button on the Actions menu.
  10.Uncheck the Take a final snapshot button, check the I acknowledge that automatic backups, 
  including system snapshots and point-in-time recovery, are no longer available when I delete an 
  instance button, and type delete me in the blank. Click Delete DB Cluster and the DB cluster will be deleted.
</ol> </p>
<h3>Delete RDS Snapshot</h3>
  <p>
11.To delete the snapshot of the DB Cluster created during the lab, select immersionday-snapshot and click the Delete snapshot button on the Actions menu.
12.Click Delete Button.
</p>
<h3>Delete AWS Secret Manager</h3>
<p>
1.We're going to delete the secret that stored a RDS credential during the lab. 
Type Secrets Manager in the AWS console search bar and then select it.
2.Select mysecret.
3.Click Delete Secret on the Action menu.
4.To prevent accidental deletion of secrets, AWS Secrets Manager has a deletion wait time of minimum 
7 days and maximum 30 days. Enter the minimum time of 7 days and press the Schedule deletion button.
</p>

<h3>Compute</h3>
<p>
1.We're going to delete the Auto Scaling Group that we used during the lab. 
Type EC2 in the AWS Console search bar and select it. Select Auto Scaling Groups from the left menu. 
Select the Web-ASG that we created in the lab and click the Delete button on the Actions menu.
2.Type Delete, click Delete Button.
</p>

<h4>Delete Application Load Balancer</h4>
<p>
3.Next, we're going to delete the Application Load Balancers. 
Select Load Balancers from the left menu. Then select the Web-ALB that we created in the lab and 
click the Delete load balancer button in the Actions menu.
4.Type confirm in the blank and click the Delete button.

<h4>Delete a Target Group</h4>
 5.We're going to delete the Target Group we created when we created the Application Load Balancer. 
 Select Target Groups from the left menu. Select the Target Group we created in the lab, web-TG, and 
 click the Delete button on the Actions menu.
 6.Click the Yes, delete button.
 <h4>Delete EC2 AMIs</h4>
 7.Select AMIs from the left menu. Select the AMI named Web Server v1 that you created in the lab. 
 Click the Deregister AMI button on the Actions
 8.Click the Deregister AMI button.
 <h4>Delete EC2 Snapshots</h4>
 9.You've just deleted an AMI, but this action doesn't automatically remove the associated snapshot. 
 So you need to remove it manually. From the left menu, choose Snapshots. 
 Be sure to note the snapshot's creation date. Then, select the snapshot you created in the lab, 
 and click the Delete snapshot button on the Actions menu.
 10.Click Delete Button.
 11.Select Launch Templates from the left menu. Select the template named Web that you created in the lab. 
 Click the Delete template button on the Actions menu.
 12.Type Delete in the blank and click the Delete button.

<h4>Delete an EC2 instance</h4>
13.If you went through the (Optional) Connect RDS Aurora section during the database lab, 
you need to delete the EC2 instance you created in the lab. Select Instances from the left menu. 
Select the EC2 instance you created during the lab, and 
click the Terminate instance button on the Instance state menu.
14. Click Terminate.
</p>

<h3>Network</h3>
 <p>
<h4>Delete VPC</h4>
1. Type VPC in the AWS Console search bar and select it. 
Select Endpoints from the left menu. Select S3 endpoint, 
the endpoint you created in the lab, and click the 
Delete VPC endpoints button on the Action Menu.
2.Type delete in the blank, and click the Delete button.
<h4>Delete NAT Gateway</h4>
3.Select NAT gateways from the left menu and select VPC-Lab-nat-public you created during the lab. Click the Delete NAT gateway button on the Actions menu.
4.Type delete in the blank and click the Delete button.
<h4>Delete an Elastic IP</h4>
5.You've just deleted the NAT gateway, but this action doesn't automatically 
delete the Elastic IP that the NAT gateway used, so you need to remove it manually. 
Select Elastic IPs from the left menu, and select VPC-Lab-eip-ap-northeast-2a. 
(The name after VPC-Lab-eip may vary depending on your region.) 
Click the Release Elastic IP addresses button on the Actions menu. 
If it says it is still associated with the NAT gateway and cannot be deleted, refresh the webpage and try again.
6.Click Release Button.
<h4>Delete Security Group</h4>
7.We're going to delete the Security Group you created during the lab. 
Select Security Groups from the left menu. Select Immersion Day - Web Server and DB-SG first, 
and then click the Delete security groups button on the Actions menu. 
The reason for not deleting all security groups at once is that some security groups 
reference other security groups in their inbound rules. A security group that is being 
referenced cannot be deleted until the security group that is referencing it is deleted. 
Therefore, delete the security groups in the following order: 
Immersion Day - Web Server, DB-SG -> ASG-Web-Inst-SG -> web-ALB-SG.
8.Type delete in the blank and click the Delete button.
9.Select ASG-Web-Inst-SG and click the Delete security groups button on the Actions menu.
10.Click the Delete button
11.Select web-ALB-SG and click the Delete security groups button on the Actions menu.
12.Click the Delete button.
<h4>Delete a VPC</h4>
13.Finally, select Your VPCs from the left menu, and 
select the VPC-Lab-vpc that you created during the lab. 
Click the Delete VPC button in the Actions menu.
14.Type delete in the blank and click the Delete button.
</p>
</pre>
