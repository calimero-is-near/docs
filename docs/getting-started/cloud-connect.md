---
title: Set up Cloud Connect for your Cluster
sidebar_position: 4
---

:::info
This step is essential if you plan to self-host the shard. If you're using Calimero hosted cluster, you can skip this step and proceed directly to creating your [workspaces](https://docs.calimero.network/getting-started/workspaces).
:::

## What is Cloud Connect?
Cloud Connect is a feature within Calimero that empowers users to manage their infrastructure in a decentralized and secure manner. It allows you to interact with private chains and workspaces, providing the flexibility to use your own AWS/GCP account credentials to create your Kubernetes clusters. By connecting these clusters to your private chain and workspace, you can establish a powerful and customizable environment.

With Cloud Connect, you can:

- Utilize your own GCP account credentials to create Kubernetes (k8s) clusters.
- Seamlessly connect these clusters to your private chain and workspace.
- Maintain complete control over your data.

## Prerequisite

To use Google Cloud Platform (GCP) with Calimero's Cloud Connect:

- Install the gcloud command-line tool. This tool provides the primary command-line interface to Google Cloud Platform. For installation instructions, follow the [official Google documentation](https://cloud.google.com/sdk/docs/install).

- To confirm the installation and setup, run the following command:
  
```bash
gcloud projects list
```
This command should list your GCP projects, ensuring your `gcloud` installation and setup are successful.

## Set up Cloud Connect for your Cluster

Follow these steps to set up Cloud Connect for your cluster:

1. Click on **Connect Cloud** to create your own cluster.

<img width="1512" alt="1-cloud-connect" src="https://github.com/calimero-is-near/docs/assets/39309699/ec431ad1-86e2-4ed1-904c-fcca0520abe4"/>


2. Click on the GCP cloud provider.

<img width="1201" alt="Screenshot 2023-10-30 at 15 27 05" src="https://github.com/calimero-is-near/docs/assets/39309699/3e402ccf-56e6-4a6a-9fd2-a1bcf9884b14"/>

3. After selecting your cloud provider, you'll need to connect your cloud to Calimero by entering your GCP account credentials. To generate the credential file, run the following script:

```bash
#!/usr/bin/env bash

set -exu
SA_NAME=dev-cc-new
GCP_PROJECT=<Your Project Name>
GCP_PROJECT_ID=$(gcloud projects describe $GCP_PROJECT | grep projectNumber | awk -F: '{print $2}' | tr -d "'" | tr -d ' '"'")

KEY_FILE=./gcp-credentials.json
SA_ID=$SA_NAME@$GCP_PROJECT.iam.gserviceaccount.com

gcloud --project $GCP_PROJECT services enable compute.googleapis.com container.googleapis.com iam.googleapis.com  cloudresourcemanager.googleapis.com
gcloud --project $GCP_PROJECT iam service-accounts create $SA_NAME

gcloud --project $GCP_PROJECT iam service-accounts keys create $KEY_FILE \
      --iam-account=$SA_ID

gcloud projects add-iam-policy-binding ${GCP_PROJECT} \
      --member=serviceAccount:${SA_ID} \
          --role=roles/iam.serviceAccountCreator

gcloud --project $GCP_PROJECT iam service-accounts add-iam-policy-binding \
    $GCP_PROJECT_ID-compute@developer.gserviceaccount.com \
      --member=serviceAccount:$SA_ID \
      --role=roles/iam.serviceAccountUser

gcloud projects add-iam-policy-binding ${GCP_PROJECT} \
      --member=serviceAccount:${SA_ID} \
          --role=roles/container.admin

gcloud projects add-iam-policy-binding ${GCP_PROJECT} \
      --member=serviceAccount:${SA_ID} \
          --role=roles/container.clusterAdmin

gcloud projects add-iam-policy-binding ${GCP_PROJECT} \
      --member=serviceAccount:${SA_ID} \
          --role=roles/storage.admin
```

- Add a unique name for your GCP cloud connection with Calimero
- Then, copy and paste the account credentials `gcp-credentials.json` generated by your cloud provider from the directory where you ran the script.

<img width="1512" alt="2-connect-cloud" src="https://github.com/calimero-is-near/docs/assets/39309699/24ea1a36-66a0-442e-9576-a8ce24bb591f"/>

Once completed, your cloud will be successfully connected.

<img width="1492" alt="Screenshot 2023-08-24 at 10 41 09" src="https://github.com/calimero-is-near/docs/assets/39309699/98f6de26-f12a-4e86-94cb-4985fa11bd14"/>

4. Specify a name for your cluster and choose the region where you want to create it. This step allows you to customize your cluster according to your needs.

<img width="1256" alt="3-cluster-name" src="https://github.com/calimero-is-near/docs/assets/39309699/8ad0c12f-f692-4217-8cbd-063e43f9de5c"/>

5. Once you've configured the cluster settings, proceed to create the cluster. Calimero will initiate the cluster creation process. Your cluster will be created successfully, and you can now create workspaces within it.

<img width="1498" alt="5-cloud connected" src="https://github.com/calimero-is-near/docs/assets/39309699/a2d6affa-80e5-4183-a460-83c06edbcaa1"/>

