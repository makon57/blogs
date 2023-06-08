# Keeping the Cloud Green with the kube-green Operator on OpenShift
June 6, 2023 | By Manna Kong

![](https://i.imgur.com/nvcP5iT.jpg)

## Sustainability in Cloud Technology

Out of the world's population of seven billion people, around 4.66 billion individuals are active internet users. However, it's important to recognize that everything we do online, from sending emails and playing games to storing data, requires energy and contributes to energy consumption([Matta, 2021](https://medium.com/environmental-justice-coalition/technologys-carbon-footprint-2ead6e5eef7)). Shockingly, technology accounts for a staggering 1.6 billion tons of greenhouse gas emissions([Griffiths, 2020](https://www.bbc.com/future/article/20200305-why-your-internet-habits-are-not-as-clean-as-you-think#:~:text=If%20we%20were%20to%20rather,of%20carbon%20dioxide%20a%20year)).

Consider this: in just one year, the energy consumed by a single email inbox is enough to power 40 light bulbs for an hour([thanks in advance](https://thanks-in-advance.com/)). Furthermore, the collective usage of emails worldwide is equivalent to the carbon dioxide emissions of 7 million cars(Matta, 2021). As the demand for technology increases, data centers have seen their energy usage rise by 10-30% annually, contributing to approximately 1.5% of global energy consumption([IEA, 2021](https://www.iea.org/reports/data-centres-and-data-transmission-networks)).

To say that technology plays a crucial role in driving more sustainable initiatives is an understatement.

## What is the [kube-green operator](https://kube-green.dev/)?

The kube-green operator is an operator that helps reduce the CO2 footprint of your kubernetes clusters. It is a kubernetes addon that automatically shuts down and starts up some of your resources at designated times when you don't need them. It suspends resources during off-hours to help reduce CO2 emission. The resources currently supported are Deployments and CronJobs.

## What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)?

An operator is a Kubernetes-native application that extends the capabilities of Kubernetes' controller concept. It consists of three components: a controller, a custom resource, and application knowledge. Operators are responsible for managing the lifecycle of your application and can be configured to oversee all aspects of it. While all operators are controllers, not all controllers are operators. To be considered an operator, a controller should incorporate some aspect of these three components.

Unline Kubernetes' built-in resources and controllers, operators are ideal for handling stateful applications that require ongoing maintenance. To create an operator, a custom resource must be defined, and the custom controller will monitor it within its control loop. The control loop works to reconcile the custom resource's current state to match the desired state definited in the custom resource definitino yaml. The custom resource definition contains application-specific knowledge, and the controller handles the entire process to ensure consistency throughout the applications lifecycle.

## What are the benefits and use cases for using the kube-green operator?

During regular business hours, clusters and resources operate normally, however, outside of those hours, the kube-green operator shuts down the resources and reduces the workload. kube-green currently supports Deployments and CronJobs, making it ideal for suspending these resources when they are not needed during off hours. The key benefit of using kube-green is its ability to save resources, energy, and money for organizations.

Several [adopters](https://kube-green.dev/docs/adopters/) of the kube-green operator have reported significant savings in their monthly cloud costs, with reductions of 30-40%. Additionally, kube-green eliminates the need for developers to spend time maintaining idle clusters during weekends and evenings when they are not being utilized.

By optimizing resource usage and reducing unnecessary energy consumption, kube-green offers tangible benefits to both the environment and organizations.

## Installing kube-green operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

_Note: This demo assumes you have an OpenShift cluster available and ready, and that you have the Joget DX Operator installed and running. If you haven't already, follow this [blog](https://cloud.redhat.com/blog/no-more-coding-headaches-getting-straight-to-application-creation-with-the-joget-dx-operator-on-openshift) to install the Joget DX operator to follow along with this demonstration. Though a disclaimer, the kube-green operator is not dependent on the Joget DX operator to function as intended. The Joget DX operator is utilized as an example only._

#### Step 1: Install the kube-green operator on OpenShift
- Search for `kube-green` in _Operators > OperatorHub_ in the OpenShift Container Platform Console.
- Install the kube-green operator (Note: kube-green is only available as an all-namespaces operator in the `openshift-operators` namespace and cannot be installed in specific namespaces.)

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
  sleepAt: '18:30'
  suspendDeployments: true
  timeZone: America/Chicago
  wakeUpAt: '08:00'
  weekdays: 1-5
```
- Once everything is as it should be, create your SleepInfo instance and watch as it manages and shuts down or spins up your Deployments at the designated time intervals.

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNjU5ZTBhOGZhNmQ5YTZkZWYzMTRmODlmZTFlZTliYTEyZWQ2ZjczMSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/1EEjwZA3Q3Gd3o7xTY/giphy.gif" width="60%"/>

SleepInfo can easily be adjusted accordingly to your needs and schedule. Openshift makes installing and configuring of kube-green quick and fairly simple; consequently making it easy to keep your cluster green and free of unnecessary resource consumption.

## Conclusion

kube-green is actively working on a remarkable initiative as sustainability in technology becomes an increasingly prominent concern within the community. Ensuring the "greenness" of the cloud as we forge ahead with innovative technologies is crucial for long-term success and progress at the forefront of the industry. By reducing the usage and energy consumption of our tools, workflows, storage, and resources, we can take important steps toward keeping our work clean and the world green.

Checkout Project [Kepler](https://cloud.redhat.com/blog/a-view-of-sustainability-in-openshift-with-project-kepler) at Red Hat to find out what other initivaes we are taking to save energy and create a cloud community that promotes sustainable technology and consciousness. Additionally, stay tuned as kube-green expands its support for other resources and develops a Green Dashboard that enables you to monitor your cluster's CO2 emissions. If you're interested in contributing to their code, be sure to explore their [codebase](https://github.com/kube-green/kube-green).

## Additional Resources

- [Technology's Carbon Footprint by Natasha Matta (2021)](https://medium.com/environmental-justice-coalition/technologys-carbon-footprint-2ead6e5eef7)
- [The Carbon Emissions of Big Tech by Rodrigo Navarro (2023)](https://www.electronicshub.org/the-carbon-emissions-of-big-tech/)
- [Intro to kube-green by Davide Bianchi (2022)](https://kube-green.dev/blog/welcome-blog-post/)
- [thanks in advance (interactive slide on technology's effect on the enviornment)](https://thanks-in-advance.com/)
- [Why your internet habits are not as clean as you think by Sarah Griffiths (2020)](https://www.bbc.com/future/article/20200305-why-your-internet-habits-are-not-as-clean-as-you-think#:~:text=If%20we%20were%20to%20rather,of%20carbon%20dioxide%20a%20year.)
- [Data Centres and Data Transmission Networks](https://www.iea.org/reports/data-centres-and-data-transmission-networks)

## Appendix

`SleepInfo` is a custom resource and the yaml we created above is it's custom resource definition. Here are some configuration values that `SleepInfo` takes in.
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

