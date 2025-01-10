# Advanced GitHub Actions: Automating CloudFormation Validation with GitHub Actions

<p align="center">
  <strong>Automate validation and testing of AWS CloudFormation templates using GitHub Actions pull request events.</strong>
</p>

<a id="readme-top"></a>

<!-- TABLE OF CONTENTS -->

## Table of Contents

1. [About The Project](#about-the-project)
   - [Built With](#built-with)
2. [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [Installation](#installation)
3. [Usage](#usage)
4. [Roadmap](#roadmap)
5. [Contributing](#contributing)
6. [License](#license)
7. [Contact](#contact)

---

## About The Project

This project provides a complete CI/CD pipeline for validating and deploying AWS CloudFormation stacks using GitHub Actions. When a pull request (PR) is created or updated, the workflow validates the CloudFormation template and deploys a test stack. After merging the PR, the workflow automatically deletes the test stack, ensuring a clean environment and minimal AWS costs.

### Key Features:

- **Trigger on PR Events:** The workflow handles multiple PR events (`opened`, `updated`, `reopened`, and `closed`).
- **Automated Cleanup:** Deletes the test CloudFormation stack after merging to keep environments clean.
- **Supports Multiple Environments:** The CloudFormation template supports different environments like `test`, `staging`, and `production`.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

### Built With

This project uses the following tools and frameworks:

- ![GitHub Actions](https://img.shields.io/badge/-GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
- ![AWS CloudFormation](https://img.shields.io/badge/-AWS_CloudFormation-F09000?style=for-the-badge&logo=amazon-aws&logoColor=white)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## Getting Started

To set up this project locally and run the GitHub Actions workflow, follow these simple steps.

### Prerequisites

You need the following tools installed:

1. **AWS CLI**
   Install the AWS Command Line Interface (CLI) by following the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

2. **GitHub CLI (optional)**
   Install GitHub CLI if you want to manage PRs from the command line:

   ```sh
   brew install gh
   ```

3. **Basic Git and GitHub Knowledge**
   Ensure you have a basic understanding of Git version control and GitHub repositories.

---

### Installation

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

## Usage

1. **Opening a PR:**
   When you open a pull request, the workflow automatically validates your CloudFormation template and deploys a test stack.

2. **Merging a PR:**
   After merging the PR, the workflow deletes the test stack, keeping your environment clean.

3. **Supported PR Events:**
   The workflow triggers on the following PR events:
   - `opened`
   - `synchronize` (PR updated)
   - `reopened`
   - `closed` (for cleanup)

_For more information, refer to the [Documentation](#)._

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## Roadmap

- [x] Add automated stack validation
- [x] Implement cleanup after merge
- [ ] Add unit tests for the CloudFormation template
- [ ] Support multi-region deployments

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## Contributing

Contributions are welcome! Follow these steps to contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -m 'Add new feature'`)
4. Push to your branch (`git push origin feature/new-feature`)
5. Open a pull request

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## License

Distributed under the MIT License. See `LICENSE` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

---

## Contact

Simon Cheam – [LinkedIn](https://www.linkedin.com/in/simoncheam)
Project Link: [https://github.com/simoncheam/lambda-cicd](https://github.com/simoncheam/lambda-cicd)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
