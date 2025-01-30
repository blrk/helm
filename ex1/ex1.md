### Ex1
* List the nodes in the cluster
```bash
kubectl get nodes
NAME            STATUS   ROLES           AGE    VERSION
kubeadm         Ready    control-plane   130d   v1.30.5
kubeadm-node1   Ready    <none>          130d   v1.30.5
```
* Get the cluster info
```bash
kubectl cluster-info
Kubernetes master is running at https://192.168.122.35:6443
CoreDNS is running at https://192.168.122.35:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
* Install tree command 
```bash
yum install tree -y
```
* Download/Install helm 
```bash
sudo zypper install helm -y
```
* verify the version of the helm 
```bash
helm version
version.BuildInfo{Version:"v3.16.3", GitCommit:"cfd07493f46efc9debd9cc1b02a0961186df7fdf", GitTreeState:"clean", GoVersion:"go1.22.9"}
```
* List all hlm commands
```bash
helm
```
* Helm help command 
```bash
helm create --help
```
* List the repo, Note: you don't see any repo by default
```bash
helm repo list
```
* Initialize a Helm Chart Repository
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
* List the repo
```bash
helm repo list
NAME    URL                               
bitnami https://charts.bitnami.com/bitnami
```
* Perform a repo update
```bash
helm repo update 
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈
```
* List the releases 
```bash
helm list
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION
```
* Note: Lists only the deployed releases in the current namespace by default. It does not show failed, superseded, or deleted releases.
* Lists all releases, regardless of their status (e.g., DEPLOYED, FAILED, DELETED, SUPERSEDED, etc.).
```bash 
helm list -a | helm list --all
```
* Finding Charts
```bash
helm search repo nginx
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION                                       
bitnami/nginx                           18.3.5          1.27.3          NGINX Open Source is a web server that can be a...
bitnami/nginx-ingress-controller        11.6.7          1.12.0          NGINX Ingress Controller is an Ingress controll...
bitnami/nginx-intel                     2.1.15          0.4.9           DEPRECATED NGINX Open Source for Intel is a lig...
```