# AWS_Capstone_Project
After completing the Cloud Architecting course on the AWS Academy, the final module was to provide a solution for a capstone project which is a way to display the knowledge that I have learned during the course. This post will allow you to see how far I have come in my cloud journey.

Problem:
To summarise the problem that the client is having. Shirley has a PHP website from where other researchers can query the data that she has collected which is stored in a MySQL database. As the website has become more popular, the increase in traffic means that the site is becoming less responsive. In addition to this, she is also experiencing security breaches.
Shirly initially moved her website and database to an EC2 instance that runs in a public subnet. She also runs an instance of MySQL on the same EC2 instance.

Shirley has approached our team to make sure that her design follows best practices. She wants a robust and secure website.
Our tasks are to complete the implementation, make sure that the website is secure, and confirm that the website returns data from the query page.

Solution:

![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/8039ed6f-7296-456e-b6a8-c26d68eec6e0)


These are the beginning resources that are set up, a bastion host, two availability zones each with a public and private subnet, and various security groups that are for us to use with the resources that we provision later on.

Initially, what I did was to understand how the resources that we start with are configured, specifically looking at the security group rules to make sure that we have the correct permissions for what we will need.

The first thing that I provisioned was a NAT Gateway in the public subnet. The reasoning for this was that to make the design more secure, I decided to create the EC2 instances for running the PHP application in the private subnet. I corrected the private route table to ensure that it allowed the private instance to talk to the Internet.
![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/c2ff2523-0a58-40f8-a212-513af9caca0e)


The following step was to create an EC2 private instance from the template that is provided to us by AWS which already has both the SQL dump file and unzipped PHP application resources built into it.

Once the instance was up and running, I set up an SSH connection to the private instance through the Bastion host. I navigated through the files to make sure that the SQL dump file and HTML files were there.

Next, I set up a load balancer with a target group directed towards the EC2 application server.
![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/9fd22391-77c3-4757-b0e0-0c88ba64b083)


Afterwards, I created an auto-scaling group to be available across the two availability zones and added the instance that I previously created to the auto-scaling group.

After this, I checked that the networking configurations and security groups of all the applications were properly done by searching the DNS name of the load balancer on the internet. It returned the website, but without the function to query which will be sorted out next. But, the important thing was that the configurations of the design so far were all correct, which meant that I could move on to the final parts of the design.

I launched an RDS instance and connected it to the EC2 instance so that it can query the data from the MySQL database in RDS. AWS has an option that automatically sorts out the security groups for the connection which helped to save time, but naturally, I checked the rules to make sure that they were correct.
![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/7a2af6dd-7cab-4bc0-8235-4e6cb4e8fdde)


I connected to the database and imported the data from the MySQL dump into RDS. It was quite a challenge as I was not at all familiar with the language required to perform this action. After some research, and admittedly a lot of trial and error, I managed to perform the data dump into the correct table inside of RDS for MySQL.

![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/3de054a9-391b-4fa6-9ba4-16474b073b44)
The code for importing the data

![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/b993ee75-a1dc-4e15-951e-332d16f70e30)
![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/63ee4c6a-3110-42f8-85da-c96d9f3eb48d)


The last and final step was to set up parameters in the Systems Manager Parameter Store.
![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/8bcc141c-1730-4756-adec-3d87341beef6)


Initially, I thought that this would be an easy final part of the project, but I was stuck on the “database” parameter for quite a while and had no idea why it wouldn’t work when I entered the database name as the value. What I came to realise after some testing and online referencing, I realised that I was entering my DB-instance-ID “capstone-project-db” instead of the DB name which is the name of the actual database “capstonedb”. Once, I managed to overcome that obstacle, I completed the project and checked the querying system once more as a final check and to my excitement, it worked!
![image](https://github.com/DylanRaimondi/AWS_Capstone_Project/assets/111287803/0b08551f-d049-496a-8c01-ef9ca814c460)


In conclusion, I thoroughly enjoyed the project in spite of my mishaps along the way which at the end of the day, is the best way to make progress. I created an architecture design that is available and scalable via the load balancer and auto-scaling group as well as emphasising security by placing the instances in the private subnets and having the traffic go through the load balancer first.

Because of this project, I learned some new skills and configuration settings that are possible and further increased my awareness of the different AWS services and offerings and how they can be used with each other in the grand scheme of things.

Thank you for reading and hope you keep up with my cloud journey!
