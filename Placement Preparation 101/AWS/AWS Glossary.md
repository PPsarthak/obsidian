---
date: 2025-09-27
tags:
  - aws
---
#aws

#### 🔑 Core Compute
EC2 (Elastic Compute Cloud) – Virtual servers (instances) you can provision, configure, and scale.
Lambda – Serverless compute service; run code without provisioning servers (event-driven).
ECS (Elastic Container Service) – Managed container orchestration for running Docker containers.
EKS (Elastic Kubernetes Service) – Managed Kubernetes clusters on AWS.
Fargate – Serverless compute engine for ECS/EKS; run containers without managing servers.
Lightsail – Simplified VPS hosting, often used for small apps, blogs, or websites.

#### 📦 Storage
S3 (Simple Storage Service) – Object storage for files, backups, and static hosting.
EBS (Elastic Block Store) – Block storage for EC2 instances (like virtual hard drives).
EFS (Elastic File System) – Managed NFS file system, can be mounted by multiple EC2 instances.
FSx – Managed file systems (e.g., Windows File Server, Lustre, NetApp).
Glacier – Archival storage for rarely accessed data, very low cost.
Storage Gateway – Hybrid cloud storage, bridging on-premises to AWS.

#### 🗄️ Databases
RDS (Relational Database Service) – Managed relational databases (MySQL, PostgreSQL, Oracle, SQL Server, Aurora).
Aurora – AWS proprietary high-performance relational database, MySQL/Postgres-compatible.
DynamoDB – NoSQL key-value and document database, fully managed.
ElastiCache – Managed in-memory cache service (Redis, Memcached).
Neptune – Managed graph database.
Redshift – Data warehouse for analytics at scale.
DocumentDB – MongoDB-compatible managed document database.
Timestream – Time-series database for IoT and metrics data.

#### 🌐 Networking & Content Delivery
VPC (Virtual Private Cloud) – Isolated virtual network for your AWS resources.
Route 53 – DNS and domain registration service.
API Gateway – Managed API management and gateway service.
CloudFront – CDN (Content Delivery Network) for caching content closer to users.
Direct Connect – Dedicated network link from on-premises to AWS.
Global Accelerator – Improves performance by routing traffic through AWS’s global network.

#### 🔐 Security, Identity & Compliance

IAM (Identity and Access Management) – User, role, and permission management.
Cognito – User authentication and access management for apps (social login, etc.).
KMS (Key Management Service) – Encryption key storage and management.
Secrets Manager – Store and rotate secrets like API keys and passwords.
GuardDuty – Threat detection and monitoring.
WAF (Web Application Firewall) – Protects apps from web exploits (SQL injection, XSS).
Shield – DDoS protection service.
Inspector – Automated security assessments for vulnerabilities.

#### 📊 Monitoring & Management
CloudWatch – Monitoring and logging service for AWS resources and apps.
CloudTrail – Logs all AWS API calls (auditing and compliance).
Config – Tracks resource configuration changes for compliance.
Systems Manager (SSM) – Manage and automate EC2 and hybrid cloud resources.
Trusted Advisor – Recommendations for cost optimization, performance, and security.

#### 🔄 Integration & Messaging
SQS (Simple Queue Service) – Message queuing service (decoupling apps).
SNS (Simple Notification Service) – Pub/Sub messaging and notifications.
EventBridge (formerly CloudWatch Events) – Event bus for app integration.
Step Functions – Serverless workflow orchestration.
AppSync – Managed GraphQL service.

#### 🤖 Machine Learning & AI
SageMaker – Build, train, and deploy ML models at scale.
Rekognition – Image and video analysis (face/object detection).
Polly – Text-to-speech service.
Lex – Conversational AI (chatbots, voice bots).
Comprehend – NLP service for text analysis.
Translate – Language translation.
Transcribe – Speech-to-text transcription.

#### 🛠️ Developer & DevOps Tools
CodeCommit – Managed Git repositories.
CodeBuild – Continuous integration service.
CodeDeploy – Automates code deployments to EC2, Lambda, or on-prem.
CodePipeline – CI/CD pipeline orchestration.
Cloud9 – Cloud-based IDE.
X-Ray – Tracing/debugging for distributed apps.

#### 🏢 Migration & Hybrid
DMS (Database Migration Service) – Migrate databases to AWS.
SMS (Server Migration Service) – Migrate on-premises VMs to AWS.
Snowball / Snowmobile – Physical data transfer appliances (for huge datasets).
Outposts – Run AWS services on-premises with AWS-managed hardware.

#### 💰 Billing & Cost Management
Cost Explorer – Analyze AWS spending.
Budgets – Set and track budget limits.
Billing & Cost Management Dashboard – Centralized billing and usage reporting.

