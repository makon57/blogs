# Keeping the Cloud Green with the Kube-Green Operator on OpenShift
May 21, 2023 | By Manna Kong

![](https://i.imgur.com/nvcP5iT.jpg)

### Sustainability in Cloud Technology

OpenShift Kepler...

### What is [kube-green](https://kube-green.dev/) and what is the [kube-green operator](https://github.com/kube-green/kube-green)?

The kube-green operator is an operator that helps reduce the CO2 footprint of your clusters. It is a k8s addon that automatically shuts down some of your resources when you don't need them at designed times that you define. 

### What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)?

An operator is a special kind of Kubernetes controller with application-specific knowledge that manages a custom resource. It extends the functionality of the Kubernetes API to create, configure, and manage instances of complex applications based off of the specifications created by the user. This means that people can use operators to manage complex applications more easily and efficiently, without having to worry about the technical details of managing the application themselves.

As an example, a Kubernetes Deployment resource has specific information regarding the number of pods to deploy, it can contain information on which namespace these pods should be deployed in as well as other data. For an operator, you can extend this capability of a Deployment and say you also want to scale your application when there is too much traffic. You can specify this according to how much more pods you need depending on your set requirements. If you need a database to gather information, you can also configure a Job in your operator to set that up as well. So, with this additional functionality that you’ve extended from a Deployment, a default Kubernetes API, you’ve created a custom resource and can assign it a name (e.g. “myapp”) and treat it as any other Kubernetes resource (e.g. "oc get myapp")

#### What are the use cases for using the kube-green operator?

During normal business hours, the cluster and resources would be running regularly, but outside of that, the kube-green operator would shut down the resources and reduce the workload. 

#### What are the benefits of using the kube-green operator?

## Installing kube-green operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)
