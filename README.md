## CloudFormation Script to spin up fault-tolerant Web-Servers

The following AWS Cloudformation template does the following: 
• Spins up a load balancer with an autoscaling group of 1 t2.nano instance. The instance serves a “hello” world page. Its in an autoscaling group so that if the instance is terminated, the autoscaling group will replace the instance.
• A second version of the App that uses a publicly available API like https://date.nager.at/ or https://api.abalin.net/, determine if today is a holiday. Included in the hello world page is a greeting appropriate to the date, if it is indeed a holiday in any country. 
• It can roll back from the second “holiday-aware” version to the simple original “hello world” version if, for example, the public API being used were to become unavailable. 



**Details :**

• Created 2 Autoscaling groups. Each will give rise to EC2 instances with the 2 different versions of the application each.
• Both the Autoscaling groups are attached to their respective Application Load Balancers. If ever were the Auto Scaling create multiple instances the load balancer would distribute the load.
• Created a Route53 Health Check which hits https://date.nager.at/ and checks the respective API which I have user whether its live.
• Mapped a registered domain to the two different load balancer urls using DNS Failover Routing.
• Here if the Health-Check passes we would redirect user http requests to the load balancer attached to the Ec2 instances which host Version 2 of the Application.
• If the Health-Check fails we would redirect user http requests to the load balancer attached to the Ec2 instances which host Version 1 of the Application.

**Note :**

• One will need to turn off CORS check on their browser in order to see the version 2 off the app in action since the 3rd party domain gives a CORS error in the browser. There’s nothing much that I could do on my end to remediate that in production since it’s a third party API.
• I already have a domain registered in my AWS Account, so Route52 has created a Hosted Zone by default hence the cloud formation template that I have written does not generate another Hosted Zone but uses the same.


**Architecture :**

![Architecture](https://github.com/pandyabhavik2494/Screenshots/blob/master/Architecture.png)
