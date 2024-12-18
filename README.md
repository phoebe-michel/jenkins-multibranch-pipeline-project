# jenkins-multibranch-pipeline-project
Sample project with jenkins multibranch pipeline configuration

To set up a basic Jenkins Multibranch Pipeline for your GitHub project, follow these steps:

## Prerequisites
1. **Jenkins Installation**: Ensure Jenkins is installed and running.
2. **GitHub Integration**: Confirm that Jenkins has access to your GitHub project, typically by creating a GitHub token and configuring it in Jenkins.
3. **Jenkins Plugins**: Install the required plugins:
   - **Git** and **GitHub Branch Source** plugins (for GitHub integration).
   - **Pipeline plugin** (for Pipeline jobs).
4. **Jenkinsfile**: Prepare a `Jenkinsfile` in your repository's root directory. This file defines the stages of your CI/CD pipeline using the Jenkins Pipeline DSL.

## Github Personal Access Token
To generate a GitHub Personal Access Token (PAT) for Jenkins or other integrations, follow these steps:

1. Navigate to GitHub's Token Settings
   - Log in to your GitHub account.
   - Go to Settings (click your profile picture in the top-right corner to find this option).
   - Scroll down to Developer settings in the sidebar and click it.
   - In Developer settings, select Personal access tokens > Tokens (classic).
2. Generate a New Token
   - Click Generate new token.
   - Provide a Note to describe what the token is for (e.g., "Jenkins Integration Token").
   - Select Expiration: Choose an expiration date for security (e.g., 90 days), or select No expiration if this token needs to be persistent.
3. Set the Required Permissions
   - For Jenkins or CI/CD integrations, enable the following permissions:
      - repo: Full control of private repositories (required if you want Jenkins to access private repos).
      - admin:repo_hook: Manage webhooks and repository services.
      - workflow: Access GitHub Actions workflows (if needed for certain CI/CD workflows).
      - Optional: read:org if the repository belongs to an organization and you want Jenkins to view the organization’s metadata.
4. Generate and Copy the Token
   - Click Generate token.
   - Copy the token immediately. You won’t be able to see it again once you leave the page.
5. Add the Token to Jenkins
   - In Jenkins, go to Manage Jenkins > Manage Credentials.
   - Under your Jenkins instance (or a specific domain), click (global) > Add Credentials.
   - Choose Secret text as the Kind, paste the token in the Secret field, add an ID and Description (e.g., “GitHub Token for Jenkins”), and click OK.
Now, your GitHub token is securely saved in Jenkins and ready for integration purposes!
