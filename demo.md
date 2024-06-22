# Demo Scripts

## Bootstrap

There is a bootstrap script in bin/bootstrap.sh, which you can follow to install these prerequisites

- docker
- kubectl
- minikube

---

**NOTE**:  The script has been configured to work with this specifications
- `uname -a` configuration `Linux instance-20240621-031648 6.5.0-1022-gcp #24~22.04.1-Ubuntu SMP Tue May 28 16:34:13 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux` so you may have to tweaks things here and there if you are in a different os or different architecture [arm/aarch64 etc]
- 16gb ram [recommended, 8gb should be enough]
- 4 vcpu
- Ensure you have sudo access on the machine before running the commands
- Keep the server up to date

### Commands

- run `make bootstrap` to have everything ready
    - run `chmod 700 bin/bootstrap.sh` in case of any execution permission issue\
    

## Check everything is running correctly

- Log out and log back in if on a server or open a new bash if running on your local machine

### Confirm docker is present

- `docker ps` 
- `docker images`

```
~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

~$ docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

```

### Confirm kubectl is installed

- `kubectl version`

```
~$ kubectl version
Client Version: v1.30.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
The connection to the server localhost:8080 was refused - did you specify the right host or port?

```

### Confirm minikube is installed

- `which minikube`

```
~$ which minikube
/usr/local/bin/minikube
```


## Starting the cluster

- Run `minikube start` to start a cluster

```
~$ minikube start
😄  minikube v1.33.1 on Ubuntu 22.04 (amd64)
✨  Automatically selected the docker driver. Other choices: none, ssh
📌  Using Docker driver with root privileges
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.44 ...
💾  Downloading Kubernetes v1.30.0 preload ...
    > preloaded-images-k8s-v18-v1...:  342.90 MiB / 342.90 MiB  100.00% 15.65 M
    > gcr.io/k8s-minikube/kicbase...:  481.58 MiB / 481.58 MiB  100.00% 14.06 M
🔥  Creating docker container (CPUs=2, Memory=3900MB) ...
🐳  Preparing Kubernetes v1.30.0 on Docker 26.1.1 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
~$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

~$ 
```

- This indicates that the cluster is up and running


## Checking the cluster status

- `kubectl cluster-info`

```
~$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

```

- `kubectl get nodes`

```
~$ kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   27m   v1.30.0
```

- `kubectl get all`

```
~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   27m

```

## Running the first pod

- `cd pod`
- `kubectl apply -f single-pod.yaml`

```
~$ kubectl apply -f single-pod.yaml
pod/nginx created
~$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          10s
~$ 

```

- confirm it running correctly, run below command in one terminal

    - `kubectl port-forward pod/nginx 8080:80`

    ```
    ~$ kubectl port-forward pod/nginx 8080:80
    Forwarding from 127.0.0.1:8080 -> 80
    Forwarding from [::1]:8080 -> 80

    ```

- and this in another terminal

    - `curl http://localhost:8080/`

    ```
    ~$ curl http://localhost:8080
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
        body {
            width: 35em;
            margin: 0 auto;
            font-family: Tahoma, Verdana, Arial, sans-serif;
        }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>

    <p>For online documentation and support please refer to
    <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="http://nginx.com/">nginx.com</a>.</p>

    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>

    ```

- delete the pod

    - `kubectl delete -f single-pod.yaml`

    ```
    ~$ kubectl delete -f single-pod.yaml
    pod "nginx" deleted
    ~$ kubectl get pods
    No resources found in default namespace.

    ```

## Running a multi container pod

- `cd pod`
- `kubectl apply -f multi-pod.yaml`

```
~$ kubectl apply -f multi-pod.yaml 
pod/multi-container-pod created
~$ kubectl get pods
NAME                  READY   STATUS              RESTARTS   AGE
multi-container-pod   0/2     ContainerCreating   0          5s
~$ 
```

- After a while all the 3 pods inside the pod should be ready

```
~$ kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
multi-container-pod   2/2     Running   0          51s

```

## Replicaset

- `cd replicaset`
- `kubectl apply -f replicaset.yaml`

```
~$ kubectl apply -f replicaset.yaml
replicaset.apps/frontend created
~$ kubectl get rs
NAME       DESIRED   CURRENT   READY   AGE
frontend   3         3         0       8s
~$ 

```

- check if pods are getting created

- `kubectl get pods`

    ```
    ~$ kubectl get pods
    NAME             READY   STATUS              RESTARTS   AGE
    frontend-lnzn6   0/1     ContainerCreating   0          27s
    frontend-wjxw4   0/1     ContainerCreating   0          27s
    frontend-zlxgn   0/1     ContainerCreating   0          27s

    ```

    after a while they will be running

    ```
    ~$ kubectl get pods -o wide
    NAME             READY   STATUS    RESTARTS   AGE    IP           NODE       NOMINATED NODE   READINESS GATES
    frontend-lnzn6   1/1     Running   0          111s   10.244.0.7   minikube   <none>           <none>
    frontend-wjxw4   1/1     Running   0          111s   10.244.0.6   minikube   <none>           <none>
    frontend-zlxgn   1/1     Running   0          111s   10.244.0.8   minikube   <none>           <none>

    ```

- scaling up the replicaset

    - `vim replicaset.yaml`
    - change `replicas: 3` to `replicas: 4`
    - save the file `:wq`
    - again apply the yaml `kubectl apply -f replicaset.yaml`
    - You will see a 4th container gets created

    ```
    ~$ kubectl apply -f replicaset.yaml
    replicaset.apps/frontend configured
    ~$ kubectl get pods
    NAME             READY   STATUS    RESTARTS   AGE
    frontend-lnzn6   1/1     Running   0          10m
    frontend-m6q6p   1/1     Running   0          5s
    frontend-wjxw4   1/1     Running   0          10m
    frontend-zlxgn   1/1     Running   0          10m

    ```

- delete the replicaset

    - `kubectl delete rs frontend`

    ```
    ~$ kubectl delete rs frontend
    replicaset.apps "frontend" deleted
    ~$ kubectl get pods
    No resources found in default namespace.
    ~$ kubectl get rs
    No resources found in default namespace.

    ```

# Extra

to get more instructions for minikube installation: https://shorturl.at/atsjs
