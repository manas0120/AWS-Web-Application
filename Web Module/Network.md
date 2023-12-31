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
This virtual network, along with the benefits of using AWS's scalable infrastructure, is very similar to the existing network operating in the customer's own data center. 
<ol>
<li>After logging in to the AWS console, select VPC from the service menu.</li>
<li>Select VPC Dashboard and click Launch VPC Wizard to create your own VPC.</li>
<li>To create a space to provision AWS resources used in this lab, we will create a VPC and Subnets.
Select VPC, subnets, etc in Resource to create tab and change name tag to VPC-Lab. Leave the default setting for IPv4 CIDR block. </li>

<img width="932" alt="1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/2262bcb4-02f9-4006-8f4d-f93bade0ee75">

<li>To design high availability architecture, we create 2 subnet space and <b>select 1a and 1c for Public Subnet and 1a and 1c for Private Subnet (Customize AZs)</b>. And set the CIDR value of the public subnet (us-east-1a : 10.0.10.0/24, us-east-1c : 10.0.20.0/24) that can communicate directly with the Internet as shown in the screen below. 
Set the CIDR value of the private subnet (us-east-1a : 10.0.100.0/24, us-east-1c : 10.0.200.0/24). </li>

<img width="264" alt="2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/1909eced-5187-49ec-bd46-86abac6abfd3">

<li>You can use a NAT gateway so that instances in your private subnets can connect to services outside your VPC, but external services cannot initiate direct connections to these instances. In this lab, we will create a NAT gateway in only one Availability Zone to save cost. Also, for DNS options, enable both DNS hostnames and DNS resolution. After confirming the setting value, click the Create VPC button.</li>

<img width="263" alt="3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/771765d9-fb8d-478c-b092-ed65a417db42">

  <li>As the VPC is created, you can see the process of creating network-related resources as shown in the screen below. For NAT Gateway, provisioning may take longer compared to other resources.</li>
  
<img width="314" alt="vpc workflow" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/415378e2-0b79-4f31-ab85-4659597585ca">

<li>You can check the information of the created VPC. Check related information such as CIDR value, route table, network ACL, etc. Check that the values you just set are correct. </li>
</ol>
<h3><b>
2.) Create VPC Endpoint - </b></h3>In this section, you create an endpoint for S3 to learn a VPC endpoint. 
<ol>
<li>In VPC Dashboard, select Endpoints. Click Create endpoint button. </li>
<li>Type S3 endpoint for name and select AWS services in Service category tab. In the search bar below, type s3 and select the list at the top. </li>
<li>For S3 VPC endpoints, there are gateway types and interface types. For this lab, select the gateway type. And for the deployment location, select the VPC-Lab-vpc created in this lab.</li>

<img width="910" alt="s3 endpoint 1" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/07c85eb9-aed0-419e-be6c-e7242b3381c5">

<li>Choose a route table to reflect the endpoint. Select the two private subnets as shown below. 
Additional routing information for using the endpoint is automatically added to the selected route table.</li>

<img width="938" alt="s3 endpoint 2" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/f948b467-aa1e-4da1-bac8-52ca03c39487">

<li>Confirm that the route to access Amazon S3 through the gateway endpoint has been automatically added to the private route table specified earlier.</li>

<img width="672" alt="s3 endpoint 3" src="https://github.com/manas0120/Highly-Available-Multi-Tier-Web-Application/assets/60257363/3b069f5b-5b3f-40e6-9757-c95b71f8fdc8">

NOTE:- VPC endpoints are communications within the AWS network and have the security and compliance advantage of being able to control traffic through the endpoints. 
You can also optimize the data processing cost if you transfer your data through a VPC endpoint rather than a NAT gateway.</li>

</p>
</ol>
