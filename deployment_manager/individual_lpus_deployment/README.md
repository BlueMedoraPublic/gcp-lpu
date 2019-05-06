# GCP LPU Role Deployments
Using the Google Cloud Deployment Manager, an LPU role can be built for a specific project, or the whole organization.

## Prerequisites
* Obtain and authenticate the Google Cloud SDK Tools

[//]: # (Break the List)
**The account used to authenticate must have permission to use the Google Cloud Deployment Manager**

## Usage
### Deploying Project Level LPUs
* **Verify that the correct project is selected during the authentication process**
\
[**Reference this documentation to learn how to switch projects**](https://cloud.google.com/sdk/gcloud/reference/init)

[//]: # (Break the List)
1. Navigate to the `project_level` directory
2. Run the following command, replace `<Deployment Name Here>` with an appropriate name.
```
gcloud deployment-manager deployments create <Deployment Name Here> \
    --config   <Required YAML configuration file>
```
**NOTE:** If the deployed role is deleted, it takes 37 total days for it to be fully removed. Role IDs cannot be identical and trying to re-deploy after deleting the role will fail. To avoid this, edit the configuration file and change the Role ID within the file. 

### Deploying Organization Level LPUs
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
Following that, replace `<Deployment Name Here>` with any name desired for the deployment. Then replace
`<Required Jinja Template File>` with the appropriate file name.
```
#!/bin/bash
ORG_ID=<YOUR ORGANIZATION ID>

gcloud deployment-manager deployments create <Deployment Name Here> \
    --template   <Required Jinja Template File> \
    --properties organizationId:$ORG_ID
```

## Folder Structure
#### organization_level folder
This folder contains multiple Jinja template files that are necessary for each LPU being deployed at the organization level.

#### project_level folder
This folder contains multiple YAML configuration files that are needed for each LPU being deployed at the project level.
