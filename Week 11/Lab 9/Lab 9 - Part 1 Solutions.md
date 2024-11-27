# Cloud Computing - Lab 9 Solutions 

## Part 1 - VPC Networking Fundamentals

### Overview

This lab explores Google Cloud's Virtual Private Cloud (VPC) networking features. You will learn how to work with default VPC networks, create custom networks, manage firewall rules, and test connectivity between VM instances.

### Objectives

- Explore the default VPC network.
- Create an auto mode VPC network with firewall rules.
- Create VM instances and explore their connectivity.
- Test firewall rules and understand their effects on traffic.

### Setup and Requirements

#### Prerequisites
- Access to a standard web browser (Chrome recommended).
- Use an incognito window to avoid account conflicts.
- Temporary credentials provided by the lab.

#### Before Starting
- Do not use your personal Google Cloud account to avoid potential charges.
- Follow the lab instructions strictly to avoid issues.

### Tasks

#### Task 1: Explore the Default Network

1. **View Subnets**:
   - Navigate to **VPC network > VPC networks**.
   - Click on the `default` network to see subnets for each Google Cloud region.

2. **View Routes**:
   - Go to **Routes** under the VPC menu.
   - Select the `default` network and `us-central1` region.
   - Observe routes for internal traffic and the default internet gateway.

3. **View Firewall Rules**:
   - Navigate to **Firewall** in the VPC menu.
   - Review the default firewall rules:
     - `default-allow-icmp`
     - `default-allow-internal`
     - `default-allow-rdp`
     - `default-allow-ssh`

4. **Delete the Default Network**:
   - Delete all firewall rules under the `default` network.
   - Delete the `default` network from **VPC networks**.

5. **Verify**:
   - Attempt to create a VM instance under **Compute Engine**.
   - Observe the error: "No more networks available in this project."

#### Task 2: Create a VPC Network and VM Instances

1. **Create an Auto Mode VPC Network**:
   - Navigate to **VPC network > VPC networks**, and click **+CREATE VPC NETWORK**.
   - Set the name to `mynetwork` and select **Automatic** for subnet creation.
   - Enable all firewall rules:
     - `allow-icmp`, `allow-custom`, `allow-ssh`, `allow-rdp`.
   - Click **CREATE**.

2. **Create VM Instances**:
   - **Instance 1**:
     - Name: `mynet-us-vm`
     - Region: `us-east1`, Zone: `us-east1-c`
     - Machine type: `e2-micro`
   - **Instance 2**:
     - Name: `mynet-second-vm`
     - Region: `us-east4`, Zone: `us-east4-a`
     - Machine type: `e2-micro`

3. **Verify Internal IP Assignment**:
   - Confirm that IP addresses are assigned from the subnets' ranges:
     - `us-east1`: `10.142.0.0/20`
     - `us-east4`: `10.150.0.0/20`

#### Task 3: Explore Connectivity for VM Instances

1. **SSH into `mynet-us-vm`**:
   - Navigate to **Compute Engine > VM instances**.
   - Click **SSH** for `mynet-us-vm`.

2. **Test Connectivity**:
   - From `mynet-us-vm`, run:
     ```bash
     ping -c 3 <mynet-second-vm's internal IP>
     ping -c 3 <mynet-second-vm's external IP>
     ```
   - Verify that the connection is successful due to the `allow-custom` and `allow-icmp` firewall rules.

#### Task 4: Test Your Understanding

1. **Answer the Questions**:
   - Which firewall rule allows the ping to `mynet-second-vm`'s external IP address?  
     - `mynetwork-allow-icmp`
   - Google Cloud firewall rules let you allow or deny traffic to and from your VM instances based on a configuration.  
     - True
   - Firewall rules can be shared among networks.  
     - False

#### Task 5: Remove `allow-icmp` Firewall Rule

1. **Delete Rule**:
   - Navigate to **Firewall** and delete the `mynetwork-allow-icmp` rule.

2. **Verify**:
   - Run:
     ```bash
     ping -c 3 <mynet-second-vm's internal IP>
     ping -c 3 <mynet-second-vm's external IP>
     ```
   - The external ping should fail.

#### Task 6: Remove `allow-custom` Firewall Rule

1. **Delete Rule**:
   - Navigate to **Firewall** and delete the `mynetwork-allow-custom` rule.

2. **Verify**:
   - Run:
     ```bash
     ping -c 3 <mynet-second-vm's internal IP>
     ```
   - The internal ping should fail.

#### Task 7: Remove `allow-ssh` Firewall Rule

1. **Delete Rule**:
   - Navigate to **Firewall** and delete the `mynetwork-allow-ssh` rule.

2. **Verify**:
   - Attempt to SSH into `mynet-us-vm`. The connection should fail.