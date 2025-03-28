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
To access the AWS console, use the credentials provided to you and follow the steps below:<br />
1. Access the AWS Console using the provided link (or if unavailable, [click here](https://console.aws.amazon.com/console/)). Use the login credentials (Account ID/alias, username, password) provided and click the <strong>"Sign In"</strong> button.
<img width="1454" alt="Screenshot 2025-03-28 at 4 51 44 PM" src="https://github.com/user-attachments/assets/d7d1c782-98a1-42ff-b548-5deb84d9255c" /><br />

2. Make sure to provision resources in the Asia Pacific (Sydney) or ap-southeast-2 only.
<img width="1455" alt="Screenshot 2025-03-28 at 5 01 55 PM" src="https://github.com/user-attachments/assets/30806eee-2ff2-4471-a6b1-1ed1ed8b2431" /><br />

> Soft reminder: <strong>Please be mindful of your costs. 💰</strong> Rest assured that you have just the sufficient access to services. But, make sure to turn off unused resources and clean up if you are exploring. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Deployment Steps
### Step 1 - Set Up AWS Resources
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
1. On the console search bar, enter "ec2" and <strong>select the "EC2"</strong> on the resulting options.
<img width="1455" alt="Screenshot 2025-03-28 at 5 26 13 PM" src="https://github.com/user-attachments/assets/c5bdf37b-27fa-485a-a05d-108abceb44ec" /><br />

2. Select the <strong>"Launch Instance" button</strong> on the righthand side of the page.
<img width="1455" alt="Screenshot 2025-03-28 at 5 35 32 PM" src="https://github.com/user-attachments/assets/7cbad07d-d084-4a71-a0b0-dbc459569ff3" /><br />

3. Specify an instance name of your choice, select <strong>"Ubuntu"</strong> as the AMI, and declare <strong>2 instances</strong> on the summary tab.
<img width="1458" alt="Screenshot 2025-03-28 at 5 41 19 PM" src="https://github.com/user-attachments/assets/d8480d08-6a44-4a5a-91d3-3955b470206f" /><br />

4. Select <strong>"Proceed without a key pair"</strong> on the Key Pair (login) section. Make sure that <strong>"t2.micro"</strong> is selected on the instance type.
<img width="1456" alt="Screenshot 2025-03-28 at 5 47 06 PM" src="https://github.com/user-attachments/assets/8d931582-4b29-4eae-91b9-dc142c9311bd" /><br />

5. Scroll down to the Network Settings section, select the <strong>Edit</strong> button on its top right side.
<img width="1456" alt="Screenshot 2025-03-28 at 5 52 41 PM" src="https://github.com/user-attachments/assets/ed5a9786-2800-4771-bd13-811d906e315f" /><br />

6. In the VPC and subnets dropdowns, select the <strong>specified VPC and specified subnet</strong>.
<img width="1456" alt="Screenshot 2025-03-28 at 5 56 33 PM" src="https://github.com/user-attachments/assets/6941469b-71c5-4ab9-ba9d-0a7eaa3d3b65" /><br />

7. Choose the <strong>"Select existing security group"</strong> option, and select the <strong>specified security group</strong> in the security groups dropdown.
<img width="1456" alt="Screenshot 2025-03-28 at 6 00 44 PM" src="https://github.com/user-attachments/assets/86edba1e-d921-4713-b2ce-32dcabb6f05a" /><br />

8. Scroll down to the bottom, and click the <strong>"Advanced Details"</strong> dropdown.
<img width="1456" alt="Screenshot 2025-03-28 at 6 07 30 PM" src="https://github.com/user-attachments/assets/2be6868d-52c9-4a4a-a4c8-46b5ec159b6e" /><br />

10. Scroll down (i know, again) to the bottom, and paste [this](https://github.com/jcmsj/ust-workshop/blob/main/sudoaws.sh) in the user data text box.
<img width="1456" alt="Screenshot 2025-03-28 at 6 10 05 PM" src="https://github.com/user-attachments/assets/68339b03-55ab-4550-8908-8a417ea97d43" /><br />

11. After pasting the user data script, click the "Launch instance" button on the right bottom righthand side.
<img width="1456" alt="Screenshot 2025-03-28 at 6 14 36 PM" src="https://github.com/user-attachments/assets/3e45114c-4018-4a80-ad57-c7dceb6277e6" /><br />

> Note: Make sure to get a <strong>green banner</strong> like this, initiating a successful creation of 2 instances! ✅<br />
>
> <img width="1456" alt="Screenshot 2025-03-28 at 6 17 42 PM" src="https://github.com/user-attachments/assets/1f9f7209-6014-43ce-8d94-7b2bca2c9d36" />

#### C. Set Up Application Load Balancer (ALB)

Sample description

### 2. Deploy Application to EC2
#### 1. Connect to your instances using "EC2 instance connect"
#### 2. Install dependencies</h4>
```sh
sudo yum update -y
sudo yum install ...
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
