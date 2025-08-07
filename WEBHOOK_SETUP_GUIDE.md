# Webhook Setup Guide for Jenkins Integration

## What is a Webhook?
A webhook is an HTTP callback that sends a notification from one system (like GitHub) to another (like Jenkins) when a specific event occurs. In this case, it will notify Jenkins whenever you push code to your repository, automatically triggering your pipeline.

## Prerequisites
1. Jenkins must be accessible from the internet (for GitHub/GitLab cloud) or your local network
2. You need to know your Jenkins URL (e.g., http://your-server:8080 or http://jenkins.yourcompany.com)

## GitHub Webhook Setup

### Step 1: Get Your Jenkins Webhook URL
1. In Jenkins, go to **Manage Jenkins** → **Configure System**
2. Find the **GitHub** section
3. Your webhook URL will be: `http://YOUR_JENKINS_URL/github-webhook/`
   (Note the trailing slash)

### Step 2: Configure Webhook in GitHub
1. Go to your repository on GitHub
2. Click **Settings** (gear icon) in the repository menu
3. Click **Webhooks & Services** or **Webhooks** in the left sidebar
4. Click **Add webhook**
5. Fill in the form:
   - **Payload URL**: Your Jenkins webhook URL (e.g., http://your-jenkins-url/github-webhook/)
   - **Content type**: Select `application/json`
   - **Secret**: Leave empty (or configure if you've set up security in Jenkins)
   - **Which events would you like to trigger this webhook?**: 
     - Select **Just the push event** (simpler option)
     - OR select **Let me select individual events** and choose:
       - Pushes
       - Pull requests (optional)
6. Make sure **Active** is checked
7. Click **Add webhook**

## GitLab Webhook Setup

### Step 1: Get Your Jenkins Webhook URL
Same as GitHub: `http://YOUR_JENKINS_URL/project/YOUR_JOB_NAME`

### Step 2: Configure Webhook in GitLab
1. Go to your project in GitLab
2. Go to **Settings** → **Webhooks & Services**
3. In the **URL** field, enter your Jenkins webhook URL
4. In **Trigger**, select:
   - **Push events** (required for automatic builds)
   - Optionally: **Merge request events**
5. Leave **Enable SSL verification** checked if your Jenkins uses HTTPS
6. Click **Add Webhook**

## Testing Your Webhook

### GitHub:
1. After creating the webhook, you'll see it in the list
2. Click on the webhook to see details
3. Click **Recent Deliveries** to see if payloads are being sent
4. You can click **Redeliver** to test sending the last payload again

### GitLab:
1. After creating the webhook, you'll see it in the list
2. Click **Test** to send a test payload
3. Check the response to see if it was successful

## Troubleshooting Webhooks

### Common Issues:
1. **Jenkins not accessible**: Make sure your Jenkins server is accessible from the internet or your Git provider
2. **Wrong URL**: Double-check your webhook URL format
3. **Firewall issues**: Ensure your firewall allows incoming connections to Jenkins
4. **SSL issues**: If using HTTPS, make sure your certificate is valid

### Checking Webhook Delivery:
1. In GitHub: Go to webhook settings and check **Recent Deliveries**
2. In GitLab: Check the webhook's delivery history
3. In Jenkins: Check the logs for webhook processing

### Jenkins Logs:
1. Go to **Manage Jenkins** → **System Log**
2. Look for GitHub/GitLab plugin logs
3. Check for any error messages related to webhook processing

## Alternative: Using Jenkins GitHub Plugin

If you're using GitHub, you can also use the GitHub plugin for Jenkins:

1. Install the **GitHub Plugin** in Jenkins
2. Configure GitHub server in **Manage Jenkins** → **Configure System**
3. In your job configuration, check **GitHub project** and enter your project URL
4. In the **Build Triggers** section, check **GitHub hook trigger for GITScm polling**

This method provides better integration with GitHub features.