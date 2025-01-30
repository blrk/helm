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
* Add a repo
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```
* List the repo
```bash
helm repo list
NAME    URL                               
bitnami https://charts.bitnami.com/bitnami
```
