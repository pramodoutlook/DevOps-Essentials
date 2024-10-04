## Using `GitWebHook` to build your code automatically using Jenkins

This lab focuses on configuring `Git WebHooks` to automatically `trigger Jenkins builds` when code changes are pushed to a GitHub repository.

### Task-1: Setting up GitHub Integration in Jenkins for Automated Builds:

1. Navigate to the Jenkins webpage and select **Manage Jenkins** > **Plugins**.
2. In the **Available** tab, search for **GitHub Integration**. Choose the **GitHub Integration Plugin** and click **Install** (without restart).
3. After installation, return to the top page.
4. In your **hello-world project**, click on **Configure**.
5. Under **Build Triggers**, enable **GitHub hook trigger for GITScm polling** and click **Save**.

### Task-2: Configuring GitHub Webhook in GitHub:

1. On your **GitHub website**, go to the **hello-world** repository > **Settings** > **Webhooks**. Click **Add Webhook**.
2. Fill in the details as follows:
   - **Payload URL Example:**
     - http://< jenkins-PublicIP >:8080/github-webhook/ (Example: http://184.72.112.155:8080/github-webhook/)
   - **Content type:** application/JSON
   - Click **Add Webhook**.

### Task-3: Verifying WebHook Functionality:

1. Make a minor change and commit in GitHub's **hello-world repository** by editing the **hello.txt** file.
2. The WebHook triggers Jenkins upon source code modification, initiating an automatic build.
3. Navigate to Jenkins, where you'll observe the ongoing build.
4. Monitor the Jenkins page for the successful completion of the build.


