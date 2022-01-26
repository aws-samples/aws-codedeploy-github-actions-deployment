## Deploy to AWS EC2 using github actions

In this post, you will use GitHub Actions to create CICD workflow and AWS CodeDeploy to deploy a sample Java SpringBoot application to EC2 instances in an Autoscaling group.

GitHub Actions is a feature on GitHub’s popular development platform that helps you automate your software development workflows in the same place you store code and collaborate on pull requests and issues. You can write individual tasks called actions, and combine them to create a custom workflow. Workflows are custom automated processes that you can set up in your repository to build, test, package, release, or deploy any code project on GitHub.

AWS CodeDeploy makes it possible to automate the deployment of code to variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and your on-premises servers.
Solution Overview

The solution utilizes following services:

•	GitHub Actions
Workflow Orchestration tool that will host the Pipeline. 

•	AWS CodeDeploy
AWS service to manage deployment on Amazon EC2 Autoscaling Group.

•	AWS Auto Scaling
AWS Service to help maintain application availability and elasticity by automatically adding or removing EC2 instances. 

•	Amazon EC2
Destination Compute server for the application deployment 

•	AWS CloudFormation
AWS IaC service used to spin up the initial infrastructure on AWS side

•	IAM OIDC identity provider
Federated authentication service to establish trust between GitHub and AWS to allow GitHub Actions to deploy on AWS without maintaining AWS Secrets and credentials. 

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
