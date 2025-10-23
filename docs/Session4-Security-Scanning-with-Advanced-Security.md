# Session 4: Security Scanning with Advanced Security

## Overview

In this assignment we will look at how to use GitHub Advanced Security to scan code for security vulnerabilities.

For this we will create a new repository by forking https://github.com/advanced-security-demo/demo-csharp into our organisation.

This is a public repository that contains known vulnerabilities.

## Step 1: Enable Advanced Security in the Repository

Go to the settings of the repository and select "Code security and analysis" in the left menu. Then enable "GitHub Advanced Security" by enabling CodeQL analysis. This will enable security features like code scanning and secret scanning in your repository.

https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security https://docs.github.com/en/code-security/code-scanning/enabling-code-scanning/configuring-default-setup-for-code-scanning

Also enable Copilot autofix

## Step 2: Configure Branch Protection

Go to the "Branches" section in the settings of your repository. Then add branch protection rules for your default branch. (Don't click "classic" ruleset).

Make sure to meet the following requirements:

- Prevent deletion of the branch
- Require a pull request before merging
- Require code scanning to be completed before merging

## Step 3: Validate Your Security Findings

At repository level, go to the "Security" tab and then select "Code scanning alerts". You should see a list of security findings in your code.

Try and resolve some of the findings, you can ask Copilot to help you with this.

## Step 4: Create a Pull Request

Create a new branch and make some changes to your code. Then create a pull request to merge your changes into the default branch.

Observe the security scanning that is performed on your pull request. You should see that the code scanning is required to pass before you can merge your pull request.