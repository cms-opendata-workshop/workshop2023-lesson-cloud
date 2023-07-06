---
title: "Demo: Run a Full Analysis Flow"
teaching: 20
exercises: 45
questions:
- "How do I follow the progress of a workflow?"
- "What are the different steps in the example workflow?"
objectives:
- "Mimic a full analysis flow"
- "Understand structure of a workflow"
- "Find the recid for a dataset and put it into the yaml file"
keypoints:
- "You can run an analysis on an entire dataset using multiple templates and scattering"
---

## Follow the workflow progress 

You can watch the progress of the workflow either on the command line or in the Argo GUI.  

On the command line, you can see the state of the workflow with

```bash
argo get @latest -n argo
```

and you can check the logs with

```bash
argo logs @latest -n argo
```

You can also get the logs from the pods (take the pod name from the listing of `argo get...` output) using 

```bash
kubectl logs <pod-name> -n argo
```

You check the status of the pod

```bash
kubectl describe pod <pod-name> -n argo
```


When the workflow  has finished, you will be able to access the output files from the http file server. You can also see the contents of the disk with

```bash
kubectl exec pv-pod -n argo -- ls /mnt/data
```


## Expanding the Yaml File

In the previous section, you downloaded a workflow definition and submitted it. It should be now running. This workflow corresponds to the analysis example presented in this workshop.

The workflow mimicks a full analysis, first processing CMS open data samples with POET and then running analysis script on the ouput files.

Open up the file `argo-poet.yaml`, and take a look through its contents. Below is an explanation of the major steps.

`argo-poet.yaml` calls and runs multiple different tasks. The file is broken up into different templates. Note that each task runs in a container, and the workflow is using the same container images that we have been using in the workshop. 

1. "cms-od-example", the first template, is the entrypoint, and it contains the outline for the rest of the workflow.

2. "prepare-template" gets the directories ready for other workflow steps.  

3. "get-metadata-template" uses `cernopendata-client` to get all files of the datasets.

4. "joblist-template" prepares the list of jobs that the next step would iterate over

5. "runpoet-template" processes the data, and it will take the longest amount of time to complete.  

6. "merge-step-template" combines the inputs from the jobs of the previous steps and combined them in a single root  

7. "analysis-step-template" runs the analysis.

The first template must have the same name as the entrypoint value, which is declared close to the top of the file.  Under the `dag` section of the first template, it calls other templates that are defined below.  It also contains information such as arguments to pass into each of these sections and dependencies that ensure the templates are run in the correct order.

The fifth template uses scattering to run the analysis.  It runs multiple pods at the same time.  The Argo GUI helps us visualize this process.

FIXME
![](../fig/poet-test3.PNG)

Depending on the resources you allocate to your cluster, there is a limit to the number of pods you have running at one time.  If you have more pods than this number, they will wait for each other to complete.  


## Workflow input parameters
The workflow takes the following parameters:

```yaml
  arguments:
    parameters:
    - name: startFile                                  
      value: 1
    - name: nFiles                               
      value: 1
    - name: nEvents                               
      value: 1000
    - name: recid
      value: 24119
    - name: nJobs
      value: 4
```

They give the input to the workflow steps.  
- `startFile` is the first file in the list to be processed
- `nFiles` is the number of files in each dataset you want to process in a single job.
- `nEvents` is the number of events per job and is relevant only for testing (i.e. when not analyzing all events in the file)
- `recid` is the dataset to be processed.
- `nJobs` is the number of separate jobs.

### Accessing and using datasets

Tho access the dataset file listing you will need its record number or `recid`, which can be found at the end of the dataset description on the CERN Open data portal. For example, in the dataset shown below, the [recid is 24119](https://opendata.cern.ch/record/24119).

![](../fig/RecidURL2.png)



Note that this is just an example workflow for demonstration purposes. The workflows can be customized to fit a variety of needs.  Now you are able to edit a yaml file to perform a full analysis flow.
