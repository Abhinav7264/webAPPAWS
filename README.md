## 🚀 Deploying a Scalable Web Application on AWS

Ensuring high availability and scalability is crucial for any cloud-based application. This guide provides a detailed step-by-step process for deploying an application using AWS services such as Elastic Beanstalk, DynamoDB, and CloudFront. By the end of this setup, your application will be optimized for performance, security, and seamless scalability. 

---

### 🔹 **Step 1: Setting Up SSH Key Pair**

#### 🔑 **Creating an SSH Key Pair**
To securely access your EC2 instances, create an SSH key pair:
1. Navigate to **AWS Management Console** → **EC2** → **Key Pairs**
2. Click on **Create Key Pair**
3. **Name:** `mod4-ssh-key`  
4. **Format:** `.pem` (for OpenSSH)  
5. Store it securely, as you’ll need it for SSH access 🔐

---

### 🔹 **Step 2: Setting Up DynamoDB & Elastic Beanstalk**

#### 🛠 **Creating the DynamoDB Table**
DynamoDB is a NoSQL database designed for fast and reliable performance. Create a new table:
- **Table Name:** `users`
- **Primary Key:** `email` (Partition Key)

#### 🚀 **Deploying the Application with Elastic Beanstalk**
Elastic Beanstalk simplifies the deployment and management of applications in AWS. Follow these steps:
1. **Create an Elastic Beanstalk Application**
   - **Application Name:** `tcb-conference`
   - **Platform:** Python 🐍
   - **Upload Code:** Use a public S3 URL to upload the package.
   - **Select Configuration Preset:** `High Availability` 🌍
   
2. **Auto Scaling & Load Balancing**
   - Elastic Beanstalk automatically creates an **EC2 Instance**, **Security Group (SG)**, **Elastic Load Balancer (ELB)**, and **Target Group (TG)** 🔄.
   - Auto Scaling ensures your application remains responsive under varying loads.

---

### 🔹 **Step 3: Configuring Service Access & IAM Roles**

#### 🔑 **Creating the Service Role for Elastic Beanstalk**
AWS requires IAM roles for managing resources. Create the following roles:
- **Service Role:** `aws-elasticbeanstalk-service-role`
  - **Permissions:**
    - ✅ `AWSElasticBeanstalkEnhancedHealth`
    - ✅ `AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy`

- **EC2 Instance Profile:** `aws-elasticbeanstalk-ec2-role`
  - **Attach Permissions:**
    - ✅ `AWSElasticBeanstalkWebTier`
    - ✅ `AWSElasticBeanstalkWorkerTier`
    - ✅ `AmazonDynamoDBFullAccess` 🔥

---

### 🔹 **Step 4: Networking, Scaling, and Load Balancing**

#### 🌐 **VPC & Public Accessibility**
- Use **Default VPC in N. Virginia** (or your preferred region)
- Enable **Public IP Allocation** 📡

#### 📈 **Auto Scaling Configuration**
- **Minimum Instances:** `2`
- **Maximum Instances:** `4`
- **Load Balancer:** Public, enable all subnets ✅
- **Scaling Triggers:**
  - CPU Utilization:
    - 🔼 Scale Up: 50%
    - 🔽 Scale Down: 40%

---

### 🔹 **Step 5: Configure Updates, Monitoring, and Logging**

…scroll down…

#### **Environment Properties**
Add environment property:
- **Name:** `AWS_REGION`
- **Value:** `us-east-1`

Click **Next** to proceed.

---

### 🔹 **Step 6: Optimizing Performance with CloudFront**

CloudFront speeds up content delivery and reduces latency by caching content in Edge Locations closer to users 🌍.

#### 🛜 **Creating a CloudFront Distribution**
- **Origin Domain:** Elastic Load Balancer from Elastic Beanstalk
- **Allowed HTTP Methods:** `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE`
- **Cache Policy:** `CachingOptimized` 🚀
- **Enable Web Application Firewall (WAF) 🔒**

---

### 🔹 **Step 7: Testing and Validating the Setup**

#### ✅ **Application Testing**
- Access the deployed application via Elastic Beanstalk's provided domain 🔗
- Add a test email to DynamoDB and confirm it gets stored ✉️
- Check CloudFront to verify it serves content efficiently ⚡

#### 📜 **Checking Logs and Debugging**
- Elastic Beanstalk Logs → Request logs → Last 100 lines 📝
- Validate `DynamoDB` table for expected entries ✅

---

### 🔹 **Step 8: Load Testing & Monitoring**

Simulate high-traffic scenarios to verify the resilience of the infrastructure 🔥.

#### 🖥 **Accessing EC2 and Running Stress Tests**
1. **SSH into an EC2 Instance:**
   ```bash
   ssh -i mod4-ssh-key.pem ec2-user@public-ip-ec2
   ```
2. **Install and Run Stress Tool:**
   ```bash
   sudo yum install stress -y
   stress -c 4
   ```
3. **Monitor System Resources:**
   ```bash
   top
   ps aux --sort=-pcpu
   ```

#### 📊 **Monitor Elastic Beanstalk Health**
- Check **EC2 Metrics, Load Balancer, and Auto Scaling Group** 🔍

---

### 🔹 **Step 9: Cleanup and Resource Deletion 🗑**

Once testing is complete, remove unnecessary resources to avoid extra costs:
- ❌ **Delete Elastic Beanstalk Application** → This will terminate associated instances 🚀
- ❌ **Disable & Delete CloudFront Distribution**
- ❌ **Remove the DynamoDB Table**

---

## 🎯 Conclusion

This step-by-step guide provides a structured approach to deploying a highly available cloud application on AWS. By leveraging **Elastic Beanstalk, DynamoDB, and CloudFront**, we ensure seamless scaling, optimized performance, and minimal latency. With proper monitoring and stress testing, the system is resilient under varying loads. 

🚀 Now, you're ready to deploy and scale cloud applications efficiently! 🔥


