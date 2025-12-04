# 🚀 AWS Public Web Server 

This project walks through how I designed and deployed a **public-facing web server** on AWS using core networking services such as **VPC, Subnets, Route Tables, Internet Gateway, Security Groups, and EC2**.
It demonstrates my understanding of AWS networking fundamentals and shows real cloud-engineering discipline.

---

## 📌 **Project Overview**

The goal of this project is to deploy a simple but fully functional **public web server** accessible from the internet.
This setup follows AWS best practices and focuses on networking concepts such as:

* How the internet reaches your server
* How VPCs and subnets isolate resources
* How routing and security controls work
* How EC2 instances become accessible publicly

This is a foundational cloud engineering project used in interviews, certifications, and hands-on practice.

---

# 🧠 **Architecture Overview**

Below is the high-level flow of how traffic moves from the internet into my EC2 web server:

**User → Internet → Internet Gateway → VPC → Public Subnet → Route Table → Security Group → EC2 Instance → Web Page**

---

# 🏗️ **Architecture Components (Well Explained)**

### **1. Virtual Private Cloud (VPC)**

A logically isolated network in AWS.
All resources (subnets, route tables, EC2, etc.) live inside this VPC.

**Example CIDR:** `10.0.0.0/16`

---

### **2. Public Subnet**

A subnet that routes traffic to the internet through the Internet Gateway.

**Example CIDR:** `10.0.1.0/24`

This is where the web server (EC2) lives.

---

### **3. Internet Gateway (IGW)**

A gateway that connects your VPC to the internet.
Without an IGW → **you cannot create a public server**.

Attached directly to the VPC.

---

### **4. Route Table**

Controls how traffic leaves the subnet.

Public route table has:

```
Destination: 0.0.0.0/0
Target: Internet Gateway
```

This makes the subnet **public**.

---

### **5. Security Group (Firewall Rules)**

Controls who can reach your server.

**Inbound**

* HTTP (80) → Anywhere
* SSH (22) → Your IP only

**Outbound**

* Allow all (default)

---

### **6. EC2 Instance (Web Server)**

A virtual machine running the website.

Installed:

* Nginx or Apache
* Static HTML page (`index.html`)

Instance type:

* `t2.micro` (Free-tier)

OS:

* Amazon Linux 2 or Ubuntu

---

### **7. Public IP / Elastic IP**

Necessary for clients on the internet to reach your EC2 instance.

* **Public IP**: auto-assigned, changes if instance stops
* **Elastic IP**: static, recommended for production

Attached to the EC2 instance.

---

# 🌐 **How Everything Works (“Traffic Flow”)**

1. A user types your EC2 public IP in their browser
2. The request travels through the internet
3. The Internet Gateway receives the traffic
4. The VPC passes it into the public subnet
5. The route table sends it to the EC2 instance
6. Security group verifies the rules (HTTP allowed)
7. Nginx/Apache handles the request
8. The webpage loads and displays to the user

---

# 🛠️ **Step-by-Step Deployment (Summary)**

### **1. Create VPC**

* CIDR: `10.0.0.0/16`

### **2. Create Public Subnet**

* CIDR: `10.0.1.0/24`
* Enable auto-assign public IP

### **3. Create and Attach Internet Gateway**

* Attach to VPC

### **4. Create Route Table**

* Add `0.0.0.0/0 → IGW`
* Associate it with the public subnet

### **5. Create Security Group**

* Allow HTTP (80) from anywhere
* Allow SSH (22) from your IP

### **6. Launch EC2 Instance**

* Choose Amazon Linux or Ubuntu
* Connect via SSH
* Install Nginx or Apache:

**Ubuntu:**

```bash
sudo apt update
sudo apt install nginx -y
```

**Amazon Linux:**

```bash
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

### **7. Deploy Website**

Replace default index.html:

```
/var/www/html/index.html
```

### **8. Test**

Open browser:
`http://<your-public-ip>`

---

# 📘 **Skills Demonstrated**

* AWS VPC networking fundamentals
* Internet gateway configuration
* Public subnet design
* Route table management
* Security group design
* Linux server provisioning
* Deploying and hosting a public website
* Cloud engineering best practices

---
