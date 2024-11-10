# Cloud Computing - Lab 4 Solutions


# Part 1: A Tour of Google Cloud Hands-on Labs

## Overview

This lab provides an introductory-level experience with Google Cloud services. You’ll gain hands-on practice with the Google Cloud Console, which is the web-based interface that allows you to interact with and manage Google Cloud services.

---

## Lab Fundamentals

### Features and Components

- **Start Lab Button**: This button initializes a temporary Google Cloud environment. When you click it, the necessary services and credentials will be provisioned for the lab.
- **Credit**: Indicates the price of the lab. Some labs are free, but others require credits.
- **Time**: Each lab has a time limit. The environment will be deleted once the timer runs out.
- **Score**: Some labs include activity tracking. You need to complete all required steps to get a passing score and receive lab completion credit.

---

## Task 1: Accessing the Cloud Console

### Solution and Explanation

#### Step 1: Start the Lab
1. Click the **Start Lab** button. This will spin up a new Google Cloud environment with your temporary credentials.
2. The timer will begin counting down, and once the **End Lab** button appears, your environment is ready.

#### Step 2: Sign in to Google Cloud Console
1. From the **Lab details pane**, click **Open Google Console** to open the Google Cloud sign-in page in a new browser tab.
2. Enter the **temporary username** (`googlexxxxxx_student@qwiklabs.net`) and **password** (provided in the Lab details).
3. Ensure you use the provided temporary student credentials—not your personal or corporate credentials.
4. You will be logged into the Cloud Console, which should look like the familiar Google login page for Gmail or other Google services.
   
### Answer to "Test Your Understanding" questions:
- **What field is NOT found in the left pane?**
  - **Answer**: *System admin* is not found in the Lab details pane. The left pane contains the **Project ID**, **Username/Password**, and **Open Google Console**.
  
- **The username in the left panel (e.g., `googlexxxxxx_student@qwiklabs.net`) is a Cloud IAM identity.**
  - **Answer**: *True*. The username provided is an IAM identity with specific roles and permissions for this lab session.

---

## Task 2: Google Cloud Projects

### Solution and Explanation

#### Step 1: Access Your Google Cloud Project
1. In the Google Cloud Console, find the **Project Info** card at the top left of the interface. The card shows the **Project Name**, **ID**, and **Number**.
2. These identifiers are unique to your session and represent your working environment during the lab.

#### Step 2: View All Projects
1. In the Cloud Console, click the drop-down menu next to your project name at the top of the page.
2. Select **All Projects**. You will see a list of projects available in your session, including **Qwiklabs Resources**. **Do not select the Qwiklabs Resources project** unless instructed in other labs.

**Explanation**: Projects are logical groupings of Google Cloud resources (such as virtual machines, storage, etc.). Projects help organize and manage permissions and billing for your resources.

### Answer to "Test Your Understanding" questions:
- **An organizing entity for anything you build with Google Cloud.**
  - **Answer**: *Google Cloud Project*. Projects are used to manage and organize resources like virtual machines, storage, and databases.
  
- **Qwiklabs Resources is shared (read-only) with all Qwiklabs users, which means that you cannot delete or modify it.**
  - **Answer**: *True*. The **Qwiklabs Resources** project is read-only and cannot be modified or deleted by students.

- **Qwiklabs Resources is the project where you run all of your lab steps.**
  - **Answer**: *False*. Your project for this lab is temporary and unique. **Qwiklabs Resources** is only available for shared resources, not for lab tasks.

---

## Task 3: Roles and Permissions

### Solution and Explanation

#### Step 1: View Roles and Permissions
1. Navigate to the **Navigation menu** (the three-line icon) and go to **IAM & Admin** > **IAM**.
2. You’ll see a list of users and their roles for the current project. Look for your username (`googlexxxxxx_student@qwiklabs.net`).
3. The **Role** column will show **Editor**. This role grants permissions to modify resources, but it does not allow managing user roles or billing settings.

**Explanation**: In Google Cloud, IAM roles define permissions for users. There are three basic roles:
- **Viewer**: Read-only access.
- **Editor**: Permissions to modify resources.
- **Owner**: Full control over the project, including managing roles and permissions.

### Answer to "Test Your Understanding" questions:
- **Offers quick access to the platform's services and also outlines its offerings.**
  - **Answer**: *Navigation menu*. The Navigation menu provides access to all Google Cloud services and tools.
  
- **Basic roles set project-level permissions and, unless otherwise specified, control access and management to all Google Cloud services.**
  - **Answer**: *True*. The three basic roles (Viewer, Editor, Owner) manage access to the project resources and services.

- **Provides all viewer permissions, plus permissions for actions that modify state, such as changing existing resources.**
  - **Answer**: *Editor role*. The Editor role allows users to modify existing resources, unlike the Viewer role which is read-only.

---

## Task 4: Enabling and Using APIs

### Solution and Explanation

#### Step 1: Enable the Dialogflow API
1. Go to the **Navigation menu** and click **APIs & Services** > **Library**.
2. In the search bar, type **Dialogflow** and click on the **Dialogflow API**.
3. Click **Enable** to activate the Dialogflow API for your project. This allows you to use Dialogflow's services for building conversational AI applications.

#### Step 2: Explore the API Documentation
1. After enabling the API, you can click **Try this API** to explore its documentation.
2. This will open a new browser tab with information on how to interact with the Dialogflow API programmatically.

**Explanation**: APIs allow you to integrate and use Google Cloud services in your own applications. The Dialogflow API is used to build conversational applications like chatbots.

### Answer to "Test Your Understanding" questions:
- **When you start a lab, you need to enable APIs in your project to start working with Google Cloud.**
  - **Answer**: *True*. Most labs automatically enable APIs for you, but when you create your own projects, you may need to enable APIs manually.

---

## Conclusion

By completing this lab, you have learned how to:
- Access the Google Cloud Console.
- Understand and work with Google Cloud Projects.
- View and manage IAM roles and permissions.
- Enable and use Google Cloud APIs like Dialogflow.

---

# Part 2: Creating a Virtual Machine

## Overview:
This lab involves creating a virtual machine (VM) using the Google Cloud Platform (GCP), installing an NGINX web server on the VM, and configuring the VM via both the Cloud Console and `gcloud` command-line tool.

## Task 1: Create a New Instance from the Cloud Console

#### **Step-by-Step Explanation:**
1. **Navigate to Compute Engine**: 
   - Open the Google Cloud Console. From the Navigation menu (top-left), go to **Compute Engine** > **VM Instances**.
   - Wait for the Compute Engine to initialize.

2. **Create a VM Instance**:
   - Click on the **Create Instance** button.
   - In the **Create an Instance** page, configure the following fields:
     - **Name**: `gcelab` (or any name of your choice for the VM).
     - **Region**: Select the appropriate region for your lab.
     - **Zone**: Select the zone for the region (e.g., `us-central1-a`).
     - **Series**: Choose **E2** (a standard series).
     - **Machine Type**: Select **e2-medium**, which has 2 vCPUs and 4GB of RAM.
     - **Boot Disk**: Choose **Debian GNU/Linux 11 (bullseye)** as the OS image with a **10GB balanced persistent disk**.
     - **Firewall**: Make sure to **Allow HTTP traffic** to install and access the NGINX web server.

3. **Click Create**:
   - The VM will be created. It will take a minute for the VM to initialize.
   - Once the VM is created, it will appear on the VM Instances page.

4. **SSH into the VM**:
   - After the VM is created, click the **SSH** button next to the instance name (`gcelab`) to connect to the VM directly from your browser.

---

## Task 2: Install an NGINX Web Server

#### **Step-by-Step Explanation:**
1. **Update the System**:
   - In the terminal of the SSH session, run the following command to ensure the system’s package list is up-to-date:
     ```bash
     sudo apt-get update
     ```

2. **Install NGINX**:
   - To install the NGINX web server, run:
     ```bash
     sudo apt-get install -y nginx
     ```

3. **Check if NGINX is Running**:
   - Once the installation is complete, verify if NGINX is running by checking the processes:
     ```bash
     ps auwx | grep nginx
     ```

   - The expected output should show that the `nginx` master and worker processes are running.

4. **Access the Web Page**:
   - In the **Cloud Console**, locate the **External IP** of your VM (found in the VM Instances page).
   - Open a new browser tab and go to `http://<EXTERNAL_IP>` to see the default NGINX web page with the message "Welcome to nginx!"

---

## Task 3: Create a New Instance with gcloud

#### **Step-by-Step Explanation:**
1. **Open Cloud Shell**:
   - From the Google Cloud Console, click on **Activate Cloud Shell** (top-right) to open a terminal within your browser.

2. **Set Region and Zone Variables**:
   - Set the region and zone for your new instance by running the following commands in Cloud Shell:
     ```bash
     export REGION=us-central1
     export ZONE=us-central1-a
     ```

3. **Create the VM Instance with gcloud**:
   - Run the following command in Cloud Shell to create the new VM instance using `gcloud`:
     ```bash
     gcloud compute instances create gcelab2 --machine-type e2-medium --zone=$ZONE
     ```

   - This will create a new VM named `gcelab2` with the **e2-medium** machine type in the specified zone. After the command runs, you should see output indicating that the instance was created successfully.

4. **SSH into the New Instance**:
   - To connect to your newly created instance, run:
     ```bash
     gcloud compute ssh gcelab2 --zone=$ZONE
     ```

   - This will initiate an SSH connection to `gcelab2`. You may need to accept the connection and generate a new SSH key if prompted.

5. **Disconnect from SSH**:
   - To exit from the SSH session, type `exit`.

---

## Task 4: Test Your Knowledge

#### **Quiz**:
- The quiz will ask you multiple-choice questions about the different ways to create a VM instance in Google Cloud. The correct answers are:
   - **The gcloud command line tool**.
   - **The Cloud console**.

---

## Conclusion and Learning Outcomes:
In this lab, you successfully:
- Created a virtual machine (VM) instance using both the Google Cloud Console and `gcloud` command-line tool.
- Installed and configured an NGINX web server on the VM.
- Learned how to connect to VMs via SSH.
- Gained a better understanding of how to interact with Google Cloud resources such as Compute Engine and the use of regions and zones.

---

## Additional Notes:
- **Regions and Zones**: A region represents a geographical area, while zones are individual data centers within those regions. When creating a VM, both the region and zone must be specified.
- **Firewall Rules**: In the lab, HTTP traffic was allowed, which is necessary for web servers to be accessible over the web.

---

# Part 3: Creating a Persistent Disk

In this section, we will focus on **creating a persistent disk** in Google Cloud, attaching it to a virtual machine (VM), and using it for storage. Persistent disks in Google Cloud offer reliable and durable block storage that persists beyond the life of a VM, making them ideal for scenarios where you need to store data securely and independently of your VM’s lifecycle.

## Objectives
By the end of this section, you will:
1. **Create a Persistent Disk**.
2. **Attach the Persistent Disk to a Virtual Machine (VM)**.
3. **Format and mount the disk** for use.
4. **Ensure the disk is automatically remounted on VM restart**.

## Task 1: Create a New Persistent Disk

To start with, you’ll need to create a new persistent disk in the same zone where your virtual machine (VM) is located. This ensures that the disk can be attached to the VM.

1. **Open Cloud Shell** and make sure your project and zone are set up.
2. Run the following command to create a new 200 GB persistent disk:

   ```bash
   gcloud compute disks create mydisk --size=200GB --zone $ZONE
   ```

   - `mydisk`: Name of the disk.
   - `200GB`: Size of the disk.
   - `$ZONE`: The zone where the disk will be created (make sure this matches the zone of your VM).

   Example Output:
   ```bash
   NAME     ZONE    SIZE_GB  TYPE        STATUS
   mydisk   Zone    200      pd-standard READY
   ```

   - **Explanation**: This command creates a new persistent disk with a capacity of 200GB. The disk type (`pd-standard`) is a standard persistent disk, which provides durable block storage at a lower cost than SSD disks.

   **Objective Complete**: The disk has been created successfully.

## Task 2: Attaching the Persistent Disk to a VM

Next, you'll attach the newly created persistent disk (`mydisk`) to your virtual machine (`gcelab`).

1. **Attach the disk** to the `gcelab` VM by running:

   ```bash
   gcloud compute instances attach-disk gcelab --disk mydisk --zone $ZONE
   ```

   - `gcelab`: The name of the virtual machine to which the disk will be attached.
   - `mydisk`: The disk you want to attach.
   - `$ZONE`: The zone where your VM is located.

   Example Output:
   ```bash
   Updated [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-xxxxxxxxxx/zones/$ZONE/instances/gcelab].
   ```

   - **Explanation**: This command attaches the disk `mydisk` to the VM `gcelab` in the specified zone. Once attached, the disk is available to the VM as a block device.

   **Objective Complete**: The disk has been successfully attached to the VM.

## Task 3: Format and Mount the Persistent Disk

After attaching the disk to the VM, you'll need to format and mount it so it can be used for storage.

1. **SSH into the VM** to configure the disk:

   ```bash
   gcloud compute ssh gcelab --zone $ZONE
   ```

   Once inside the VM, list the available disks using:

   ```bash
   ls -l /dev/disk/by-id/
   ```

   You should see a device corresponding to your new persistent disk (e.g., `google-persistent-disk-1`).

2. **Create a mount point** for the disk:

   ```bash
   sudo mkdir /mnt/mydisk
   ```

3. **Format the disk** with an ext4 filesystem:

   ```bash
   sudo mkfs.ext4 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1
   ```

   - This command formats the disk and prepares it to store data using the ext4 filesystem.

   **Note**: The `discard` option helps to optimize the disk by enabling automatic trim operations, which is useful for SSDs.

4. **Mount the disk** to the mount point:

   ```bash
   sudo mount -o discard,defaults /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk
   ```

   - **Explanation**: This mounts the newly formatted disk to `/mnt/mydisk` so it can be used as a storage volume.

   **Objective Complete**: The disk is formatted and mounted successfully.

## Task 4: Automatically Mount the Disk on Restart

By default, if your VM restarts, the disk will not automatically remount. To ensure that the disk is remounted after a reboot, you need to update the `/etc/fstab` file.

1. **Edit the fstab file** to configure the auto-mount:

   ```bash
   sudo nano /etc/fstab
   ```

2. **Add an entry for the new disk** below the existing lines:

   ```bash
   /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk ext4 defaults 1 1
   ```

   - **Explanation**: This line ensures that the disk is automatically mounted at `/mnt/mydisk` after each reboot.

3. **Save and exit** the editor by pressing `CTRL+O`, then `ENTER`, and `CTRL+X`.

   **Objective Complete**: The disk will now be automatically remounted after a VM restart.

---

## Key Concepts Recap:
1. **Persistent disks** in Google Cloud are independent storage volumes that retain data even if the associated VM is deleted.
2. **Formatting and mounting** the disk is essential to use it as a file system within your VM.
3. **Auto-mounting** ensures that the disk is available across reboots.

---

# Part 4: Getting Started with Cloud Shell and gcloud

## Overview:
In this lab, you will explore Google Cloud Shell and the `gcloud` command-line interface (CLI) to manage and interact with Google Cloud resources. Cloud Shell provides a web-based terminal, preconfigured with tools such as the `gcloud` CLI, to allow you to perform tasks like creating virtual machines (VMs), configuring environment settings, and managing cloud resources efficiently.

## Prerequisites:
- Familiarity with standard Linux text editors (e.g., Vim, Emacs, or Nano).
- Basic understanding of cloud concepts such as virtual machines, regions, and zones.

## Lab Setup & Requirements:
- Access to a web browser (preferably Chrome).
- Incognito mode is recommended to avoid conflicts with your personal Google Cloud account.
- Temporary credentials will be provided for this lab. These credentials should be used for the duration of the lab.
- You cannot pause the lab once started, so ensure you are ready before clicking "Start Lab".

---

## Step 1: Setting Up Your Cloud Shell Environment

### **1.1 Sign into the Google Cloud Console**

1. Click the "Start Lab" button. A pop-up will appear prompting you to enter a payment method (if applicable).
2. Once you're signed into the Google Cloud Console, open Cloud Shell by clicking the **Activate Cloud Shell** icon in the top-right corner.
3. Cloud Shell will automatically set the project and authenticate you using the temporary credentials.

### **1.2 Configure Your Environment**

- **Set Region & Zone**: Regions and zones help define where resources are located. For this exercise, you'll set the default region and zone for your project using the following commands:

    ```bash
    gcloud config set compute/region <REGION>
    gcloud config set compute/zone <ZONE>
    ```

    Replace `<REGION>` and `<ZONE>` with the appropriate region and zone for your project. You can retrieve the zone and region metadata by running:

    ```bash
    gcloud compute project-info describe --project $(gcloud config get-value project)
    ```

- **Set Environment Variables**: Create environment variables for your project ID and zone, which will be useful for scripting and simplifying commands:

    ```bash
    export PROJECT_ID=$(gcloud config get-value project)
    export ZONE=$(gcloud config get-value compute/zone)
    ```

    Verify that these variables are set correctly:

    ```bash
    echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"
    ```

---

## Step 2: Using `gcloud` to Create a Virtual Machine (VM)

1. **Create a Virtual Machine**: Use the `gcloud` tool to create a new virtual machine in your selected zone:

    ```bash
    gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE
    ```

    This will create a new VM named `gcelab2` with the `e2-medium` machine type in the specified zone. After running the command, you should see output confirming the VM creation.

2. **Explore the gcloud Command Options**: You can always check for help or detailed options with:

    ```bash
    gcloud compute instances create --help
    ```

---

## Step 3: Connecting to Your VM Using SSH

1. **SSH into the VM**: Once the VM is created, connect to it using SSH via `gcloud compute ssh`:

    ```bash
    gcloud compute ssh gcelab2 --zone $ZONE
    ```

2. **Install nginx Web Server**: After connecting to your VM, you can install a basic web server (nginx) by running:

    ```bash
    sudo apt install -y nginx
    ```

3. **Exit SSH**: To exit the SSH session, simply type `exit` and you will return to your Cloud Shell environment.

---

## Step 4: Update the Firewall Rules to Allow HTTP Traffic

1. **List Firewall Rules**: Initially, your firewall will not allow HTTP traffic to your VM. You can see the existing rules by running:

    ```bash
    gcloud compute firewall-rules list
    ```

2. **Add Tags to the VM**: To make your VM accessible via HTTP (port 80), add tags to your VM:

    ```bash
    gcloud compute instances add-tags gcelab2 --tags http-server,https-server
    ```

3. **Create a Firewall Rule**: Now, create a new firewall rule to allow incoming HTTP traffic to your VM:

    ```bash
    gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
    ```

4. **Verify the Firewall Rule**: Ensure the new rule is in place by listing the firewall rules that allow HTTP traffic:

    ```bash
    gcloud compute firewall-rules list --filter=ALLOW:'80'
    ```

---

## Step 5: Testing HTTP Access to the VM

1. **Verify HTTP Connectivity**: You can test the HTTP connectivity by running the following command, which will use `curl` to access the VM's public IP address and display the nginx default web page:

    ```bash
    curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')
    ```

    You should see the default nginx landing page.

---

## Step 6: Viewing System Logs

1. **View Logs**: Use `gcloud` to view the available logs for your project:

    ```bash
    gcloud logging logs list
    ```

    This will list all logs available for your project. To narrow down the results to compute-related logs:

    ```bash
    gcloud logging logs list --filter="compute"
    ```

2. **Read Specific Logs**: You can read logs related to a specific instance, for example, `gcelab2`, by running:

    ```bash
    gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5
    ```

---

## Task Completion: Testing Your Understanding

At the end of the lab, you will be presented with a multiple-choice question to test your understanding of the core concepts covered in this lab. Here are the key learning points:
- Understanding regions and zones in Google Cloud.
- Using `gcloud` CLI to manage resources like virtual machines.
- Configuring firewall rules to allow necessary traffic to your VM.
- Accessing and viewing logs for resources on Google Cloud.

---

## Conclusion
In this lab, you’ve learned how to set up and configure your environment using Cloud Shell, create and manage virtual machines, adjust firewall settings to allow HTTP traffic, and view logs for your Google Cloud resources. These foundational skills are crucial for managing Google Cloud resources and will serve as the basis for more advanced operations in future labs.

---

# Part 5: Introduction to APIs in Google Cloud 

## Overview

In this section of the lab, you will explore how to interact with Google Cloud Storage using the Cloud Storage REST API, which allows you to create buckets and upload files programmatically. This will give you hands-on experience with API calls, authentication, and performing tasks that you would encounter when working with APIs in a cloud environment.

---

## Learning Objectives

By the end of this part of the lab, you will:

1. Understand the fundamentals of API architecture, HTTP methods, and RESTful APIs.
2. Learn how to enable and work with Google APIs.
3. Use the Cloud Storage API to create a bucket and upload files programmatically via Cloud Shell.
4. Understand the authentication and authorization process involved in API communication.

---

## Task 1: Using the API Library

In this task, you will enable an API in the Google Cloud API Library.

### Steps:

1. **Access the API Library:**
   - Open the **Navigation Menu** in the Google Cloud Console.
   - Navigate to **APIs & Services > Library**.

2. **Enable the API:**
   - In the **Search for APIs and Services** search bar, type "Fitness API" and press **Enter**.
   - Select the **Fitness API** from the search results.
   - Click the **Enable** button to enable the API.

3. **View API Quotas and Usage:**
   - Once enabled, you can view various details about the API, including tutorials, documentation, and quotas.
   - To check quotas, select the **QUOTAS & SYSTEM LIMITS** tab to view how many queries are allowed per day.

#### **What You Did:**
- You enabled the **Google Fitness API**. In the following tasks, you'll continue to use this API, as well as learn about how APIs work in Google Cloud.

---

## Task 2: Creating a JSON File in the Cloud Console

In this task, you will create a JSON file that contains the configuration for creating a Cloud Storage bucket.

### Steps:

1. **Create a JSON file:**
   - In Cloud Shell, run the following command to create and open a file called `values.json`:
     ```bash
     nano values.json
     ```
   
2. **Add JSON Content:**
   - Copy and paste the following content into the file, replacing `PROJECT_ID` with your unique project ID. This configuration defines the bucket’s name, location, and storage class.
     ```json
     {
       "name": "PROJECT_ID-bucket",
       "location": "us",
       "storageClass": "multi_regional"
     }
     ```
   - Exit the `nano` editor by pressing **CTRL + X**, then **Y**, and finally **Enter**.

### Explanation:
- This `values.json` file will be used later to create a Cloud Storage bucket via the API. It specifies the bucket’s name, location, and storage class, all of which are necessary when making an API request to create a bucket.

---

## Task 3: Authenticate and Authorize the Cloud Storage JSON/REST API

In this task, you will authenticate and authorize the Cloud Storage API using OAuth 2.0 to ensure that you have the necessary permissions to interact with Cloud Storage.

### Steps:

1. **Generate an OAuth Token:**
   - Open a new tab and go to the **OAuth 2.0 Playground**.
   - Scroll down and select **Cloud Storage API V1**.
   - Under **OAuth Scopes**, select `https://www.googleapis.com/auth/devstorage.full_control`.
   - Click **Authorize APIs**. Sign in to your Google account and click **Allow** to grant permissions.
   - After authorization, click **Exchange authorization code for tokens** to get the access token.

2. **Set the OAuth Token:**
   - Return to your Cloud Shell session and set the OAuth 2.0 token as an environment variable:
     ```bash
     export OAUTH2_TOKEN=<YOUR_TOKEN>
     ```
     Replace `<YOUR_TOKEN>` with the OAuth token you generated.

### Explanation:
- Authentication and authorization are necessary steps to ensure secure communication between your application and Google Cloud services. OAuth is used here to authenticate and allow access to Cloud Storage resources.

---

## Task 4: Create a Bucket with the Cloud Storage JSON/REST API

In this task, you will use the `curl` command to make a POST request to create a Cloud Storage bucket using the JSON data you prepared earlier.

### Steps:

1. **Set Project ID:**
   - Set your **Project ID** as an environment variable:
     ```bash
     export PROJECT_ID=$(gcloud config get-value project)
     ```

2. **Make the POST Request to Create the Bucket:**
   - Run the following command to send a POST request to the Cloud Storage API and create a new bucket:
     ```bash
     curl -X POST --data-binary @values.json \
         -H "Authorization: Bearer $OAUTH2_TOKEN" \
         -H "Content-Type: application/json" \
         "https://www.googleapis.com/storage/v1/b?project=$PROJECT_ID"
     ```

3. **Verify the Bucket Creation:**
   - After running the command, you should see a response with details of the newly created bucket, including its name, location, and other metadata.

### Solution Example:
You should see a response similar to:
```json
{
  "kind": "storage#bucket",
  "name": "PROJECT_ID-bucket",
  "location": "US",
  "storageClass": "MULTI_REGIONAL",
  "timeCreated": "2024-11-10T12:00:00.000Z"
}
```

---

## Task 5: Upload a File Using the Cloud Storage JSON/REST API

In this final task, you will upload a file to the Cloud Storage bucket using the JSON/REST API.

### Steps:

1. **Upload a File to Cloud Storage:**
   - First, upload the file you will use (`demo-image.png`) to Cloud Shell:
     - Click the **three-dotted menu** in the top-right of Cloud Shell, select **Upload > Choose File**, and select the image file.
   
2. **Set File Path as an Environment Variable:**
   - Run the following command to get the full path of the uploaded file:
     ```bash
     realpath demo-image.png
     ```
   - Set the file path as an environment variable:
     ```bash
     export OBJECT=<DEMO_IMAGE_PATH>
     ```

3. **Set Bucket Name:**
   - Set your bucket name as an environment variable:
     ```bash
     export BUCKET_NAME=PROJECT_ID-bucket
     ```

4. **Upload the File:**
   - Run the following `curl` command to upload the file:
     ```bash
     curl -X POST --data-binary @$OBJECT \
         -H "Authorization: Bearer $OAUTH2_TOKEN" \
         -H "Content-Type: image/png" \
         "https://www.googleapis.com/upload/storage/v1/b/$BUCKET_NAME/o?uploadType=media&name=demo-image"
     ```

5. **Verify File Upload:**
   - You should see a JSON response similar to:
     ```json
     {
       "name": "demo-image",
       "bucket": "PROJECT_ID-bucket",
       "contentType": "image/png",
       "size": "401951"
     }
     ```

---


## Conclusion

In this section, you learned how to interact with the Google Cloud Storage API to create buckets and upload files programmatically. You gained experience using API calls, authenticating with OAuth 2.0, and working with RESTful APIs, all of which are essential skills when integrating Google Cloud services into your applications.