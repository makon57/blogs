July 3, 2023 | By Matthias Goerens, Edmund Ochieng, & Manna Kong

<!-- ###### tags: EXE Blog Template *(optional)* -->

![Free stock images can be found on Pexels and converted to online images via Imgur](https://i.imgur.com/I67Bp2w.jpg)

## What is [GitLab](https://about.gitlab.com/) & the [GitLab Operator](https://docs.gitlab.com/operator/)?

Founded in 2011, with now over 30 million users, GitLab is an open-source DevSecOps platform presented as a single application built to change the way Development, Security, and Ops teams collaborate and build software. GitLab's core objective revolves around providing a space for every individual to contribute, firmly believing that such inclusivity fuels the pace of innovation. They are firm believers of remote work, open-source principles, DevSecOps methodologies, and iterative processes.

The GitLab Operator plays a crucial role in overseeing the complete lifecycle management of GitLab instances within Kubernetes or OpenShift container platforms. Its primary objective is to simplify the process of installing and configuring GitLab instances, ensuring a seamless transition between different versions. This development initiative aims to enhance user experience by streamlining the installation and upgrade processes for GitLab instances. 

## What is an [Operator](https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/#:~:text=K8s%20Operators%20are%20controllers%20for,Custom%20Resource%20Definitions%20(CRD).)?

An operator refers to a Kubernetes native application that expands upon the controller concepts of Kubernetes resources. It incorporates specific knowledge related to an application and can be customized to oversee the complete lifecycle management of applications, including tasks such as installation and autoscaling of pods. The operator comprises three key components: a custom resource, a custom controller, and application-specific knowledge. Essentially, an operator functions as a controller that monitors the custom resource and alters the state of the Kubernetes cluster based on the application-specific knowledge integrated into the custom resource definition. Operators are highly capable Kubernetes tools that possess the ability to automate the comprehensive management of an application, simplifying the operational tasks involved.

---

# Installing the GitLab Operator on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift)

### Step 1: Pre-requistes
- Deploy **Custom SCC**
    ```
    allowHostDirVolumePlugin: false
    allowHostIPC: false            
    allowHostNetwork: false                     
    allowHostPID: false                         
    allowHostPorts: false  
    allowPrivilegeEscalation: true
    allowPrivilegedContainer: false
    allowedCapabilities: null
    apiVersion: security.openshift.io/v1        
    defaultAddCapabilities:         
    - NET_BIND_SERVICE                              
    fsGroup:                        
      type: MustRunAs            
    groups: []                   
    kind: SecurityContextConstraints
    metadata: 
      name: gitlab-nginx-ingress-scc
    priority: null   
    readOnlyRootFilesystem: false
    requiredDropCapabilities:
    - ALL            
    runAsUser:         
      type: MustRunAs
      uid: 101
    seLinuxContext:
      type: MustRunAs
    ```
- Deploy the **IngressClass**
    ```
    apiVersion: networking.k8s.io/v1
    kind: IngressClass
    metadata:
      name: gitlab-nginx
    spec:
      controller: "k8s.io/ingress-nginx"
    ```
- Deploy **cert-manager** via OLM into your OpenShift cluster

### Step 2: Install GitLab Operator
- Install the GitLab Operator 

    ![](https://i.imgur.com/AkCp4jy.png)
    
    ![](https://i.imgur.com/ya2Lpto.png)

- Create a GitLab instance & Check that the instance is running
    ```
    $ oc -n gitlab-system get gitlab
    
    NAME     STATUS    VERSION
    gitlab   Running   6.10.3
    ```
- Check that the GitLab pods are all running and healthy
    ```
    $ oc -n gitlab-system get po
    
    NAME                                               READY   STATUS      RESTARTS   AGE
    gitlab-controller-manager-77dd5cfb98-99787         2/2     Running     0          19m
    gitlab-gitaly-0                                    1/1     Running     0          17m
    gitlab-gitlab-exporter-594bdf655b-l6f62            1/1     Running     0          16m
    gitlab-gitlab-shell-9fdbdcf87-2t655                1/1     Running     0          10m
    gitlab-gitlab-shell-9fdbdcf87-p5x8g                1/1     Running     0          16m
    gitlab-kas-798947c9df-7pg7h                        1/1     Running     0          10m
    gitlab-kas-798947c9df-p6pxg                        1/1     Running     0          16m
    gitlab-migrations-1-40b-1-sc87g                    0/1     Completed   0          16m
    gitlab-minio-68796dfbf7-vc7sf                      1/1     Running     0          17m
    gitlab-minio-create-buckets-1-8j2wg                0/1     Completed   0          17m
    gitlab-nginx-ingress-controller-57c7fdcf99-pqnfn   1/1     Running     0          18m
    gitlab-nginx-ingress-controller-57c7fdcf99-zgrh5   1/1     Running     0          18m
    gitlab-postgresql-0                                2/2     Running     0          17m
    gitlab-redis-master-0                              2/2     Running     0          17m
    gitlab-registry-556c46c55c-k4stp                   1/1     Running     0          10m
    gitlab-registry-556c46c55c-xp9rh                   1/1     Running     0          16m
    gitlab-shared-secrets-1-5p3-hm8p8                  0/1     Completed   0          18m
    gitlab-shared-secrets-1-9ah-selfsign-cv7dg         0/1     Completed   0          17m
    gitlab-sidekiq-all-in-1-v2-774fb74b69-cvvtg        1/1     Running     0          11m
    gitlab-toolbox-57d6b56fdc-nsnzt                    1/1     Running     0          16m
    gitlab-webservice-default-588bbd84f5-h7mgp         2/2     Running     0          11m
    gitlab-webservice-default-588bbd84f5-mtsfj         2/2     Running     0          10m
    ```
    
### Step 3: Configure your GitLab instance
- Check that the ingress was created 
    ```
    $ oc -n gitlab-system get ing
    
    NAME                        CLASS          HOSTS               ADDRESS                          PORTS     AGE
    gitlab-kas                  gitlab-nginx   kas.opdev.io        ...us-east-1.elb.amazonaws.com   80, 443   55s
    gitlab-minio                gitlab-nginx   minio.opdev.io      ...us-east-1.elb.amazonaws.com   80, 443   86s
    gitlab-registry             gitlab-nginx   registry.opdev.io   ...us-east-1.elb.amazonaws.com   80, 443   55s
    gitlab-webservice-default   gitlab-nginx   gitlab.opdev.io     ...us-east-1.elb.amazonaws.com   80, 443   54s
    ```
- Update DNS to match the hostnames used in the ingress
- Browse to the domain in the ingress
    ![](https://i.imgur.com/OQ9207f.png)
- Obtain the initial root credentials to the GitLab instance
    ```
    oc -n gitlab-system get secrets gitlab-gitlab-initial-root-password -o yaml | yq e '.data.password' -  | base64 -d
    ```
    ![](https://i.imgur.com/1Am7B3O.png)
    

## Conclusion

Now that you're offically a pro at installing the GitLab Operator and configuring a GitLab instance on OpenShift, you can experiment away with the things this operator offers! 

GitLab is a powerful, open-source platform with a big community contributing code daily, transforming collaboration and software development in Development, Security, and Ops teams. The GitLab Operator is a great tool to quickly spin up and manage the lifecycle of a GitLab instance simplifying the installation, usage, and upgrade of your instances. For more information explore their [website](https://about.gitlab.com/) and [documentation](https://docs.gitlab.com/operator/).

