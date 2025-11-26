# AWS Networking Assignment: Application Load Balancer

# Introduction

This project demonstrates the deployment of an Application Load Balancer (ALB) in AWS to distribute traffic across multiple EC2 instances. The implementation highlights load balancing, health checks, and proper security group isolation to ensure high availability and secure access.

The setup follows a standard high‑availability pattern where:

- The ALB acts as the single entry point for all incoming traffic.

- Two EC2 instances are deployed across different Availability Zones to provide redundancy.

- Security Groups enforce isolation so that EC2 instances are only accessible through the ALB.

- Health checks ensure that traffic is routed only to healthy targets.

---

## Summary of Configuration



## Step by Step

## Task 1: Two EC2 Instances

- Launch **two EC2 instances** in the same VPC.
    
- Place them in **different Availability Zones** for redundancy.
    
- Use **User Data** to install Apache and serve a simple web page.

```
#!/bin/bash
yum update -y
yum install -y httpd

systemctl start httpd
systemctl enable httpd

echo "<h1>Welcome to My EC2 Instance - $(hostname -f)</h1>" > /var/www/html/index.html
```
    
- Ensure each instance returns **different content** (e.g., hostname or instance ID) for testing.
    

📸 _Screenshot: EC2 instances list in AWS console_ 
![EC2 instances](./images/ec2instances.png)


📸 _Screenshot: User data script used at launch_ 
![User Data Script](./images/userdatascript.png)

📸 _Screenshot: Instance public IP showing unique content in browser_
![Instance public IP](./images/public-ip.png)

---

## Task 2: Set Up the ALB

- Create an **Application Load Balancer** in **two public subnets**.
    
- Add an **HTTP listener (port 80)**.
    
- Create a **Target Group** (HTTP, port 80).
    
- Register both EC2 instances with the target group.
    
- Configure a **health check** on path `/`.
    

📸 _Screenshot: ALB creation wizard_

![ALB creation](./images/albcreation.png)

![ALB creation](./images/albcreation2.png)

📸 _Screenshot: Target group with registered instances_ 
![ALB creation](./images/targetgroup.png)

![ALB creation](./images/targetgroup2.png)

📸 _Screenshot: Health check configuration_


---

## Task 3: Security Groups

- **ALB Security Group**: allow inbound HTTP (port 80) from anywhere.
    
- **EC2 Security Group**: allow inbound HTTP only from the ALB SG.
    
- Do **not** allow direct public access to EC2 instances.
    

📸 _Screenshot: ALB SG inbound rules_ 
![ALB SG](./images/alb-sg.png)

📸 _Screenshot: EC2 SG inbound rules referencing ALB SG_
![EC2 SG](./images/ec2-sg.png)

---

## Task 4: Testing

- Visit the **ALB DNS name** in your browser.
    
- Refresh multiple times to verify traffic alternates between both instances.
    
- Confirm **health checks** show both instances as healthy.
    
- Verify direct access to EC2 public IPs fails once SGs are locked down.
    

📸 _Screenshot: Browser showing alternating responses_
![Browser](./images/browser.png)

![Browser2](./images/browser2.png)

📸 _Screenshot: Target group health status (healthy)_
![Health Status](./images/healthstatus.png)

## What I Learned

- Understood how an Application Load Balancer provides high availability and redundancy.

- Learned how to configure Target Groups and health checks to ensure traffic only flows to healthy instances.

- Practiced security group isolation, ensuring EC2 instances are protected and only accessible via the ALB.

- Observed how load balancing alternates traffic between instances, demonstrating distribution in action.

- Strengthened awareness of resilient architectures by deploying resources across multiple Availability Zones.

- Built confidence in designing scalable, fault‑tolerant web applications using AWS core services.