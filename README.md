# Deploying a Node.js Application on AWS EKS with Helm

The aim of this project is to demonstrate a streamlined process for deploying a NodeJS application on an AWS Elastic Kubernetes Service (EKS) cluster. Leveraging Docker for containerization, Terraform for infrastructure provisioning, and Helm for Kubernetes application deployment, this project provides a comprehensive guide for setting up a scalable and maintainable environment.

## Prerequisites

- AWS account with necessary permissions.
- EC2 instance (e.g., t2.medium) running Ubuntu.
- Access to the instance via SSH.

## Steps

### 1. Update and Upgrade the System

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install Required Tools

- Docker 
- awscli 
- eksctl 
- kubectl 
- terraform 
- helm

### 3. Configure Docker for Non-Sudo Access

```bash
sudo usermod -aG docker $USER
```

### 4. IAM User and Access Key

- Create an IAM user with required policies for Terraform and EKS.
- Generate Access Key and Secret Access Key.

### 5. AWS CLI Configuration

```bash
aws configure
```

Enter the Access Key, Secret Access Key, and preferred region.

### 6. Sample Node.js Code

Create or pull a sample Node.js code and test it locally.

### 7. Dockerize the Node.js Application

```bash
docker build -t <image-name> .
```

```bash
docker run -itd -p <host-port>:<container-port> <image>
```

 - Tip: Open the required port on the EC2 instance's security group.

### 8. Push Docker Image to Docker Hub

```bash
docker login
```

```bash
docker push <repository-name/image-name>
```

### 9. Terraform Setup

- Configure provider.tf and EKS.tf.
- Run terraform init to download required modules.
- Run terraform plan and terraform apply -auto-approve to deploy resources.

### 10. Update Kubeconfig

```bash
aws eks --region <your_aws_region> update-kubeconfig --name <your_eks_cluster_name>
kubectl get all
```

### 11. Helm Chart Configuration

```bash
helm create <chart-name>
cd <chart-name>
sudo nano values.yml
```

### 12. Configure image, service, and other settings in "values.yml" file.

```yml
image:
    repository: <image-name>
    pullPolicy: IfNotPresent
    # Overrides the image tag whose default is the chart appVersion.
    tag: "<image-tag>"
```

```yml
service:
    type: LoadBalancer
    port: <container-port>"
```

 - Tip: Increase the replica count in the "values.yml" file for high availability.

### 12. Helm Deployment

```bash
helm install <deployment-name> <chart-name>
```

```bash
kubectl get svc
```

 - Tip: Run `helm lint <chart-name>` and `helm install <deployment-name> --dry-run <chart-name>` to validate the deployment.

### 13. Access the Application

Fetch the LoadBalancer URL from the output of the previous command and access the application on any browser.
