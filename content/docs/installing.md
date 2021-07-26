+++
title = "Installing cf-for-k8s"
weight = "3"
+++


## Preparation

To prepare to install cf-for-k8s:

- First, set the context for kubectl on your local machine using the following command:

  ```
  doctl kubernetes cluster kubeconfig save <cluster-name>
  ```

- Next, clone the cf-for-k8s repo from GitHub.

  ```
  git clone https://github.com/cloudfoundry/cf-for-k8s.git
  ```

- Switch into the cf-for-k8s directory.

  ```
  cd cf-for-k8s
  ```

- Create a temporary directory used as a swap space for storing some YAML files.

  ```
  mkdir ~/tempdir
  ```

## Setting configuration  

Next, we need to generate and set up configuration for your cf-for-k8s instance.

- Use the autogen script available in the repo to generate the YAML file for your cf-for-k8s deployment.

  ```
  ./hack/generate-values.sh -d <cf-domain> > ~/tempdir/cf-values.yml
  ```
  
- Digital Ocean does not include a metrics server by default when creating a Kubernetes cluster. So, please add this line to the YAML file manually to instruct the cluster to add a metrics server when installing cf-for-k8s.

  ```
  echo 'add_metrics_server_components: true' >> ~/tempdir/cf-values.yml
  ```

- You will also need to add credentials to a container registry. Here we are using Docker hub. If you have a private container registry you prefer to use, you can substitute the values to point to your own.

  ```
  app_registry:
    hostname: https://index.docker.io/v1/
    repository_prefix: “**********”
    username: “**********”
    password: 
  ```

- Use the templating tool to generate the final YAML files which will be used for installation.

  ```
  ytt -f config -f ~/tempdir/cf-values.yml > ~/tempdir/cf-for-k8s-rendered.yml
  ```

## Installing cf-for-k8s

Install cf-for-k8s by using the kapp command to deploy the artifacts indicated in the YAML file generated in the previous step

```
kapp deploy -a cf -f ~/tempdir/cf-for-k8s-rendered.yml -y
```

> Please note — you don’t have to use the YAML generator. You can create the file by yourself. This is a convenience. Also, FYI — when using the autogen script, you will receive a warning about it being a temporary/demo script.

## Configuring DNS

In this tutorial, we are using hostnames to point to the Kubernetes cluster running Cloud Foundry. A couple of additional steps are required to make this functional.

1. Add the DigitalOcean nameservers to the DNS management console of your domain registrar. This will render them as a custom nameserver entry.

![adding DigitalOcean nameservers](/cf4k8s-do/img/do-3.png "Adding DigitalOcean DNS nameservers")

2. Add the domain name with three nameservers (NS) entries on Digital Ocean. These entries are found under the “Networking” tab of the 

![adding the domain name](/cf4k8s-do/img/do-4.png "Adding the domain name")

3. Add an A name entry to the same table which will enable a wildcard subdomain redirection. This will be of the form “*.domain.ext” and should point to the external IP of the load balancer created during installation. [2]

![adding an a-name record](/cf4k8s-do/img/do-5.png "Adding an a-name record")

Once you complete this, an A name entry will be added to the DNS records.

![seeing the a-name record](/cf4k8s-do/img/do-6.png "Seeing the a-name record")

4. One default load balancer behavior on DigitalOcean cloud is the assignment of health checks to a random port. This will render the load balancers’ status as “down”. To change this, point the health check to port 80.

![changing load balancer health check port](/cf4k8s-do/img/do-7.png "Changing the load balancer health check port")

> Note — This step should be done only after the cf-for-k8s installation is completed successfully.

This completes the whole installation process.
