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
bjects during the month, and the storage class. There are also per-request fees. Click for more information.

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
<h3>Enable Static Hosting</h3>
<h4>A static website refers to a website that contains static content (HTML, image, video) 
or client-side scripts (Javascript) on a web page. In contrast, dynamic websites require 
server-side processing, including server-side scripts such as PHP, JSP, or ASP.NET. 
Server-side scripting is not supported on Amazon S3. If you want to host a dynamic website, 
you can use other services such as EC2 on AWS.
</h4>
<ol>
 <li>In the S3 console, select the bucket you just created, and 
 click the Properties tab. Scroll down and click the Edit button on 
 Static website hosting.</li>
 <li>Activate the static website hosting function and select the hosting type and 
 enter the index.html value in the Index document value, then click the save changes button.</li>
 <li>Click Bucket website endpoint created in the Static website hosting entry to access the static website.</li>
</ol> 
<h3> Move Objects</h3>
<ol>
<li>Create a temporary bucket for moving objects between buckets 
(Bucket name: immersion-day-myname-target). Substitute myname with your name. 
Rememeber the naming rules for the bucket. Block all public access 
Uncheckbox for quick configuration.</li>
<li>Check the notification window below and select Create bucket.</li>
<li>In the Amazon S3 Console, select the bucket that contains the object 
(the first bucket you created) and click the checkbox for the object you want to move. 
Select the Actions menu at the top to see the various functions you can perform on that object.
Select Move from the listed features.</li>
<li>Select the destination as bucket, then click the Browse S3 button to find the new bucket you just created.</li>
<li>Click the bucket name in the pop-up window, then select the destination (arrival) bucket. 
Click the Choose destination button.</li>
<li>Check that the object has moved to the target bucket.</li>
</ol>
<h4>Enable Versioning</h4>
 <ol>
<li>In the Amazon S3 Console, select the first S3 bucket we created. Select the Properties menu. 
Click the Edit button in Bucket Versioning.</li>
<li>Click the enable radio button on Bucket Versioning, then click Save changes.</li>
<li>In this lab, the index.html file will be modified and re-uploaded with the same name. 
Make some changes to the index.html file. Then upload the modified file to the same S3 bucket.</li>
<li>When the changed file is completely uploaded, click the object in the S3 Console. 
You can view current version information by clicking the Versions tab on the page that contains object details.</li>
</ol>