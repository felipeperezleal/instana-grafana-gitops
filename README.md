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

## Getting Started
### 1. Log in to your Openshift Cluster from the terminal
a. Navigate to your Openshift cluster's web console created in the Prerequisites

b. Click the user dropdown and click on Copy login command

c. Click Display Token

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