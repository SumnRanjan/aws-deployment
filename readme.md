# AWS EC2 Docker Deployment

A simple Hello World web application containerized using Docker and deployed on an AWS EC2 instance. The application is publicly accessible through port **8081**.

## 🚀 Live Application

**Application URL:** http://3.107.9.244:8081/

## 🛠️ Tech Stack

- **Cloud Provider:** Amazon Web Services (AWS)
- **Compute:** Amazon EC2
- **Operating System:** Ubuntu Server 24.04 LTS
- **Containerization:** Docker
- **Web Server:** Nginx (Alpine)
- **Frontend:** HTML and CSS
- **Version Control:** Git and GitHub

## 🏗️ Architecture

1. An Ubuntu EC2 instance is provisioned on AWS.
2. Docker is installed and configured on the EC2 instance.
3. A static Hello World application is created using HTML and CSS.
4. A Docker image is built using the Nginx Alpine base image.
5. The Docker container is started with port mapping `8081:80`.
6. AWS Security Group allows inbound TCP traffic on port `8081`.
7. Users can access the application through the EC2 public IP.

## 📁 Project Structure

```text
aws-docker-deployment/
├── Dockerfile
├── index.html
└── README.md
```

## 🐳 Docker Configuration

The application uses the following Dockerfile:

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

## ⚙️ Deployment Instructions

### 1. Connect to EC2

Connect to your Ubuntu EC2 instance using SSH:

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

Navigate to the project directory:

```bash
cd aws-devops-task
```

Build the image:

```bash
docker build -t hello-world-app .
```

### 4. Run the Container

```bash
docker run -d \
  --name hello-world \
  -p 8081:80 \
  hello-world-app
```

### 5. Verify the Container

```bash
docker ps
```

Test the application locally on EC2:

```bash
curl http://localhost:8081
```

### 6. Access the Application

Open the following URL in your browser:

```text
http://YOUR_EC2_PUBLIC_IP:8081
```

## 🔐 AWS Security Group Configuration

Configure the EC2 Security Group with these inbound rules:

| Type | Protocol | Port | Source |
|---|---|---:|---|
| SSH | TCP | 22 | My IP |
| Custom TCP | TCP | 8081 | 0.0.0.0/0 |

Port `8081` is publicly accessible so users can access the web application.

## 📌 Docker Commands

| Command | Purpose |
|---|---|
| `docker images` | List Docker images |
| `docker ps` | List running containers |
| `docker logs hello-world` | View container logs |
| `docker stop hello-world` | Stop the container |
| `docker start hello-world` | Start the container |
| `docker rm -f hello-world` | Remove the container |

## 🎯 Project Outcome

- Successfully provisioned an AWS EC2 instance.
- Installed and configured Docker.
- Built a custom Docker image.
- Deployed a containerized web application.
- Configured AWS Security Group rules for public access.
- Verified application accessibility through the EC2 public IP on port `8081`.

## 🔮 Future Improvements

- Push the Docker image to Amazon Elastic Container Registry (ECR).
- Deploy the same containerized application using Amazon ECS.
- Configure HTTPS using a domain and TLS certificate.
- Implement automated deployment using CI/CD.

---

**Project:** AWS EC2 Docker Deployment  
**Status:** Successfully deployed on AWS EC2  
**Author:** Suman Ranjan