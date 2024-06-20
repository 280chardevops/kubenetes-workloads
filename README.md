# kubenetes-workloads


some of this may be overwhelming, we can anytime sync after this for questions but try to understand the flow of everything
This could be repetitive for some of us but its always good to brush up with the basics


- k8s and why it exists
- 10 years and what it means for the industry
    - ubiqutous
    - stable
    - removes all the management overhead
    - true multi cloud
    - revolutionized the way we deploy, scale, and manage containerized applications, 
    - de facto standard for cloud-native development. 

on that note let me start a cluster


show the workload image

we will try to understand all this

- **Pods:**
  - **Use Case:** ephemeral containers.
- **Deployments:**
  - **Use Case:** Stateless applications, rolling updates.
- **ReplicaSets:**
  - **Use Case:** Used primarily by Deployments.
- **StatefulSets:**
  - **Use Case:** Databases, distributed systems.
- **DaemonSets:**
  - **Use Case:** Node-level logging, monitoring, or other system daemons.
- **Jobs:**
  - **Use Case:** Batch processing, short-lived tasks.
- **CronJobs:**
  - **Use Case:** Periodic tasks, scheduled reports.


# POD

why a pod not a container?

k8s is like running your own cloud
in that context pod is like a vm
and containers are like processes running in it

containers can talk to each other inside a pod
- mention side car containers
- init containers
- probes

# Replica set

what if i want to create multiple pods, lets say 3
what if one goes down, who will start it back?

Replica set [mention replication controller was its predecssor]


# Deployments

it manages replicasets
rollbacks etc

how? labels


# jobs

if you have one time tasks instead of constantly running services, eg file processing 

# Cron jobs

# Daemon sets

when we always need 

# Statefulsets

the pods running above were stateless
ok for apps
not ok for databases

stateful set includes running pods in a sequence in 


# Api server 

## controller manager
    - there are various controllers which handle this

# Services

- services help connect to the deployments


# Additional resources

- sayam pathak playlist
- kubectl cheatsheet



#### **4. Follow along **

## Bootstrap scripts for follow along

[[ if you are on windows, docker desktop can work]]

[[ for mac there will be different set of steps ]]


to get more instructions for minikube installation

https://shorturl.at/atsjs


## copying images to minikube
