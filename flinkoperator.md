# A Quick Guide to Automated Data-Streaming with the Flink Operator on OpenShift

September 29th, 2023 | By Manna Kong

###### tags: kubernetes, OpenShift, operators

![](https://i.imgur.com/CK7f75y.jpg)

## What is the [Flink](https://flink.apache.org/what-is-flink/flink-architecture/)?

What is Flink, or Apache Flink to be specific? Apache Flink is a framework that enables real-time data stream processing. Real-time data processing is intregal to meeting customer expectations. With Apache Flink, companies can provide their customers with real-time information, whether it be delayed shipments or fraudulent card transactions--customers are receiveing instant access to their data. For more information Apache Flink, check out these [resources](#Appendix).

## What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)? 

An Operator is a Kubernetes native application that extends upon the controller concept found in Kubernetes resources. An Operator is made of three key components: custom resource, custom controller, and application-specific knowledge. Utilizing already existing Kubernetes controller concepts, an operator is a custom controller that manages the state of our cluster based on the application-specific knowledge embedded in your custom resource definition. Operators are personalized controllers that can manage the complete lifecycle of an application from deployment to troubleshooting to even autoscaling. It is a powerful tool that takes the heavy lifting from application management.

## What is the [Flink Operator](https://nightlies.apache.org/flink/flink-kubernetes-operator-docs-main/)?

The Flink Operator works as a control plane that deploys and manages the entire lifecycle of Apache Flink applications. The goal of the Flink Operator is to manage applications as a human operator would. It manages cluster startup, deploys jobs, updates apps, and resolves prevailent problems. It is highly capable of automating operational tasks and comprehensively manages Apache Flink applications.

## Installing the Flink Operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

Installing the Flink Operator on Red Hat OpenShift Container Platform(RHOCP) is a simple and easy process, all you need is an OpenShift cluster and admin privileges. Follow along to get the Flink Operator on your cluster. 

### Step 1: Install the Flink Operator

Starting from the RHOCP homepage: 

- Navigate to the side bar and click `Operators`. 
- In the dropdown, select `OperatorHub`. 
- Once you've completed that, search for *Flink* in the search box and select the `Flink Kubernetes Operator` (it is a community operator and will be labeled so).
- Select `Install`, and install the operator with the default values, unless you have other preferences.
- After the Flink Operator is done installing, navigate to the operator via `View Operator` or *Operators > Installed Operators > Flink Kubernetes Operator*
- On the operator details page, create an instance of both the *Flink Deployment* and *Flink Session Job*.

Once those are created you have successfully created yourself an Apache Flink application. Hurray!

### Step 2: Accessing the Apache Flink Web Dashboard

To access your web dashboard simply port-forward the service: 

`oc port-forward svc/basic-example-rest 8081`

You can also create a route to view the web dashboard if you don't want to keep a terminal up and running. In your terminal, apply this resource to create a route resource.

```
oc apply -f -<< EOF 
kind: Route
apiVersion: route.openshift.io/v1
metadata:
  name: flink-route
  namespace: openshift-operators
spec:
  to:
    kind: Service
    name: basic-example-rest
  tls: null
  port:
    targetPort: 8081
EOF
```

Once the route has been created, use this command `oc get route` to retrieve the Apache Flink web dashboard url which can be found under *HOST/PORT*. Now that concludes the guide, but feel free to explore the dashboard and what Apache Flink has to offer.

## Conclusion

Apache Flink is a mighty tool for data-streaming and will continue to be a shining assest to companies who use it. Providing real-time data to customers is essential to meeting customer expectations. The Flink Operator and operators in general automate and eases the job of developers and human operators substantially freeing up time and resources for your developers to focus and work on more important matters. 

Among the Flink Operator, there are a multitude of other operators available on RHOCP in OperatorHub that can be of use to you and be installed at a click of a button. Feel free to explore the large variety of community operators as well as certifed or marketplace opertors available in OperatorHub. Explore the Flink Operator further and ease the workflow and management of your Apache Flink applications. Sit back and relax as the Flink Operator manages your application for you!

## Appendix

#### Apache Flink
- [Intro to Stream Processing with Apache Flink | Apache Flink 101](https://www.youtube.com/watch?v=WajYe9iA2Uk)

#### Red Hat OpenShift
- [Red Hat OpenShift Container Platform](https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform)


