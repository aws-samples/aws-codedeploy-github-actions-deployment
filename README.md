## Integrating with GitHub Actions – CICD pipeline to Deploy a Web App to Amazon EC2

Many Organizations adopt [DevOps Practices](https://aws.amazon.com/devops/what-is-devops/) in an attempt to innovate faster through automating and streamlining the software development and infrastructure management processes. Besides cultural adoption, DevOps also suggests following certain best practices and Continuous Integration and Continuous Delivery(CICD) is amongst the important ones to start with. CICD practice helps reduce time to release new software updates by automating deployment activities. There are many tools available to implement this practice. While AWS has set of native tools to help achieve your CICD goals, it also offers flexibility and extensibility in integrating with a vast number of third party tools.

In this post, you will use GitHub Actions to create CICD workflow and AWS CodeDeploy to deploy a sample Java SpringBoot application to EC2 instances in an Autoscaling group.


[GitHub Actions](https://help.github.com/en/actions) is a feature on GitHub’s popular development platform that helps you automate your software development workflows in the same place you store code and collaborate on pull requests and issues. You can write individual tasks called actions, and combine them to create a custom workflow. Workflows are custom automated processes that you can set up in your repository to build, test, package, release, or deploy any code project on GitHub.

[AWS CodeDeploy](https://aws.amazon.com/codedeploy/) is a deployment service that automates application deployments to EC2 instances, on-premises instances, serverless AWS Lambda functions, or Amazon Elastic Container Service (Amazon ECS) services.


## Solution Overview

The solution utilizes following services:

1.	[GitHub Actions](https://docs.github.com/en/actions) : Workflow Orchestration tool that will host the Pipeline. 
2.	[AWS CodeDeploy](https://aws.amazon.com/codedeploy/) : AWS service to manage deployment on Amazon EC2 Autoscaling Group.
3.	[AWS Auto Scaling](https://aws.amazon.com/ec2/autoscaling/) : AWS Service to help maintain application availability and elasticity by automatically adding or removing EC2 instances. 
4.	[Amazon EC2](https://docs.aws.amazon.com/ec2/index.html?nc2=h_ql_doc_ec2#amazon-ec2) : Destination Compute server for the application deployment 
5.	[AWS CloudFormation](https://aws.amazon.com/cloudformation/) : AWS IaC service used to spin up the initial infrastructure on AWS side
6.	[IAM OIDC identity provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html) : Federated authentication service to establish trust between GitHub and AWS to allow GitHub Actions to deploy on AWS without maintaining AWS Secrets and credentials. 
7.	[Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) : Amazon S3 to store the deployment artifacts.

The following diagram illustrates the architecture for the solution:
![Alt Text](aws-coodedeplooy-github-action-deploymentV3.png?raw=true  "Title")

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

## Download the source code

Clone the source code repository aws-codedeploy-github-actions-deployment 

    git clone https://github.com/aws-samples/aws-codedeploy-github-actions-deployment.git

Create an empty repository in your personal GitHub account. To create a GitHub repository, see Create a repo . Clone this repo to your laptop. Ignore the warning about cloning an empty repository.

    git clone https://github.com/<username>/<repoName>.git

    e.g. git clone https://github.com/mahbir/GitActionsDeploytoAWS.git
    
Copy over the code. We need contents from hidden .github folder for GitHub actions to work

    cp -r aws-codedeploy-github-actions-deployment/. <new repository> 

    e.g. GitActionsDeploytoAWS

## Deploying the CloudFormation template
To deploy the CloudFormation template, complete the following steps:

    1.	Open AWS CloudFormation console. Enter your account ID, user name and Password. 
    2.	Check your region, this solution uses us-east-1.
    3.	If this is  new AWS CloudFormation account, click Create New Stack. Otherwise, click Create Stack.
    4.	Select Template is Ready
    5.	Click Upload a template file
    6.	Click Choose File, 
    Navigate to template.yml file in your cloned repository at “aws-codedeploy-github-actions-deployment/cloudformation/template.yaml” 
    7.	Select the template.yml file and click next.
    8.	In Specify Stack Details, add or modify values as needed.
        * Stack name = CodeDeployStack.
        * VPC and Subnets = (these are pre-populated for you) you can change these values if you prefer to use your own Subnets)
        * GitHubThumbprintList = 6938fd4d98bab03faadb97b34396831e3780aea1
        * GitHubRepoName – Name of your GitHub personal repository which you created.
    9.	On the Options page, click Next.
    10.	Select the acknowledgement box to allow the creation of IAM resources, and then click Create. 
    It will take CloudFormation about 10 minutes to create all the resources. This stack would create below resources.
       * 2 EC2 Linux instances with Tomcat server and CodeDeploy agent installed 
       * Autoscaling group with Internet Application load balancer
       * CodeDeploy application name and deployment group
       * S3 bucket to store build artifacts
       * IAM OIDC identity provider
       * Instance profile for EC2 
       * Service role for CodeDeploy
       * Security groups for ALB and EC2
        
## GitHub configuration and Testing

    Please follow the [blog post] (https://aws.amazon.com/blogs/devops/integrating-with-github-actions-ci-cd-pipeline-to-deploy-a-web-app-to-amazon-ec2/) to setup GitHub actions and test the CICD flow.

## Clean up

To avoid incurring future changes, clean up the resources you created.
  1.	Empty the S3 bucket
  2.	Delete the CloudFormation stack (CodeDeployStack) from AWS console 


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
