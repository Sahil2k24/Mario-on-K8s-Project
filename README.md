# 🎮 **Deploying Super Mario on Kubernetes** 🌟

![image](https://github.com/user-attachments/assets/49d90308-b5e4-44ed-b353-61ab9e0fc016)

Hey folks, remember the thrill of 90's gaming? Let's step back in time and relive those exciting moments! With the game deployed on Kubernetes, it's time to dive into the nostalgic world of Mario. Grab your controllers, it's game time!

Super Mario is a classic game loved by many. In this guide, we’ll explore how to deploy a Super Mario game on Amazon’s Elastic Kubernetes Service (EKS). Utilizing Kubernetes, we can orchestrate the game’s deployment on AWS EKS, allowing for scalability, reliability, and easy management.

## Prerequisites
Before you begin, ensure you have the following prerequisites:
- An Ubuntu Instance
- IAM role
- Terraform installed on instance
- AWS CLI and KUBECTL on Instance

## Let’s Deploy

### Step 1: Launch Ubuntu instance
- Sign in to AWS Console and navigate to EC2 Dashboard.
- Launch an instance with Ubuntu AMI, instance type t2.micro, and 8GB storage.

### Step 2: Create IAM role
- Search for IAM in the AWS console and create a role for EC2 with Administrator Access.

### Step 3: Cluster provision
- Clone the repository containing deployment scripts.
- Provide executable permission to script.sh and run it.
- Install Terraform, AWS CLI, Kubectl, and Docker.
- Navigate to the EKS-TF directory and run Terraform init, validate, and plan.
- Run Terraform apply to provision the cluster.

### Update the Kubernetes configuration
- Update the kubeconfig using the AWS CLI.

### Apply Deployment and Service
- Apply deployment and service configurations for Super Mario.
- Retrieve the LoadBalancer Ingress URL to access the game.

### Destruction
- Delete service and deployment for Mario.
- Destroy the cluster using Terraform.

After following these steps, the resources provisioned for the Super Mario game will be removed, ensuring that there are no lingering resources in your AWS environment.

This deployment not only brings back the nostalgia of 90's gaming but also showcases the capabilities of Kubernetes for orchestrating and managing containerized applications on cloud infrastructure. So, grab your controllers, dive into the nostalgic world of Mario, and enjoy the game!

This is an official image by MR CLOUD BOOK available on Docker Hub as sevenajay/mario:latest.
