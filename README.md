# GitOps to Deploy Instana and Grafana

## Goals

In this lab, you will learn:

- How to use Instana to monitor an OpenShift cluster.
- How to monitor a microservices application running on the cluster with Instana.
- How to use Grafana to display charts based on metrics captured by Instana.

You will utilize GitOps to deploy the following components to an OpenShift Cluster:

- Instana Agent
- Robot Shop Application
- Grafana

## Prerequisites

Before starting this lab, ensure you have the following environments set up:

- Instana Server
- OpenShift 4.13 Cluster

Additionally, you need to have the following installed locally:

- Git CLI (or, if you are on a Mac, you can use the command: `xcode-select --install`)
- Helm CLI
- OC CLI version 4.7 or higher

## Bootstrap the cluster
### 1. Log in to your Openshift Cluster from the terminal
a. Navigate to your Openshift cluster's web console created in the Prerequisites

b. Click the user dropdown and click on Copy login command

![image](https://github.com/user-attachments/assets/38dffb86-fe8b-420a-84e6-4b96cba6bad8)

c. Log into your account. Click display token

d. Copy the oc login command. It should look something like this:

```bash
oc login --token=<insert-your-API-token-here>
```

### 2. Clone the [multi-tenancy-gitops](https://github.com/cloud-native-toolkit/multi-tenancy-gitops) repo

a. Create a directory and change into the new directory

```bash
mkdir instana-lab &&
cd instana-lab
```

b. Clone the [multi-tenancy-gitops](https://github.com/cloud-native-toolkit/multi-tenancy-gitops) repo and change into the cloned directory.

> [!TIP]
> You can also clone this repo, as it is a fork of the one mentioned above.

```bash
git clone https://github.com/cloud-native-toolkit/multi-tenancy-gitops.git &&
cd multi-tenancy-gitops
```

### 3. Run the bootstrap script

a. Create an environment variable to tell the script to use Gitea

```bash
export USE_GITEA=true
```

b. Create an environment variable to tell the script the output directory to clone the multi-tenancy-gitops repos into

> [!IMPORTANT]
> The OUTPUT_DIR will be referenced throughout the lab and if you use another value than gitops-instana you will have to update the commands before executing them

```bash
export OUTPUT_DIR=gitops-instana
```

c. Run the bootstrap script. The script installs the Gitea operator into the cluster, clones the multi-tenancy-gitops repos and pushes them to Gitea, then bootstraps Argo CD.

> [!NOTE]
> The script takes around 20 minutes to run so after you start it, it would be a good time for a break :-)

```bash
./scripts/bootstrap.sh
```

d. When the script completes, it displays the URLs and commands to retrieve credentials to log in to Argo CD and Gitea.

Retrieve Gitea credentials
```bash
oc extract secrets/gitea-access --keys=username,password -n tools --to=-
```

e. Copy ArgoCD/GitOps UI URL returned from the script and open it in a browser. Click the LOG IN VIA OPENSHIFT button

f. Click the Allow selected permissions button to authorize access

g. The Argo CD UI displays the Argo applications

### 4. Review the gitops repositories and local file structure created by the bootstrap script

a. The bootstrap script cloned the multi-tenancy-gitops repositories into the gitops-org organization in Gitea. Click Organizations then click on the gitops-org organization

b. The local gitops repos are under the OUTPUT_DIR you defined above. The structure is seen below where gitops-0-bootstrap is the local repo for the multi-tenancy-gitops repo; gitops-1-infra is the local repo for the multi-tenancy-gitops-infra repo and gitops-2-services is the local repo for the multi-tenancy-gitops-services repo. 

c. In the gitops-0-bootstrap directory the bootstrap script configured the single-cluster GITOPS_PROFILE in the 0-bootstrap/single-cluster directory. That directory contains the following 

d. The 1-infra directory contains the yaml files that defines references to the infrastructure objects that are deployed from the multi-tenancy-gitops-infra repo. 

e. The 2-services directory contains the yaml files that defines references to the services objects that are deployed from the multi-tenancy-gitops-services repo. 

f. The 3-apps directory contains the yaml files that defines references to the application objects that are deployed from the multi-tenancy-gitops-apps repo. This repo isn't used for the single-cluster GITOPS_PROFILE so it wasn't cloned and pushed to Gitea. 

## Install Instana Agent

a. In the left panel, open "More" and click on the "Agents" button

b. Click on the "Install Agents" button

c. This time, we'll be deploying the agent using a Helm chart. Click on the "OpenShift - Helm chart" button

d. Enter a cluster name and optionally the agent zone that you want the cluster to be part of

e. Copy the helm command and paste it into your terminal

f. Once it has been deployed, you can check its status using:

```bash
kubectl get all -n instana-agent
```

If everything went well, all pods should be running and in ready state