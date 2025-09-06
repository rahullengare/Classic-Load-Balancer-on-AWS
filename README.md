# Classic Load Balancer on AWS
Setup of a Classic Load Balancer in AWS to distribute traffic across EC2 servers.
## About Project:

This project demonstrates how to build a **highly available, fault-tolerant web application infrastructure** on **Amazon Web Services (AWS)** using **Classic Load Balancer (CLB)** with multiple EC2 instances.

Specifically, it involves launching **three Amazon Linux EC2 instances** running Apache Web Server and distributing traffic across them using a **Classic Load Balancer**.

The primary goals of this project are:

- **High Availability**: Incoming traffic is automatically distributed across multiple EC2 instances in different availability zones.
- **Load Distribution**: Ensures no single server is overloaded by balancing requests between multiple servers.
- **Fault Tolerance**: If one instance fails, traffic is redirected to healthy instances.
- **Scalability (Manual)**: More instances can be added behind the Classic Load Balancer to handle increased demand.

## Technology Used:

- **Amazon EC2** – For creating Linux servers
- **Amazon Classic Load Balancer** – For traffic distribution
- **Amazon VPC & Subnets** – For network configuration
- **Apache Web Server (httpd)** – For hosting a simple web page

## What is Load Balancer?

A **Load Balancer** is a networking solution that automatically distributes incoming traffic across multiple targets (servers) to ensure no single server is overloaded. It improves **application availability, fault tolerance, and scalability**.

## What is Classic Load Balancer?

The **Classic Load Balancer (CLB)** is one of AWS’s first-generation load balancers. It operates at both the **request level (Layer 7)** and **connection level (Layer 4)** and is designed for applications built within the **EC2-Classic network**.

## Step 1: Launch an EC2 Instance

1. Go to AWS Console → EC2 
2. Click Launch Instance.
    - Name → LB-Classic
    - Choose AMI → Amazon Linux.
    - Instance type → t2.micro
    - Key pair → pem_server_key
    - security group → launch-wizard-2
    - Add Script →
        
        ```bash
        #! /bin/bash
        sudo yum update -y
        sudo yum install -y httpd
        sudo systemctl start httpd
        sudo systemctl enable httpd
        echo "<h1>Now you are in $(hostname -f) server.</h1>" > /var/www/html/index.html
        ```
        
    - No of instance → 3
    - Click on Launch
    - After Launch Rename to (LB-Classic-a), (LB-Classic-c), (LB-Classic-c)

![Project Screenshot](/images/create-server1.png)
![Project Screenshot](/images/create-server2.png)
![Project Screenshot](/images/create-server3.png)
![Project Screenshot](/images/server-done.png)

## Step 2: Create a Classic Load Balancer

1. Go to AWS → EC2 service → Click On Load Balancer 
2. Load balancer name → Classic-LB
3. Network mapping → Select Different AZs 
    
    (Select at least one Availability Zone and one subnet for each zone.)
    
4. Security group → launch-wizard-2
5. Instances → Select 3 instances (a,b,c)
6. Click on Create Load Balancer

![Project Screenshot](/images/LB-create1.png)
![Project Screenshot](/images/LB-create2.png)
![Project Screenshot](/images/LB-create3.png)
![Project Screenshot](/images/LB-create4.png)
![Project Screenshot](/images/LB-create5.png)
![Project Screenshot](/images/LB-done.png)

## Step 3: Checking Running IP

1. Click On Load Balancer 
2. Select your Load Balancer 
3. Copy DNS Name 
    
    ```bash
    Classic-LB-1028813329.us-east-1.elb.amazonaws.com
    ```
    
4. Paste this DNS name and run 
5. Check to refers then your IP is Change 
6. 3 IP are see after repeated refreshed (Your Server is Highly Available)

![Project Screenshot](/images/server1.png)
![Project Screenshot](/images/server2.png)
![Project Screenshot](/images/server3.png)

## Step 4: Delete Classic Load Balancer

1. Click on Load Balancer 
2. Select LB you want to delete 
3. Click Action → Delete Load Balancer
4. Click on Delete 

![Project Screenshot](/images/delete-LB1.png)
![Project Screenshot](/images/delete-LB2.png)

## Step 5: Delete instance

1. Click on Instance 
2. Select instance you want to delete 
3. Click instance type 
4. Click on terminate or delete instance

![Project Screenshot](/images/delete-server-1.png)
![Project Screenshot](/images/delete-server-2.png)
