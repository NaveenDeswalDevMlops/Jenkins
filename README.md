🚀 Jenkins CI/CD Pipeline for EC2 Deployment via S3 & SSM
This Jenkins pipeline automates the deployment of a specified release from a GitHub repository to an EC2 instance. It uses AWS services such as S3, SSM, and SNS for secure, scalable, and monitored deployment.

📁 Pipeline Overview
Key Features:
1.Checks out code from a local GitHub repo by release tag.

2.Prepares API artifacts and uploads them to Amazon S3.

3.Deploys to EC2 via SSM without requiring SSH.

4.Installs Python dependencies in a virtual environment.

5.Restarts services on EC2.

6.Sends deployment notifications via SNS.

7.Supports rollback to a previous deployment.

🔧 Requirements
Jenkins:
Jenkins agent labeled cicd

AWS CLI

Pipeline: AWS Steps Plugin

AWS:
S3 bucket for artifact storage

IAM Role with permissions for:

s3:PutObject, s3:GetObject

ssm:SendCommand, ssm:ListCommandInvocations

sns:Publish

EC2 instance with SSM agent installed and configured

SNS topic for notifications

🔐 Environment Variables
These variables must be configured in Jenkins (either in the UI or pipeline environment block):

Variable	Description
S3_BUCKET	S3 bucket name for storing artifacts
AWS_REGION	AWS region of deployment (e.g., ap-southeast-2)
EC2_INSTANCE_ID	Target EC2 instance ID
IAM_ROLE	IAM Role ARN to assume for AWS operations
GITHUB_REPO_PATH	Path to the local GitHub repo on the agent

📦 Pipeline Parameters
Parameter	Description
RELEASE_TAG	Git tag to check out and deploy

🧩 Pipeline Stages
Checkout Local GitHub Repo – Pulls code for specified tag.

Prepare API Folder – Creates a clean API directory.

Timestamp Folder – Generates a unique timestamp-based deployment folder name.

Upload to S3 – Uploads API contents to the defined S3 path.

Approval – Requires manual approval before proceeding.

Deploy to EC2 via SSM – Uses aws ssm send-command to pull artifacts onto EC2.

Install VENV Packages – Activates venv and installs Python packages from requirements.txt.

Restart Services – Restarts API and nginx services.

SNS Notification – Publishes deployment completion message.

Rollback/Proceed – Option to roll back to a previous deployment folder on S3.

🛠 Rollback
To roll back, enter the exact folder name (e.g., App_Engg_20250517_143501) stored in the S3 bucket during the Rollback/Proceed stage prompt.

🔔 Notifications
Deployment success and rollback events trigger an SNS notification to the topic:

ruby
Copy
Edit
arn:aws:sns:${AWS_REGION}:account:Dev-Monitoring-Alerts
Update with your actual account ID.

📌 Notes
Ensure the EC2 instance is associated with an IAM role allowing S3 and SSM access.

Jenkins agent must have Git and AWS CLI installed and configured.

This pipeline assumes the EC2 instance has a virtual environment set up at /home/ubuntu/AppEngg/venv.

✅ Sample Execution
Run this pipeline manually or trigger it from a CI job by providing a valid RELEASE_TAG. Follow the approval step to proceed or cancel.

