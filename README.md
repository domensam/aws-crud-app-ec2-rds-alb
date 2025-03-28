# ⚒️ Hands-on Lab: Deploying CRUD App on AWS using EC2, RDS, and ALB
<a id="readme-top"></a>
## Overview

### 📌 What are we trying to achive?
We are familiar with the demand on modern web application, even if its incoming/outgoing traffic **increase or decrease dramatically**, our application should **stay online without crashing**. To achieve this, we can **spread the workload across multiple servers** and use AWS managed services to keep things running smoothly.

### 📚 What are we going to learn? </br>
In this workshop, you will be able to deploy a [web application](https://github.com/jcmsj/ust-workshop.git) on AWS and keep it highly available. Don't worry, we will guide you along the way!
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

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Deployment Steps
### 1. Set Up AWS Resources
#### A. Create RDS Database

1. Type <strong>"rds"</strong> on the console's search bar and select <strong>"Amazon and RDS"</strong> on the resulting options.
<img width="1447" alt="Screenshot 2025-03-28 at 2 52 52 PM" src="https://github.com/user-attachments/assets/9f14c53f-3b94-43f3-8302-b36cfc90e8f1" /><br />

2. Select the option <strong>"Databases"</strong> on the lefthand side of the page.
<img width="1446" alt="Screenshot 2025-03-28 at 3 01 03 PM" src="https://github.com/user-attachments/assets/0b36e6c4-54c1-4744-8f6a-9d37ec0e75da" /><br />

3. Select the <strong>"Create Database"</strong> button on the righthand side of the page.
<img width="1468" alt="Screenshot 2025-03-28 at 3 07 20 PM" src="https://github.com/user-attachments/assets/21ab028c-0126-445a-ad0a-042a61a88955" /><br />

4. Select <strong>"PostgreSQL"</strong> for the Engine Type and leave "Standard Create" for the Creation Method 
<img width="1453" alt="Screenshot 2025-03-28 at 3 36 19 PM" src="https://github.com/user-attachments/assets/35319eab-6b51-433b-b11f-f3a66e9908bc" /><br />

5. Select <strong>"Free Tier"</strong> option in the "Templates" section.
<img width="1453" alt="Screenshot 2025-03-28 at 3 40 43 PM" src="https://github.com/user-attachments/assets/d9736850-b545-421c-8be5-ff815db46dc4" /><br />

6. Select <strong>"Self managed"</strong> on the Credentials Management section. Then, input the following on the specified text fields:
```sh
DB Instance Identifier = "fil"
Master username = "root"
Master password = "ustworkshop"
Confirm password = "ustworkshop"
```
<img width="1456" alt="Screenshot 2025-03-28 at 3 51 59 PM" src="https://github.com/user-attachments/assets/0e442df0-daba-4249-ac8c-1cb982ff4d47" /><br />

7. Then, navigate to the <strong>"VPC Security Group (Firewall)"</strong> under the Connectivity section, and select the <strong>provided VPC</strong> on the drop down options.
<img width="1451" alt="Screenshot 2025-03-28 at 4 23 33 PM" src="https://github.com/user-attachments/assets/9e15663e-1d01-4a0a-a8f2-aa485d926585" /><br />

8. Click the <strong>"Create Database"</strong> button on the bottom-most right side.
<img width="1452" alt="Screenshot 2025-03-28 at 4 33 03 PM" src="https://github.com/user-attachments/assets/55fd0e37-6881-4f94-afde-0d8c1594ebdf" /><br />

> Note: Before proceeding, wait for the <strong>green banner</strong> on the top to show that the RDS is succesfully created.<br />
> This takes <strong>around 3-5 minutes</strong>, so grab a cup of coffee while waiting! ☕️ <br />
> 
> <img width="1156" alt="Screenshot 2025-03-28 at 4 35 40 PM" src="https://github.com/user-attachments/assets/7ba11fec-3bf4-49ab-a503-d9e9bac707ee" />

<p align="right">(<a href="#readme-top">back to top</a>)</p>

#### B. Create EC2 Instances

#### C. Set Up Application Load Balancer (ALB)

Sample description

### 2. Deploy Application to EC2
#### 1. Connect to your instances using "EC2 instance connect"
#### 2. Install dependencies</h4>
```sh
sudo yum update -y
sudo yum install python3 pip -y
```
#### 3. Clone the repository
```sh
git clone https://github.com/your-repo/aws-crud-app.git
cd aws-crud-app
```
#### 4. Set environment variables
```sh
export DB_HOST="your-rds-endpoint"
export DB_USER="your-db-username"
export DB_PASSWORD="your-db-password"
```
#### 5. Run the application

### 3. Test the Setup
   - Open **ALB DNS URL** in a browser.
   - Use **Postman or cURL** to test CRUD operations:
  ```sh
  curl -X POST http://your-alb-dns/items -H "Content-Type: application/json" -d '{"name": "Sample Item"}'
  ```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Troubleshooting & Common Issues
### 1. ALB Not Routing Traffic
- Ensure EC2 **Security Group allows ALB inbound traffic** on port **5000**.
- Verify **Target Group health checks** are passing.

### 2. RDS Connection Errors
- Ensure RDS **Public Access** is enabled (for testing).
- Check **Security Group** allows **EC2 IP access on port 3306/5432**.
- Use:
  ```sh
  telnet your-rds-endpoint 3306
  ```
  to test connectivity.

### 3. Application Not Starting
- Check logs: `journalctl -u your-app --no-pager -n 50`

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
