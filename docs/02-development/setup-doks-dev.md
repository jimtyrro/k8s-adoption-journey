## Introduction

This section will show you how to create a [DigitalOcean Kubernetes Cluster](https://docs.digitalocean.com/products/kubernetes/) (**DOKS**) cluster that can be used for remote development, targeting the [online boutique](https://github.com/digitalocean/kubernetes-sample-apps/tree/master/microservices-demo) sample application used as a reference in this guide.

## Prerequisites

To complete this section you will need:

1. A container registry already set up as explained in the [Set up DOCR](setup-docr.md) section.
2. Doctl utility already installed as explained in the [Installing Required Tools -> Doctl](installing-required-tools.md#installing-doctl) section.

## Provisioning a Development DOKS Cluster for Microservices

In this step, you will create a new Kubernetes cluster running on the DigitalOcean platform, using the [doctl](https://docs.digitalocean.com/reference/doctl/) utility.

Following command will create a DigitalOcean Kubernetes cluster named `microservices-demo-dev`, with a pool size of `2 nodes`, each having `2 vCPUs` and `4GB` of RAM, in the `nyc1` region:

```shell
doctl k8s cluster create microservices-demo-dev \
  --auto-upgrade=true \
  --maintenance-window "saturday=21:00" \
  --node-pool "name=basicnp;size=s-2vcpu-4gb;count=2;tag=adoption-journey;label=type=basic" \
  --region nyc1
```

!!! notes
    - The example cluster created above is using 2 nodes, each having **2vCPU/4GB** size, which amounts to **48$/month**.
    - For simplicity and consistency through all the guide, the **microservices-demo-dev** name was picked for the example cluster. You can choose any name you like, but you need to make sure the naming convention stays consistent.
    - It is recommended to use a region for your cluster that is closest to you for faster interaction. Run the following command - `doctl k8s options regions` to check available regions.
    - Cluster [auto upgrade](https://docs.digitalocean.com/products/kubernetes/how-to/upgrade-cluster/#automatically) is enabled (`--auto-upgrade=true`). Kubernetes clusters should be auto-upgraded to ensure that they always contain the latest security patches.

Next, you can verify the cluster details. First, fetch your `DOKS` cluster `ID`:

```shell
doctl k8s cluster list
```

Finally, check if the `kubectl` context was set to point to your `DOKS` cluster. The `doctl` utility should do this automatically:

```shell
kubectl config current-context
```

For more info on this topic please see this [Kubernetes Starter Kit DOKS Creation](https://github.com/digitalocean/Kubernetes-Starter-Kit-Developers/tree/main/01-setup-DOKS#step-3---creating-the-doks-cluster).

Next, you will learn how to perform local microservices development using [Tilt](https://tilt.dev/).
