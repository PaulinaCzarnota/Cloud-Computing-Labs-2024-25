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
