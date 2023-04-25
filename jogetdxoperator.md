Manna Kong

# No More Coding Headaches: Getting straight to Application Creation with the Joget DX Operator on OpenShift

![](https://i.imgur.com/bG3Hzof.jpg)

### What is [Joget DX](https://www.joget.org/dx-8/)?

Joget DX is a platform that enables people who aren't experienced coders to create applications without having to learn how to code from scratch. This is because it uses a no-code/low-code approach, which means users can build applications by dragging and dropping pre-built components instead of writing code. This makes it easier and more accessible for people to create applications, even if they don't have a background in programming. It is designed to be user-friendly and combines different tools to help people quickly and easily build, deliver, monitor, and maintain enterprise applications. Joget DX is an open-source platform, which means that it's free to use and can be customized to meet the needs of different organizations. 

### What is the [Joget DX Operator](https://catalog.redhat.com/software/containers/joget/joget-dx-operator/5e70e12569aea31642a9c910)?

The Joget DX Operator is a tool that helps people easily manage and use the Joget DX low-code application platform on Kubernetes clusters. It automates the process of installing, configuring, scaling, and upgrading Joget DX. This helps users focus on building and deploying their applications instead of having to deal with the complicated parts of Kubernetes. It's like having a helpful assistant that takes care of the technical details so people can focus on what they want to create.

#### What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)?

An operator is a special kind of Kubernetes controller with application-specific knowledge that manages a custom resource. It extends the functionality of the Kubernetes API to create, configure, and manage instances of complex applications based off of the specifications created by the user. This means that people can use operators to manage complex applications more easily and efficiently, without having to worry about the technical details of managing the application themselves.

As an example, a Kubernetes Deployment resource has specific information regarding the number of pods to deploy, it can contain information on which namespace these pods should be deployed in as well as other data. For an operator, you can extend this capability of a Deployment and say you also want to scale your application when there is too much traffic. You can specify this according to how much more pods you need depending on your set requirements. If you need a database to gather information, you can also configure a Job in your operator to set that up as well. So, with this additional functionality that you’ve extended from a Deployment, a default Kubernetes API, you’ve created a custom resource and can assign it a name (e.g. “myapp”) and treat it as any other Kubernetes resource (e.g. "oc get myapp")

#### What are the use cases for Joget DX Operator?

1. <u>Rapid application development</u>: The Joget DX Operator makes it easier and faster for developers to create and deploy applications on Kubernetes clusters. With the operator, developers can quickly spin up a Joget DX instance and start building applications without having to worry about the underlying infrastructure.

2. <u>Scalability</u>: Joget DX is designed to be scalable, and the Joget DX Operator makes it even easier to scale up or down your Joget DX instance as needed. With the operator, you can easily increase or decrease the number of instances running in your Kubernetes cluster to match the demands of your applications.

3. <u>High availability</u>: The Joget DX Operator supports deploying Joget DX in a high-availability (HA) configuration, allowing your applications to be available and accessible to users.

#### What are the benefits of using Joget DX Operator?

1. <u>Simplified management</u>: With the Joget DX Operator, you can manage your Joget DX instance directly from Kubernetes, making it easier to manage and scale your applications.

2. <u>Consistency</u>: The Joget DX Operator ensures that your Joget DX instance is deployed consistently across your Kubernetes cluster, reducing the risk of misconfigurations or inconsistencies.

3. <u>Automation</u>: The Joget DX Operator automates many of the tasks involved in deploying and managing Joget DX, saving time and reducing the risk of human error.

In summary, the Joget DX Operator simplifies the deployment and management of Joget DX on Kubernetes, making it easier for developers to build and deploy applications on the platform. The operator provides benefits such as simplified management, consistency, and automation, making it a useful tool for organizations looking to build scalable, high-performance applications on Kubernetes.

## Installing Joget DX Operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

Pre-requistes: OpenShift 4.12 Cluster


#### Step 1: Create your project

- Login to your OpenShift cluster as the Administrator and navigate to *Home > Projects*
- Create a new project named `joget`, or whatever you'd like

![creating your project on openshift](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNjNkYmZhYzBmMjEwN2IyZGQyOGQzYjFiMzNmZGU3NjRjZTNjYWUzMSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/UHeWtvWj2EppTpUHRY/giphy.gif)


#### Step 2: Deploy a MySQL database

- Set `joget` as the current project and navigate to *Home > Projects > `joget` > Workloads*
- Select the little book and add a template to the project
- Search for MySQL (**not** Ephemeral) and instantiate a template
- Set properties as:
    ```
    Namespace: joget
    Database Service Name: jogetdb
    MySQL Connection Username: joget
    MySQL Connection Password: joget
    MySQL Database Name: jwdb
    ```
![deploying mysql database](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExOGRiNzg0MzFmNTY0ZDNiNmJkNWJmY2RlMzgyYWUyOTc4ZjM1MWU3MSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/K7RAErLt5qSKMaxgQP/giphy.gif)   


#### Step 3: Install Joget DX Operator from OperatorHub

- Navigate to *Operators > OperatorHub*
- Search for and install `Joget DX Operator` (the certifed version) in the `joget` namespace 

![installing joget dx operator](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZmQyYmJiMjY3MTVmMDQ3ZTY4ZTNiOGViN2Q5MTI3N2ZjMGE4ZDQ0NSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/NYsQZkTnGGa0xo57Oq/giphy.gif)   


#### Step 4: Deploy Joget DX instance

- Navigate to Operators > Installed Operators > JogetDX Operator
- Under *Overview > Provided APIs*, create an instance of `Joget DX on JBoss EAP 7`
- In the YAML view, you can change the name or namespace and increase or decrease the size as you wish
- Once you've added your specifications, create the instance
- You can verify that it was created by clicking on its name and selecting the *Resources* tab. There should be five resources available, a deployment, two services, a replicaset, and a pod.

![creating joget dx instance](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMDNiMTFkNjQ3OTQzNzQyOGZjZTQ4Y2QwNmMxOTg0MzBkMDY0NGZkZSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/Qjh8bmn8zYRN8Pgwh3/giphy.gif)   



#### Step 5: Set up database on Joget DX

- To set up the database, navigate back to *Home > Projects > `joget` > Workloads*
- Under *Operator Backend Service*, select the `example-joget` deployment and select *Resources* in the side panel that pops up
- Under *Routes*, click and navigate to the provided link
- Fill in the form with the following configured database information and save
    ```
    Database Type: MySQL
    Database Host: jogetdb
    Database Port: 3306
    Database Username: joget
    Database Password: joget
    Include Sample Apps [x] 
    Include Sample Users [x]
    ```
- Upon successfully setting up the database, save the default login and select *Done* to take navigate to the home page
- Login to Joget DX with the default username and password: `admin`
- Create you applications!

![connecting mysql database to joget application](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZGI0NGQ1NjgwMThiOWQ1ZjMyMmFhYWJhODMzZjEwZDAxYWRlYzVlNCZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/oho9lDzr1f9ZLncePQ/giphy.gif)   

## Wrap Up!

So, why should you use the Joget DX operator? It's easy, simple, consistent, and automatable. You don't need to think over how you'll manage your applications and can solely focus on creating them. 

Now that you have that set up, you can get on to creating your own application without getting a headache from the complexity of managing your platform. Joget has more resources on their site with lots of tutorials and additional information on how to use Joget DX which I've linked below.

If you'd like more information on operators and possibly how to create your own, I've got some resources linked below that can be helpful and a start to diving into operators.

### Resources

- [Joget DX Documentation](https://dev.joget.org/community/display/DX7/Get+Started)
- [Operator Framework](https://operatorframework.io/what/)
