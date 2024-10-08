# This guide provides instructions for deploying the Dockerized Calculation Tools project to AWS.

## 1. Prerequisites

Before deploying to AWS, ensure you have the following:

* **AWS Account**: An AWS account with appropriate permissions.
* **AWS CLI**: Install and configure the AWS CLI on your local machine.
* **Docker Installed**: Ensure Docker is installed and running on your local machine.

## 2. Pushing Docker Images to AWS ECR

### 2.1 Set Up Amazon ECR

1. **Create an ECR Repository**:

   * Navigate to the Amazon ECR console.
   * Create a new repository for your Docker images (e.g., `calculation-tools-repo`).

### 2.2 Authenticate and Push Docker Images

1. **Authenticate Docker to ECR**:

   * Run the following command to authenticate Docker with ECR:

     ``` 
     aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
     ```

1. **Tag and Push Your Docker Image**:

   * Tag the Docker image for your project:

     ``` 
     docker tag calculationtools_api-backend <aws_account_id>.dkr.ecr.<region>.amazonaws.com/calculation-tools-repo:latest
     ```

   * Push the Docker image to the ECR repository:

     ``` 
     docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/calculation-tools-repo:latest
     ```

## 3. Deploying Docker Containers on AWS

### 3.1 Using Amazon ECS

1. **Create an ECS Cluster**:

   * Go to the Amazon ECS console and create a new cluster.

1. **Define a Task Definition**:

   * Create a new task definition specifying the container image from ECR, along with CPU, memory, and networking configurations.

1. **Run the Task**:

   * Deploy the container by running the task in the ECS cluster.

### 3.2 Auto-Scaling and Load Balancing

1. **Configure Auto Scaling**:

   * Set up auto-scaling for your ECS service based on CPU and memory usage.

1. **Set Up a Load Balancer**:

   * Use an Application Load Balancer (ALB) to distribute traffic across multiple containers.

## 4. Post-Deployment Tasks

### 4.1 Monitoring and Logging

* Use Amazon CloudWatch to monitor your ECS cluster and containers.
* Set up CloudWatch Logs to capture and analyze container logs.

### 4.2 Security Configuration

* Use AWS IAM roles to ensure containers have the least privilege necessary.
* Configure security groups and network ACLs to control inbound and outbound traffic.

## 5. Maintenance and Updates

### 5.1 Updating Containers

* **Rebuild and Push**: When updates are made, rebuild the Docker image and push it to ECR.
* **Rolling Updates**: Deploy updates using ECS with minimal downtime.

### 5.2 Backup and Recovery

* Regularly back up any persistent data stored in AWS services.
* Document and test recovery procedures.
