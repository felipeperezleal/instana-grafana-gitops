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
- OpenShift 4.8 Cluster

Additionally, you need to have the following installed locally:

- Git CLI (or, if you are on a Mac, you can use the command: `xcode-select --install`)
- Helm CLI
- OC CLI version 4.7 or higher

## Getting Started

1. **Fork this repository** to your GitHub account.
2. **Clone your forked repository** to your local machine.
3. Follow the instructions in the lab to deploy the components using GitOps.

## 1. Bootstrap the cluster
