# Cloud Computing - Lab 8 Solutions

## Part 1 - Build a Serverless Web App with Firebase

### Overview

This lab is a fully functional serverless web application built with Firebase. It demonstrates the use of Firebase Hosting, Firestore Database, and Firebase Authentication to create a real-time appointment scheduling app for "Pet Theory," a veterinary clinic chain. The app allows users to log in with Google, manage their profiles, and schedule appointments.

---

### Objectives

By completing this project, you will learn how to:
1. Configure Firestore security for automated server-side authentication and authorization.
2. Add Google Sign-In functionality to a web app.
3. Configure the Firestore database for customer information.
4. Deploy a Firebase app and enable real-time updates.

---

### Architecture

The app uses Firebase Hosting for serverless deployment, Firebase Authentication for user management, and Firestore for storing customer data. 

---

### Prerequisites

1. Familiarity with Google Cloud Console and shell environments.
2. Basic understanding of Firebase and Firestore.
3. Text editor for modifying project files.

Recommended Pre-Lab:
- **Importing Data to a Firestore Database**

---

### Step-by-Step Instructions

#### Task 1: Register a Firebase Application
1. Open [Firebase Console](https://console.firebase.google.com) in an incognito window.
2. Log in using:
   - **Username**: `student-01-dd75d7014145@qwiklabs.net`
   - **Password**: `t6AjonbfFM8G`
3. Select the provisioned project: `qwiklabs-gcp-01-eead9cddc34f`.
4. Click the **Web Icon**, name the app `Pet Theory`, and check the option to set up Firebase Hosting.
5. Configure the hosting domain with `student-bucket-qwiklabs-gcp-01-eead9cddc34f-1`.
6. Complete the registration process.

#### Task 2: Enable Firebase Products
##### Firebase Authentication:
1. Navigate to **Build > Authentication** in Firebase Console.
2. Enable Google Sign-In under the **Sign-In Method** tab.
3. Add `student-bucket-qwiklabs-gcp-01-eead9cddc34f-1.web.app` as an authorized domain.

##### Firestore Database:
1. Navigate to **Build > Firestore Database**.
2. Create the database with default settings.
3. Update the security rules:
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /customers/{email} {
         allow read, write: if request.auth.token.email == email;
       }
       match /customers/{email}/{document=**} {
         allow read, write: if request.auth.token.email == email;
       }
     }
   }
   ```

#### Task 3: Install the Firebase CLI
1. Open the provided IDE link: `https://ide-service-essoujd77q-uc.a.run.app`.
2. Clone the project repository:
   ```bash
   git clone https://github.com/rosera/pet-theory.git
   ```
3. Navigate to `pet-theory/lab02` and install dependencies:
   ```bash
   npm i
   ```

#### Task 4: Authorize Firebase Access
1. Log in to Firebase CLI:
   ```bash
   firebase login --no-localhost
   ```
2. Follow the on-screen instructions to authenticate using the provided credentials.

#### Task 5: Initialize Firebase Products
1. Initialize Firebase in the project:
   ```bash
   firebase init
   ```
2. Select the following products:
   - Firestore
   - Hosting
3. Use the project ID: `qwiklabs-gcp-01-eead9cddc34f`.
4. Keep default settings for rules and configuration files.

#### Task 6: Deploying to Firebase
1. Update `firebase.json` with your site ID:
   ```json
   {
     "hosting": {
       "site": "student-bucket-qwiklabs-gcp-01-eead9cddc34f-1",
       ...
     }
   }
   ```
2. Deploy the application:
   ```bash
   firebase deploy --only hosting:student-bucket-qwiklabs-gcp-01-eead9cddc34f-1
   ```

#### Task 7: Add a Customer Page to the Web App
1. Update `public/customer.js` with the following code:
   ```javascript
   let user;
   firebase.auth().onAuthStateChanged(function(newUser) {
     user = newUser;
     if (user) {
       const db = firebase.firestore();
       db.collection("customers").doc(user.email).onSnapshot(function(doc) {
         const cust = doc.data();
         if (cust) {
           document.getElementById('customerName').setAttribute('value', cust.name);
           document.getElementById('customerPhone').setAttribute('value', cust.phone);
         }
         document.getElementById('customerEmail').innerText = user.email;
       });
     }
   });

   document.getElementById('saveProfile').addEventListener('click', function(ev) {
     const db = firebase.firestore();
     var docRef = db.collection('customers').doc(user.email);
     docRef.set({
       name: document.getElementById('customerName').value,
       email: user.email,
       phone: document.getElementById('customerPhone').value,
     })
   })
   ```
2. Update the CSS in `public/styles.css` for better styling.

---

### Testing
1. Visit the hosted URL: `https://student-bucket-qwiklabs-gcp-01-eead9cddc34f-1.web.app`.
2. Log in using the provided credentials.
3. Verify Google Sign-In and profile update functionality.