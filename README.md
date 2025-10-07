# AWS Cost Optimization Suite

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange.svg)

## 📋 Project Summary

The AWS Cost Optimization Suite is a comprehensive toolkit designed to help organizations reduce their AWS infrastructure costs through automation, monitoring, and intelligent resource management. This suite combines Python automation scripts, serverless Lambda functions, and CloudWatch dashboards to provide:

- **Automated Resource Discovery**: Identifies unused and underutilized AWS resources
- **Rightsizing Recommendations**: Analyzes instance metrics and suggests optimal configurations
- **Cost Analysis Reports**: Generates detailed reports with actionable insights
- **Real-time Monitoring**: CloudWatch dashboards for continuous cost tracking
- **Automated Cleanup**: Lambda functions for scheduled resource optimization

## 🚀 Features

- 🔍 Identify unused EC2 instances, EBS volumes, and EIPs
- 📊 Generate cost savings reports
- ⚙️ Automated rightsizing for EC2 and RDS instances
- 📈 CloudWatch dashboard integration
- 🔔 SNS notifications for cost alerts
- 🤖 Serverless Lambda functions for automation
- 📝 Detailed logging and audit trails

## 🛠️ Setup Instructions

### Prerequisites

- Python 3.8 or higher
- AWS Account with appropriate IAM permissions
- AWS CLI configured with credentials
- boto3 library installed

### IAM Permissions Required

Your AWS user/role needs the following permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics",
        "ce:GetCostAndUsage",
        "rds:Describe*",
        "s3:ListBucket",
        "lambda:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

### Installation

1. Clone the repository:

```bash
git clone https://github.com/sandeepkatakam21/aws-cost-optimization-suite.git
cd aws-cost-optimization-suite
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Configure AWS credentials:

```bash
aws configure
```

4. Set up environment variables:

```bash
export AWS_REGION=us-east-1
export SNS_TOPIC_ARN=arn:aws:sns:region:account-id:topic-name
```

## 📖 Usage Examples

### 1. Identify Unused EC2 Instances

```bash
python scripts/find_unused_ec2.py --region us-east-1 --days 30
```

This script identifies EC2 instances with CPU utilization below 5% for the past 30 days.

**Output:**
```
Found 5 unused EC2 instances:
- i-1234567890abcdef0 (t2.micro) - Potential savings: $8.76/month
- i-0987654321fedcba0 (t3.medium) - Potential savings: $30.37/month
Total potential monthly savings: $39.13
```

### 2. Find Unused EBS Volumes

```bash
python scripts/find_unused_ebs.py --region us-west-2
```

Identifies unattached EBS volumes that are incurring costs.

### 3. Rightsizing Recommendations

```bash
python scripts/rightsizing_recommendations.py --instance-type all --region us-east-1
```

Analyzes CloudWatch metrics to recommend optimal instance types.

**Sample Output:**
```
Rightsizing Recommendations:
- i-1234567890abcdef0: Current: m5.xlarge → Recommended: m5.large (Save 50%)
- i-abcdef1234567890: Current: t3.2xlarge → Recommended: t3.xlarge (Save 50%)
```

### 4. Generate Cost Report

```bash
python scripts/generate_cost_report.py --start-date 2025-09-01 --end-date 2025-10-01 --output report.json
```

Creates a comprehensive cost analysis report in JSON format.

### 5. Deploy Lambda Functions

```bash
# Deploy cost optimization Lambda
cd lambda_functions
zip -r cost_optimizer.zip cost_optimizer.py
aws lambda create-function \
  --function-name cost-optimizer \
  --runtime python3.9 \
  --handler cost_optimizer.lambda_handler \
  --role arn:aws:iam::account-id:role/lambda-execution-role \
  --zip-file fileb://cost_optimizer.zip
```

## 📊 Dashboard Screenshots

### Cost Overview Dashboard
*Screenshot placeholder: Add your CloudWatch dashboard screenshot here*

The main dashboard displays:
- Monthly cost trends
- Top 10 most expensive services
- Budget vs actual spending
- Cost anomaly alerts

### Resource Utilization Dashboard
*Screenshot placeholder: Add your resource utilization dashboard here*

Shows:
- EC2 CPU and memory utilization
- EBS volume usage
- Network traffic patterns
- Idle resource indicators

### Savings Opportunities Dashboard
*Screenshot placeholder: Add your savings dashboard here*

Highlights:
- Identified cost savings opportunities
- Historical savings achieved
- Rightsizing recommendations
- Reserved Instance recommendations

## 🔧 Lambda Script Guide

### Available Lambda Functions

#### 1. `cost_optimizer.py`
Automatically stops underutilized EC2 instances during off-hours.

**Trigger:** EventBridge (CloudWatch Events) - Daily at 8 PM

**Environment Variables:**
- `MIN_CPU_THRESHOLD`: Minimum CPU % (default: 5)
- `EVALUATION_DAYS`: Days to evaluate (default: 7)

#### 2. `snapshot_cleanup.py`
Removes EBS snapshots older than retention period.

**Trigger:** EventBridge - Weekly on Sunday

**Environment Variables:**
- `RETENTION_DAYS`: Snapshot retention (default: 30)

#### 3. `cost_alert.py`
Sends SNS notifications when cost thresholds are exceeded.

**Trigger:** EventBridge - Daily at 9 AM

**Environment Variables:**
- `DAILY_THRESHOLD`: Daily cost limit (default: 100)
- `SNS_TOPIC_ARN`: SNS topic for alerts

### Lambda Deployment Instructions

1. Package the Lambda function:
```bash
cd lambda_functions
pip install -r requirements.txt -t .
zip -r function.zip .
```

2. Deploy via AWS CLI:
```bash
aws lambda create-function \
  --function-name my-cost-optimizer \
  --runtime python3.9 \
  --handler lambda_function.lambda_handler \
  --role arn:aws:iam::ACCOUNT_ID:role/lambda-role \
  --zip-file fileb://function.zip \
  --timeout 300 \
  --memory-size 512
```

3. Set up EventBridge trigger:
```bash
aws events put-rule \
  --name daily-cost-check \
  --schedule-expression "cron(0 20 * * ? *)"

aws events put-targets \
  --rule daily-cost-check \
  --targets "Id"="1","Arn"="arn:aws:lambda:region:account:function:my-cost-optimizer"
```

## 📄 License

This project is licensed under the MIT License - see below for details:

```
MIT License

Copyright (c) 2025 Sandeep Katakam

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

### How to Contribute

1. **Fork the repository**
   ```bash
   git clone https://github.com/sandeepkatakam21/aws-cost-optimization-suite.git
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Write clear, commented code
   - Follow PEP 8 style guidelines for Python
   - Add tests for new features

4. **Test your changes**
   ```bash
   python -m pytest tests/
   ```

5. **Commit your changes**
   ```bash
   git commit -m "Add: Brief description of your changes"
   ```

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Submit a Pull Request**
   - Provide a clear description of the changes
   - Reference any related issues
   - Ensure all tests pass

### Contribution Guidelines

- **Code Style**: Follow PEP 8 for Python code
- **Documentation**: Update README.md for new features
- **Testing**: Include unit tests for new functionality
- **Commit Messages**: Use clear, descriptive commit messages
- **Issues**: Feel free to open issues for bugs or feature requests

### Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/aws-cost-optimization-suite.git

# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
python -m pytest tests/ -v

# Run linting
flake8 scripts/ lambda_functions/
```

## 🙏 Acknowledgments

- AWS Cost Explorer API documentation
- CloudWatch metrics best practices
- Open source community contributors

## 📞 Support

For questions, issues, or suggestions:

- Open an issue on GitHub
- Contact: [Your contact information]
- AWS Blog: [Link to relevant AWS cost optimization resources]

## 🗺️ Roadmap

- [ ] Multi-account support via AWS Organizations
- [ ] Machine learning-based cost predictions
- [ ] Integration with Slack/Teams for notifications
- [ ] Web-based dashboard UI
- [ ] Support for additional AWS services (EKS, Fargate, etc.)
- [ ] Automated Reserved Instance recommendations

---

**Built with ☁️ by Sandeep Katakam**

If you find this project useful, please consider giving it a ⭐ star on GitHub!
