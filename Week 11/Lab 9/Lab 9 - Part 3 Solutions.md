# Cloud Computing - Lab 9 Solutions 

## Part 3 - Cloud Run Functions: Qwik Start - Command Line

### Overview

A Cloud Run function is a serverless solution that executes in response to events like HTTP requests or Pub/Sub messages. It reduces the overhead of managing infrastructure, offering a scalable and cost-efficient event-driven architecture. 

In this tutorial, you will:
1. Create a Cloud Run function.
2. Deploy the function with a Pub/Sub trigger.
3. Test the function using the command line.
4. View logs to confirm the function's execution.

---

### **Setup**

### Prerequisites
1. Access to Google Cloud.
2. A Qwiklabs-provided temporary account (use the credentials given during the lab).
3. Ensure your browser is in **Incognito/Private mode** to prevent conflicts with personal Google accounts.

---

### **Steps to Complete**

#### **Task 1: Create a Cloud Run Function**

1. **Set the Default Region**  
   In Cloud Shell, set the default region for your function:
   ```bash
   gcloud config set run/region us-east1
   ```

2. **Create a Directory for the Function Code**  
   ```bash
   mkdir gcf_hello_world && cd $_
   ```

3. **Create and Edit `index.js`**  
   Open `index.js` in a text editor:
   ```bash
   nano index.js
   ```
   Add the following code:
   ```javascript
   const functions = require('@google-cloud/functions-framework');

   // Define the CloudEvent callback for Pub/Sub triggers
   functions.cloudEvent('helloPubSub', cloudEvent => {
     const base64name = cloudEvent.data.message.data;
     const name = base64name
       ? Buffer.from(base64name, 'base64').toString()
       : 'World';

     console.log(`Hello, ${name}!`);
   });
   ```
   Save and exit (`Ctrl + X`, then `Y`).

4. **Create and Edit `package.json`**  
   Open `package.json` in a text editor:
   ```bash
   nano package.json
   ```
   Add the following content:
   ```json
   {
     "name": "gcf_hello_world",
     "version": "1.0.0",
     "main": "index.js",
     "scripts": {
       "start": "node index.js",
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "dependencies": {
       "@google-cloud/functions-framework": "^3.0.0"
     }
   }
   ```
   Save and exit (`Ctrl + X`, then `Y`).

5. **Install Dependencies**  
   Run the following command to install required packages:
   ```bash
   npm install
   ```
   **Expected Output**:
   ```
   added 140 packages, and audited 141 packages in 9s
   ```

---

#### **Task 2: Deploy the Function**

1. **Deploy the Function**  
   Deploy the `helloPubSub` function with a Pub/Sub trigger:
   ```bash
   gcloud functions deploy nodejs-pubsub-function \
     --gen2 \
     --runtime=nodejs20 \
     --region=us-east1 \
     --source=. \
     --entry-point=helloPubSub \
     --trigger-topic cf-demo \
     --stage-bucket qwiklabs-gcp-03-caa26f4c8fd6-bucket \
     --service-account cloudfunctionsa@qwiklabs-gcp-03-caa26f4c8fd6.iam.gserviceaccount.com \
     --allow-unauthenticated
   ```

2. **Verify the Deployment Status**  
   Check if the function is active:
   ```bash
   gcloud functions describe nodejs-pubsub-function \
     --region=us-east1
   ```
   **Expected Output**:
   ```
   State: ACTIVE
   Url: https://us-east1-[PROJECT_ID].cloudfunctions.net/nodejs-pubsub-function
   ```

---

#### **Task 3: Test the Function**

1. **Publish a Test Message**  
   Trigger the Pub/Sub topic with sample data:
   ```bash
   gcloud pubsub topics publish cf-demo --message="Cloud Function Gen2"
   ```
   **Expected Output**:
   ```
   messageIds:
   - '11927162971409664'
   ```

2. **View Logs**  
   Check the logs to confirm execution:
   ```bash
   gcloud functions logs read nodejs-pubsub-function \
     --region=us-east1
   ```
   **Sample Log Output**:
   ```
   LOG: Hello, Cloud Function Gen2!
   ```

---

### **Key Notes**

- Logs might take a few minutes to appear. Alternatively, use the Google Cloud Console's **Logging > Logs Explorer**.
- Use the `--region` flag consistently to avoid deployment or access issues.

---

### **Task 5: Test Your Understanding**

#### Question:  
**Serverless lets you write and deploy code without the hassle of managing the underlying infrastructure.**  
- **Answer:** True