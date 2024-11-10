# Cloud Computing - Lab 6 Solutions


## Part 1 - Set Up Network and Application Load Balancers 

### **Task 1: Set the Default Region and Zone for All Resources**

Before creating any resources, it’s important to set the default region and zone where your virtual machines (VMs) will reside. This is done using the `gcloud` command-line tool.

#### Commands:
1. Set the default region to `us-central1`:
   ```bash
   gcloud config set compute/region us-central1
   ```
2. Set the default zone to `us-central1-f`:
   ```bash
   gcloud config set compute/zone us-central1-f
   ```

**Explanation:**  
- The region (`us-central1`) is the geographical area where your cloud resources will be deployed.  
- The zone (`us-central1-f`) is a specific data center within that region. This setting ensures that all resources you create later default to these configurations, so you don't have to specify them each time.

---

### **Task 2: Create Multiple Web Server Instances**

In this task, you'll create three VM instances (web servers), each with Apache installed and a simple HTML page indicating which server is serving the content. This will simulate a basic web server setup that will later be balanced by the load balancer.

#### Commands:
1. Create VM `www1`:
   ```bash
   gcloud compute instances create www1 \
     --zone=us-central1-f \
     --tags=network-lb-tag \
     --machine-type=e2-small \
     --image-family=debian-11 \
     --image-project=debian-cloud \
     --metadata=startup-script='#!/bin/bash
       apt-get update
       apt-get install apache2 -y
       service apache2 restart
       echo "<h3>Web Server: www1</h3>" | tee /var/www/html/index.html'
   ```
2. Create VM `www2`:
   ```bash
   gcloud compute instances create www2 \
     --zone=us-central1-f \
     --tags=network-lb-tag \
     --machine-type=e2-small \
     --image-family=debian-11 \
     --image-project=debian-cloud \
     --metadata=startup-script='#!/bin/bash
       apt-get update
       apt-get install apache2 -y
       service apache2 restart
       echo "<h3>Web Server: www2</h3>" | tee /var/www/html/index.html'
   ```
3. Create VM `www3`:
   ```bash
   gcloud compute instances create www3 \
     --zone=us-central1-f \
     --tags=network-lb-tag \
     --machine-type=e2-small \
     --image-family=debian-11 \
     --image-project=debian-cloud \
     --metadata=startup-script='#!/bin/bash
       apt-get update
       apt-get install apache2 -y
       service apache2 restart
       echo "<h3>Web Server: www3</h3>" | tee /var/www/html/index.html'
   ```

4. Create a firewall rule to allow HTTP traffic (port 80) to the VMs:
   ```bash
   gcloud compute firewall-rules create www-firewall-network-lb \
     --target-tags network-lb-tag --allow tcp:80
   ```

**Explanation:**  
- These commands create three VM instances (www1, www2, www3) with the `debian-11` OS and install Apache web server.
- The `startup-script` ensures Apache runs and serves a custom HTML page that indicates which server is currently responding.
- The firewall rule allows incoming HTTP traffic (port 80) to the VMs.

---

### **Task 3: Configure the Load Balancing Service**

Now that you have three web servers running, you'll configure a **Network Load Balancer** (NLB) to distribute incoming traffic among them. A Network Load Balancer routes traffic to backend instances based on IP address and port.

#### Commands:
1. Create a static external IP address for the NLB:
   ```bash
   gcloud compute addresses create network-lb-ip-1 \
     --region us-central1
   ```
2. Create an HTTP health check to monitor the health of the web servers:
   ```bash
   gcloud compute http-health-checks create basic-check
   ```
3. Create a target pool for the load balancer, linking the health check:
   ```bash
   gcloud compute target-pools create www-pool \
     --region us-central1 --http-health-check basic-check
   ```
4. Add the instances (`www1`, `www2`, `www3`) to the target pool:
   ```bash
   gcloud compute target-pools add-instances www-pool \
     --instances www1,www2,www3
   ```
5. Create a forwarding rule to forward traffic from the external IP to the target pool:
   ```bash
   gcloud compute forwarding-rules create www-rule \
     --region us-central1 \
     --ports 80 \
     --address network-lb-ip-1 \
     --target-pool www-pool
   ```

**Explanation:**  
- The **static external IP** (`network-lb-ip-1`) is created for the load balancer to use.
- The **HTTP health check** ensures that traffic is only sent to healthy instances.
- A **target pool** is created to group the backend instances, and the instances are added to this pool.
- A **forwarding rule** is set up so that external traffic on port 80 will be forwarded to the instances in the pool.

---

### **Task 4: Sending Traffic to Your Instances**

Once the load balancer is configured, you can begin sending traffic to the instances and observe the load balancer in action. Traffic should be randomly distributed between the three web servers.

#### Commands:
1. Get the external IP of the load balancer:
   ```bash
   gcloud compute forwarding-rules describe www-rule --region us-central1
   ```
2. Extract the IP address:
   ```bash
   IPADDRESS=$(gcloud compute forwarding-rules describe www-rule --region us-central1 --format="json" | jq -r .IPAddress)
   ```
3. Use `curl` to continuously send requests to the load balancer's IP:
   ```bash
   while true; do curl -m1 $IPADDRESS; done
   ```

**Explanation:**  
- The `curl` command sends HTTP requests to the external IP of the load balancer. The traffic should be distributed across the three web servers, with the response alternating between them.

---

### **Task 5: Create an Application Load Balancer**

The **Application Load Balancer (ALB)** is different from a Network Load Balancer because it can perform content-based routing and can handle more complex traffic patterns, such as routing based on URL paths.

#### Commands:
1. Create an instance template for the backend service:
   ```bash
   gcloud compute instance-templates create lb-backend-template \
      --region=us-central1 \
      --network=default \
      --subnet=default \
      --tags=allow-health-check \
      --machine-type=e2-medium \
      --image-family=debian-11 \
      --image-project=debian-cloud \
      --metadata=startup-script='#!/bin/bash
        apt-get update
        apt-get install apache2 -y
        a2ensite default-ssl
        a2enmod ssl
        vm_hostname="$(curl -H "Metadata-Flavor:Google" \
        http://169.254.169.254/computeMetadata/v1/instance/name)"
        echo "Page served from: $vm_hostname" | \
        tee /var/www/html/index.html
        systemctl restart apache2'
   ```
2. Create a managed instance group (MIG) using the instance template:
   ```bash
   gcloud compute instance-groups managed create lb-backend-group \
      --template=lb-backend-template --size=2 --zone=us-central1-f
   ```
3. Allow health checks to pass through the firewall:
   ```bash
   gcloud compute firewall-rules create fw-allow-health-check \
     --network=default \
     --action=allow \
     --direction=ingress \
     --source-ranges=130.211.0.0/22,35.191.0.0/16 \
     --target-tags=allow-health-check \
     --rules=tcp:80
   ```
4. Create a global static external IP for the ALB:
   ```bash
   gcloud compute addresses create lb-ipv4-1 \
     --ip-version=IPV4 \
     --global
   ```
5. Create a health check for the ALB:
   ```bash
   gcloud compute health-checks create http http-basic-check \
     --port 80
   ```
6. Create a backend service:
   ```bash
   gcloud compute backend-services create web-backend-service \
     --protocol=HTTP \
     --port-name=http \
     --health-checks=http-basic-check \
     --global
   ```
7. Add the instance group as a backend to the backend service:
   ```bash
   gcloud compute backend-services add-backend web-backend-service \
     --instance-group=lb-backend-group \
     --instance-group-zone=us-central1-f \
     --global
   ```
8. Create a URL map for routing requests:
   ```bash
   gcloud compute url-maps create web-map-http \
       --default-service web-backend-service
   ```
9. Create a target HTTP

 proxy for the ALB:
   ```bash
   gcloud compute target-http-proxies create http-lb-proxy \
     --url-map=web-map-http
   ```
10. Create a global forwarding rule to route traffic to the target HTTP proxy:
   ```bash
   gcloud compute forwarding-rules create http-content-rule \
     --global \
     --target-http-proxy=http-lb-proxy \
     --ports=80 \
     --address=lb-ipv4-1
   ```

**Explanation:**  
- The steps above create a new Application Load Balancer (ALB) that distributes HTTP traffic across backend instances based on a URL map, and uses health checks to ensure that only healthy instances receive traffic.