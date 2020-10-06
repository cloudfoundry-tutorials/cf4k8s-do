+++
title = "Prerequisites"
weight = "1"
+++

## Required tools

You will need the following tools to complete the installation:

- **doctl**: the command-line interface (CLI) for the DigitalOcean API. See: https://github.com/digitalocean/doctl#installing-doctl

- **kubectl**: the Kubernetes command line interface used to interact with Kubernetes clusters. See: https://kubernetes.io/docs/tasks/tools/install-kubectl/

- **cf**: the Cloud Foundry command line interface used to interact with Cloud Foundry instances. See: https://github.com/cloudfoundry/cli/wiki/V7-CLI-Installation-Guide

- **bosh**: the BOSH command line interface used (in this case) to generate a yaml file. See: https://bosh.io/docs/cli-v2-install/

- **ytt**: a templating tool for YAML used to generated the necessary configuration files. Download your binary: https://github.com/k14s/ytt/releases

- **kapp**: used to deploy and manage workloads to Kubernetes as applications. Download your binary: https://github.com/k14s/kapp/releases


## Digital Ocean account

You will need a functional DigitalOcean account to complete this tutorial. In this tutorial, you will create a two node Kubernetes cluster in the account.

You can sign up for a DigitalOcean account here: https://www.digitalocean.com/.