Aws-3tier-architecture-demo
Hands-on implementation of a three-tier architecture on AWS using a single EC2 instance. This project demonstrates the separation of frontend, backend, and database layers, along with proper configuration, deployment, and communication between components to # AWS 3-Tier Architecture on AWS

## Project Overview

This project demonstrates the implementation of a highly available and secure 3-Tier Architecture on AWS using EC2, VPC, Application Load Balancer, NAT Gateway, and Amazon RDS.

The architecture follows industry best practices by separating the application into:

- Presentation Layer (Web Tier)
- Application Layer (App Tier)
- Data Layer (Database Tier)

This design improves security, scalability, maintainability, and fault tolerance.

---

# Architecture Diagram

![Architecture Diagram](architecture-diagram/3tier-architecture.png)

---

# Architecture Overview

```text
Internet
    |
Application Load Balancer
    |
Web Tier (EC2)
    |
Application Tier (EC2)
    |
Database Tier (RDS MySQL)
```

---

# AWS Services Used

| Service | Purpose |
|----------|----------|
| VPC | Network Isolation |
| Public & Private Subnets | Resource Segmentation |
| Internet Gateway | Internet Access |
| NAT Gateway | Outbound Internet Access |
| EC2 | Web and Application Servers |
| ALB | Load Balancing |
| RDS | Database |
| Security Groups | Firewall Rules |
| IAM | Access Management |
| CloudWatch | Monitoring |

---

# Network Design

## VPC Configuration

### CIDR Block

```text
10.0.0.0/16
```

### Subnets

| Subnet | CIDR |
|----------|----------|
| Public Subnet A | 10.0.1.0/24 |
| Public Subnet B | 10.0.2.0/24 |
| Private App A | 10.0.3.0/24 |
| Private App B | 10.0.4.0/24 |
| Private DB A | 10.0.5.0/24 |
| Private DB B | 10.0.6.0/24 |

---

# Layer 1: Presentation Layer (Web Tier)

## Purpose

The Web Tier acts as the entry point for client requests.

## Components

- Application Load Balancer
- EC2 Web Servers
- Public Subnets

## Responsibilities

- Receive HTTP/HTTPS requests
- Serve frontend content
- Route traffic to application servers
- Load balancing

## Security

- Internet-facing ALB
- Traffic allowed only through ports 80 and 443
- Web Servers accept traffic only from ALB

## Traffic Flow

```text
User
  |
  v
ALB
  |
  v
Web Tier EC2
```

---

# Layer 2: Application Layer (App Tier)

## Purpose

Processes business logic and application requests.

## Components

- EC2 Application Servers
- Private Subnets

## Responsibilities

- Process application requests
- Handle APIs
- Validate user data
- Communicate with database

## Security

- No public IP
- Accessible only from Web Tier

## Traffic Flow

```text
Web Tier
   |
   v
Application Tier
```

---

# Layer 3: Database Layer (Data Tier)

## Purpose

Stores persistent application data.

## Components

- Amazon RDS MySQL
- Database Subnets

## Responsibilities

- Store records
- Process queries
- Data persistence
- Backups

## Security

- Private access only
- Accessible only from App Tier

## Traffic Flow

```text
Application Tier
      |
      v
Amazon RDS
```

---

# Security Architecture

## Security Groups

### ALB Security Group

| Port | Source |
|--------|---------|
| 80 | Anywhere |
| 443 | Anywhere |

### Web Tier Security Group

| Port | Source |
|--------|---------|
| 80 | ALB SG |

### App Tier Security Group

| Port | Source |
|--------|---------|
| 8080 | Web Tier SG |

### Database Security Group

| Port | Source |
|--------|---------|
| 3306 | App Tier SG |

---

# Request Flow

```text
1. User sends request

2. Application Load Balancer receives request

3. ALB forwards request to Web Tier

4. Web Tier forwards request to Application Tier

5. Application Tier processes request

6. Application Tier queries RDS

7. Database returns data

8. Response sent back to user
```

---

# Deployment Steps

## Step 1 - Create VPC

- Create custom VPC
- Configure CIDR block

## Step 2 - Create Subnets

- Public Subnets
- Private Application Subnets
- Private Database Subnets

## Step 3 - Configure Internet Gateway

- Attach to VPC
- Update Route Tables

## Step 4 - Configure NAT Gateway

- Deploy NAT Gateway
- Configure Private Route Tables

## Step 5 - Configure Security Groups

- ALB SG
- Web Tier SG
- App Tier SG
- Database SG

## Step 6 - Launch EC2 Instances

- Web Tier Servers
- Application Tier Servers

## Step 7 - Create RDS Database

- MySQL Instance
- DB Subnet Group

## Step 8 - Create Application Load Balancer

- Target Group
- Listener Rules

## Step 9 - Verify Connectivity

- ALB → Web Tier
- Web Tier → App Tier
- App Tier → RDS

---

# High Availability Features

- Multi-AZ Deployment
- Load Balancing
- Redundant Subnets
- Private Network Isolation
- NAT Gateway for Secure Outbound Access

---

# Challenges Faced

## Challenge 1

Application could not connect to RDS.

### Cause

Incorrect Security Group configuration.

### Resolution

Allowed MySQL port (3306) from Application Security Group.

---

## Challenge 2

Private instances could not access internet.

### Cause

No NAT Gateway configured.

### Resolution

Configured NAT Gateway and updated Route Tables.

---

# Screenshots

## VPC

![VPC](screenshots/vpc-created.png)

## Subnets

![Subnets](screenshots/subnets-created.png)

## Security Groups

![Security Groups](screenshots/security-groups.png)

## Application Load Balancer

![ALB](screenshots/load-balancer.png)

## RDS

![RDS](screenshots/rds-instance.png)

## Final Output

![Output](screenshots/final-output.png)

---

# Key Learnings

- AWS Networking
- VPC Design
- EC2 Deployment
- Security Groups
- Application Load Balancer
- Amazon RDS
- High Availability Architecture
- Cloud Security Best Practices

---

# Future Improvements

- Auto Scaling Groups
- Route 53 Domain
- SSL/TLS Certificates
- CloudWatch Dashboard
- Terraform Automation
- CI/CD Pipeline using GitHub Actions

---

# Author

Anuj
DevOps | AWS | Cloud Computing