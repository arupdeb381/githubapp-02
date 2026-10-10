
## 📱 Step 1: Create the GitHub App on GitHub

Establish a dedicated system identity within GitHub with narrow scopes.

1. Navigate to your GitHub Account or Organization **Settings** ➡️ **Developer settings** ➡️ **GitHub Apps** ➡️ click **New GitHub App**.
2. Configure the baseline information:
   - **GitHub App name:** `Arup-Jenkins-CI` *(Must be globally unique)*
   - **Homepage URL:** `http://98.92.140.56:8080` *(Your Jenkins base URL)*
3. Configure the Webhook configurations:
   - **Active:** 🟩 Checked
   - **Webhook URL:** `http://98.92.140.56:8080/github-webhook/` *(Ensure trailing slash)*
   - **Webhook secret:** *(Leave completely blank)*
4. Set the following specific **Repository permissions**:
   - **Checks:** Read & Write *(To publish pipeline status icons to GitHub)*
   - **Contents:** Read-only *(To scan and clone code files)*
   - **Metadata:** Read-only *(Assigned by default)*
5. Under **Subscribe to events**, scroll down and check 🟩 **Push**.
6. Set visibility to **Only on this account** and click **Create GitHub App**.
7. **Save Metadata:** On the app landing page, copy down the numerical **App ID**. Scroll to the bottom and click **Generate a private key**. Save the downloaded `.pem` file.

## 🔑 Step 2: Convert Key to PKCS#8 Format

Convert the private key file into a layout Jenkins explicitly natively accepts.

1. Open your local terminal inside your downloads directory and run the following command:

2. Open the generated `jenkins-github-app.pem` in a text editor and copy its full contents block.

## 🔐 Step 3: Add GitHub App Credentials into Jenkins

1. Log into your **Jenkins Dashboard**.
2. Go to **Manage Jenkins** ➡️ **Credentials** ➡️ **System** ➡️ **Global credentials (unrestricted)**.
3. Click **Add Credentials** in the upper right.
4. Configure the parameters:
   - **Kind:** `GitHub App`
   - **ID:** `github-app-jenkins-auth`
   - **App ID:** *Paste the numeric App ID recorded from Step 1.*
   - **Key:** Paste the entire contents of the `jenkins-github-app.pem` block.
5. Click **Create**.

## ⚙️ Step 4: Map the Global GitHub System Server

1. Go to **Manage Jenkins** ➡️ **System**.
2. Scroll down to the **GitHub** section. Click **Add GitHub Server** ➡️ select **GitHub Server**.
3. Configure the connection parameters:
   - **Name:** `GitHub Cloud`
   - **API URL:** `https://github.com`
   - **Credentials:** Select your new `github-app-jenkins-auth` credential block.
4. Click the **Test connection** button. It should report a successful authentication with your App name.
5. Click **Save** at the bottom of the page.

## 📦 Step 5: Activate and Install the App on Repositories

1. Go back to your GitHub Developer Settings App interface on GitHub and select **Install App** on the left panel.
2. Click **Install** next to your target account or organization structure.
3. Select either **All repositories** or choose **Only select repositories** to specify your individual application repo.
4. Click **Install / Save**.

## 🚀 Step 6: Link the Jenkins Job & Baseline Run

1. Navigate to your Jenkins Pipeline job config page, and verify under **Build Triggers** that 🟩 **GitHub hook trigger for GITScm polling** is enabled.
2. Scroll down to the **Pipeline SCM** block:
   - Swap out your old personal tokens or SSH keys in the SCM Credentials drop-down for your new **GitHub App** credential identity.
3. Click **Save**.
4. **Activate Mapping:** Click **Build Now** once manually. This initial pass updates Jenkins' dynamic lookup mappings so that future inbound push event payloads from your GitHub App accurately kick off automatic builds.

> **Security Architecture Reminder:** Avoid hardcoding cloud tokens or environment variables directly inside your `Jenkinsfile`. Always inject environment configurations dynamically at pipeline execution runtime using Jenkins' secure credentials binding plugins.
