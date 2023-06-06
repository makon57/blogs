# Keeping the Cloud Green with the Kube-Green Operator on OpenShift
May 21, 2023 | By Manna Kong

![](https://i.imgur.com/nvcP5iT.jpg)

## Sustainability in Cloud Technology

Of the seven billion people on earth, 4.66 billion are active internet users. Everything we do online requires energy and consumes energy from emails, games, data storage, and more. Technology makes up 1.6 billion tons of greenhouse gas emission. In a year, one email inbox consumes enough energy to light 40 light bulbs for one hour. The world's usage of emails can generate 7 million cars worth of carbon dioxide. To handle increasing workloads, data centers have increased their energy usage by 10-30% a year which accounts for about 1.5% of global energy consumption. To say that technology is an important factor for more sustainable initatives is an understatement.

## What is [kube-green](https://kube-green.dev/) and what is the [kube-green operator](https://github.com/kube-green/kube-green)?

So, what is the kube-green operator? An operator to reduce CO2 footprint of your clusters. The kube-green operator is an operator that helps reduce the CO2 footprint of your clusters. It is a k8s addon that automatically shuts down some of your resources when you don't need them at designed times that you define.

## What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)?

An operator is a Kubernetes native application that extends the capabilities of Kubernete's controller concept. It can be broken down into three components, controller, custome resource, and application knowledge. Operators manage the lifecycle of yoru application and can be configured to oversee it all. An operator is a controller but not all controllers are operators. In this sense, for a controller to be considered an operator it should contain some aspect of those three components. Kubernetes' built-in resources and controllers have no regard for stateful applications while operators are created to manage complex applications that do require the mantainence of state. To create an operator, there must be a custom resource in which the custom controller is watching within its control loop. The control loop then works to reconcile with current state of the applicaiton with the state declared in the custom resource definition. Application specific knowledge is embedded in the custom resource definition and the controller handles it all so that their likeness may be the same. To learn more about operators, checkout the operator framework [here](https://operatorframework.io/).

## What are the benefits and use cases for using the kube-green operator?

During normal business hours, the cluster and resources would be running regularly, but outside of that, the kube-green operator would shut down the resources and reduce the workload. kube-green supports Deployments and CronJobs currently so its best usage is when Deployments and CronJobs should be suspended when they are not needed during off hours. The benefit of kube-green is that it saves resources, energy from the enviornment and money for organizations. Some [adopters](https://kube-green.dev/docs/adopters/) of the kube-green operator have seen significant savings in their monthly cloud costs which declined 30-40%. This also saves devs time needed to maintain weekend and evening clusters that aren't being utilized. 

## Installing kube-green operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

_Note: This demo assumes you have an OpenShift cluster available and ready, and that you have the Joget DX Operator installed and running. If you haven't already, follow this [blog](https://cloud.redhat.com/blog/no-more-coding-headaches-getting-straight-to-application-creation-with-the-joget-dx-operator-on-openshift) to install the Joget DX operator to follow along with this demonstration._

#### Step 1: Install the Joget DX Operator on OpenShift
- Search for "kube-green" in _Operators > OperatorHub_ in the OpenShift Container Platform Console.
- Install the kube-green operator (Note: kube-green is only available as an all-namespaces operator in the 'openshift-operators' namespace and cannot be installed in specific namespaces.)

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExOTdkY2IzMDdmYzZhMjk2Nzg3MGFiZWIxMGE4ZTBjZjQzZTZmNmNkNCZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/itEiFOUyLPBs8Bl5Ob/giphy.gif" width="60%"/>


#### Step 2: Configure a new instance of SleepInfo
- Configure an instance of SleepInfo with the correct namespace, suspended resource type, timezone, wake and sleep times, and designated days (checkout the [Appendix](#appendix)).

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZDJjYmFlM2U1ZWFlY2Q0MTQxMTk5NDk5N2RmZjdkZmFlNjE0ZTk0YiZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/hI6fofKPMT3xnpHntH/giphy.gif" width="60%"/>

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

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNjU5ZTBhOGZhNmQ5YTZkZWYzMTRmODlmZTFlZTliYTEyZWQ2ZjczMSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/1EEjwZA3Q3Gd3o7xTY/giphy.gif" width="60%"/>

SleepInfo can easily be changed and adjusted accordingly to your needs and schedule. OpenShift really does make setting up and configuration of kube-green fairly simple and quick as this is all it takes to use the operator and help keep you cluster green and free of unesscary resource consumption. 

## Conclusion

kube-green is working on something great as sustainability in technology grows to be a more prominent concern in the community. Keeping the cloud green as we continue to develop more innovative technology is essential to longevity as we pioneer to the forfront of technology. Limitting the usage and energy consumption of our tools, workflow, storage, and resources are steps that will help us continue to keep our work clean and the world green. Checkout the [Kepler](https://next.redhat.com/project/kepler/) project at Red Hat to find out what other initivaes we are taking to save energy and create a cloud community that promotes sustainable technology and consciousness. Keep an eye out as kube-green continues to develop more support for other resources and as they create a Green Dashboard for you to keep track of your cluster's CO2 emissions. If you'd like to help contriubte to there code, check out their [codebase](https://github.com/kube-green/kube-green).

## Appendix

`SleepInfo` configuration values
- `weekdays` *(required)*: `*` = everyday, `1` is Monday, `1-5` = Monday-Friday
- `sleepAt` *(required)*: indicates when deployments/cronjobs should be put to sleep, 24-hour format, ex. `19:00` or `*:*` (every hour & minute)
- `wakeUpAt`: indicates when deployments/cronjobs should restart, 24-hour format, ex. `19:00` or `*:*` (every hour & minute)
- `timeZone`: specifies the timezone in which the `sleepAt` and `wakeUpAt` times should be relative to _(default is UTC)_
- `suspendDeployments`: if set to `false`, deployments will not be suspended _(default is true)_
- `suspendCronJobs`: if set to `false`, cronjobs will not be susspended _(default is true)_
- `excludeRef`: an array object that contains specific information for a resource that should be excluded from being put to sleep
  - `apiVersion`: version of the resource
  - `kind`: the kind of resource (Deployment or CronJob)
  - `name`: the name of the resource
  - `matchLabels`: an object of strings that contain labels to identify the resource

