This repository contains a collection of labs completed for the Cloud Computing module during my third year of Computer Science at TU Dublin.
 
# Lab Summaries

### Lab 1

This lab focuses on using Google Cloud Shell to explore essential Linux command-line skills. It covers file creation and management, modifying file permissions, manipulating text files, and managing system processes. Key commands include chmod, grep, sort, and ps, providing a foundation for efficient cloud-based system administration.

### Lab 2

In this lab, I explored the practical aspects of virtualization using Oracle VM VirtualBox, focusing on creating and managing a virtual machine (VM) running Ubuntu Linux. The activities I undertook included:

1. **Creating a New Virtual Machine**: I set up a VM configured to run Ubuntu Linux, specifying system resources based on my host machine's capabilities.

2. **Boot Configuration**: I configured the VM to boot from a virtual Linux CD by attaching the Ubuntu ISO file to the virtual CD-ROM.

3. **Shared Folder Setup**: I established a shared folder between my host and the guest VM to facilitate file transfer.

4. **Ubuntu Installation**: I installed the Ubuntu operating system in the VM, configuring user accounts and system settings as part of the process.

5. **Guest Additions Installation**: I installed Oracle VM Guest Additions to enhance the performance and integration of the VM with the host system.

6. **Exporting the VM**: I exported the configured VM as an OVA file for portability and ease of use on different systems.

7. **Importing a VM**: I practiced importing an existing OVA file into VirtualBox, showcasing the simplicity of managing virtual appliances.

8. **Direct Backup**: I conducted a direct backup of the VM to an external drive, ensuring quick access and storage.

9. **Adding Existing VM**: I demonstrated the process of adding an existing VM from an external storage device, emphasizing quick VM deployment.

### Lab 4

In Lab 4, I explored various Google Cloud tools and services, gaining hands-on experience in managing resources, creating virtual machines (VMs), working with persistent disks, configuring network rules, and interacting with Google Cloud APIs. Below is a detailed summary of what I did in each part of the lab:

#### Part 1: Introduction to Google Cloud and Virtual Machines

In Part 1, I learned the fundamentals of setting up Google Cloud resources and managing them using the Google Cloud Console and Cloud Shell. The primary focus was on creating and configuring virtual machines (VMs). Here’s what I did:

- **Navigating the Google Cloud Console**: I accessed the Google Cloud Console, which allowed me to manage cloud resources such as Compute Engine, Networking, and Cloud Storage. This gave me a high-level understanding of how Google Cloud organizes and manages resources.
- **Creating a Virtual Machine**: I used the `gcloud` command-line tool to create a virtual machine, specifying machine types, regions, and zones. This task helped me understand how to deploy VMs in Google Cloud and how to choose the right configuration for specific use cases.
- **Connecting to the VM via SSH**: After creating the VM, I connected to it using the `gcloud compute ssh` command. This gave me the opportunity to remotely manage the VM and perform administrative tasks.
- **Installing Nginx**: I installed Nginx on the VM to set up a basic web server. I verified that the server was running by accessing the public IP address of the VM and ensuring that the default Nginx landing page was displayed.
- **Configuring Firewall Rules**: I created custom firewall rules to allow HTTP (port 80) and HTTPS (port 443) traffic to the VM, ensuring that the web server was accessible from the internet. This step was crucial in learning how to control traffic to cloud resources.

Through this part, I gained hands-on experience in managing VMs, installing software on them, and securing them with appropriate firewall rules.

#### Part 2: Networking and Firewall Rules

In Part 2, I focused on Google Cloud networking and firewall rules, which are essential for controlling access to resources. The tasks I completed included:

- **Creating a Virtual Private Cloud (VPC)**: I created a Virtual Private Cloud (VPC), which is Google Cloud's isolated network environment. This was a critical first step in understanding how networking works in Google Cloud and how resources are isolated within a VPC.
- **Configuring Firewall Rules**: I created custom firewall rules that allowed or blocked traffic to the VM based on specific ports, IP ranges, and protocols. This task helped me understand how to secure resources and control traffic flow into and out of a VPC.
- **Securing Google Cloud Resources**: I ensured that only necessary traffic (e.g., HTTP and SSH) could reach my VM, blocking other types of unwanted traffic. By configuring firewall rules, I learned how to apply security best practices, such as limiting the sources that can access the VM.

This part of the lab gave me a solid understanding of networking in Google Cloud and how to use firewall rules to secure my cloud resources effectively.

#### Part 3: Creating a Persistent Disk

Part 3 was focused on using Google Cloud's persistent disks, which provide durable and high-performance storage that remains intact even after a VM is stopped or restarted. Here’s what I did:

- **Creating a Persistent Disk**: I created a new persistent disk using the `gcloud compute disks create` command. This disk was attached to the VM, enabling me to store data separately from the VM’s root disk.
- **Formatting and Mounting the Disk**: After attaching the disk to the VM, I formatted it and mounted it into the VM's file system using Linux commands. This step helped me understand how to manage additional storage for VMs.
- **Configuring Auto-Mount**: I configured the disk to automatically remount when the VM is rebooted. This ensured that the data stored on the persistent disk remained accessible after VM restarts.
- **Testing Disk Functionality**: To verify that the disk was properly configured, I stored files on it and accessed them. This confirmed that the disk was functioning as expected and provided persistent storage.

Through this task, I learned how to manage persistent storage in Google Cloud, a crucial skill for working with stateful applications.

#### Part 4: Getting Started with Cloud Shell and gcloud

In Part 4, I was introduced to Cloud Shell and the `gcloud` command-line tool, which are essential for managing Google Cloud resources. I learned how to interact with Google Cloud programmatically and automate tasks. Here’s a breakdown of what I did:

- **Using Cloud Shell**: I used Cloud Shell, a browser-based terminal that comes pre-configured with the `gcloud` CLI and other development tools. This allowed me to perform all lab tasks without needing to set up any software on my local machine.
- **Setting Up Cloud Shell**: I configured Cloud Shell by setting default regions, zones, and environment variables. This setup streamlined future tasks and made working with cloud resources more efficient.
- **Creating and Managing VMs**: Using `gcloud`, I created a virtual machine, connected to it via SSH, and installed Nginx. I also learned how to interact with the VM through the command line, which made it easy to automate processes and manage resources.
- **Configuring Firewall Rules**: I added firewall rules using `gcloud` to allow HTTP traffic to the VM. This task demonstrated how to programmatically configure access to cloud resources.
- **Verifying the Setup**: After configuring the firewall, I tested the setup by accessing the Nginx web server using `curl`. This confirmed that the VM was accessible and properly configured.

This part helped me become comfortable with using Cloud Shell and the `gcloud` CLI, which are indispensable tools for managing Google Cloud resources efficiently.

#### Part 5: Introduction to APIs in Google Cloud

Part 5 introduced me to working with Google Cloud APIs, specifically the Cloud Storage REST API. I gained experience interacting with Google Cloud services programmatically and automating tasks. The tasks I performed included:

- **Enabling the Google Fitness API**: I enabled the Google Fitness API from the Google Cloud API Library. This was my first experience with enabling and using an API in Google Cloud.
- **Creating JSON Files for API Requests**: I created a `values.json` file that contained configuration data for creating a Cloud Storage bucket. The JSON file included the bucket’s name, location, and storage class. This file was used to automate API requests.
- **OAuth 2.0 Authentication**: I generated an OAuth 2.0 token using the OAuth 2.0 Playground. This token was used to authenticate API requests to Google Cloud services. This part taught me how to securely authenticate and authorize API calls using OAuth.
- **Creating a Cloud Storage Bucket**: I used the `curl` command to make a POST request to the Cloud Storage API, which created a new storage bucket based on the configuration in the `values.json` file. I then verified the bucket creation by inspecting the JSON response.
- **Uploading Files to Cloud Storage**: I uploaded a file (such as an image) to the Cloud Storage bucket using the `curl` command. After uploading, I verified the upload by checking the response JSON, which included metadata about the uploaded file.

Through this part, I learned how to interact with Google Cloud services programmatically using APIs. I gained valuable experience in using authentication methods, making API calls, and automating cloud tasks.
