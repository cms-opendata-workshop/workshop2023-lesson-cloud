---
title: "Next steps"
teaching: 10
exercises: 10
questions:
- "What are the factors for efficient use of cloud resources"
objectives:
- "Get an understanding of costs"
- "Get an overview of different options"
keypoints:
- "There are several factors to take into consideration when running workflows on public cloud resources."
---


## Cost and time
The charges in cloud computing come from the cluster, disk space, and networking. An example of billing details for some days of testing:

<img src="../fig/billing.png" alt="Billing details" width="550" />

For this workshop, we are using a scalable max 4-node cluster that can efficiently run at least two parallel jobs (pods) on each node. The pricing depends on the zone, and whether we've chosen to allow nodes on "[Spot-VMs](https://cloud.google.com/kubernetes-engine/docs/concepts/spot-vms)", i.e. VMs that may get deleted on short notice. For our chosen solution, the price is approximately 0.2-0.3 USD/hour and it shows under "Kubernetes Engines". Note that the "autopilot" clusters were not a choice because their settings do not allow using the nfs disk as we do it. The estimated cost is visible during the cluster creation on the GKE web interface.

For the persistent disk, we've selected a standard 100 GB disk that costs approximately 0.04 USD/GB/month depending on the zone. In addition, each cluster node creates a 100 GB disk that gets deleted when the node is deleted. Some care is to be taken when setting things up as a more expensive "balanced" disk appears as default. The costs related to disks appear under "Compute Engine". The GCP documentation provides an [overview of disk costs](https://cloud.google.com/compute/disks-image-pricing#disk).

Uploading ("ingress") and downloading ("egress") data to and from a cloud provider has cost. This appears under "Networking". In our case, these are the costs related to the Argo GUI and the file server through a "load balancer". You can use the [GKE pricing calculator](https://cloud.google.com/products/calculator) to estimate all costs, including download in the Networking Egress tab.

As an example, running over 1M of events (1%) in our chosen dataset took 1 hour 10 mins to run, and the resulting output file, including the full list of [particle flow candidates](https://cms-opendata-workshop.github.io/workshop2023-lesson-advobjects/02-particleflow/index.html), is 4.6 GB.

## Further optimization

### Resource requests
A kubernetes resource can have some resource requests in terms of CPU and memory. In our workflow, we requested a certain minimum amount of CPU for the heaviest CMSSW task. This is necessary to make the cluster autoscale, i.e. increase the number of nodes when needed. Much further optimization could be done for the best choice of resource requests and limits.

### Container pull
Pulling the container from the docker of GitLab image registry takes approximately 6-7 minutes for each job. It would be worth studying whether uploading the container image to the cloud provider's registry makes it faster. The cost of uploading and storing the image would need to be evaluated.

### Data upload
We have not estimated whether it would be convenient to upload the data to the cloud provider's disk. It will certainly make the data access for processing faster, but the cost to upload ("ingress") and host some TB of input data needs to be taken into account. 

### Data download 
Downloading data from cloud resources ("egress") is not free. For quick checks of the workflow, you would rather have a lightweight output, i.e. a plot instead of a full histogram file. The final cost estimate needs to include the costs to download the full output data.


