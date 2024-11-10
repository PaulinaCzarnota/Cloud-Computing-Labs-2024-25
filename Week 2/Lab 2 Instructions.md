# Cloud Computing - Lab 2 Instructions

## Introduction
The purpose of this week's lab activities is to give you practical exposure to virtualization technology using the Oracle VM VirtualBox Manager. You will also gain insights into resource optimization by running multiple operating systems concurrently on the same physical host.  

### Important:
- This lab must be completed using the Desktop PCs provided in the lab.
- Prerequisites:
    - Windows 11 as the host OS.
    - Virtualization enabled in BIOS.
    - Oracle VM VirtualBox Manager installed.
    - Minimum of 16GB RAM, 2 CPUs, and 10GB free hard disk space.
    
After completing this lab, you will be able to:
- Create and manage virtual machines (VMs).
- Install an OS (Ubuntu) within a virtual environment.
- Share folders between the host and guest OS.
- Export, import, and backup VMs.

---

## Task 1 – Create a New Virtual Machine configured to run Ubuntu Linux
1. Download the Ubuntu 32-bit operating system (i386) desktop ISO image from [this link](https://releases.ubuntu.com/16.04/ubuntu-16.04.6-desktop-i386.iso).
2. Open the Oracle VM VirtualBox Manager and create a new Virtual Machine:
    - **Name**: Ubuntu
    - **Type**: Linux
    - **Version**: Ubuntu (32-bit)
3. Allocate memory:
    - 8192MB for machines with 16GB or more RAM.
    - 6144MB for machines with between 8GB and 16GB RAM.
4. Create a virtual hard disk (25GB).
5. Adjust processor settings to 2 CPUs if you have more than two processors.
6. Increase video memory to 128MB under the Display settings.

---

## Task 2 – Configure the Virtual Machine to boot from a virtual Linux CD
1. In the Oracle VM VirtualBox Manager, select your Ubuntu VM and click on "Settings."
2. Under **Storage**, add the Ubuntu ISO image as a virtual CD:
    - Navigate to the downloaded ISO file and select it.
3. Ensure the Ubuntu virtual machine will boot from the virtual CD.

---

## Task 3 – Create and configure a shared-folder between the Host machine and the guest Virtual Machine
1. On your host machine, create a folder at `c:\temp\sharedfolder`.
2. Right-click on your Ubuntu VM in Oracle VM VirtualBox and select **Settings** > **Shared Folders**.
3. Add the folder `c:\temp\sharedfolder` and name it `sharedfolder`.
4. Ensure that **Auto-mount** is checked.

---

## Video Demonstration
Watch the **VirtualBox Tutorial 06 - VM Configuration Settings Explained** video provided by the lecturer. It will help you understand the virtualized components and configurations.

---

## Task 4 – Install the Ubuntu Linux Operating System as a guest Virtual Machine
1. Start your Ubuntu VM.
2. Select **Install Ubuntu** and check the boxes for downloading updates and installing third-party software.
3. Choose **Erase disk and install Ubuntu**.
4. Complete the setup:
    - Set timezone to Dublin.
    - Choose keyboard layout.
    - Enter user details (Full name, username: testuser, password: testpass01).
5. After installation, restart the VM.

---

## Task 5 – Install the Oracle VM Guest Additions extension
1. With the Ubuntu VM running, go to **Devices** > **Install Guest Additions CD Image**.
2. Follow the prompts to install the Guest Additions software.
3. After installation, shut down the Ubuntu VM and remove the ISO from the virtual CD-ROM.

---

## Task 6 – Export your Virtual Machine to an OVA file so that you may put it on your USB key and use it anywhere
1. Ensure your Ubuntu VM is powered off.
2. In Oracle VM VirtualBox, go to **File** > **Export Appliance**.
3. Select the Ubuntu VM and choose a destination folder (`c:\temp\Ubuntu.ova`).
4. Complete the export process and save the OVA file to your USB key.

---

## Task 7 – Import an existing Virtual Machine (OVA file)
1. In Oracle VM VirtualBox, go to **File** > **Import Appliance**.
2. Select the `Ubuntu.ova` file from your USB key and import it.
3. Wait for the import process to complete.

---

## Task 8 – Backup an existing Virtual Machine (directly)
1. Locate the default machine folder where your Ubuntu VM is stored (e.g., `C:\Users\YourName\VirtualBox VMs`).
2. Copy the entire Ubuntu VM folder to an external drive.
3. This backup allows you to add the VM quickly to another system without re-importing.

---

## Task 9 – Add an existing Virtual Machine directly from your external hard drive
1. Ensure the Ubuntu VM is removed from Oracle VM VirtualBox.
2. Go to **Machine** > **Add** in Oracle VM VirtualBox.
3. Select the `Ubuntu.vbox` file from your external hard drive and click **Open**.
4. The VM will be added back to Oracle VM VirtualBox.