### install helm in the cluster
* https://helm.sh/docs/intro/install/
* Install in amazon Linux using the helm script
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```
* Add execute previlage
```bash
chmod 700 get_helm.sh
```
* Execute the script
```bash
 ./get_helm.sh
```
* verify the installation
```bash
helm 
```
* It will show the helm commands, environment variables and flags