# GCP Master LPU Role Deployments
Using the Google Cloud Deployment Manager, an LPU role can be built for a specific project, or the whole organization.

## Prerequisites
* Obtain and authenticate the Google Cloud SDK Tools

[//]: # (Break the List)
**The account used to authenticate must have permission to use the Google Cloud Deployment Manager**

## Usage
### Deploying the Master LPU Role to a Project
* **Verify that the correct project is selected during the authentication process**
\
[**Reference this documentation to learn how to switch projects**](https://cloud.google.com/sdk/gcloud/reference/init)

[//]: # (Break the List)
Run the following command, replace `<Deployment Name Here>` with an appropriate name.
```
gcloud deployment-manager deployments create <Deployment Name Here> \
    --template   bm_project_gcp_masterlpu_role.jinja
```

### Deploying the Master LPU role for an Organization
#### Prerequisites for Deploying an Organizational Role
The Google Cloud Deployment manager uses the **Google APIs Service Account** to authenticate against other APIs.
This service account `[PROJECT_NUMBER]@cloudservices.gserviceaccount.com` must be given `Organizational Role Administrator` privileges.
\
\
For this to work, two things are required.
1. At the organization level, a **Google APIs Service Account** must be added as a member.
   - This service account will come from a project.
2. After being added as a member at the organization level, grant the service account `Organizational Role Administrator`
privileges.

[//]: # (Break the List)

Write a script that models the script below, replace `<YOUR ORGANIZATION ID>` with your Organization ID.
It is recommended that you store your Organization ID in a secure location, then
retrieve the Organzation ID and assign it to the ORG_ID variable.
```
#!/bin/bash
ORG_ID=<YOUR ORGANIZATION ID>

gcloud deployment-manager deployments create <Deployment Name Here> \
    --template   bm_organization_gcp_masterlpu_role.jinja \
    --properties organizationId:$ORG_ID
```

## Jinja Template Files
#### bm_organization_gcp_masterlpu_role.jinja
Defines permissions for a master LPU to be used across all projects within an Organization

#### bm_project_gcp_masterlpu_role.jinja
Defines permissions for a master LPU to be used within a specified project
