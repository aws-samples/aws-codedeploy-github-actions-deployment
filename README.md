## Integrating with GitHub Actions – CICD pipeline to Deploy a Web App to Amazon EC2

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

## Prerequisites
Before you begin, you need to complete the following prerequisites:
    
   * An [AWS account](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fportal.aws.amazon.com%2Fbilling%2Fsignup%2Fresume&client_id=signup) with permissions to create the necessary resources.
   * A [Git Client](https://git-scm.com/downloads) to clone the source code provided.
   * A [GitHub account](https://github.com/) with permissions to configure GitHub repositories, create workflows, and configure GitHub secrets.

## Walkthrough
The following steps provide a high-level overview of the walkthrough:

  1.	Clone the project from the AWS code samples repository.
  2.	Deploy the AWS CloudFormation template to create the required services.
  3.	Update the source code.
  4.	Setup GitHub repo and secrets.
  5.	Integrate CodeDeploy with GitHub
  6.	Trigger the GitHub Action to build and deploy the code.
  7.	Verify the deployment.
## Clean up

To avoid incurring future changes, clean up the resources you created.
  1.	Empty the S3 bucket
  2.	Delete the CloudFormation stack (CodeDeployStack) from AWS console 


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
