# Cloud Computing - Lab 8 Solutions

## Part 2 - Hosting a Web App on Google Cloud Using Compute Engine (AWS)

### Overview

This lab focuses on deploying, managing, and scaling a web application on Google Cloud's Compute Engine using managed instance groups, autoscaling policies, and content delivery optimization.

---

### **Task 1: Setting Up Managed Instance Groups**

Managed instance groups (MIGs) ensure consistent deployment and automated scaling of VM instances.

1. **Create the frontend managed instance group (`fancy-fe-mig`):**
   ```bash
   gcloud compute instance-groups managed create fancy-fe-mig \
     --template fancy-fe-template \
     --base-instance-name fancy-fe \
     --size 2 \
     --zone ZONE
   ```

2. **Create the backend managed instance group (`fancy-be-mig`):**
   ```bash
   gcloud compute instance-groups managed create fancy-be-mig \
     --template fancy-be-template \
     --base-instance-name fancy-be \
     --size 2 \
     --zone ZONE
   ```

   Replace `ZONE` with your desired Compute Engine zone (e.g., `us-central1-a`).

---

### **Task 2: Configuring HTTP(S) Load Balancer**

#### **Step 1: Create a Backend Service**

1. Create a backend service for the frontend:  
   ```bash
   gcloud compute backend-services create fancy-fe-frontend \
       --protocol HTTP \
       --port-name http \
       --global
   ```

2. Create a backend service for the backend:  
   ```bash
   gcloud compute backend-services create fancy-be-backend \
       --protocol HTTP \
       --port-name http \
       --global
   ```

3. Add the instance groups to the backend services:
   ```bash
   gcloud compute backend-services add-backend fancy-fe-frontend \
       --instance-group fancy-fe-mig \
       --instance-group-zone ZONE \
       --global
   ```

   ```bash
   gcloud compute backend-services add-backend fancy-be-backend \
       --instance-group fancy-be-mig \
       --instance-group-zone ZONE \
       --global
   ```

---

#### **Step 2: Create the Load Balancer**

1. Reserve a global IP address:
   ```bash
   gcloud compute addresses create fancy-lb-ip --global
   ```

2. Create a URL map and target proxy:
   ```bash
   gcloud compute url-maps create fancy-url-map \
       --default-service fancy-fe-frontend
   ```

   ```bash
   gcloud compute target-http-proxies create fancy-http-proxy \
       --url-map fancy-url-map
   ```

3. Set up the forwarding rule:
   ```bash
   gcloud compute forwarding-rules create fancy-http-lb \
       --address fancy-lb-ip \
       --global \
       --target-http-proxy fancy-http-proxy \
       --ports 80
   ```

---

### **Task 3: Enabling Autoscaling**

Autoscaling dynamically adjusts the number of VM instances in response to traffic demands.

1. **Set autoscaling for the frontend instance group:**
   ```bash
   gcloud compute instance-groups managed set-autoscaling fancy-fe-mig \
       --max-num-replicas 5 \
       --target-load-balancing-utilization 0.6
   ```

2. **Set autoscaling for the backend instance group:**
   ```bash
   gcloud compute instance-groups managed set-autoscaling fancy-be-mig \
       --max-num-replicas 5 \
       --target-load-balancing-utilization 0.6
   ```

   - **Explanation**: These policies maintain optimal performance while controlling costs.

---

### **Task 4: Enabling Content Delivery Network (CDN)**

Improve web performance by caching static assets.

1. **Enable CDN for the frontend backend service:**
   ```bash
   gcloud compute backend-services update fancy-fe-frontend \
       --enable-cdn --global
   ```

2. Verify that CDN is enabled:
   ```bash
   gcloud compute backend-services describe fancy-fe-frontend \
       --global | grep cdnPolicy
   ```

   - **Outcome**: Faster content delivery and reduced latency.

---

### **Task 5: Updating the Web Application**

#### **Step 1: Update the Instance Template**

1. **Change the machine type:**
   ```bash
   gcloud compute instances set-machine-type frontend \
       --machine-type custom-4-3840
   ```

2. **Create a new instance template:**
   ```bash
   gcloud compute instance-templates create fancy-fe-new \
       --source-instance frontend \
       --source-instance-zone ZONE
   ```

3. Verify the new machine type:
   ```bash
   gcloud compute instances describe frontend | grep machineType
   ```

---

#### **Step 2: Push Website Updates**

1. Navigate to the updated React app:
   ```bash
   cd ~/monolith-to-microservices/react-app/src/pages/Home
   mv index.js.new index.js
   ```

2. Build the React app:
   ```bash
   cd ~/monolith-to-microservices/react-app
   npm install && npm run-script build
   ```

3. Upload the updated app to the Cloud Storage bucket:
   ```bash
   gsutil -m cp -r ~/monolith-to-microservices gs://fancy-store-$DEVSHELL_PROJECT_ID/
   ```

4. Trigger a rolling update to deploy the new template:
   ```bash
   gcloud compute instance-groups managed rolling-action replace fancy-fe-mig \
       --max-unavailable=100%
   ```

---

### **Task 6: Testing Failover**

1. Identify an instance in the frontend instance group:
   ```bash
   gcloud compute instance-groups list-instances fancy-fe-mig
   ```

2. SSH into the instance:
   ```bash
   gcloud compute ssh [INSTANCE_NAME]
   ```

3. Stop the application:
   ```bash
   sudo supervisorctl stop nodeapp
   sudo killall node
   ```

4. Monitor repair operations:
   ```bash
   watch -n 2 gcloud compute operations list \
       --filter='operationType~compute.instances.repair.*'
   ```

5. Verify that the instance is recreated and operational:
   ```bash
   watch -n 2 gcloud compute backend-services get-health fancy-fe-frontend --global
   ```

---

### **Task 7: Access the Updated Website**

1. Retrieve the load balancer IP address:
   ```bash
   gcloud compute forwarding-rules list --global
   ```

2. Open the browser and navigate to `http://[LB_IP]` to verify the changes.

---

#### **Outcome**

- Your web app is now fully hosted on Compute Engine with autoscaling and a CDN for optimized performance.
- Rolling updates ensure minimal downtime during changes.
- Failover mechanisms guarantee high availability.