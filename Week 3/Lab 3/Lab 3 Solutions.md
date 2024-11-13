# Cloud Computing - Lab 3 Solutions


### Overview

This lab provides step-by-step instructions and solutions for completing the Cloud Computing - Week 3 Lab, which focuses on Network Attached Storage (NAS) and RAID Storage Technology using FreeNAS within Oracle VirtualBox.

### Prerequisites

1. **Operating System**: Windows 10/11 with virtualization enabled in BIOS.
2. **Software**: Oracle VirtualBox (version 7.x in TU Dublin labs), 16GB RAM, 2 CPUs, and 10GB free disk space.
3. **Download FreeNAS ISO**: Version 11.0-U4 [available here](https://download.freenas.org/11/11.0-U4/x64/FreeNAS-11.0-U4.iso).

### Task 1 – Network Attached Storage (NAS)

#### Goal
Gain familiarity with configuring FreeNAS as a Network Attached Storage device, creating shared folders, and enabling access from Windows and WebDAV-enabled clients.

#### Steps and Solutions

1. **Creating the FreeNAS Virtual Machine**:
   - Open Oracle VirtualBox and create a new VM for FreeNAS. 
   - Set up the VM with **dynamically allocated storage** (see video at 3:05), and configure video memory to **128MB** to improve VM performance (see video at 4:27).
   - Set the network adapter to **Host-Only Adapter** instead of Bridge Adapter to ensure stable connectivity within the host machine’s environment.

2. **Installing FreeNAS**:
   - Mount the downloaded FreeNAS ISO and start the VM to install FreeNAS. Follow the prompts to complete the installation.

3. **Configuring FreeNAS**:
   - Boot the FreeNAS VM and access the FreeNAS Web Administrative Console through the IP address displayed on the console screen.
   - **Solution**: If the VM is not showing an IP address, ensure the network adapter is set to Host-Only and restart the VM.

4. **Setting Up Shared Folders**:
   - Create a Windows shared folder in FreeNAS and connect to it from a Windows client.
     - **Solution**: When setting up the network location, use the three-dot menu in File Explorer (see video at 12:30) and select "Add a Network Location."
     - If the **network location setup fails** (error at 13:20), note this is due to a security policy in TU Dublin labs. Watch the video for context but proceed to the next task.
   
5. **Creating a WebDAV Folder**:
   - In FreeNAS, create a WebDAV shared folder and connect to it from a WebDAV-enabled client.
   - **Solution**: Use a WebDAV-compatible browser or application to test the connection and ensure proper folder access.

### Task 2 – RAID Storage Volumes

#### Goal
Learn to configure RAID storage volumes, create and access shared RAID volumes, and set up FTP server access.

#### Steps and Solutions

1. **Creating a RAID Stripe Volume**:
   - Watch **FreeNAS 11 Beginner 09** to configure a RAID 0 (Stripe) volume in FreeNAS.
   - **Solution**: Ensure the VM storage settings match the video (see 1:24) by removing any extra virtual drives.
   - **Access Issue**: If unable to connect to `stripeshare`, this is due to disabled Guest access in SMB2 on Windows 10/11. A workaround exists but is not recommended in lab settings for security reasons.

2. **Creating a RAID 5 Volume**:
   - Follow **FreeNAS 11 Beginner 10** to set up a RAID 5 volume.
   - **Solution**: In VirtualBox 7.x, select the option similar to **LsilLogic (Default SCSI)** (see 1:47) instead of "Add SCSI Controller" in version 6.x.

3. **Configuring FTP Server**:
   - Watch **FreeNAS 11 Beginner 11** to set up an FTP server on FreeNAS and create a user with upload/download permissions.
   - **Solution**: If working on a personal laptop, use **FileZilla Portable** to connect to the FTP server if FileZilla is not already installed.