# **GitHub Actions CI/CD for Lambda Functions and CloudFormation Validation**

This repository contains two GitHub Actions workflows to automate the deployment and validation of AWS infrastructure:

1. **Automatically Deploying Lambda Functions**
2. **Automating CloudFormation Validation for Pull Requests**

Both workflows are designed to enhance DevOps practices by ensuring that infrastructure changes are consistently deployed and validated before merging.

---

## **Table of Contents**

1. [About The Project](#about-the-project)
   - [Part 1: Automatically Deploying Lambda Functions](#part-1-automatically-deploying-lambda-functions)
   - [Part 2: Automating CloudFormation Validation](#part-2-automating-cloudformation-validation)
2. [Built With](#built-with)
3. [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [Installation](#installation)
4. [Usage](#usage)
5. [Contributing](#contributing)
6. [License](#license)
7. [Contact](#contact)

---

## **About The Project**

This repository features two distinct GitHub Actions workflows:

### **Part 1: Automatically Deploying Lambda Functions**

This workflow triggers on **push events** to the main branch. When changes are detected in the `lambda/` directory, it automatically deploys the updated Lambda function to AWS. This ensures that any code changes to the Lambda function are consistently deployed without manual intervention.

Key features:

- **Automated deployments** on code push to the main branch.
- **AWS credentials management** via GitHub Secrets.
- **Error handling** to prevent further steps if deployment fails.

### **Part 2: Automating CloudFormation Validation**

This workflow triggers on **pull request events** and handles the following tasks:

- Validates CloudFormation templates when a PR is opened or updated.
- Deploys a test CloudFormation stack to AWS for validation.
- Comments on the pull request with deployment status and details.
- Cleans up (deletes) the test stack when the PR is merged.

Key features:

- Ensures infrastructure changes are validated before merging.
- Automatically removes test stacks after merging.
- Supports multiple PR event types, including `opened`, `updated`, `reopened`, and `closed`.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## **Built With**

This project uses the following tools and frameworks:

- ![GitHub Actions](https://img.shields.io/badge/-GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
- ![AWS Lambda](https://img.shields.io/badge/-AWS_Lambda-F09000?style=for-the-badge&logo=amazon-aws&logoColor=white)
- ![AWS CloudFormation](https://img.shields.io/badge/-AWS_CloudFormation-F09000?style=for-the-badge&logo=amazon-aws&logoColor=white)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## **Getting Started**

### **Prerequisites**

You need the following tools installed:

1. **AWS CLI**
   Install the AWS Command Line Interface by following the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

2. **GitHub CLI (optional)**
   Install GitHub CLI if you want to manage PRs from the command line:

   ```sh
   brew install gh
   ```

3. **Basic Git and GitHub Knowledge**
   Ensure you have a basic understanding of Git version control and GitHub repositories.

---

### **Installation**

1. **Clone the Repository:**

   ```sh
   git clone https://github.com/simoncheam/lambda-cicd.git
   cd lambda-cicd
   ```

2. **Set Up AWS Credentials:**
   Use the AWS CLI to configure your credentials locally:

   ```sh
   aws configure
   ```

3. **Push to GitHub and Create a PR:**
   Push your changes to a branch and open a pull request:

   ```sh
   git checkout -b feature/add-new-cloudformation-template
   git push origin feature/add-new-cloudformation-template
   ```

4. **Review Workflow Execution:**
   Go to the **Actions** tab in your GitHub repository to monitor workflow execution.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## **Usage**

### **Part 1: Automatically Deploying Lambda Functions**

1. **Set Up AWS Credentials**
   Add AWS credentials as GitHub Secrets to securely authenticate deployments:

   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`

2. **Create a Push Workflow**
   Add the following workflow file in `.github/workflows/lambda.yml`:

   ```yaml
   name: Deploy AWS Lambda

   on:
     push:
       branches:
         - main
       paths:
         - 'lambda/**'

   jobs:
     deploy-lambda:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2

         - name: Set Up Python
           uses: actions/setup-python@v2
           with:
             python-version: '3.11'

         - name: Install Dependencies
           run: |
             python -m pip install --upgrade pip
             pip install -r lambda/requirements.txt -t lambda/

         - name: Configure AWS Creds
           uses: aws-actions/configure-aws-credentials@v2
           with:
             aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
             aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
             aws-region: us-east-1

         - name: Deploy Lambda Function
           run: |
             cd lambda/
             zip -r lambda.zip .
             aws lambda update-function-code --function-name my-test-cicd-lambda --zip-file fileb://lambda.zip
   ```

3. **Trigger the Workflow**
   Push changes to the `lambda/` directory on the `main` branch. The workflow will automatically deploy the updated Lambda function.

4. **Verify the Deployment**
   Check the AWS Lambda Console to verify that the function was updated.

---

### **Part 2: Automating CloudFormation Validation**

1. **Create a CloudFormation Template**
   Add the following CloudFormation template in `cloudformation/s3-bucket.yml`:

   ```yaml
   AWSTemplateFormatVersion: '2010-09-09'
   Description: 'S3 Bucket for our CICD PR'

   Parameters:
     Environment:
       Type: String
       Default: test
       AllowedValues:
         - test
         - staging
         - production

   Resources:
     MyS3Bucket:
       Type: AWS::S3::Bucket
       Properties:
         BucketName: !Sub '${AWS::StackName}-${Environment}-bucket-v2'
         Tags:
           - Key: Environment
             Value: !Ref Environment
           - Key: Environment
             Value: GithubActions-CFN-Validation-Logic
   ```

2. **Create a PR Workflow**
   Add the following workflow file in `.github/workflows/cfn-validate-pr.yml`:

   ```yaml
   name: Validate CloudFormation on PR

   on:
     pull_request:
       paths:
         - 'cloudformation/**'

   jobs:
     validate-cfn:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2

         - name: Configure AWS credentials
           uses: aws-actions/configure-aws-credentials@v1
           with:
             aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
             aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
             aws-region: us-east-1

         - name: Validate CloudFormation template
           run: |
             aws cloudformation validate-template --template-body file://cloudformation/s3-bucket.yml
   ```

---

## **Contributing**

Contributions are welcome! Follow these steps to contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -m 'Add new feature'`)
4. Push to your branch (`git push origin feature/new-feature`)
5. Open a pull request

---

## **License**

Distributed under the MIT License. See `LICENSE` for more information.

---

## **Contact**

Simon Cheam – [LinkedIn](https://www.linkedin.com/in/simoncheam)
Project Link: [https://github.com/simoncheam/lambda-cicd](https://github.com/simoncheam/lambda-cicd)
