<img width="1456" alt="Screenshot 2025-03-31 at 10 33 10 AM" src="https://github.com/user-attachments/assets/7b17ac2c-bf30-4aa0-9932-08fd0fab1f1f" /><p align="center"><img width="200" alt="Leads" src="https://github.com/user-attachments/assets/95ebd1a9-59d6-427a-9948-8217db510d31"/></p>
<p align="center"><img width="200" alt="CTP" src="https://github.com/user-attachments/assets/96189c71-72ed-4729-a9b2-972489b2344d"/></p>

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

![CRUD App Architecture (UST) drawio](https://github.com/user-attachments/assets/e302bb14-12dd-4e11-a0f7-564681f7883b)

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

### Step 1 - Create RDS Database

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
DB Instance Identifier = "ctp"
Master username = "root"
Master password = "ustworkshop"
Confirm password = "ustworkshop"
```

<img width="1456" alt="Screenshot 2025-03-28 at 3 51 59 PM" src="https://github.com/user-attachments/assets/27c523aa-9e9a-46dc-ad5c-62b818da721c" /><br />

7. Then, navigate to the <strong>"VPC Security Group (Firewall)"</strong> under the Connectivity section, then select the <strong>Workshop VPC</strong> on the VPC drop down options, and select <strong>workshop-dbsubnetgroup</strong> on the DB Subnet Group drop down options.
<img width="1456" alt="Screenshot 2025-03-31 at 10 01 53 AM" src="https://github.com/user-attachments/assets/8ffae2a7-2a0f-4e70-b9dd-8d388eebc748" /><br />

8. Scroll down to the security group drop down, and select the option containing <strong>DBSecurityGroup</strong>.
<img width="1456" alt="Screenshot 2025-03-31 at 10 02 10 AM" src="https://github.com/user-attachments/assets/7bac359a-d61e-44d7-a3cb-af15071d237f" /><br />

9. Click the <strong>"Create Database"</strong> button on the bottom-most right side.
<img width="1452" alt="Screenshot 2025-03-28 at 4 33 03 PM" src="https://github.com/user-attachments/assets/55fd0e37-6881-4f94-afde-0d8c1594ebdf" /><br />

> Note: Before proceeding, wait for the <strong>green banner</strong> on the top to show that the RDS is succesfully created.<br />
> This takes <strong>around 3-5 minutes</strong>, so grab a cup of coffee while waiting! ☕️ <br />
>
> <img width="1156" alt="Screenshot 2025-03-28 at 4 35 40 PM" src="https://github.com/user-attachments/assets/7ba11fec-3bf4-49ab-a503-d9e9bac707ee" /><br />

10. Once done, take note of the "RDS Endpoint", you will use this later :)
<img width="1456" alt="Screenshot 2025-03-31 at 10 45 30 AM" src="https://github.com/user-attachments/assets/9ec4399f-7f3c-4e0c-83ca-05eb321423e7" /><br />

<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Step 2 - Create EC2 Instances

#### Step 2.1 - Create 1st EC2 Instance
1. On the console search bar, enter "ec2" and <strong>select the "EC2"</strong> on the resulting options.
   <img width="1455" alt="Screenshot 2025-03-28 at 5 26 13 PM" src="https://github.com/user-attachments/assets/c5bdf37b-27fa-485a-a05d-108abceb44ec" /><br />

2. Select the <strong>"Launch Instance" button</strong> on the righthand side of the page.
   <img width="1455" alt="Screenshot 2025-03-28 at 5 35 32 PM" src="https://github.com/user-attachments/assets/7cbad07d-d084-4a71-a0b0-dbc459569ff3" /><br />

3. Specify an instance name of your choice, select <strong>"Ubuntu"</strong> as the AMI.
   <img width="1458" alt="Screenshot 2025-03-28 at 5 41 19 PM" src="https://github.com/user-attachments/assets/d8480d08-6a44-4a5a-91d3-3955b470206f" /><br />

4. Select <strong>"Proceed without a key pair"</strong> on the Key Pair (login) section. Make sure that <strong>"t2.micro"</strong> is selected on the instance type.
   <img width="1456" alt="Screenshot 2025-03-28 at 5 47 06 PM" src="https://github.com/user-attachments/assets/8d931582-4b29-4eae-91b9-dc142c9311bd" /><br />

5. Scroll down to the Network Settings section, select the <strong>Edit</strong> button on its top right side.
   <img width="1456" alt="Screenshot 2025-03-28 at 5 52 41 PM" src="https://github.com/user-attachments/assets/6f480140-b524-4b0a-9b79-f190fd2a05e2" /><br />

6. In the VPC and subnets dropdowns, select the <strong>Workshop VPC</strong> on the VPC Dropdown, and <strong>Workshop-AppPrivateSubnet1</strong> on the subnet dropdown.
  <img width="1456" alt="Screenshot 2025-03-31 at 10 25 10 AM" src="https://github.com/user-attachments/assets/b714fc12-a0eb-4151-9f7d-b35502fa1130" />
 <br />

7. Enable the <strong> Select Existing Security Group</strong> option, select the subnet group containing <strong>AppSecurityGroup</strong>.
   <img width="1456" alt="Screenshot 2025-03-31 at 10 25 16 AM" src="https://github.com/user-attachments/assets/5df3575f-5d5f-433f-bb94-20bff3d98e86" /><br />

8. Scroll down to the bottom, and click the <strong>"Advanced Details"</strong> dropdown.
   <img width="1456" alt="Screenshot 2025-03-31 at 10 33 10 AM" src="https://github.com/user-attachments/assets/e6c7c67a-a0a9-4a59-8ca5-a4b64ea2c309" /><br />

9. Scroll down to the bottom, and paste [this](https://github.com/jcmsj/ust-workshop/blob/main/sudoaws.sh) in the user data text box.
   <img width="1456" alt="Screenshot 2025-03-31 at 10 34 29 AM" src="https://github.com/user-attachments/assets/26b40f50-b67f-4144-a804-1316bc00f64c" /><br />

10. Replace the "$DB_HOST" with the RDS Endpoint from the
   ```sh
   sed -i 's/DB_HOST=127.0.0.1/DB_HOST=$DB_HOST/g' .env
   ```
11. After pasting the user data script, click the "Launch instance" button on the right bottom righthand side.
    <img width="1456" alt="Screenshot 2025-03-28 at 6 14 36 PM" src="https://github.com/user-attachments/assets/3e45114c-4018-4a80-ad57-c7dceb6277e6" /><br />

#### Step 2.2 - Create 2nd EC2 Instance

> Note: Make sure to get a <strong>green banner</strong> like this, initiating a successful creation of 2 instances! ✅<br />
>
> <img width="1456" alt="Screenshot 2025-03-28 at 6 17 42 PM" src="https://github.com/user-attachments/assets/1f9f7209-6014-43ce-8d94-7b2bca2c9d36" />

### Step 3 - Set Up Application Load Balancer (ALB)

1. Back in the EC2 console, click Load Balancers in the left-hand menu.
![1](https://github.com/user-attachments/assets/f2e8fbf9-6e83-43ec-b2f7-81316ca67499)

2. Click Create Load Balancer.
![2](https://github.com/user-attachments/assets/bb21b006-cf52-4af3-bc27-58249b31b65d)

3. From the Application Load Balancer card, click Create.
![3](https://github.com/user-attachments/assets/69175485-f02f-4956-9546-77ac8a483fa7)

4. For Load balancer name, enter Workshop-ALB.
![4](https://github.com/user-attachments/assets/0528c1fb-6cc5-4dde-9d39-77f3a7f1d9ed)

5. Under Network mapping, click the VPC dropdown, and select the listed Workshop-VPC.
![5](https://github.com/user-attachments/assets/ffb6756c-04f3-476e-936b-838eae585cac)

6. When the Availability Zones list pops up, select each one (us-east-1a, us-east-1b, and us-east-1c).
![6](https://github.com/user-attachments/assets/692769a8-5721-4cf6-8f39-445bb54c8e5a)

7. Under Security groups, deselect the default security group listed, and select the one from the dropdown with ctp-ALBSecurityGroup in its name.
![7](https://github.com/user-attachments/assets/c8ded176-8c0e-437a-87a8-87da1415e43b)

8. Under Listeners and routing, ensure that the Protocol is set to HTTP and the Port is 80. Then, under Default action, click Create target group This will open a new tab. Keep this first tab open to complete later.
![8](https://github.com/user-attachments/assets/1356c033-de81-4177-9fe4-755c0a58acf5)

9. For Target group name, enter Workshop-TG.
![image](https://github.com/user-attachments/assets/8cf3b3e8-fe17-43de-9a08-0bdd11a5bb8b)

10. Select the VPC.
![image](https://github.com/user-attachments/assets/b7b4413a-0c73-4d94-9100-a8858a18d537)

11. Under the health check path, type "/up".
![image](https://github.com/user-attachments/assets/26d10b10-796f-4843-a237-7c04e39c315b)

12. Click Next. Under Available instances, select both targets that are listed. Then click Include as pending below.
![image](https://github.com/user-attachments/assets/2cb059c4-15cf-4d6a-8e01-07539736b169)

13. Click Create target group.
![image](https://github.com/user-attachments/assets/8e8893c6-aa56-4f3a-bd2b-677f8a06d33c)

14. Back in the first tab, under Default action, click the refresh button (looks like a circular arrow), and in the dropdown, select the TargetGroup you just created.
![image](https://github.com/user-attachments/assets/013d8aa5-f7b4-4cfa-8099-d3e668510d69)

15. Click Create load balancer. Wait a few minutes for the load balancer to finish provisioning and enter an active state.
![image](https://github.com/user-attachments/assets/fc7ce96f-f097-4729-848a-3fb96751cb5d)

16. Copy its DNS name, and paste it into a new browser tab. You should see the application now.
![image](https://github.com/user-attachments/assets/f8e0e5e7-5670-4761-9093-15e801e390ed)

### Step 4 - Enable Target Group's Sticky Sessions

1. Back on Load Balancers page, find the Listeners tab and click the TargetGroup link in the Default action column
![image](https://github.com/user-attachments/assets/b81d7fb1-369b-41fa-9b49-748e2eb1afca)

2. Select the Attributes tab.
![image](https://github.com/user-attachments/assets/4a730c5d-8262-4ae5-a0ae-eea3ef77a82b)

3. Click Edit.
![image](https://github.com/user-attachments/assets/4143902c-bb4f-4322-9099-0174e743b19a)

4. Check the box next to Stickiness to enable it. Leave Stickiness type set to Load balancer generated cookie.
![image](https://github.com/user-attachments/assets/51fb7e3a-8c6c-42ef-8548-689b2766fd37)

5. Leave Stickiness duration set to 1 days.
![image](https://github.com/user-attachments/assets/08a3e374-2bee-4f34-877e-df57714d0a86)

6. Click Save changes
![image](https://github.com/user-attachments/assets/f3f00bdd-0192-4187-8f19-4b67eb220c75)

7. Refresh the tab where you navigated to the load balancer's public IP. This time, no matter how many times you refresh, it will stay on the same instance (noted by the local IP).✅

### Step 5 - Start accessing your app!

Congratulations! You've successfully deployed a web application using AWS. Along the way, you worked with these key services:

✅ EC2 – For hosting your application
✅ RDS – For managing your database
✅ ALB – For handling traffic efficiently

Great job! Keep exploring and experimenting with AWS to deepen your skills. 🚀

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

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Wrap up and clean resources


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
