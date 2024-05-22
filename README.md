<h1 align="center">AWS VPC Project Integrated With AutoScaling group and Application Load balancer</h1>

## Overview
This project involves setting up a Virtual Private Cloud (VPC) on Amazon Web Services (AWS) and creating an auto-scaling group to manage EC2 instances. We will also set up a bastion host for secure access to private instances and create an application load balancer for distributing traffic.


<p align="center">
  <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/61247779-827a-443b-befb-aadcbf9dd611" alt="AWS VPC Project Diagram" width="480" height="480" />
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

---

## Steps to Create an Auto Scaling Group

1. **Create Auto Scaling Group:**
   - Go back to the Auto Scaling Groups page.
   - Click on "Create Auto Scaling Group."
   - Select the launch template you created earlier.
   - Click "Next."

2. **Configure VPC and Subnets:**
   - Add the VPC you created earlier.
   - Select two private subnets for the auto-scaling group.
   - Click "Next."

        <p>
        <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/bd9aaf1b-3e2c-4b10-bb99-e39bac8ea346" alt="AWS VPC Project Diagram" width="600" height="400" />
        </p>
        
3. **Configure Load Balancer:**
   - Select "No load balancer" for now.
   - We will create an application load balancer separately in the public subnet but not for auto scaling group.
   - Click "Next."

4. **Set Capacity:**
   - Set the desired, minimum, and maximum capacity for your auto-scaling group.
   - Click "Next" until you reach the final step, then click "Create Auto Scaling Group."
  
        <p>
        <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/23508449-c89f-4d50-88df-1478228f9a90" alt="AWS VPC Project Diagram" width="600" height="400" />
        </p>

5. **Verify Auto Scaling Group:**
   - Ensure the instances are launched with private IPs.
  
        <p>
        <img src="https://github.com/mayaworld13/aws_vpc_project/assets/127987256/db72010e-ada6-47d8-aefe-910674caae68" alt="AWS VPC Project Diagram"  />
        </p>

An auto-scaling group has been created. Before creating the application load balancer, we need to install the application on the servers. To install the application on the servers, we need to log in to them, but they do not have public IPs. To log in to these servers, we need to launch a bastion host (instance) using the same VPC and key pair, with a public subnet. Additionally, we need to transfer this key to the instance.

---

## Steps to Set Up Bastion Host

1. **Launch Bastion Host:**
   - Launch an EC2 instance in the same VPC with a public subnet.
   - Use the same key pair.

2. **Transfer Key to Bastion Host:**
   - Use the following command to transfer the key to the bastion instance:
     ```sh
     scp -i your-key.pem your-key.pem ubuntu@<Bastion-Host-Public-IP>:/home/ubuntu
     ```

       <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/1408fc94-dd0b-4e58-97d3-917f9114419b" alt="AWS VPC Project Diagram"  />
        </p>


3. **Login to Private Instances:**
   - SSH into the bastion host.
   - From the bastion host, SSH into the private subnet instances using the transferred key.


        <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/8785bb92-4de0-4367-9d9e-f71a4619203f" alt="AWS VPC Project Diagram" width="600" height="400" />
        </p>
        
   - lets make the html page  which contain ip and hi hello in first private subnet
     
        <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/877de78b-d4c4-4794-b28d-baefb8eb121b" alt="AWS VPC Project Diagram"  />
        </p>
  
   - lets make another html page for another private subnet

        <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/c5963d3b-c1c7-49ba-b86b-61d4a5bc1584" alt="AWS VPC Project Diagram"  />
        </p>

   - run the application on port 8000 using this command

     ```bash
     python3 -m http.server 8000
     ```

---

## Steps to Create a Target Group

1. **Navigate to Target Groups:**
   - Open the EC2 console.
   - Click on "Target Groups" under the "Load Balancing" section on the left panel.

2. **Create Target Group:**
   - Click on "Create target group."
   - Choose "Instances" as the target type.
   - Enter a name for your target group.
   - Select the VPC where your instances are running.
   - Choose the protocol and port for routing traffic to the targets (e.g., my application is running on  port 8000).
   - Click "Next."

3. **Configure Health Checks:**
   - Configure the health check settings.
   - Set the protocol and path for health checks (e.g., HTTP and /).
   - Adjust the advanced health check settings if needed (e.g., healthy threshold, unhealthy threshold, timeout, interval).

4. **Register Targets:**
   - Select the instances you want to include in this target group.
   - Click "Include as pending below" to add the instances.
   - Click "Create target group."


        <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/f962990a-1e11-4de0-8411-e2dbbba8648e" alt="AWS VPC Project Diagram"  />
        </p>

5. **Verify Target Group:**
   - Ensure that your instances are showing as healthy in the target group.


        <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/f91d094c-194c-43ba-849f-f4904e6bcd0b" alt="AWS VPC Project Diagram" width="800" height="400" />
        </p>

---

## Associate Target Group with Load Balancer

1. **Navigate to Load Balancer Creation:**
   - Open the EC2 console.
   - Click on "Load Balancers" on the left panel.

2. **Create Application Load Balancer:**
   - Click on "Create Load Balancer."
   - Select "Application Load Balancer."
   - Configure it to be in the public subnet in same VPC.
   - Add the necessary listener and target group configurations.
   - Complete the setup.


        <p>
        <img src="https://github.com/mayaworld13/aws_vpc-project/assets/127987256/0f848a4e-21b1-44cb-af5c-02f62cf83a84" alt="AWS VPC Project Diagram" width="800" height="400" />
        </p>

    - if the listner port is not allowed then add in the security section of load balancer

---

## Test of Project

 - copy the dns of Load balancer browse it and let see what happens

   ![image](https://github.com/mayaworld13/aws_vpc-project/assets/127987256/173367ef-c9d1-4f11-99db-f57d8b33e471)

   ![image](https://github.com/mayaworld13/aws_vpc-project/assets/127987256/af440223-05dd-459c-9021-3d88ba0b6d21)


