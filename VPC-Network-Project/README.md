# AWS Networking Assignment: VPC and Network Setup

## Introduction
This project demonstrates the configuration of a custom Virtual Private Cloud (VPC) environment in AWS designed for secure and scalable deployments.  
The implementation includes both public and private subnets, internet connectivity through an Internet Gateway (IGW) and NAT Gateway (NGW), and EC2 instances configured with proper network segmentation and routing.

The setup follows a standard two-tier architecture pattern where:
- The **public subnet** hosts internet-facing resources such as a bastion host or web server.
- The **private subnet** hosts internal resources without direct public internet access.
- Secure access is achieved through controlled routing and security group configurations.

---

## Steps and Configuration Details

### 1. Create the VPC
- **CIDR Block:** `10.0.0.0/16`
- **DNS Resolution:** Enabled  
- **DNS Hostnames:** Disabled  
- This forms the foundational virtual network environment for all subsequent components.

**Screenshot:**
![VPC Creation](./images/VPC-setup.png)

---

### 2. Create Subnets
- **Public Subnet:** `10.0.0.0/20`  
  - Hosts resources that require internet access.
  - Configured to auto-assign public IPs.
- **Private Subnet:** `10.0.16.0/20`  
  - Hosts internal resources that do not require direct internet access.

Both subnets were created within the same Availability Zone for simplicity and cost efficiency.

**Screenshot:**
![Subnets Configuration](./images/subnets.png)

---

### 3. Create and Attach Internet Gateway (IGW)
- Created an **Internet Gateway** and attached it to the custom VPC.
- This enables outbound and inbound internet connectivity for the public subnet.

**Screenshot:**
![Internet Gateway](./images/igw.png)

---

### 4. Create Route Tables
Two separate route tables were configured:

- **Public Route Table**
  - Routes:  
    - `10.0.0.0/16` → Local  
    - `0.0.0.0/0` → Internet Gateway  
  - Associated with the **Public Subnet**.

**Screenshot:**
![Route Tables](./images/public-route-table.png)

- **Private Route Table**
  - Routes:  
    - `10.0.0.0/16` → Local  
    - `0.0.0.0/0` → NAT Gateway  
  - Associated with the **Private Subnet**.

**Screenshot:**
![Route Tables](./images/private-route-table.png)

---

### 5. Create NAT Gateway (NGW)
- Deployed in the **Public Subnet**.
- Allocated and attached an **Elastic IP**.
- Allows instances in the private subnet to initiate outbound connections to the internet securely.

**Screenshot:**
![NAT Gateway](./images/nat.png)

---

### 6. Launch EC2 Instances
- **Public Subnet Instance:**  
  - Deployed with a public IP and configured as a **bastion host** for secure SSH access.
  
- **Private Subnet Instance:**  
  - Deployed without a public IP.
  - Accessible only through the bastion host in the public subnet.

**Screenshot:**
![EC2 Instances](./images/ec2-instances.png)


