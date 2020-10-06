+++
title = "Using your Cloud Foundry"
weight = "4"
+++

You can check if the Cloud Foundry on Kubernetes is functional by targeting and logging in:

```
cf api --skip-ssl-validation api.padarekha.com
cf login
```

You should use the default admin credentials specified in the `cf-values.yml` file generated during installation.

Thereafter, you should be able to deploy an application just like you would on any Cloud Foundry installation. Installing cf-for-k8s will allow you to experience the power of Kubernetes with the simplicity of Cloud Foundry.