# Cloud Computing - Lab 7 Solutions


## Part 2 - Migrating Customer Data to Firestore 

### Overview
In this lab, we will guide you through the process of migrating Pet Theory's customer data from their legacy database to Google Cloud's Firestore. We'll walk through setting up Firestore, importing data, creating pseudo-random test data, and verifying the migration in the Firestore database.

The tasks will be broken into five main objectives:
1. Set up Firestore in Google Cloud.
2. Write database import code.
3. Create test customer data.
4. Import the test customer data into Firestore.
5. Inspect the data in Firestore.

Each task will be accompanied by steps and code to help you complete the process.

---

### Task 1: Set up Firestore in Google Cloud

#### Objective:
Create a Firestore database to store Pet Theory's customer data.

#### Steps:
1. In the **Google Cloud Console**, navigate to **Firestore** via the **Navigation Menu**.
2. Click on **+Create Database**.
3. Choose **Native mode** (optimized for real-time updates and client apps).
4. In the **Region** dropdown, select **us-east1**.
5. Click **Create Database**.

This will create a Firestore database that is ready to accept data.

---

### Task 2: Write Database Import Code

#### Objective:
Write a script to read data from a CSV file and write it to Firestore.

#### Steps:
1. Clone the Pet Theory repository to your Cloud Shell environment:
    ```bash
    git clone https://github.com/rosera/pet-theory
    cd pet-theory/lab01
    ```
2. Install the required dependencies:
    ```bash
    npm install @google-cloud/firestore @google-cloud/logging csv-parse
    ```
3. Open the `importTestData.js` file and modify it to write to Firestore.

##### Key Changes:
- Import the **Firestore** and **Logging** modules.
- Update the `writeToDatabase` function to **write data to Firestore** using **batch operations**.

```javascript
const { Firestore } = require("@google-cloud/firestore");
const { Logging } = require('@google-cloud/logging');

// Firestore batch write function
async function writeToFirestore(records) {
  const db = new Firestore();
  const batch = db.batch();
  records.forEach((record) => {
    const docRef = db.collection("customers").doc(record.email);
    batch.set(docRef, record, { merge: true });
  });
  await batch.commit();
  console.log("Batch executed");
}

// Import CSV function
async function importCsv(csvFilename) {
  const parser = csv.parse({ columns: true, delimiter: ',' }, async function (err, records) {
    if (err) {
      console.error('Error parsing CSV:', err);
      return;
    }
    try {
      await writeToFirestore(records);
      console.log(`Wrote ${records.length} records to Firestore`);
    } catch (e) {
      console.error(e);
      process.exit(1);
    }
  });
  await fs.createReadStream(csvFilename).pipe(parser);
}
```

4. Run the script to import customer data into Firestore:
    ```bash
    node importTestData customers_1000.csv
    ```

---

### Task 3: Create Test Data

#### Objective:
Generate pseudo-random customer data to test the import script.

#### Steps:
1. Install the `faker` library:
    ```bash
    npm install faker@5.5.3
    ```
2. Open the `createTestData.js` file and modify it to generate test customer data. The script will use `faker` to generate random names, emails, and phone numbers.

```javascript
const fs = require('fs');
const faker = require('faker');
const { Logging } = require('@google-cloud/logging');

// Initialize Logging
const logName = "pet-theory-logs-createTestData";
const logging = new Logging();
const log = logging.log(logName);
const resource = { type: "global" };

// Generate random customer data
async function createTestData(recordCount) {
  const fileName = `customers_${recordCount}.csv`;
  var f = fs.createWriteStream(fileName);
  f.write('id,name,email,phone\n')
  for (let i = 0; i < recordCount; i++) {
    const id = faker.datatype.number();
    const firstName = faker.name.firstName();
    const lastName = faker.name.lastName();
    const name = `${firstName} ${lastName}`;
    const email = faker.internet.email(firstName, lastName);
    const phone = faker.phone.phoneNumber();
    f.write(`${id},${name},${email},${phone}\n`);
  }
  console.log(`Created file ${fileName} containing ${recordCount} records.`);
  
  // Log the success
  const success_message = `Success: createTestData - Created file ${fileName} containing ${recordCount} records.`;
  const entry = log.entry({ resource: resource }, { message: success_message });
  log.write([entry]);
}

// Generate 1000 test records
const recordCount = parseInt(process.argv[2]);
if (recordCount < 1 || isNaN(recordCount)) {
  console.error('Include the number of test data records to create. Example: node createTestData.js 100');
  process.exit(1);
}
createTestData(recordCount);
```

3. Run the script to generate `customers_1000.csv` with 1000 records:
    ```bash
    node createTestData 1000
    ```

---

### Task 4: Import the Test Customer Data

#### Objective:
Test the import functionality by using the generated test data and import it into Firestore.

#### Steps:
1. Ensure that your import script (`importTestData.js`) is working correctly and uses the `customers_1000.csv` file.
2. Run the import command:
    ```bash
    node importTestData customers_1000.csv
    ```

3. If you encounter errors like `Error: Cannot find module 'csv-parse'`, install the missing dependency:
    ```bash
    npm install csv-parse
    ```

4. The output should show progress like:
    ```
    Writing record 500
    Writing record 1000
    Wrote 1000 records
    ```

---

### Task 5: Inspect the Data in Firestore

#### Objective:
Verify that the imported data is successfully written to Firestore.

#### Steps:
1. Go to the **Google Cloud Console**.
2. Navigate to **Firestore** via the **Navigation Menu**.
3. In the **Firestore** console, click on the pencil icon to enter the data collection path `/customers`.
4. Refresh the page and you should see the list of customer records that were imported from the CSV file.

---

### Resources:
- [Firestore Documentation](https://cloud.google.com/firestore/docs)
- [Node.js Firestore Client](https://googleapis.dev/nodejs/firestore/latest/)