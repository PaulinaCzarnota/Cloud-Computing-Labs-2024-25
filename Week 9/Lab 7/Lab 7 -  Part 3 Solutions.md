# Cloud Computing - Lab 7 Solutions


## Part 3 - Pub/Sub: Qwik Start - Command Line

### Overview

In this lab, you'll learn the basics of **Google Cloud Pub/Sub**—a messaging service used for exchanging event data between applications and services. By decoupling senders and receivers, Pub/Sub ensures secure, reliable, and highly available communication. Pub/Sub is widely used for implementing asynchronous workflows, distributing event notifications, and streaming data across different processes or devices.

#### What You'll Learn

- How to create, list, and delete Pub/Sub topics and subscriptions
- How to publish messages to a topic
- How to use a pull subscriber to receive messages

---

### Prerequisites

- Basic understanding of Google Cloud Console and Cloud Shell
- Familiarity with the terminal and command-line tools

---

### Setup and Requirements

Before starting the lab, ensure you have:

- Access to a web browser (Google Chrome is recommended).
- A private/incognito browser window to avoid conflicts with personal Google Cloud accounts.
- Time to complete the lab (you cannot pause once it starts).

Once you start the lab, you will be provided temporary credentials to access the Google Cloud Console for the lab duration.

---

### Task 1: Pub/Sub Topics

In this task, you'll create, list, and delete Pub/Sub topics using Google Cloud commands.

#### Step 1: Create a Topic

1. Open **Cloud Shell** in the Google Cloud Console (click the **Activate Cloud Shell** icon).
2. To create a new topic, run the following command:
   ```bash
   gcloud pubsub topics create myTopic
   ```
   This creates a topic called `myTopic`.

3. To verify the topic creation, list all topics:
   ```bash
   gcloud pubsub topics list
   ```
   Your output should show:
   ```plaintext
   name: projects/PROJECT_ID/topics/myTopic
   ```

#### Step 2: Create Additional Topics

1. Create two more topics called `Test1` and `Test2` by running:
   ```bash
   gcloud pubsub topics create Test1
   gcloud pubsub topics create Test2
   ```

2. List all topics again:
   ```bash
   gcloud pubsub topics list
   ```
   Your output should now display all three topics:
   ```plaintext
   name: projects/PROJECT_ID/topics/myTopic
   name: projects/PROJECT_ID/topics/Test1
   name: projects/PROJECT_ID/topics/Test2
   ```

#### Step 3: Clean Up

1. Delete the `Test1` and `Test2` topics:
   ```bash
   gcloud pubsub topics delete Test1
   gcloud pubsub topics delete Test2
   ```

2. Verify the topics were deleted:
   ```bash
   gcloud pubsub topics list
   ```
   The output should now only show `myTopic`:
   ```plaintext
   name: projects/PROJECT_ID/topics/myTopic
   ```

---

### Task 2: Pub/Sub Subscriptions

In this task, you'll create, list, and delete subscriptions to your topics.

#### Step 1: Create a Subscription

1. Create a subscription called `mySubscription` to the `myTopic` topic:
   ```bash
   gcloud pubsub subscriptions create --topic myTopic mySubscription
   ```

2. Verify the subscription was created:
   ```bash
   gcloud pubsub topics list-subscriptions myTopic
   ```
   Your output should show:
   ```plaintext
   projects/PROJECT_ID/subscriptions/mySubscription
   ```

#### Step 2: Create Additional Subscriptions

1. Create two more subscriptions called `Test1` and `Test2` to the `myTopic` topic:
   ```bash
   gcloud pubsub subscriptions create --topic myTopic Test1
   gcloud pubsub subscriptions create --topic myTopic Test2
   ```

2. List all subscriptions for `myTopic`:
   ```bash
   gcloud pubsub topics list-subscriptions myTopic
   ```
   Your output should display:
   ```plaintext
   projects/PROJECT_ID/subscriptions/mySubscription
   projects/PROJECT_ID/subscriptions/Test1
   projects/PROJECT_ID/subscriptions/Test2
   ```

#### Step 3: Delete Subscriptions

1. Delete the `Test1` and `Test2` subscriptions:
   ```bash
   gcloud pubsub subscriptions delete Test1
   gcloud pubsub subscriptions delete Test2
   ```

2. Verify the subscriptions were deleted:
   ```bash
   gcloud pubsub topics list-subscriptions myTopic
   ```
   Your output should show:
   ```plaintext
   projects/PROJECT_ID/subscriptions/mySubscription
   ```

---

### Task 3: Pub/Sub Publishing and Pulling a Single Message

Now, you will publish messages to a topic and pull a single message using a subscription.

#### Step 1: Publish a Message

1. Publish the message `"Hello"` to the `myTopic` topic:
   ```bash
   gcloud pubsub topics publish myTopic --message "Hello"
   ```

2. Publish a few more messages with custom content:
   ```bash
   gcloud pubsub topics publish myTopic --message "Publisher's name is <YOUR NAME>"
   gcloud pubsub topics publish myTopic --message "Publisher likes to eat <FOOD>"
   gcloud pubsub topics publish myTopic --message "Publisher thinks Pub/Sub is awesome"
   ```

#### Step 2: Pull a Single Message

1. Pull the messages from the `mySubscription` subscription:
   ```bash
   gcloud pubsub subscriptions pull mySubscription --auto-ack
   ```

2. Your output will show a message similar to this:
   ```plaintext
   Data: Publisher likes to eat <FOOD>
   Message_ID: [ID]
   Attributes: {}
   ```

#### Key Point:

- The pull command by default will only pull **one message** at a time, even if multiple messages are published to the topic.
- Once a message is pulled and acknowledged, it cannot be retrieved again.
  
3. Run the `pull` command several more times to pull additional messages:
   ```bash
   gcloud pubsub subscriptions pull mySubscription --auto-ack
   gcloud pubsub subscriptions pull mySubscription --auto-ack
   gcloud pubsub subscriptions pull mySubscription --auto-ack
   ```
   After all messages are pulled, running the command again will return:
   ```plaintext
   Listed 0 items.
   ```

---

### Task 4: Pub/Sub Pulling All Messages from Subscriptions

In this task, you'll pull all messages from a subscription using a flag to retrieve multiple messages at once.

#### Step 1: Add More Messages

1. Publish additional messages to `myTopic`:
   ```bash
   gcloud pubsub topics publish myTopic --message "Publisher is starting to get the hang of Pub/Sub"
   gcloud pubsub topics publish myTopic --message "Publisher wonders if all messages will be pulled"
   gcloud pubsub topics publish myTopic --message "Publisher will have to test to find out"
   ```

#### Step 2: Pull All Messages

1. To pull all three messages at once, use the `--limit` flag to specify how many messages to pull:
   ```bash
   gcloud pubsub subscriptions pull mySubscription --auto-ack --limit=3
   ```

2. Your output will look like this, displaying all three messages in one request:
   ```plaintext
   Data: Publisher is starting to get the hang of Pub/Sub
   Data: Publisher wonders if all messages will be pulled
   Data: Publisher will have to test to find out
   ```

---

### Answers to Test Your Understanding

**1. To receive messages published to a topic, you must create a subscription to that topic.**

- **Answer**: True  
Explanation: To receive messages from a topic, you must create a subscription. The subscription acts as a consumer of the messages published to the topic.