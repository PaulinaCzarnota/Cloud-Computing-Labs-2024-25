# Cloud Computing - Lab 6 Solutions


## Part 2 - Get Started with API Gateway

## 1. API Gateway: Qwik Start

### **Overview**
In this lab, you will deploy an API on **Google Cloud API Gateway**, which routes requests to a backend Cloud Function. You'll secure the API using an **API key** and test your setup.

---

### **Pre-requisites**
- **Cloud Console Access**: You'll work within the Google Cloud Console, so be sure to follow the instructions to sign in with the temporary credentials provided.
- **Browser**: Chrome browser is recommended for the lab.

---

### **Setup and Requirements**
- Read through the lab instructions.
- Open the Cloud Console in **Incognito Mode** to avoid conflicts with your personal account.
- Complete the tasks in sequence. You can’t pause the lab, and the timer starts once you click **Start Lab**.

---

### **Task 1: Deploying an API Backend**
In this task, you’ll deploy a **Cloud Function** as the backend of your API.

1. **Clone the Cloud Function Sample Repository:**
   - Open **Cloud Shell** and run the following command to clone the sample repository:
     ```bash
     git clone https://github.com/GoogleCloudPlatform/nodejs-docs-samples.git
     ```

2. **Navigate to the Cloud Function Directory:**
   - Change to the directory that contains the Cloud Function:
     ```bash
     cd nodejs-docs-samples/functions/helloworld/helloworldGet
     ```

3. **Deploy the Cloud Function:**
   - To deploy the Cloud Function with an HTTP trigger, run:
     ```bash
     gcloud functions deploy helloGET --runtime nodejs20 --trigger-http --allow-unauthenticated --region us-central1
     ```
   - If prompted, enter `Y` to enable the API.

4. **Verify Deployment:**
   - Once the deployment is complete, get the URL of the deployed function:
     ```bash
     gcloud functions describe helloGET --region us-central1
     ```
   - Note the `httpsTrigger.url` value, which should look something like:
     ```
     https://us-central1-qwiklabs-gcp-03-4c74a6064fd5.cloudfunctions.net/helloGET
     ```

5. **Test the Cloud Function:**
   - Run the following `curl` command to test the deployed Cloud Function:
     ```bash
     curl -v https://us-central1-qwiklabs-gcp-03-4c74a6064fd5.cloudfunctions.net/helloGET
     ```
   - You should see the message: `Hello World!`

   **Click "Check my progress" to verify completion.**

---

### **Task 2: Test the API Backend**
Now that your backend is deployed, you’ll test it via the API Gateway.

1. **Set the Project ID:**
   - Set the `PROJECT_ID` as an environment variable:
     ```bash
     export PROJECT_ID=qwiklabs-gcp-03-4c74a6064fd5
     ```

2. **Test the Cloud Function (again):**
   - To test, run the following `curl` command:
     ```bash
     curl -v https://us-central1-qwiklabs-gcp-03-4c74a6064fd5.cloudfunctions.net/helloGET
     ```
   - Verify that you see the message `Hello World!`.

   **Click "Check my progress" to verify completion.**

---

### **Task 3: Creating a Gateway**
In this task, you’ll create the **API Gateway** that will route incoming requests to your Cloud Function.

1. **Generate an API ID:**
   - Run the following command to generate a unique API ID:
     ```bash
     export API_ID="hello-world-$(cat /dev/urandom | tr -dc 'a-z' | fold -w ${1:-8} | head -n 1)"
     echo $API_ID
     ```

2. **Navigate to API Gateway:**
   - In the **Cloud Console**, go to **API Gateway** by searching for it in the top search bar.

3. **Create a New Gateway:**
   - Click **Create Gateway**.
   - **API Section**: Select **Create new API**.
     - Enter **Hello World API** as the display name.
     - For the **API ID**, use the value generated above by the command:
       ```bash
       echo $API_ID
       ```

4. **Create API Config:**
   - Select **Create new API config**.
   - Upload the `openapi2-functions.yaml` file created in a previous task.
   - Enter **Hello World Config** as the Display Name.
   - Use the **Compute Engine default service account**.

5. **Set Gateway Details:**
   - For **Display Name**, enter **Hello Gateway**.
   - Set the **Location** to `us-central1`.
   - Click **Create Gateway**.

   **Note**: This may take 10 minutes to deploy. Monitor the status using the Notification icon in the console.

   **Click "Check my progress" to verify completion.**

---

### **Task 4: Securing Access by Using an API Key**
In this task, you’ll secure the API with an API key.

1. **Create API Key:**
   - Navigate to **APIs & Services > Credentials**.
   - Click **Create credentials** and select **API Key**.
   - **Copy the API Key** shown and store it in Cloud Shell:
     ```bash
     export API_KEY=REPLACE_WITH_COPIED_API_KEY
     ```

2. **Enable API Key Support:**
   - Run the following to obtain the name of your Managed Service:
     ```bash
     MANAGED_SERVICE=$(gcloud api-gateway apis list --format json | jq -r .[0].managedService | cut -d'/' -f6)
     echo $MANAGED_SERVICE
     ```
   - Enable API key support:
     ```bash
     gcloud services enable $MANAGED_SERVICE
     ```

3. **Modify OpenAPI Spec to Require API Key:**
   - Create a new YAML file (`openapi2-functions2.yaml`) with the following content to enable API key validation:
     ```yaml
     swagger: '2.0'
     info:
       title: API_ID description
       description: Sample API on API Gateway with a Google Cloud Functions backend
       version: 1.0.0
     schemes:
       - https
     produces:
       - application/json
     paths:
       /hello:
         get:
           summary: Greet a user
           operationId: hello
           x-google-backend:
             address: https://us-central1-qwiklabs-gcp-03-4c74a6064fd5.cloudfunctions.net/helloGET
           security:
             - api_key: []
           responses:
             '200':
               description: A successful response
               schema:
                 type: string
     securityDefinitions:
       api_key:
         type: "apiKey"
         name: "key"
         in: "query"
     ```

4. **Replace Variables in OpenAPI Spec:**
   - Run the following commands to replace the `API_ID` and `PROJECT_ID` in the spec file:
     ```bash
     sed -i "s/API_ID/${API_ID}/g" openapi2-functions2.yaml
     sed -i "s/PROJECT_ID/$PROJECT_ID/g" openapi2-functions2.yaml
     ```

5. **Download the Updated Spec:**
   - Run:
     ```bash
     cloudshell download $HOME/openapi2-functions2.yaml
     ```
   - **Click Download** to save the file locally.

   **Click "Check my progress" to verify completion.**

---

### **Task 5: Create and Deploy a New API Config to Your Existing Gateway**
In this task, you’ll deploy the updated OpenAPI spec to your existing gateway.

1. **Edit Gateway Config:**
   - Go to **API Gateway** in the Cloud Console.
   - Click on **Hello Gateway**.
   - Click **Edit** at the top.
   - Under **API Config**, select **Create new API config**.
   - Upload the `openapi2-functions2.yaml` file.
   - Enter **Hello Config** as the Display Name.
   - Select **Qwiklabs User Service Account** for the Service Account.
   - Click **Update**.

   **Note**: It may take several minutes for the update to complete.

   **Click "Check my progress" to verify completion.**

---

### **Task 6: Testing Calls Using Your API Key**
Finally, test your secured API using the API key.

1. **Retrieve the Gateway URL:**
   - Run the following to get the API Gateway URL:
     ```bash
     export GATEWAY_URL=$(gcloud api-gateway gateways describe hello-gateway --location us-central1 --format json | jq -r .defaultHostname)
     echo $GATEWAY_URL
     ```

2. **Test Without API Key:**
   - Run this command to test without the API key:
     ```bash
     curl -sL $GATEWAY_URL/hello
     ```
   - You should get an error like:
     ```
     UNAUTHENTICATED: Method doesn't allow unregistered callers
     ```

3. **Test With API Key:**
   - Run the command with the API key as a query parameter:
     ```bash
     curl -sL -w "\n" $GATEWAY_URL/

hello?key=$API_KEY
     ```
   - You should see the response:
     ```
     Hello World!
     ```

---

## 2. Pub/Sub: Qwik Start - Console

This is a hands-on lab to get familiar with Google Cloud Pub/Sub, a messaging service for exchanging event data between applications. In this lab, you'll learn how to:

- Set up a Pub/Sub topic
- Create a subscription
- Publish and consume messages using a pull subscriber

---

## Overview

### What is Pub/Sub?

Google Cloud Pub/Sub is a messaging service that allows producers to publish messages to a topic, and consumers to receive these messages through subscriptions. The service supports both pull and push mechanisms for subscribers. Pull subscribers can request messages from the topic, and each subscriber must acknowledge the messages within a set time.

### What You'll Learn

- **Create a Pub/Sub topic** to hold messages.
- **Subscribe to a topic** to receive messages.
- **Publish and consume messages** through a pull subscription.

---

## **Setup and Requirements**

Before you start:

1. **Web Browser**: Use a standard internet browser (Google Chrome recommended).
2. **Incognito Window**: Use an Incognito window to avoid conflicts between your personal Google account and the lab’s temporary credentials.
3. **Time**: The lab has a fixed time duration and cannot be paused, so ensure you have about **30 minutes** to complete it.
4. **Google Cloud Credentials**: You will use temporary credentials provided for the lab, so do not use your personal Google Cloud account.

---

## **Step-by-Step Instructions**

### **How to Start the Lab and Sign In**

1. **Start the Lab**: Click the *Start Lab* button on the lab page. You may need to provide a payment method if required.
2. **Sign In**:
   - When prompted, sign in using the temporary username and password provided in the lab details panel.
   - You may need to click *Use Another Account* if you see the *Choose an account* dialog.

3. **Activate Cloud Shell**:
   - After signing in, click *Activate Cloud Shell* at the top-right corner of the Google Cloud Console to open a Cloud Shell session.

4. **Confirm Project**:
   - Verify your active project using the following command:
     ```bash
     gcloud config list project
     ```
   - Make sure the correct project is set.

---

### **Task 1: Setting Up Pub/Sub**

1. **Create a Topic**:
   - From the Navigation menu, select **Pub/Sub > Topics** under the **Analytics** section.
   - Click *Create Topic*.
   - Name the topic `MyTopic` in the *Topic ID* field.
   - Leave the rest of the settings at their default values and click *Create*.
   
2. **Verify Task**:
   - Click *Check my progress* to confirm the topic was created.

---

### **Task 2: Add a Subscription**

1. **Create a Subscription**:
   - In the **Topics** page, find the topic you just created (`MyTopic`).
   - Click the three vertical dots beside the topic name and select *Create Subscription*.
   - Enter a subscription name like `MySub` and set the *Delivery Type* to **Pull**.
   - Leave all other settings as default and click *Create*.

2. **Verify Task**:
   - Click *Check my progress* to confirm the subscription was created successfully.

---

### **Task 3: Test Your Understanding**

**Multiple Choice Questions**:

1. A publisher application creates and sends messages to a _____. Subscriber applications create a ____ to a topic to receive messages from it.
   - A) subscription, subscription
   - B) topic, topic
   - C) subscription, topic
   - D) topic, subscription

   **Answer**: D) topic, subscription

2. Cloud Pub/Sub is an asynchronous messaging service designed to be highly reliable and scalable.
   - A) True
   - B) False

   **Answer**: A) True

---

### **Task 4: Publish a Message to the Topic**

1. **Publish a Message**:
   - Navigate to **Pub/Sub > Topics** and click on `MyTopic`.
   - In the *Messages* tab, click *Publish Message*.
   - Type `Hello World` in the *Message* field and click *Publish*.

2. **Verify Task**:
   - Click *Check my progress* to confirm that the message was successfully published.

---

### **Task 5: View the Message**

1. **Pull the Message**:
   - In Cloud Shell, enter the following command to pull the message from your subscription `MySub`:
     ```bash
     gcloud pubsub subscriptions pull --auto-ack MySub
     ```
   - The output will show the `Hello World` message in the *DATA* field.

2. **Verify Task**:
   - Click *Check my progress* to confirm that the message was successfully pulled from the topic.

---

## 3. Cloud Run Functions: Qwik Start - Console

In this lab, you will learn how to create, deploy, and test a Cloud Run function using Google Cloud Console. Cloud Run functions are event-driven, meaning they are triggered by events such as HTTP requests, messages from messaging services, or file uploads.

---

## Overview

### What is Cloud Run?

Cloud Run is a fully managed compute platform for deploying and managing containerized applications. Cloud Run functions are serverless, meaning they automatically scale depending on the events that trigger them. These functions are ideal for tasks that need to be run in response to events like HTTP requests, file uploads, or changes in databases, without needing to run all the time.

---

### What You'll Do

1. **Create a Cloud Run Function**: Set up a simple function.
2. **Deploy and Test the Function**: Deploy the function and test it.
3. **View Logs**: Learn how to view function execution logs.

---

## **Setup and Requirements**

Before starting the lab:

1. **Web Browser**: Use a standard internet browser (Chrome recommended).
2. **Incognito Window**: Open the lab in an Incognito window to avoid conflicts between your personal account and the student account.
3. **Time**: This lab takes about **20 minutes** to complete, and the timer cannot be paused once started.
4. **Google Cloud Account**: Use the temporary credentials provided for the lab to avoid incurring charges to your personal Google Cloud account.

---

## **Step-by-Step Instructions**

### **How to Start the Lab and Sign In**

1. **Start the Lab**: Click the *Start Lab* button. A pop-up window may appear asking for payment information; proceed as needed.
2. **Sign In**:
   - When prompted, use the **temporary username** and **password** provided in the lab details panel.
   - Select *Use Another Account* if the *Choose an account* dialog appears.

3. **Activate Cloud Shell**:
   - After logging in, click *Activate Cloud Shell* in the top-right corner of the Google Cloud Console to open the Cloud Shell.
   
4. **Confirm Project**:
   - Verify the active project using the following command:
     ```bash
     gcloud config list project
     ```
   - Ensure that the project is set correctly.

---

### **Task 1: Create a Function**

1. **Navigate to Cloud Run Functions**:
   - From the Navigation menu, go to **Serverless > Cloud Run Functions** under **View All Products**.
   - Click on *Create function*.

2. **Fill in the Function Details**:
   - **Function name**: `GCFunction`
   - **Region**: Select your preferred region (or the default region).
   - **Trigger type**: Choose **HTTPS**.
   - **Authentication**: Set to **Allow unauthenticated invocations**.
   - **Memory allocated**: Leave it at the default value.
   - **Autoscaling**: Set the **Maximum number of instances** to **5**.
   
3. **Enable APIs** (if prompted):
   - A pop-up may appear asking you to enable required APIs. Click **ENABLE** when prompted.

4. **Click Next**:
   - Click *Next* to proceed to the source code step.

---

### **Task 2: Deploy the Function**

1. **Select Source Code**:
   - In the *Source code* section, use the default **helloWorld** function in the *Inline Editor* for `index.js`.

2. **Deploy the Function**:
   - Scroll down and click *Deploy* to deploy your Cloud Run function.

3. **Wait for Deployment**:
   - After clicking *Deploy*, the console will redirect you to the **Cloud Run Functions Overview** page.
   - Wait for the function to be deployed. The icon next to the function will show a green check mark once deployment is complete.

4. **Verify Task**:
   - Click *Check my progress* to confirm that the function was successfully deployed.

---

### **Task 3: Test the Function**

1. **Test the Function**:
   - From the **Cloud Run Functions Overview** page, click on your function (`GCFunction`).
   - On the function details page, click *TESTING*.
   
2. **Enter Test Data**:
   - In the *Triggering event* field, enter the following test data:
     ```json
     {"message":"Hello World!"}
     ```
   - Click *Test the function*.

3. **Check the Output**:
   - In the *Output* field, you should see:
     ```text
     Success: Hello World!
     ```
   - In the *Logs* field, look for a status code **200**, which indicates that the function was successful (it may take a minute for the logs to appear).

4. **Verify Task**:
   - Click *Check my progress* to verify that the function was tested successfully.

---

### **Task 4: View Logs**

1. **View Logs**:
   - From the **Cloud Run Functions Overview** page, find your function (`GCFunction`) and click the blue arrow to navigate back to the overview page.
   - In the function menu, click *View logs*.

2. **Review Logs**:
   - In the **Query results** section, you should see the logs generated by the function. This confirms that your function was triggered and executed successfully.

3. **Verify Task**:
   - Click *Check my progress* to confirm that you can view the logs.

---

### **Task 5: Test Your Understanding**

Answer the following multiple-choice questions to reinforce your understanding of Cloud Run functions.

1. **Cloud Run functions is a serverless execution environment for event-driven services on Google Cloud.**
   - A) True
   - B) False

   **Answer**: A) True

2. **Which type of trigger is used while creating Cloud Run functions in the lab?**
   - A) HTTP
   - B) Firebase
   - C) Pub/Sub
   - D) Cloud Storage

   **Answer**: A) HTTP

---

## 4. Getting Started with API Gateway: Challenge Lab

# Getting Started with API Gateway: Challenge Lab

In this challenge lab, you will be tasked with deploying and managing backend services through API Gateway, creating and deploying a Cloud Function, and integrating the function with Pub/Sub for event-driven messaging. Unlike other labs, this challenge lab requires you to apply your previously learned skills, solve problems, and complete tasks on your own to meet the scenario's objectives.

---

## **Overview**

You have recently started as a junior data analyst and are tasked with helping a newly formed development team expose backend services as APIs using **API Gateway**. The objectives for this lab are:

1. Develop a backend system using **Cloud Function**.
2. Deploy and manage an **API Gateway** that exposes the backend service.
3. Subscribe to messages published on a **Pub/Sub topic** to react to events.

---

## **Setup**

Before starting the lab:

- **Browser**: Use a standard browser (preferably Chrome).
- **Incognito Window**: Run the lab in an Incognito window to avoid conflicts with your personal Google account.
- **Lab Timer**: The lab has a time limit of **45 minutes**, and you cannot pause it. Plan your time accordingly.
- **Google Cloud Account**: Use the **temporary credentials** provided for the lab. Do not use your personal Google Cloud account to avoid incurring charges.
- **Region Setup**: During the lab, the region and some other details will be pre-filled, which you'll need to use in various steps.

---

## **Challenge Scenario**

Your role is to help a new development team with exposing backend services via API Gateway. The tasks will guide you through deploying a Cloud Function, configuring an API Gateway to access it, and finally integrating a Pub/Sub topic to publish messages from the Cloud Function.

---

## **Tasks Overview**

### **Task 1: Create a Cloud Function**

1. **Create a 2nd Gen Cloud Function** named `GCFunction` in the specified region.
2. **Deploy the Cloud Function** with a basic "Hello World!" response.
3. **Allow unauthenticated access** to the function so it can be invoked without authentication.

**Command to allow unauthenticated invocations** (after deployment):
```bash
gcloud functions add-invoker-policy-binding GCFunction \
    --region="your-region" \
    --member="allUsers"
```

After completing this task, click **Check my progress** to confirm your success.

---

### **Task 2: Create an API Gateway**

1. **Deploy an API Gateway** to proxy requests to your Cloud Function created in Task 1.
2. **Use the OpenAPI specification** (`openapispec.yaml`) to configure the gateway and link it to your Cloud Function.
3. **Set the API Gateway properties**:
   - **API Name**: `gcfunction-api`
   - **Service Account**: Compute Engine default service account
   - **Config Name**: `gcfunction-api`
   - **Swagger File** (the OpenAPI spec) should reference the Cloud Function URL for the backend.
   
**Swagger YAML Example**:
```yaml
swagger: '2.0'
info:
  title: GCFunction API
  description: Sample API on API Gateway with a Google Cloud Functions backend
  version: 1.0.0
schemes:
  - https
produces:
  - application/json
paths:
  /GCFunction:
    get:
      summary: gcfunction
      operationId: gcfunction
      x-google-backend:
        address: https://REGION-PROJECT_ID.cloudfunctions.net/GCFunction
      responses:
       '200':
          description: A successful response
          schema:
            type: string
```

4. **Check deployment status**: Deployment might take **10 minutes** to complete. You can track the status via the notification icon in the Google Cloud Console.

Once the API Gateway is deployed, click **Check my progress** to confirm that you've completed this task.

---

### **Task 3: Create a Pub/Sub Topic and Publish Messages via API Backend**

1. **Create a Pub/Sub topic** named `demo-topic`.
2. **Update the Cloud Function** to push messages to this topic. Use the following code in `index.js`:
   - **Install dependencies** (`@google-cloud/functions-framework` and `@google-cloud/pubsub`):
     ```json
     {
       "dependencies": {
         "@google-cloud/functions-framework": "^3.0.0",
         "@google-cloud/pubsub": "^3.4.1"
       }
     }
     ```
   - **Update `index.js`** to publish messages to `demo-topic`:
     ```javascript
     const { PubSub } = require('@google-cloud/pubsub');
     const pubsub = new PubSub();
     const topic = pubsub.topic('demo-topic');
     const functions = require('@google-cloud/functions-framework');

     exports.helloHttp = functions.http('helloHttp', (req, res) => {
       // Send a message to the topic
       topic.publishMessage({ data: Buffer.from('Hello from Cloud Functions!') });
       res.status(200).send("Message sent to Topic demo-topic!");
     });
     ```

3. **Redeploy the Cloud Function** after modifying `index.js` and `package.json`.
4. **Invoke the API Gateway**: After deployment, invoke the API Gateway endpoint to test if a message is successfully sent to the `demo-topic`.