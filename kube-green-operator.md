# Keeping the Cloud Green with the Kube-Green Operator on OpenShift
May 21, 2023 | By Manna Kong

![](https://i.imgur.com/nvcP5iT.jpg)

### Sustainability in Cloud Technology

Of the seven billion people on earth, 4.66 billion are active internet users. Everything we do online requires energy and consumes energy from emails, games, data storage, and more. Technology makes up 1.6 billion tons of greenhouse gas emission. In a year, one email inbox consumes enough energy to light 40 light bulbs for one hour. The world's usage of emails can generate 7 million cars worth of carbon dioxide. To handle increasing workloads, data centers have increased their energy usage by 10-30% a year which accounts for about 1.5% of global energy consumption.

### What is [kube-green](https://kube-green.dev/) and what is the [kube-green operator](https://github.com/kube-green/kube-green)?

The kube-green operator is an operator that helps reduce the CO2 footprint of your clusters. It is a k8s addon that automatically shuts down some of your resources when you don't need them at designed times that you define. 

### What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)?

An operator is a Kubernetes native application that extends the capabilities of Kubernete's controller concept. It can be broken down into three components, controller, custome resource, and application knowledge. Operators manage the lifecycle of yoru application and can be configured to oversee it all. An operator is a controller but not all controllers are operators. In this sense, for a controller to be considered an operator it should contain some aspect of those three components. Kubernetes' built-in resources and controllers have no regard for stateful applications while operators are created to manage complex applications that do require the mantainence of state. To create an operator, there must be a custom resource in which the custom controller is watching within its control loop. The control loop then works to reconcile with current state of the applicaiton with the state declared in the custom resource definition. Application specific knowledge is embedded in the custom resource definition and the controller handles it all so that their likeness may be the same. 

#### What are the use cases for using the kube-green operator?

During normal business hours, the cluster and resources would be running regularly, but outside of that, the kube-green operator would shut down the resources and reduce the workload. 

#### What are the benefits of using the kube-green operator?

## Installing kube-green operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

Note: This demo assumes you have an OpenShift cluster available and ready, and that you have the Joget DX Operator installed and running. If you haven't already, follow this [blog](https://cloud.redhat.com/blog/no-more-coding-headaches-getting-straight-to-application-creation-with-the-joget-dx-operator-on-openshift) to install the Joget DX operator to follow along with this demonstration.

#### Step 1: Install the Joget DX Operator on OpenShift
- Search for "Joget DX Operator" in _Operators > OperatorHub_ in the OpenShift Container Platform Console.
- Install the operator (Note: Joget is only available as an all-namespaces operator in the 'openshift-operators' namespace and cannot be installed in specific namespaces.)

#### Step 2: Configure a new instance of SleepInfo
- Configure an instance of SleepInfo with the correct namespace, suspended resource type, timezone, wake and sleep times, and designated days.

```
kind: SleepInfo
apiVersion: kube-green.com/v1alpha1
metadata:
  labels:
    app: kube-green
  name: sleepinfo-joget
  namespace: joget
spec:
  sleepAt: '10:28'
  suspendDeployments: true
  timeZone: America/Chicago
  wakeUpAt: '08:00'
  weekdays: 1-5
```
- Once everything is as it should be, create your SleepInfo instance and watch as it manages and shuts down or spins up your Deployments at the designated time intervals.

The setting up and configuration of kube-green is fairly simple and quick as this is all it really takes to use the operator and help keep you cluster green and free of unesscary resource consumption. It can be changed and adjusted accordingly to your needs as the season change. 

## Conclusion

kube-green is working on something great as sustainability in technology grows to be a more prominent concern in the community. Keep the cloud green as we continue to develop more innovative technology is essential to longevity as we pioneer to the forfront of technology. Limitting the usage and energy consumption of our tools, workflow, storage, and resources are steps that will help us continue to keep our work clean and the world green.
