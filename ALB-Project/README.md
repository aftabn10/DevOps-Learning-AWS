# AWS Networking Assignment:

# Introduction

A common DevOps pattern: multiple EC2 instances behind an ALB. This teaches load balancing, health checks, and proper security group isolation.

## Step by Step

**Task 1: Two EC2 Instances**

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

**Task 2: Set Up the ALB**

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

**Task 3: Security Groups**

- **ALB Security Group**: allow inbound HTTP (port 80) from anywhere.
    
- **EC2 Security Group**: allow inbound HTTP only from the ALB SG.
    
- Do **not** allow direct public access to EC2 instances.
    

📸 _Screenshot: ALB SG inbound rules_ 
![ALB SG](./images/alb-sg.png)

📸 _Screenshot: EC2 SG inbound rules referencing ALB SG_
![EC2 SG](./images/ec2-sg.png)

---
### Task 4: Testing

- Visit the **ALB DNS name** in your browser.
    
- Refresh multiple times to verify traffic alternates between both instances.
    
- Confirm **health checks** show both instances as healthy.
    
- Verify direct access to EC2 public IPs fails once SGs are locked down.
    

📸 _Screenshot: Browser showing alternating responses_
![Browser](./images/browser.png)

![Browser2](./images/browser2.png)

📸 _Screenshot: Target group health status (healthy)_
![Health Status](./images/healthstatus.png)