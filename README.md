# AWS Scalable Web Application Infrastructure Setup with ALB Logging

This project involves setting up a scalable web application infrastructure on AWS and configuring an Application Load Balancer (ALB) to store access logs in an S3 bucket.

## Overview

This tutorial guides you through the process of setting up a scalable web application infrastructure on AWS. The key steps include:

- Launching EC2 Apache Instances with Apache installed using user data scripts to automate the setup process.
- Creating an S3 bucket for storing the access logs of the ALB.
- Creating a Target Group named "webserverTG" and registering the EC2 instances as targets.
- Creating an Application Load Balancer (ALB) and configuring it to use the "webserverTG" target group for routing traffic.
- Enabling Access Logs in Monitoring for the ALB and specifying the S3 bucket for centralized log management and analysis.
- Testing the load balancer's functionality by accessing its DNS name in a web browser and refreshing the page multiple times.
- Verifying access logs in the specified S3 bucket to confirm the successful configuration and operation of the ALB.

## Prerequisites

- An AWS account with the necessary permissions to create EC2 instances, S3 buckets, ALBs, and IAM roles.
- Basic understanding of AWS services such as EC2, S3, ALB, and IAM.

## Step-by-Step Instructions

Please refer to the detailed instructions provided in the tutorial for step-by-step guidance on setting up the web application infrastructure on AWS.

## Conclusion

By following the instructions in this tutorial, you will have successfully set up a scalable web application infrastructure on AWS and configured an ALB to store access logs in an S3 bucket. This enhances the scalability, reliability, and manageability of your web application on AWS.
