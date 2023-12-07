<h3>Clean up Resources</h3>
<h4>NOTE-  If Completed this workshop with your own account, it is strongly recommend to delete the resources and avoid incurring costs</h4>
<br>

<h4>Delecting Objects and Buckets</h4>
<ol>
<li>In the Amazon S3 Console, select the Bucket that you want to delete. Then click Delete. A dialog box appears for deletion.</li>

 <img width="899" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/0b03ffee-7402-41eb-b947-e94b068386e3">

<li>There is a warning that buckets cannot be deleted because they are not empty. Select empty bucket configuration to empty buckets.</li>

<img width="497" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/4d121286-8cca-494d-8410-73b601ec4f89">

<li>Empty bucket performs a one-time deletion of all objects in the bucket. 
Confirm by typing permanently delete in the box. Then click the Empty button.</li>

<li>Now the bucket is empty. Perform task 1 again. Enter a bucket name and press the Delete bucket button.</li>
</ol>

<img width="479" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/f265aed5-4910-4d7c-8815-6682bfae3a65">

<h3> Delete Database</h3>
<ol>
<li>After accessing to the Amazon RDS console, select DB Instances.</li>

 <img width="662" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/af00aca1-534a-4087-8b94-4f954b19a2b3">
 
<li>By default, an Amazon RDS cluster has delete protection enabled to prevent accidental deletions. To disable it, select the Cluster and click the Modify button.</li>

<img width="736" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/444bb673-5e65-4cb1-a0dc-2fa993116e17">

<li>Uncheck the Enable deletion protection button and click the Continue button. </li>

<img width="469" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/85a8d84b-d5b1-4ccb-ac83-5a351561f843">

<li>For immediate deletion, select Apply immediately and click the Modify cluster button.</li>

<img width="401" alt="4" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/4084fa9b-ca90-4657-b4cf-1d01dce74114">

<li>In order to delete a DB Cluster, you must first delete the DB instances included in the cluster. They can be deleted in any order, but we will delete the Writer instance first. Select the Writer instance, and click the Delete button on the Actions menu.

<li>Type delete me in the blank and click the Delete button.</li>

<li>This time, we will delete the Reader instance. Select the Reader instance and click the Delete button on the Actions menu.</li>

<li>Type delete me in the blank and click the Delete button.</li>

<li>Lastly, we will delete the DB Cluster. Click the Delete button on the Actions menu.</li>

<img width="753" alt="5" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/9ccdee1e-a9d8-4923-96e1-f2bee1d49a3b">

<li>Uncheck the Take a final snapshot button, check the I acknowledge that automatic backups, including system snapshots and point-in-time recovery, are no longer available when I delete an instance button, and type delete me in the blank. Click Delete DB Cluster and the DB cluster will be deleted.

<img width="424" alt="8" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/ad1e1651-5f06-4315-9bcd-ca38995b9fc4">

</li>
</ol>


<h3>Delete RDS Snapshot</h3>
<ol>
<li>To delete the snapshot of the DB Cluster created during the lab, select immersionday-snapshot and click the Delete snapshot button on the Actions menu.</li>

<li>Click Delete Button.</li>

<img width="770" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/b525c849-f383-40c2-a40f-ec8a4c495b85">
</ol>

<h3>Delete AWS Secret Manager</h3>
<ol>
<li>We're going to delete the secret that stored a RDS credential during the lab. Type Secrets Manager in the AWS console search bar and then select it.</li>

<li>Select mysecret.</li>

<li>Click Delete Secret on the Action menu.</li>

<img width="865" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/c233ed17-6343-4307-b82a-b17106b9466e">

<li>To prevent accidental deletion of secrets, AWS Secrets Manager has a deletion wait time of minimum 7 days and maximum 30 days. Enter the minimum time of 7 days and press the Schedule deletion button.</li>

<img width="333" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/15d2688b-6736-4497-9bfe-15ae14fce154">
</ol>

<h3>Compute</h3>
<ol>
<li>We're going to delete the Auto Scaling Group that we used during the lab. Type EC2 in the AWS Console search bar and select it. Select Auto Scaling Groups from the left menu. Select the Web-ASG that we created in the lab and click the Delete button on the Actions menu.</li>

 <li>Type Delete, click Delete Button.</li>

<img width="765" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/a8a2f25f-8849-42da-9863-8ee778023181">
</ol>

<h4>Delete Application Load Balancer</h4>
<ol>
<li>Next, we're going to delete the Application Load Balancers. 
Select Load Balancers from the left menu. Then select the Web-ALB that we created in the lab and click the Delete load balancer button in the Actions menu.</li>

 <img width="779" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/1d82e3f4-ba48-4200-8877-df05caf347c4">

<li>Type confirm in the blank and click the Delete button.</li>
</ol>


<h4>Delete a Target Group</h4>
<ol>
<li>We're going to delete the Target Group we created when we created the Application Load Balancer. Select Target Groups from the left menu. Select the Target Group we created in the lab, web-TG, and click the Delete button on the Actions menu.</li>

<img width="794" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/785343ff-e060-4dc9-ba8a-7e516f96cf2b">

<li>Click the Yes, delete button.</li>
</ol>

<h4>Delete EC2 AMIs</h4>
<ol>
<li>Select AMIs from the left menu. Select the AMI named Web Server v1 that you created in the lab.  Click the Deregister AMI button on the Actions menu.</li>

<img width="791" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/a217b44b-44db-4217-9121-0631f1c2a92f">

<li>Click the Deregister AMI button.</li>

<img width="338" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/dc1c761b-3e4c-4e52-a3c0-dad7f09468af">
</ol>

<h4>Delete EC2 Snapshots</h4>
<ol>
 <li>You've just deleted an AMI, but this action doesn't automatically remove the associated snapshot.  So you need to remove it manually. From the left menu, choose Snapshots.  Be sure to note the snapshot's creation date. Then, select the snapshot you created in the lab,  and click the Delete snapshot button on the Actions menu.</li>

 <img width="806" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e153f934-c0aa-4b80-9bc6-8ff5aea032f7">

<li>Click Delete Button.</li>

<li>Select Launch Templates from the left menu. Select the template named Web that you created in the lab.  Click the Delete template button on the Actions menu.</li>

<img width="788" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/1409abc7-2bb0-4447-bb36-e5fe67399b7d">

<li>Type Delete in the blank and click the Delete button.</li>
</ol>

<h4>Delete an EC2 instance</h4>

<li>If you went through the (Optional) Connect RDS Aurora section during the database lab, you need to delete the EC2 instance you created in the lab. Select Instances from the left menu. </li>
<li>Select the EC2 instance you created during the lab, and click the Terminate instance button on the Instance state menu.</li>
<li> Click Terminate.</li>


<h3>Network</h3>
<br>
<h4>Delete VPC</h4>
<li>Type VPC in the AWS Console search bar and select it. Select Endpoints from the left menu. Select S3 endpoint, the endpoint you created in the lab, and click the Delete VPC endpoints button on the Action Menu.</li>

<img width="784" alt="VPC" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/85920afd-d0d3-4c37-911d-ff4e454e4d8e">

<li>Type delete in the blank, and click the Delete button.</li>
</ol>

<h4>Delete NAT Gateway</h4>
<ol>
<li>Select NAT gateways from the left menu and select VPC-Lab-nat-public you created during the lab. Click the Delete NAT gateway button on the Actions menu.</li>

 <img width="796" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e353ffbc-5a5b-4ff7-95be-7d2f86984b36">
 
<li>Type delete in the blank and click the Delete button.</li>

<h4>Delete an Elastic IP</h4>
<li>You've just deleted the NAT gateway, but this action doesn't automatically delete the Elastic IP that the NAT gateway used, so you need to remove it manually. Select Elastic IPs from the left menu, and select VPC-Lab-eip-us-east-1a. 
(The name after VPC-Lab-eip may vary depending on your region.) 
Click the Release Elastic IP addresses button on the Actions menu. 
If it says it is still associated with the NAT gateway and cannot be deleted, refresh the webpage and try again.</li>

<img width="806" alt="Elastic IP" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/ea0f1309-0e7d-453a-8f17-aa0a8593f14e">

<li>Click Release Button.</li>
</ol>

<h4>Delete Security Group</h4>
<ol>
<li>We're going to delete the Security Group you created during the lab. 
Select Security Groups from the left menu. Select Multi-Web-Inst SG and DB-SG first, and then click the Delete security groups button on the Actions menu. The reason for not deleting all security groups at once is that some security groups reference other security groups in their inbound rules. A security group that is being referenced cannot be deleted until the security group that is referencing it is deleted. 
Therefore, delete the security groups in the following order: 
Multi-tier Web Server, DB-SG -> AutoScaling-Web-Inst-SG -> Web-ALB-SG</li>

<img width="800" alt="SecurityGroup" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/c0c45e54-e2ed-4842-aee9-3ee2ceaf38f2">

<li>Type delete in the blank and click the Delete button.</li>

<li>Select AutoScaling-Web-Inst-SG and click the Delete security groups button on the Actions menu.</li>
<li>Click the Delete button</li>
<li>Select web-ALB-SG and click the Delete security groups button on the Actions menu.</li>
<li>Click the Delete button.</li>
</ol>
 <img width="799" alt="SecurityGroup2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/2350b3e0-9872-4fa9-a415-d1269f494a0b">

<h4>Delete a VPC</h4>
<ol>
<li>Finally, select Your VPCs from the left menu, and select the VPC-Lab-vpc that you created during the lab. Click the Delete VPC button in the Actions menu.</li>


<li>Type delete in the blank and click the Delete button.</li>

<img width="784" alt="VPC" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/e2d6408f-d8e2-412e-9898-336ae44a059e">

</ol>
