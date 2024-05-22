<h1 align="center">AWS VPC Project Integrated With AutoScaling group and Application Load balancer</h1>

## Overview
This project involves setting up a Virtual Private Cloud (VPC) on Amazon Web Services (AWS) and creating an auto-scaling group to manage EC2 instances. We will also set up a bastion host for secure access to private instances and create an application load balancer for distributing traffic.

<p align="center">
  <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/d03cf9ab-b3a7-4b56-8826-5a197f9946bc" alt="AWS VPC Project Diagram" width="480" height="480" />
</p>

---

## Steps to Create the VPC

1. **Navigate to VPC Creation:**
   - Open the AWS Management Console.
   - Click on "VPC and more."

2. **Create VPC:**
   - Under the "Auto-generate" section, enter the name of your VPC.
   - Set NAT gateways to 1 per availability zone (Z).
   - Set endpoint as "none."
   - Leave all other settings as default.
   - Click on "Create VPC."

## Steps to Create a Launch Template

1. **Navigate to Auto Scaling Group:**
   - Open the EC2 console.
   - Click on "Auto Scaling Groups" on the left panel.

        <p>
        <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/f31903a7-5f85-4908-a4ba-b38582869265" alt="AWS VPC Project Diagram" width="600" height="400" />
        </p>

2. **Create Launch Template:**
   - Click on "Create launch template."
   - Provide a name and description for your template.
  
        <p>
        <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/24944d32-6169-40fc-a7aa-66accfbc63ba" alt="AWS VPC Project Diagram" width="600" height="400" />
        </p>
        
   - Create a new security group within the same VPC.
  
        <p>
        <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/2a0ac662-e91f-4aa8-a4d7-8d3b4662e18e" alt="AWS VPC Project Diagram" width="600" height="400" />
        </p>
        
   - Configure the template with the necessary instance details (AMI ID, instance type, key pair, etc.).
   - Click on "Create launch template."
