# Cost-Estimation-in-AWS-with-Terraform-Infracost-and-Slack-via-CI-CD-Pipeline
## Description

This project automates Terraform plan, cost estimation with Infracost, and Slack notifications for cost estimates using GitHub Actions CI/CD pipeline. It also includes a conditional step for applying Terraform changes based on the Infracost estimate.

## Workflow Overview

The workflow is triggered on the following events:
- Push to the `main` branch for files in the `terraform/` directory
- Pull requests to the `main` branch with changes in the `terraform/` directory
- Manual dispatch through GitHub's `workflow_dispatch` event

### Environment Variables
The following environment variables are required and stored in GitHub Secrets:
- `AWS_ACCESS_KEY_ID`: AWS access key for accessing resources
- `AWS_SECRET_ACCESS_KEY`: AWS secret access key
- `BUCKET_TF_STATE`: S3 bucket for Terraform state
- `AWS_REGION`: AWS region (default: `us-west-2`)
- `INFRACOST_API_KEY`: API key for Infracost
- `SLACK_BOT_TOKEN`: Slack bot token for notifications

## Workflow Steps

### 1. Terraform Plan
This job runs the Terraform plan and formats the Terraform files.
- **Checkout source code**
- **Setup Terraform**
- **Terraform init**: Initialize Terraform backend
- **Terraform format**: Check formatting of Terraform files
- **Terraform validate**: Validate the Terraform configuration
- **Generate Terraform plan JSON**: Output the plan in JSON format
- **Save Terraform plan**: Upload the plan as an artifact

### 2. Estimate Cost
This job runs Infracost to estimate the cost of the infrastructure and sends a notification to Slack.
- **Checkout source code**
- **Setup Infracost**: Install and configure Infracost
- **Run Infracost breakdown**: Get cost breakdown in JSON format
- **Inspect Infracost output**: Display the cost breakdown
- **Set cost estimates as environment variables**: Extract hourly, daily, monthly, and yearly costs
- **Post Infracost result to PR**: Comment cost breakdown in the pull request

### 3. Slack Notification
- **Notify Slack**: Send a Slack notification with cost estimates to the `infrastructure-budgets` channel

### 4. Terraform Apply
This job applies Terraform changes if only the cost estimation is acceptable. If changed to ```true```
- **Checkout source code**
- **Setup Terraform**
- **Terraform init**
- **Terraform apply**: Apply the Terraform plan if the previous steps succeed

## Usage

1. Set up the required GitHub Secrets in your repository:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `BUCKET_TF_STATE`
   - `INFRACOST_API_KEY`
   - `SLACK_BOT_TOKEN`

2. Commit your Terraform configuration files in the `terraform/` directory.

3. Push changes or create a pull request to trigger the workflow.

## Notifications

Cost estimation results are posted in the following locations:
- Slack: Cost estimates sent to the `infrastructure-budgets` channel

## License
This project is licensed under the MIT License.

## Author
Kehinde Omokungbe