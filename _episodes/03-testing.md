---
title: "Workflow composition"
teaching: 10
exercises: 10
questions:
- "How to debug my workflow"
- "How can I test that a step has been made"
- "Analyzing my architecture"
objectives:
- "Learn tools that will help you get a sense of notion of the cluster"
- "Understand your own workflow structure"
keypoints:
- "Testing will no doubtedly bill you"
---


## Testing your pod
So you've run a workflow but want to make an analysis of your own, this is not an easy process, but nonetheless doable. Take into consideration the following questions:
* What image suits best my workflow step?
* What's the estimate amount of resources this will take?
* Am I repeating the same step over and over? 

These and many more will haunt you throughout your preperation process, but when you are already understanding your arquitecture is key.

Get a look of everything you've got running:
```bash
kubectl get all -n argo
```

Get the amount of nodes your cluster has deployed:
```bash
kubectl get nodes
```

We can get a sense of till what point our workflow is running printing an echo, keep in mind that you musst take into consideration what programming language and version your image works with, for example in bash you can use:

```bash
echo "Is running till this point!"
```

Entering a pod to see what you can do is as simple as adding a sleep after a process you've gone through, make sure it's enough time for you to enter and do the heavy work, say a ```sleep 120``` should be enough.
Once you've added a sleep to your workflow step, execute it and with the help of your Agro GUI, you should have an idea of when you're wrokflow has reached the ```sleep command```. 
First get your running pod with:
```bash
kubectl get pods -n argo
```
Next enter this pod to see what is going on or try running what gives you error. You can enter the pod with the following:
```bash
kubectl exec --stdin --tty <POD NAME> -n argo  -- /bin/bash
```
