# Cloud Computing - Lab 9 Solutions 

## Part 2 - Service Accounts and Roles: Fundamentals

### Overview

In this lab, you will set up and use a service account to securely query a public BigQuery dataset from a Compute Engine virtual machine. This approach demonstrates how to manage secure access to resources in Google Cloud using Identity and Access Management (IAM) roles and service accounts.

---

### Task 1: Create a Service Account and Assign Roles

#### Steps

1. Open the **Google Cloud Console** and navigate to **IAM & Admin > Service Accounts**.
2. Click **+ CREATE SERVICE ACCOUNT**.
3. Fill in the details:
   - **Service Account Name**: `bigquery-qwiklab`
4. Click **Create and Continue**.
5. Assign the following roles to the service account:
   - **BigQuery Data Viewer**: Grants the ability to view datasets and tables in BigQuery.
   - **BigQuery User**: Allows running queries and accessing public datasets.
6. Click **Done** to complete the creation of the service account.

#### Solution
This creates a service account with the necessary permissions to access and query BigQuery datasets securely.

---

### Task 2: Set Up a Compute Engine Instance with the Service Account

#### Steps

1. In the **Google Cloud Console**, go to **Compute Engine > VM Instances**.
2. Click **Create Instance** and configure the following:
   - **Name**: `bigquery-instance`
   - **Region**: `us-west1`
   - **Zone**: `us-west1-a`
   - **Series**: `E2`
   - **Machine Type**: `e2-medium`
   - **Boot Disk**: Debian GNU/Linux 11 (bullseye)
   - **Service Account**: Select `bigquery-qwiklab`.
   - **Access Scopes**: Set to **Allow full access to all Cloud APIs**.
3. Click **Create** to finalize the instance.

#### Solution
The VM is now configured to securely authenticate using the `bigquery-qwiklab` service account. This setup avoids embedding sensitive credentials into your code.

---

### Task 3: Install Dependencies and Query BigQuery

#### Steps

1. SSH into the VM:
   - In the **Google Cloud Console**, navigate to **Compute Engine > VM Instances**.
   - Click **SSH** for the `bigquery-instance`.
2. Update the system and install the required tools:
   ```bash
   sudo apt-get update
   sudo apt-get install -y git python3-pip
   pip3 install --upgrade pip
   pip3 install google-cloud-bigquery pyarrow pandas db-dtypes
   ```
3. Create a Python script named `query.py`:
   ```bash
   nano query.py
   ```
   Paste the following code into the script:
   ```python
   from google.auth import compute_engine
   from google.cloud import bigquery

   credentials = compute_engine.Credentials()

   query = """
   SELECT
       year,
       COUNT(1) AS num_babies
   FROM
       `bigquery-public-data.samples.natality`
   WHERE
       year > 2000
   GROUP BY
       year
   ORDER BY
       year DESC
   """

   client = bigquery.Client(credentials=credentials)

   print(client.query(query).to_dataframe())
   ```
4. Save and close the file.
5. Run the script:
   ```bash
   python3 query.py
   ```

#### Solution
The script executes a query against the BigQuery `natality` dataset, returning a summary of the number of babies born each year after 2000.

**Expected Output**:
The output should look similar to:
```
   year   num_babies
0  2008   4255156
1  2006   4273225
...
```