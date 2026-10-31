# AWS EC2 & ECS Docker Deployment

A simple Hello World web application containerized using Docker and deployed on **AWS EC2** and **Amazon ECS Fargate**. The Docker image is stored in Amazon Elastic Container Registry (ECR), and the application is publicly accessible.

## 🚀 Live Application

**EC2 Deployment:**  
http://3.107.9.244:8081/

**ECS Fargate Deployment:**  
http://3.26.151.244/

> Note: Public IP addresses can change when instances or ECS tasks are replaced. Update these URLs if the IP changes.

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Cloud Provider | Amazon Web Services (AWS) |
| Compute | Amazon EC2 |
| Container Orchestration | Amazon ECS Fargate |
| Container Registry | Amazon ECR |
| Operating System | Ubuntu Server 24.04 LTS |
| Containerization | Docker |
| Web Server | Nginx (Alpine) |
| Frontend | HTML, CSS |
| Version Control | Git and GitHub |

## 🏗️ Architecture

The application follows this deployment workflow:

1. Provision an Ubuntu EC2 instance on AWS.
2. Install and configure Docker on the EC2 instance.
3. Develop a static Hello World application using HTML and CSS.
4. Create a Docker image using the Nginx Alpine base image.
5. Run the Docker container on EC2 using port mapping `8081:80`.
6. Create an Amazon ECR repository and push the Docker image.
7. Configure an IAM role to allow EC2 to authenticate with Amazon ECR.
8. Create an ECS cluster using AWS Fargate.
9. Create an ECS task definition referencing the ECR image.
10. Deploy the application using an ECS service with one desired task.
11. Configure a public IP and security group to allow inbound traffic on port `80`.

## 📁 Project Structure

```text
aws-devops-task/
├── Dockerfile
├── index.html
└── README.md
```

## 🐳 Docker Configuration

The application uses Nginx Alpine to serve the static HTML page.

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

## ⚙️ EC2 Deployment

### 1. Connect to EC2

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

### 2. Install Docker

```bash
sudo apt update
sudo apt install -y docker.io

sudo systemctl start docker
sudo systemctl enable docker
```

### 3. Build the Docker Image

```bash
cd aws-devops-task

docker build -t hello-world-app .
```

### 4. Run the Container

```bash
docker run -d \
  --name hello-world \
  -p 8081:80 \
  hello-world-app
```

### 5. Verify the Deployment

```bash
docker ps

curl http://localhost:8081
```

Open the EC2 application:

```text
http://YOUR_EC2_PUBLIC_IP:8081
```

## 📦 Amazon ECR Configuration

An ECR repository named `hello-world-app` was created in the `ap-southeast-2` region.

**ECR Repository URI:**

```text
293532442732.dkr.ecr.ap-southeast-2.amazonaws.com/hello-world-app
```

### 1. Authenticate with ECR

The EC2 instance uses an IAM role named `EC2-ECR-Push-Role` with the `AmazonEC2ContainerRegistryPowerUser` managed policy.

```bash
aws sts get-caller-identity

aws ecr get-login-password --region ap-southeast-2 \
  | docker login \
  --username AWS \
  --password-stdin 293532442732.dkr.ecr.ap-southeast-2.amazonaws.com
```

### 2. Tag the Docker Image

```bash
docker tag hello-world-app:latest \
  293532442732.dkr.ecr.ap-southeast-2.amazonaws.com/hello-world-app:latest
```

### 3. Push the Image to ECR

```bash
docker push \
  293532442732.dkr.ecr.ap-southeast-2.amazonaws.com/hello-world-app:latest
```

## ☁️ Amazon ECS Fargate Deployment

The same Docker image stored in ECR was deployed using Amazon ECS Fargate.

### ECS Configuration

| Resource | Configuration |
|---|---|
| Cluster | `hello-world-cluster` |
| Launch Type | AWS Fargate |
| Task Definition | `hello-world-task:1` |
| Service | `hello-world-service` |
| Desired Tasks | 1 |
| CPU | 1 vCPU |
| Memory | 3 GB |
| Operating System | Linux/X86_64 |
| Container Port | 80 |
| Protocol | TCP |
| Image Registry | Amazon ECR |

### Deployment Steps

1. Create an ECS cluster named `hello-world-cluster`.
2. Create a task definition named `hello-world-task`.
3. Select AWS Fargate as the launch type.
4. Configure the container image using the ECR repository URI.
5. Set the container port to `80`.
6. Create an ECS service named `hello-world-service`.
7. Set the desired task count to `1`.
8. Configure the default VPC and public subnets.
9. Enable public IP assignment.
10. Configure a security group allowing inbound TCP traffic on port `80`.
11. Deploy and verify that the ECS task reaches the `RUNNING` state.

## 🔐 AWS Security Group Configuration

### EC2 Security Group

| Type | Protocol | Port | Source |
|---|---|---:|---|
| SSH | TCP | 22 | My IP |
| Custom TCP | TCP | 8081 | 0.0.0.0/0 |

### ECS Security Group

| Type | Protocol | Port | Source |
|---|---|---:|---|
| HTTP | TCP | 80 | 0.0.0.0/0 |

The ECS security group allows public HTTP access to the application running inside the Fargate task.

## 🖥️ Infrastructure Specifications

### EC2 Instance

| Resource | Configuration |
|---|---|
| Instance Type | t3.small |
| vCPU | 2 |
| RAM | 2 GiB |
| Storage | 15 GiB gp3 |
| Operating System | Ubuntu Server 24.04 LTS |

The `t3.small` instance was selected as a lightweight environment suitable for running Docker and hosting a simple static web application.

### ECS Fargate Task

| Resource | Configuration |
|---|---|
| CPU | 1 vCPU |
| Memory | 3 GB |
| Desired Tasks | 1 |
| Launch Type | Fargate |

Fargate was selected to deploy the container without managing the underlying servers.

## 📌 Useful Docker Commands

| Command | Purpose |
|---|---|
| `docker images` | List Docker images |
| `docker ps` | List running containers |
| `docker logs hello-world` | View container logs |
| `docker stop hello-world` | Stop the container |
| `docker start hello-world` | Start the container |
| `docker rm -f hello-world` | Remove the container |

## 🎯 Project Outcome

- Provisioned and configured an AWS EC2 instance.
- Installed Docker and containerized a static web application.
- Built and tested a custom Docker image.
- Configured public access through EC2 Security Groups.
- Created an Amazon ECR repository.
- Authenticated EC2 with ECR using an IAM role.
- Pushed the Docker image to Amazon ECR.
- Created an ECS Fargate cluster and task definition.
- Deployed the container using an ECS service.
- Verified the application through its public ECS task IP.

## 🔮 Future Improvements

- Configure HTTPS using a domain and TLS certificate.
- Automate image builds and deployments using GitHub Actions.
- Add application monitoring and centralized logging.
- Configure automated scaling for ECS services.

---

**Project:** AWS EC2 & ECS Docker Deployment  
**Status:** Successfully deployed on EC2 and Amazon ECS Fargate  
**Author:** Suman Ranjan