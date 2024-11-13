# Cloud Computing - Lab 7 Solutions


## Part 1 - Google Cloud IAM Custom Roles 

### Overview

In this lab, you'll learn how to create, update, delete, and restore custom roles in Google Cloud Identity and Access Management (IAM). Custom roles allow you to tailor permissions based on specific job functions and requirements, ensuring users only have the permissions they need to perform their tasks.

### Prerequisites

- Familiarity with IAM roles in Google Cloud.
- Basic understanding of Google Cloud's resource hierarchy.
- Access to Google Cloud Platform via Cloud Console or Cloud Shell.

### Setup Instructions

1. **Access the Lab Environment**: 
   - Use the provided credentials to log into the Google Cloud Console (details provided by the lab environment).
   - Once logged in, activate the Cloud Shell to run commands.

2. **Set the Compute Region**:
   ```bash
   gcloud config set compute/region us-east4
   ```

---

### Tasks

#### Task 1: View Available Permissions for a Resource

Before creating a custom role, it's important to know what permissions are available for your resource.

1. Run the following command to list all testable permissions for your project:
   ```bash
   gcloud iam list-testable-permissions //cloudresourcemanager.googleapis.com/projects/$DEVSHELL_PROJECT_ID
   ```

   This will show a list of permissions such as `appengine.applications.create`, `appengine.applications.get`, etc., that can be applied to the project.

---

#### Task 2: Get the Role Metadata

You may want to understand the metadata for both predefined and custom roles before creating custom roles. This metadata includes role ID, permissions, and description.

1. Run the following command to get role metadata:
   ```bash
   gcloud iam roles describe roles/viewer
   ```

   Example output:
   ```plaintext
   description: Read access to all custom roles in the project.
   includedPermissions:
   - iam.roles.get
   - iam.roles.list
   - resourcemanager.projects.get
   - resourcemanager.projects.getIamPolicy
   name: roles/iam.roleViewer
   stage: GA
   title: Viewer
   ```

---

#### Task 3: View the Grantable Roles on Resources

Next, you can view the roles that can be granted on a specific resource.

1. Run the following command to list all grantable roles for your project:
   ```bash
   gcloud iam list-grantable-roles //cloudresourcemanager.googleapis.com/projects/$DEVSHELL_PROJECT_ID
   ```

   This will return roles such as `roles/appengine.appAdmin` and `roles/appengine.appViewer`.

---

#### Task 4: Create a Custom Role

You can create custom roles in Google Cloud using the `gcloud` command. A custom role can be created using a YAML file or flags.

##### Option 1: Create a Custom Role Using a YAML File

1. Create a new YAML file (`role-definition.yaml`) with the role definition:
   ```yaml
   title: "Role Editor"
   description: "Edit access for App Versions"
   stage: "ALPHA"
   includedPermissions:
   - appengine.versions.create
   - appengine.versions.delete
   ```

2. Create the role with the following command:
   ```bash
   gcloud iam roles create editor --project $DEVSHELL_PROJECT_ID --file role-definition.yaml
   ```

   If successful, the output will confirm that the role was created:
   ```plaintext
   Created role [editor].
   description: Edit access for App Versions
   includedPermissions:
   - appengine.versions.create
   - appengine.versions.delete
   ```

##### Option 2: Create a Custom Role Using Flags

Alternatively, you can specify the role definition using command-line flags:
```bash
gcloud iam roles create viewer --project $DEVSHELL_PROJECT_ID \
--title "Role Viewer" --description "Custom role description." \
--permissions compute.instances.get,compute.instances.list --stage ALPHA
```

If successful, the output will confirm that the role was created:
```plaintext
Created role [viewer].
description: Custom role description.
includedPermissions:
- compute.instances.get
- compute.instances.list
```

---

#### Task 5: List Custom Roles

You can list all custom roles in your project using the following command:
```bash
gcloud iam roles list --project $DEVSHELL_PROJECT_ID
```

This will show a list of all roles, including custom ones you have created:
```plaintext
- name: projects/[PROJECT_ID]/roles/editor
  title: Role Editor
  description: Edit access for App Versions
- name: projects/[PROJECT_ID]/roles/viewer
  title: Role Viewer
  description: Custom role description.
```

---

#### Task 6: Update an Existing Custom Role

You can update a custom role by modifying its permissions. Use the `gcloud iam roles update` command.

##### Option 1: Update a Custom Role Using a YAML File

1. First, get the current definition of the role:
   ```bash
   gcloud iam roles describe editor --project $DEVSHELL_PROJECT_ID
   ```

2. Create a new YAML file (`new-role-definition.yaml`) with additional permissions:
   ```yaml
   description: Edit access for App Versions
   etag: BwVxIAbRq_I=
   includedPermissions:
   - appengine.versions.create
   - appengine.versions.delete
   - storage.buckets.get
   - storage.buckets.list
   name: projects/[PROJECT_ID]/roles/editor
   stage: ALPHA
   title: Role Editor
   ```

3. Update the custom role:
   ```bash
   gcloud iam roles update editor --project $DEVSHELL_PROJECT_ID --file new-role-definition.yaml
   ```

##### Option 2: Update a Custom Role Using Flags

To add permissions directly via flags:
```bash
gcloud iam roles update viewer --project $DEVSHELL_PROJECT_ID \
--add-permissions storage.buckets.get,storage.buckets.list
```

If successful, the output will confirm the permissions were added.

---

#### Task 7: Disable a Custom Role

To disable a custom role, set the `--stage` flag to `DISABLED`:
```bash
gcloud iam roles update viewer --project $DEVSHELL_PROJECT_ID --stage DISABLED
```

This will inactivate the role, meaning it will no longer grant permissions when assigned to users.

---

#### Task 8: Delete a Custom Role

To permanently delete a custom role, use the following command:
```bash
gcloud iam roles delete viewer --project $DEVSHELL_PROJECT_ID
```

Once deleted, the role cannot be used for new IAM policy bindings. Existing bindings will remain but inactive.

---

#### Task 9: Restore a Custom Role

You can restore a custom role within 7 days of deletion by setting the `--stage` to `ALPHA` or `BETA`:
```bash
gcloud iam roles undelete viewer --project $DEVSHELL_PROJECT_ID
```

This will make the role available again for use.

---

### Conclusion

In this lab, you've learned how to:

- View available permissions and role metadata.
- Create, update, disable, delete, and restore custom roles.
- Apply these skills to manage permissions in a Google Cloud project effectively.

By using custom roles, you can ensure the principle of least privilege and provide your users only with the permissions they need to perform their tasks.