# Automated CI/CD Pipeline for Deploying a Scalable Web Application (PostgreSQL, Node.js, React) on AWS using Jenkins, Ansible, Docker, and Terraform

## Project Overview

This project focuses on creating a Jenkins pipeline to deploy a web application built with Node.js and React on AWS Cloud Infrastructure using Ansible for configuration management and Docker for containerization. The infrastructure consists of one Jenkins server, serving as the Ansible control node, and three EC2 instances dedicated to PostgreSQL, Node.js, and React. These components are managed in Docker containers. The deployment leverages AWS services, Terraform for infrastructure setup, and AWS ECR for Docker image storage.

## Problem Statement

The DevOps team is tasked with this deployment using Ansible and Jenkins. The application comprises:
- PostgreSQL: Database server, accessible on port 5432.
- Node.js: Backend server, accessible on port 5000.
- React: Frontend server, accessible on port 3000.

The deployment requirements are:
- Web application accessible via web browser on port 3000.
- EC2 instances and security groups created using Terraform.
- The deployment process controlled from the Jenkins server.
- Application components containerized with Docker.

## Requirements
### Infrastructure Setup:
- Create EC2 instances and security groups using Terraform.
- Security groups should allow necessary traffic (PostgreSQL: 5432, Node.js: 5000, React: 3000).

### Jenkins Configuration:
- Use a Jenkins server as the control node.
- Pull application code from the private GitHub repository into Jenkins.
- Manage EC2 instances using dynamic inventory (inventory_aws_ec2.yml).
- Place the Ansible configuration file (ansible.cfg) on the Jenkins server.
- Install Docker on all worker nodes using Ansible.
- Build Docker images on the Jenkins server and push them to AWS ECR.

### Component Deployment:
#### PostgreSQL:
- Create Docker image for PostgreSQL container using dockerfile-postgresql.
- Place init.sql file in the necessary folder.
- Build the Docker image on the Jenkins server and push it to AWS ECR.
- Deploy PostgreSQL container on the worker node, setting the password as an environmental variable.
- Ensure security group allows traffic from Node.js EC2 on port 5432 and port 22 from anywhere.
- Configure Docker volumes to persist database data.

#### Node.js:
- Update the .env file in the server folder with PostgreSQL environment variables using envsubst.
- Create Docker image for Node.js container using dockerfile-nodejs.
- Build the Docker image on the Jenkins server and push it to AWS ECR.
- Deploy Node.js container on the worker node, exposing port 5000.
- Ensure security group allows traffic on port 5000 and port 22 from anywhere.

#### React:
- Update the .env file in the client folder with Node.js environment variables using envsubst.
- Create Docker image for React container using dockerfile-react.
- Build the Docker image on the Jenkins server and push it to AWS ECR.
- Deploy React container on the worker node, exposing port 3000.
- Ensure security group allows traffic on port 3000 and port 80 from anywhere.

#### AWS ECR:
- Use AWS ECR as the image repository.
- Enable instances with IAM roles for ECR access.
- Install AWS CLI v2 on worker nodes to use aws ecr commands.

#### Jenkins Pipeline:
- Launch a Jenkins server with a security group allowing SSH (port 22) and HTTP (ports 80, 8080).
- Use a pre-configured Terraform file for the Jenkins server.
- Create a Jenkins pipeline with stages for:
        - Infrastructure setup using Terraform.
        - Creating an ECR repository.
        - Building and pushing Docker images to ECR.
        - Deploying the application on EC2 instances using Ansible.
        - Cleaning up infrastructure and resources on failure.

## Project Skeleton

```bash
project-208
│
├── Readme.md                  # Project description and instructions
├── dockerfile-postgresql      # Dockerfile for PostgreSQL
├── dockerfile-nodejs          # Dockerfile for Node.js
├── dockerfile-react           # Dockerfile for React
├── main.tf                    # Terraform file for managed nodes
├── Jenkinsfile                # Jenkins pipeline script
├── Ansible-Playbook           # Ansible playbooks
├── student_files              # Application code (PostgreSQL, Node.js, React)
│   ├── server                 # Node.js backend code
│   ├── client                 # React frontend code
│   └── database               # Database initialization SQL file
├── ansible.cfg                # Ansible configuration file
├── inventory_aws_ec2.yml      # Ansible dynamic inventory file
├── install-jenkins.tf         # Terraform file for Jenkins server
├── variables.tf               # Terraform variables for Jenkins server
├── jenkins.sh                 # Shell script for Jenkins setup
├── node-env-template          # Template for Node.js environment variables
└── react-env-template         # Template for React environment variables
```

## Expected Outcome
### At the end of the project, you will learn:

- Jenkins Pipeline Configuration
- Creating infrastructure with Terraform
- Deploying applications with Ansible
- Ansible playbook preparation
- Docker image creation and management
- Docker container deployment using Ansible
- AWS ECR for Docker image storage
- AWS IAM Policy and Role Configuration
- AWS EC2 Launch Template and Security Group Configuration
- Git and GitHub for version control

## Learning Objectives
By the end of this project, students will be able to:

- Configure Jenkins pipelines for infrastructure and application deployment.
- Use Terraform to create and manage AWS infrastructure.
- Deploy applications using Ansible and Docker.
- Create and manage Docker images and containers.
- Use AWS ECR for storing and managing Docker images.
- Configure AWS IAM roles, policies, and security groups.
- Apply Git commands and use GitHub for version control.

## Resources

- [Ansible Documentation](https://docs.ansible.com/ansible/2.5/user_guide/index.html)

- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)

- [Authenticating Amazon ECR Repositories for Docker CLI with Credential Helper](https://aws.amazon.com/blogs/compute/authenticating-amazon-ecr-repositories-for-docker-cli-with-credential-helper/)

- [Docker Reference Page](https://docs.docker.com/reference/)

- [Jenkins Handbook](https://www.jenkins.io/doc/book/)