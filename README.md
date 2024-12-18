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

## Triggering Builds with Commits

To trigger a Jenkins Multibranch Pipeline build with a commit, follow these steps:

### 1. Ensure Webhooks are Configured in GitHub
A webhook is required for Jenkins to be notified of new commits.

**Steps to Set Up Webhooks in GitHub:**
1. Go to your GitHub repository > Settings > Webhooks > Add webhook.
2. Enter the Payload URL as:

   ```perl
   http://<your-jenkins-server>/github-webhook/
   ```
Replace <your-jenkins-server> with your Jenkins server’s public URL.

3. Set Content type to application/json.
4. Select the events to trigger the webhook:
   - Choose Push events to trigger builds when a commit is pushed to any branch.
   - (Optional) Select Pull request events if you also want builds to trigger for pull request updates.
5. (Optional) Add a Secret for security. If you do, configure the same secret in Jenkins (under Manage Jenkins > Configure System > GitHub Plugin).
6. Save the webhook.

### 2. Configure Your Multibranch Pipeline in Jenkins

**Steps to Set Up Multibranch Pipeline:**

1. Create a Multibranch Pipeline:
   - Go to Jenkins > New Item > Multibranch Pipeline.
   - Add a name for the pipeline and click OK.
2. Set Up Branch Source:
   - Under Branch Sources, add your GitHub repository.
   - Use credentials (e.g., a GitHub Personal Access Token) if the repo is private.
3. Scan Repository Triggers:
   - In the pipeline configuration, enable the Periodically if not otherwise run trigger under Scan Multibranch Pipeline Triggers.
   - Set a reasonable interval (e.g., 1 minute). This ensures Jenkins picks up changes if the webhook fails.
4. Ensure your repository has a valid Jenkinsfile in each branch you want to build.

### 3. Test the Trigger with a Commit
   - Make a commit in a branch other than main (or any branch you want Jenkins to monitor).
   - Push the commit to GitHub.
   - Jenkins should automatically detect the push event via the webhook and trigger a pipeline build for the affected branch.

### 4. Verify Webhook and Build Trigger
**Check Webhook Activity in GitHub:**
   - Go to your GitHub repository > Settings > Webhooks.
   - Click on your webhook and check Recent Deliveries to see if the push event was sent successfully.
**Check Jenkins Logs:**
   - In Jenkins, go to Manage Jenkins > System Log and look for incoming webhook events.
   - Ensure Jenkins is correctly triggering the pipeline for the branch where the commit occurred.

### 5. Handling Specific Branches in Your Jenkinsfile
If you want to customize builds based on the branch:

   ```groovy
   pipeline {
       agent any
       stages {
           stage('Branch Specific Actions') {
               steps {
                   script {
                       if (env.BRANCH_NAME == 'main') {
                           echo 'Building the main branch...'
                           // Add main branch-specific steps here
                       } else {
                           echo "Building branch: ${env.BRANCH_NAME}"
                           // Add non-main branch-specific steps here
                       }
                   }
               }
           }
       }
   }
```
Now, any commit pushed to the specified branch will trigger a build automatically.

