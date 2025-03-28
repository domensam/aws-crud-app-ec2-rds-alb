# Hands-on Lab: Deploying CRUD App on AWS using EC2, RDS, and ALB
<a id="readme-top"></a>
## Overview

### 📌 What are we trying to achive?
We are familiar with the demand on modern web application, even if its incoming/outgoing traffic **increase or decrease dramatically**, our application should **stay online without crashing**. To achieve this, we can **spread the workload across multiple servers** and use AWS managed services to keep things running smoothly.

### 📚 What are we going to learn? </br>
In this workshop, you will be able to deploy a [web application](example.com) on AWS and keep it highly available. Don't worry, we will guide you along the way!
- Use **Application Load Balancer (ALB)** for distributing traffic
- Provision **Two EC2 instances** running the same backend application
- Setup **Amazon RDS** as the database backend
- Utilize **Security Group and IAM roles** to keep everything secure and safe.

### ❓ Basically, how does the project work as a whole?
1. A user visits the app from their web browser.
2. The **Application Load Balancer (ALB)** will decide where the traffic will go (in which EC2 instance).
3. The **Elastic Compute Cloud (EC2) instances** you setup will process the request.
4. If the app needs to save or retrieve data, it will talk to the **Amazon RDS database**.

## Architecture Diagram
![Architecture](images/aws-architecture.png)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Prerequisites
### 💻 Environment Setup
To access the AWS console, use the credentials provided to you and follow the steps below:
* 
* 

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Deployment Steps
### 1. Set Up AWS Resources

<details>
<summary>**A. Create RDS Database**</summary>
   1. Go to AWS **RDS Console**.
   2. Click **Create Database** → Select **MySQL/PostgreSQL**.
   3. Choose **DB Instance Class** (e.g., `db.t3.micro`).
   4. Set database credentials (`DB_USER`, `DB_PASSWORD`).
   5. Enable **Public Access** (for testing, restrict later via Security Groups).
   6. Click **Create Database**.
</details>
<details>
<summary>**B. Create EC2 Instances**</summary>
   1. Go to AWS **EC2 Console**.
   2. Click **Launch Instance** → Choose **Amazon Linux 2** (or Ubuntu).
   3. Select **Instance Type** (`t3.micro` for free tier).
   4. Attach **IAM Role** with RDS access.
   5. Configure **Security Groups**:
      - Allow **port 22 (SSH)** (your IP only)
      - Allow **port 5000 (app port)** from ALB
   6. Click **Launch**.
</details>

<details>
   <summary>**C. Set Up Application Load Balancer (ALB)**</summary>
   1. Go to AWS **EC2 Console → Load Balancers**.
   2. Click **Create Load Balancer** → Choose **Application Load Balancer**.
   3. Configure:
      - **Listener**: HTTP (port 80)
      - **Target Group**: Attach **EC2 instances**
      - **Health Check Path**: `/health` (or `/` if not defined)
   4. Click **Create Load Balancer**.
</details>

### **2. Deploy Application to EC2**
1. **SSH into EC2 instance**:
   ```sh
   ssh -i your-key.pem ec2-user@your-ec2-ip
   ```
2. **Install dependencies**:
   ```sh
   sudo yum update -y
   sudo yum install python3 pip -y
   ```
3. **Clone the repository**:
   ```sh
   git clone https://github.com/your-repo/aws-crud-app.git
   cd aws-crud-app
   ```
4. **Set environment variables**:
   ```sh
   export DB_HOST="your-rds-endpoint"
   export DB_USER="your-db-username"
   export DB_PASSWORD="your-db-password"
   ```
5. **Run the application**:
   ```sh
   python3 app.py
   ```

### **3. Test the Setup**
- Open **ALB DNS URL** in a browser.
- Use **Postman or cURL** to test CRUD operations:
  ```sh
  curl -X POST http://your-alb-dns/items -H "Content-Type: application/json" -d '{"name": "Sample Item"}'
  ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Troubleshooting & Common Issues
### **1. ALB Not Routing Traffic**
- Ensure EC2 **Security Group allows ALB inbound traffic** on port **5000**.
- Verify **Target Group health checks** are passing.

### **2. RDS Connection Errors**
- Ensure RDS **Public Access** is enabled (for testing).
- Check **Security Group** allows **EC2 IP access on port 3306/5432**.
- Use:
  ```sh
  telnet your-rds-endpoint 3306
  ```
  to test connectivity.

### **3. Application Not Starting**
- Check logs: `journalctl -u your-app --no-pager -n 50`
- Verify Python dependencies: `pip install -r requirements.txt`

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Repository & Deployment Automation
### **1. GitHub Actions for CI/CD (Optional)**
1. Create `.github/workflows/deploy.yml` with deployment steps.
2. Configure **AWS IAM Role** for GitHub Actions.
3. Automate deployments via `git push`.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## License
Distributed under MIT License. See `LICENSE.txt` for details.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Contact
Get in touch with Computrade Technology Philippines (CTP)!
![CTP Logo](images/ctp-logo.png)
- Website: [Computrade Technology Philippines](https://computradetech.com.ph/)
- LinkedIn: [Computrade Technology Philippines](https://www.linkedin.com/company/computrade-technology-philippines-inc./)
- Facebook: [Computrade Technology Philippines](https://www.facebook.com/computradetechph)
- Instagram: [Computrade Technology Philippines](https://www.instagram.com/computradetechph/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
