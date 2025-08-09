# High-Availability WordPress Infrastructure on AWS

## Overview

This CloudFormation template represents the culmination of my technical assessment for transitioning into a Solutions Architect role at AWS, completed under the direct mentorship of a Senior Solutions Architect Manager. The project demonstrates proficiency in designing and implementing enterprise-grade, scalable web infrastructure using AWS best practices.

## Architecture Highlights

### 🏗️ **Multi-Tier Architecture**
- **VPC Design**: Custom VPC with public and private subnets across 2 Availability Zones (us-west-2a, us-west-2b)
- **High Availability**: Multi-AZ deployment ensuring 99.9% uptime
- **Security**: Defense-in-depth strategy with multiple security groups and private subnet isolation

### 🚀 **Auto-Scaling & Load Balancing**
- **Application Load Balancer**: Internet-facing ALB with session stickiness
- **Auto Scaling Group**: Dynamic scaling based on CPU utilization (2-4 instances)
- **Health Checks**: ELB health checks with custom WordPress endpoints
- **Zero-Downtime**: Rolling deployments and automatic failover

### 💾 **Data & Storage Strategy**
- **RDS MySQL**: Multi-AZ deployment with automated backups (14-day retention)
- **EFS Integration**: Shared file system for WordPress uploads and themes
- **Parameter Store**: Secure configuration management for database credentials

### 🔒 **Security & Access**
- **EC2 Instance Connect**: Secure SSH access without bastion hosts
- **Security Groups**: Principle of least privilege access control
- **IAM Roles**: Fine-grained permissions for EC2 instances
- **VPC Flow Logs**: Network traffic monitoring and analysis

## Infrastructure Components

| Component | Purpose | Configuration |
|-----------|---------|---------------|
| **VPC** | Network isolation | 10.0.0.0/20 CIDR |
| **Public Subnets** | ALB placement | 2 subnets across AZs |
| **Private Subnets** | EC2/RDS isolation | 2 subnets across AZs |
| **NAT Gateways** | Outbound internet access | 2 gateways for redundancy |
| **RDS MySQL** | WordPress database | Multi-AZ, encrypted |
| **EFS** | Shared storage | Encrypted, general purpose |
| **ALB** | Load balancing | Internet-facing, HTTP/HTTPS |
| **ASG** | Auto scaling | 2-4 instances, CPU-based |

## Deployment Instructions

### Prerequisites
- AWS CLI configured with appropriate permissions
- EC2 Key Pair created in us-west-2 region
- CloudFormation deployment permissions

### Quick Deploy
```bash
# Clone the repository
git clone [your-repo-url]
cd wordpress-cloudformation

# Deploy the stack
aws cloudformation create-stack \
  --stack-name wordpress-ha \
  --template-body file://wordpress-alb-no-bastion-v2-asg-efs.yml \
  --parameters ParameterKey=KeyPairName,ParameterValue=your-key-pair \
               ParameterKey=DBMasterPassword,ParameterValue=SecurePassword123! \
  --capabilities CAPABILITY_IAM \
  --region us-west-2
```

### Parameters Configuration
Key parameters to customize:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `VpcCidr` | 10.0.0.0/20 | VPC CIDR block |
| `EC2InstanceType` | t3.small | Web server instance type |
| `DBInstanceType` | db.t3.medium | Database instance type |
| `KeyPairName` | *Required* | EC2 Key Pair for SSH access |
| `DBMasterPassword` | *Required* | MySQL root password |

## Monitoring & Observability

### CloudWatch Alarms
- **Database CPU**: Alert at 80% utilization
- **ASG Scaling**: CPU-based scaling policies
- **Custom Metrics**: WordPress-specific performance monitoring

### Scaling Policies
- **Scale Up**: CPU > 70% (Target Tracking)
- **Scale Down**: CPU < 40% for 15 minutes
- **Cooldown**: 5-minute intervals to prevent flapping

## Security Considerations

### Network Security
- Private subnets for compute and database tiers
- Security groups with minimal required access
- VPC Flow Logs for traffic analysis

### Access Control
- EC2 Instance Connect for secure SSH (no bastion required)
- IAM roles with least-privilege permissions
- SSM Parameter Store for sensitive configuration

### Data Protection
- RDS encryption at rest
- EFS encryption at rest and in transit
- Automated backups with 14-day retention

## WordPress Optimizations

### Performance Enhancements
- PHP 8.2 with optimized configuration
- Increased memory limits for data processing
- EFS for scalable shared storage
- Session stickiness for consistent user experience

### Load Balancer Configuration
- Health checks on WordPress static assets
- X-Forwarded headers for proper client IP handling
- SSL termination ready (HTTPS support)

## Outputs & Integration

The template provides comprehensive outputs for integration with other systems:
- VPC and subnet IDs for additional resources
- Security group IDs for service integration
- Database endpoint for application configuration
- Load balancer DNS for domain configuration

## Skills Demonstrated

✅ **AWS Architecture**: Multi-tier, highly available infrastructure design  
✅ **Infrastructure as Code**: Advanced CloudFormation templating  
✅ **Security**: Defense-in-depth, least-privilege access  
✅ **Automation**: Auto-scaling, self-healing infrastructure  
✅ **Monitoring**: CloudWatch integration and alerting  
✅ **Best Practices**: AWS Well-Architected Framework principles

## Cost Optimization

The template includes several cost optimization features:
- Burstable performance instances (t3 family)
- GP3 storage for cost-effective IOPS
- Target tracking scaling to minimize idle resources
- Reserved capacity recommendations for production workloads

---

*This infrastructure template showcases enterprise-grade AWS architecture patterns and demonstrates readiness for Solutions Architect responsibilities in designing, implementing, and maintaining scalable cloud solutions.*
