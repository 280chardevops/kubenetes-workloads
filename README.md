---
theme : "night"
transition: "slide"
---

#### Juggling Kubernetes Workloads

![alt text](image-3.png)


---

![alt text](giphy.gif)

Darshan Raul

```
Senior devops engineer, Vayana Network
Linkedin: https://www.linkedin.com/in/darshanraul/
Twitter/X: https://x.com/280chardevops

```

---

###  k8s turns 10

> what it means for the industry?

--

- ubiqutous
- truly cloud native
- stable and the de facto standard
- revolutionized the way we deploy, scale, and manage containerized applications
- and much much more!

---

### Agenda 

:fa-angle-down: :fa-angle-down: ALL OF THESE :fa-angle-down: :fa-angle-down:

--

nope just the workloads :D  

![](workload.png)

---

### Lets start ??

- Some of this may be overwhelming. Just try to understand the flow of everything

- This could be repetitive for some of us but its always good to brush up with the basics

- Starting with a birds eye view of the k8s cluster components

- And before that lets go through server world

---

A typical cluster looks like this

![alt text](image.png)

---

# POD

- the smallest deployable unit
- environment for running container(s). 

--

### why a pod not a container?

--

- k8s is like running your own cloud
in that context pod is like a vm
and containers are like processes running in it

--

### Characteristics of a Pod

- immutable
- shared storage and network resources
- Pods have a lifecycle - they are created, assigned a unique ID (UID), and scheduled to run on a node until their termination or deletion.
- Each pod gets its own IP address, which allows containers within the pod to communicate with each other
-  can access storage volumes which can persist beyond the lifetime of an individual container.

--

single container example

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2 
    ports:
    - containerPort: 80
```

--

multi container example


```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-fluentd-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
    volumeMounts:
    - name: shared-logs
      mountPath: /var/log/nginx
  - name: fluentd
    image: fluent/fluentd:latest
    volumeMounts:
    - name: shared-logs
      mountPath: /var/log/nginx
  volumes:
  - name: shared-logs
    emptyDir: {}

```

--

- side car containers
- init containers
- ephemeral containers

--

### you never run a pod directly

- designed as ephemeral, disposable entities

![alt text](problematic.gif)

---

### Replicaset


- ensures a specified number of replicas (i.e., copies) of a pod are running at any given time.
- Self-Healing: If a replica fails, the ReplicaSet automatically restarts it to maintain the desired number of replicas.
- can scale up or down based on the desired number of replicas.
- including a selector that specifies how to identify Pods it can acquire

--

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: us-docker.pkg.dev/google-samples/containers/gke/gb-frontend:v5

```

--

Cons

- Manual Process: With ReplicaSets, you need to manually update the image or other configurations and ensure that the new pods replace the old ones correctly.
- No Historical Tracking

![alt text](notime.gif)
---

### Deployment

--

![alt text](image-4.png)

--

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```

- probes

---

### Summary

 workload |  Purpose
------------ | -------------
pods | ephemeral containers
Deployments | Stateless applications, rolling updates
ReplicaSets | Used primarily by Deployments
StatefulSets | Databases, distributed systems
DaemonSets | logging, monitoring, or other daemons
Jobs | Batch processing, short-lived tasks
CronJobs | Periodic tasks, scheduled reports



---



# Replica set

what if i want to create multiple pods, lets say 3
what if one goes down, who will start it back?

Replica set [mention replication controller was its predecssor]

---

# Deployments

it manages replicasets
rollbacks etc

how? labels

---

# jobs

if you have one time tasks instead of constantly running services, eg file processing 


---

# Cron jobs


---

# Daemon sets

when we always need 


---

# Statefulsets

the pods running above were stateless
ok for apps
not ok for databases

stateful set includes running pods in a sequence in 

---


---

### Additional resources

  - sayam pathak playlist
  - kubectl cheatsheet




