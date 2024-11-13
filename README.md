This repository contains a collection of labs completed for the Cloud Computing module during my third year of Computer Science at TU Dublin.

# Lab Summaries

### Lab 1: Google Cloud Shell and Linux Command Line Basics

This lab focuses on using Google Cloud Shell to explore essential Linux command-line skills. It covers file creation and management, modifying file permissions, manipulating text files, and managing system processes. Key commands include `chmod`, `grep`, `sort`, and `ps`, providing a foundation for efficient cloud-based system administration.

### Lab 2: Virtualization with Oracle VM VirtualBox

In this lab, I explored virtualization using Oracle VM VirtualBox, focusing on creating and managing a virtual machine (VM) running Ubuntu Linux. Key activities included:

1. **Creating a New Virtual Machine**: Set up a VM configured to run Ubuntu Linux.
2. **Boot Configuration**: Configured the VM to boot from a virtual Linux CD.
3. **Shared Folder Setup**: Established a shared folder between the host and guest VM.
4. **Ubuntu Installation**: Installed the Ubuntu OS and configured user accounts.
5. **Guest Additions Installation**: Enhanced VM performance and integration.
6. **Exporting the VM**: Exported the VM as an OVA file for portability.
7. **Importing a VM**: Practiced importing an OVA file into VirtualBox.
8. **Direct Backup**: Backed up the VM to an external drive.
9. **Adding Existing VM**: Demonstrated adding an existing VM from external storage.

### Lab 3: Network Attached Storage (NAS) and RAID with FreeNAS in Oracle VirtualBox

This lab involved setting up FreeNAS in a virtual machine using Oracle VirtualBox to explore Network Attached Storage (NAS) and RAID technology. Tasks included:

1. **Installing FreeNAS**: Set up FreeNAS on a VM.
2. **Creating Storage Volumes**: Configured storage volumes and RAID arrays for redundancy.
3. **Setting Up Network Shares**: Enabled file sharing with SMB, NFS, and iSCSI.
4. **Configuring RAID**: Implemented RAID 1 and RAID 5 for fault tolerance.
5. **Testing**: Verified access to storage from client systems.

This lab provided practical experience in creating resilient storage solutions with FreeNAS and RAID in a virtualized environment.

### Lab 4: Google Cloud Resource Management

#### Part 1: Introduction to Google Cloud and Virtual Machines

This part introduced setting up and managing Google Cloud resources, particularly virtual machines (VMs), including:

- **Google Cloud Console**: Managed cloud resources such as Compute Engine, Networking, and Storage.
- **Creating a Virtual Machine**: Used `gcloud` to configure and deploy VMs.
- **SSH Access and Nginx Installation**: Connected to the VM via SSH and installed Nginx for a basic web server.
- **Firewall Rules Configuration**: Set custom rules to allow HTTP/HTTPS traffic.

#### Part 2: Networking and Firewall Rules

Explored networking and firewall rules for access control:

- **Virtual Private Cloud (VPC)**: Created a VPC for isolated networking.
- **Firewall Rules**: Configured rules to control inbound and outbound traffic.

#### Part 3: Persistent Disk Management

Learned how to create and manage persistent disks:

- **Disk Creation**: Created a persistent disk and attached it to a VM.
- **Formatting and Mounting**: Formatted and mounted the disk in the VM.
- **Auto-Mount Configuration**: Ensured the disk remounted on VM reboot.

#### Part 4: Cloud Shell and gcloud CLI

Introduction to Cloud Shell and the `gcloud` CLI for managing Google Cloud resources:

- **Cloud Shell Setup**: Configured default regions, zones, and variables.
- **Managing VMs with gcloud**: Created, managed, and connected to VMs via SSH.
- **Firewall Rules and Verification**: Set and tested firewall rules.

#### Part 5: Google Cloud APIs

Gained experience with Google Cloud APIs, focusing on Cloud Storage:

- **API Enabling and OAuth Authentication**: Enabled APIs and authenticated with OAuth.
- **API Calls**: Created a Cloud Storage bucket and uploaded files programmatically.

### Lab 5: Docker Basics

In this lab, I explored Docker containerization, covering topics like:

- **Running Containerized Applications**: Launched applications inside containers.
- **Creating Docker Images**: Used Dockerfiles to define container environments.
- **Volume Management**: Persisted data outside containers.
- **Port Mapping**: Exposed services by mapping ports.
- **Image Sharing**: Published images on Docker Hub.

This lab was based on Part 1 of the "DevOps with Docker" course from the University of Helsinki, completed in Google Cloud Shell.

### Lab 6: Load Balancers, API Gateway, Pub/Sub, and Cloud Run Functions

#### Part 1: Set Up Network and Application Load Balancers

This part introduced Network and Application Load Balancers in Google Cloud.

1. **Default Region and Zone Setup**: Configured defaults for efficient deployment.
2. **Web Server Instances Creation**: Deployed three VMs running Apache.
3. **Network Load Balancer (NLB) Configuration**: Set up NLB with target pools, health checks, and forwarding rules.
4. **Testing NLB**: Verified load balancing across VMs using `curl`.
5. **Application Load Balancer (ALB) Setup**: Created ALB with backend services and content-based routing.

#### Part 2: API Gateway, Pub/Sub, and Cloud Run Functions

Worked with API Gateway, Pub/Sub, and Cloud Run Functions to deploy and manage secure APIs and handle serverless functions:

1. **Deploying an API Backend**: Deployed a backend API secured through API Gateway.
2. **Creating and Configuring API Gateway**: Set up routing and security using API keys.
3. **Securing API Access**: Controlled access to the API with API keys.
4. **Deploying API Config**: Updated API configuration without affecting public access.
5. **Testing API Calls**: Verified API functionality and security with API keys.
6. **Setting up Pub/Sub**: Established Pub/Sub for real-time messaging between services.
7. **Creating and Deploying Cloud Functions**: Created and tested Cloud Run functions for event-driven responses.
8. **Challenge Lab**: Applied skills to create a Cloud Function, API Gateway, and Pub/Sub integration.

### Lab 7: Google Cloud IAM Custom Roles, Firestore Migration, and Pub/Sub Messaging

#### Part 1: IAM Custom Roles and Permissions Management

In this part, I explored Google Cloud IAM by creating and managing custom roles to fine-tune access control for cloud resources:

1. **Creating a Custom Role**: Defined a custom IAM role with permissions specifically for interacting with a Cloud Storage bucket, focusing on principles of least privilege.
2. **Assigning the Custom Role**: Assigned the role to a test user, allowing for controlled access to bucket data while limiting actions outside of the assigned permissions.
3. **Testing Role Permissions**: Verified access by testing read and write operations and ensuring the role met all security requirements.
4. **Revoking Access**: Removed the custom role to confirm that only authorized users with assigned roles could access resources, reinforcing IAM’s role-based access control.

This task provided valuable experience in implementing principle-based access management on Google Cloud and utilizing IAM for secure role assignments.

#### Part 2: Firestore Data Migration

This part focused on migrating customer data to Firestore, showcasing Firestore as a scalable, NoSQL cloud database for real-time data handling:

1. **Firestore Setup**: Configured Firestore in Datastore mode to leverage its document-oriented storage and transaction capabilities, suitable for handling structured customer data.
2. **Data Import Code**: Developed and executed code to automate the data migration process from legacy formats, ensuring efficient and accurate data transfer to Firestore.
3. **Creating Test Data**: Created sample customer data in the local environment to simulate real-world scenarios and verify the migration code’s accuracy.
4. **Verifying Data Import**: Checked the migrated data in Firestore to ensure consistency and integrity, demonstrating Firestore’s ability to manage and scale application data.

This task highlighted the steps for successful data migration and Firestore’s role in modernizing data storage for high-availability applications.

#### Part 3: Pub/Sub Basics and Message Handling

In this part, I explored the Google Cloud Pub/Sub messaging service to handle asynchronous communication and event streaming between services:

1. **Creating Topics and Subscriptions**: Set up topics for publishing messages and subscriptions for message retrieval, demonstrating Pub/Sub’s support for decoupled architecture.
2. **Publishing Messages**: Published multiple custom messages to a topic, representing various event types, simulating real-time event data streaming in a microservices environment.
3. **Pulling and Processing Messages**: Configured a pull subscription to retrieve and auto-acknowledge messages from the topic. Learned how Pub/Sub ensures reliable message delivery, even in scenarios with multiple subscribers.
4. **Batch Pulling and Testing**: Utilized the `--limit` flag to retrieve multiple messages simultaneously, exploring options for efficient message processing and scalability in data pipelines.

This part illustrated the use of Pub/Sub for building flexible, asynchronous systems, enhancing the understanding of event-driven architecture and reliable messaging in distributed cloud applications.
