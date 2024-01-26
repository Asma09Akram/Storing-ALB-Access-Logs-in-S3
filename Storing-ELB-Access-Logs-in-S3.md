### Task 1 Login to AWS Management Console

### Task 2 Launch EC2 Apache instance

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/a82a409f-655b-4b01-ab7f-62bf5b226005)

2.2 Choose Amazon Linux 2

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/ca53fae6-841c-4fea-a688-7b0f8e7a2fff)

2.3 Choose t2.micro

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/ff9ece02-6ca4-4d22-8594-22777d71efcb)

2.4 Create new key pair

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/d06dfffe-9916-47e8-8dbc-0b28dbedb249)

2.5 Create a new Security Group

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/175db926-273d-49d4-95c6-9bcdb2eb8644)

2.6 Add inbound rule of SSH and HTTP

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/a02b1189-768e-44e6-a084-056ee637db4b)

2.7 Go to User Data under Advanced Details and paste the following script

#!/bin/bash			
sudo su		
yum update -y		
yum install -y httpd		
systemctl start httpd		
systemctl enable httpd		
echo "Hello I am your Web Server 1" > /var/www/html/index.html

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/d9824aa4-2b01-40e9-81e7-1be835129839)

Note : The above script creates an HTML page served by Apache HTTP Server.
2.8 Click on Launch Instance

### Task 3 Launch another instance

3.1 give the name as demo web server 2

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/1b3b744d-83e8-491a-96f1-b3aab6db939d)

3.2 Choose Amazon Linux 2

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/0ebd1076-7466-43eb-b8fb-1db79c18e501)

3.3 Choose t2.micro

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/b786a95f-1207-4eeb-87b9-025e2f5930ea)

3.4 Choose demo key pair

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/bdeccb40-7e1c-4590-abfe-47e3a5fe5219)

3.5 Choose existing security group

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/3c903fde-7df4-4fad-aad9-805174010b4d)

3.6 Enter the script in User Data in Advanced Settings

#!/bin/bash		
sudo su		
yum update -y		
yum install -y httpd		
systemctl start httpd		
systemctl enable httpd	
echo "Hello I am your Web Server 2" > /var/www/html/index.html	

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/c71410ce-4d3e-4382-aaa5-b12b1273e6cc)

* Click on Launch Instance
  
3.7 Go to the EC2 dashboard to see webserver-A and webserver-B running as shown below:

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/ac19d47f-0e32-4314-a2a0-7b91372ac2e2)


###  Task 4 Creating a Target Group 
4.1 Go to Target Groups in the left-side panel under Load Balancer in the Load Balancing section. Click on Create Target Group

4.2 Choose Instances from Basic configuration

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/e8d5b2ef-0175-45a8-a513-58e8cb6ce6df)

4.3 Give Target Group name as webserverTG

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/d410700d-bbfc-4341-97b8-642f2b579669)

4.4 Health Checks:

Health check protocol : Select HTTP
Health check path : Enter /index.html
Click and expand Advanced health check settings
Healthy threshold : Enter 3
Unhealthy threshold : 2 (Default) 
Timeout : 5 seconds (Default)
Interval : Enter 6 seconds
Success code : 200 (Default)

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/86bd1df6-d8a4-4d9f-872c-ef6ff51f9f2f)

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/15537ebb-6e01-4332-9c14-539a7daacf71)

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/6fea1f25-78a9-46dd-98d5-50ef5b5b67f8)

4.5 Leave everything as default and click on Next button.

4.6 Register targets:
* Select the two instances we have created i.e demo web server 1 and demo web server 2
* Click on Include as pending below and scroll down.

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/12f3047e-a0e0-4ab1-9da1-0027dc239763)

4.7 Click on Create target group button.   
4.8 Your Target group has been successfully created.

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/66708587-6c28-4738-9c33-a34565d34d62)

### Task 5 Creating an Application Load Balancer
5.1 Navigate to Load Balancers in the left-side panel under Load Balancing. Create Load Balancer

5.2 Choose Application Load Balancer since we are testing the high availability of the web application and click on Create button.

5.3 Enter Web-Server-Load-Balancer

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/12738659-0cd2-4941-b2c1-60c31852b283)

5.4 Network mapping:
VPC : Select Default
Mappings : Check All Availability Zones

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/3a45bb77-250a-464d-9f74-2f24d2639113)

5.5 Security groups : Remove Default security group and Select an existing security group i.e Web Server Security Group from the drop down menu.

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/23373eba-e481-474f-8c35-232ddfeda5f9)

5.6 Listeners and routing:
Protocol : Select HTTP
Port: Enter 80
Default action: Select webserverTG from the drop-down menu

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/25ee406f-09c2-4133-be01-9eeb409a1dbb)

5.7  Leave everything as default and click on Create load balancer button.

5.8. You have successfully created Application Load Balancer.

### Task 6 Configuring the Load Balancer to store Access logs in S3 bucket

6.1 Go to Load Balancers and then select the load balancer that you have created in the above step.
* Click on Actions and then click on Edit load balancer attributes to enable the access log feature.
  
![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/cf842adb-bb93-4a25-b63f-17945c096884)

6.2 Scroll Down and Check the box next to the Access logs in Monitoring section

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/80a0727e-789f-4667-910e-21a4797621bc)

6.3 Click on View button and you will be redirected to S3 Console or if it not works you can go to S3 from Services. 

* Click on Create bucket button and Enter any unique bucket name.
* AWS Region: Select US East (N. Virginia) us-east-1
* Keep rest things as default and click on Create bucket button.
* Click on the bucket created and click on the Permissions tab.
* Scroll down to Bucket Policy and click on Edit button.
* Paste the policy and replace the Bucket ARN in the code.

'''
{		
"Version": "2012-10-17",
    "Statement": [	
        {	
            "Effect": "Allow",	
            "Principal": {	
                "AWS": "arn:aws:iam::127311923021:root"

			},	
            "Action": "s3:PutObject",	
            "Resource": "<Enter Bucket ARN>/awslogs/AWSLogs/*/*"	
        }	
    ]		
}	
'''

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/6c65fc5c-455d-40f1-b8ba-b1675110114f)

* Click on Save changes
* Navigate back to Load Balancer tab and click on Browse S3 button and select the Bucket created.
* Add /awslogs at the end of Bucket URL and click on Save changes button

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/91161df5-6ea0-4533-ad8f-34aecea3aa56)


### Task 7 Testing the Load Balancer and Stored Access Logs

7.1 Navigate to Load balancers and Select our load balancer. Click on Description , copy the DNS name and paste it in the browser.

Example DNS URL: Web-Server-Load-Balancer-1326461726.us-east-1.elb.amazonaws.com

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/0cdac5f0-d3a4-4fb1-8357-6ba68414503a)

7.2 Refresh the browser couple of times and you will see the request is serving from both servers .i.e you will see the response either of the following two:  

Hello I am your Web Server 1

Hello I am your Web Server 2 

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/a96cc43a-36f6-4396-b5d3-9257bde04a8d)

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/7ab44039-3c92-469f-93d9-ad07dc9440b1)



7.3 Navigate to the S3 console and enter into the bucket that you created to store ELB access logs. You will find the access logs under AWSLogs folder

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/97b9eac6-7ae1-465c-8730-84d2dd87d89c)

7.4 Click on the directory containing the load balancer URL to see whether the access logs are in the bucket. You should see a new folder as shown below.

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/98971834-c6ce-4a1d-b1e3-94692cac08bd)

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/cdf854a4-ea83-4347-8236-58106675e60a)

* You can download the generated access log files (.zip file) to your local machine for review.
*The log file will be present in a hierarchy, which goes like this:

(Bucket_name) / AWSLogs / (Account_number) / elasticloadbalancing / us-east-1 / (Year) / (Month) / (Day) / (LogFile)

* Select the file and click on the Actions button as above and choose Download. (Incase, you are unable to download the log file, click on the Object actions button above and choose the option to Make public, then try downloading again.)
* You can extract the download file using Winzip.
* Your log file entry will look like something like the snippet below:
Note: Only 1 file will be created, and it will be updated as you access the ELB DNS more.

![image](https://github.com/Asma09Akram/Storing-ELB-Access-Logs-in-S3/assets/124654068/521e95ae-e8ef-4bf9-87d0-7668d2556917)


Do you know ?

### Storing ELB access logs in S3 enables seamless integration with other AWS analytics services, such as Amazon Athena, Amazon Redshift, or Amazon EMR (Elastic MapReduce). 
### You can directly query and analyze your log data using these services, gaining valuable insights into your application's performance and user behavior.


