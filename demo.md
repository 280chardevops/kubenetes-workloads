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


# Extra

to get more instructions for minikube installation: https://shorturl.at/atsjs
