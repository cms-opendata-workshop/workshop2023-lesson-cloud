---
title: "Introduction"
teaching: 5
exercises: 5
questions:
- "Why use Google Kubernetes Engine?"
- ""
objectives:
- "Get your GCP environment ready to run an argo workflow"
- "Get the tools running to observe the working processes"
keypoints:
- "It is important to have accountability of all the resources you are using and delete them all correspondingly, else you might get billed for something you leave open!"
---

## Access your GKE Cluster
For the CMS-Workshop 2022 we have added you to a GCP project where we will provide temporary resources to run an Argo Workflow. Before we get into details please follow the following steps to get things started!

* Go to the [Google Cloud Console](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiT1aCX6av5AhWKtYQIHYGlASQQjBB6BAgEEAE&url=https%3A%2F%2Fconsole.cloud.google.com%2F%3Fhl%3Des&usg=AOvVaw32wCy6el4RVbIZO1m5wyNI) and login with your google account.
* Change to the CMS-opendata Project 
* Find your cluster here - FIXME
* Connect to your cluster in cloud shell

### Get your services
Your cluster has been built altogether with [Terraform](https://www.terraform.io), including all the configurations seen in the [cloud pre-exercises](https://cms-opendata-workshop.github.io/workshop2022-lesson-introcloud/). 

To get the external IP of both the `Argo GUI` and the `http server` run the following command:

```bash
kubectl get svc -n argo
```

* Argo GUI (```argo-server-<NUMBER>```)
In a new tab open the follwoing address, replace with corresponding external IP as follows:
```https://<EXTERNAL-IP>:2746```

* Http File Server (```http-fileserver-<NUMBER>```)
In a new tab open the follwoing address, replace with corresponding external IP as follows:
```<EXTERNAL-IP>```

### Argo
Argo is already installed! To make it executable download the binary and move it to it's respective path with the following commands:

```bash
curl -sLO https://github.com/argoproj/argo/releases/download/v2.11.1/argo-linux-amd64.gz
gunzip argo-linux-amd64.gz
chmod +x argo-linux-amd64
sudo mv ./argo-linux-amd64 /usr/local/bin/argo
```

OK! Let's spice things up...

{% include links.md %}

