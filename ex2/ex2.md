### Ex2
#### Download a chart from a repository
```bash
helm pull bitnami/nginx -d mycharts/
```
* List the mycharts directory 
```bash
ls mycharts/
nginx-18.3.5.tgz
```
* Extract the chart
```bash
tar -xvf mycharts/nginx-18.3.5.tgz -C mycharts/ 
```
* verify the list files extracted and the contents of templates directory 
```bash
ls mycharts/nginx
Chart.lock  charts  Chart.yaml  README.md  templates  values.schema.json  values.yaml

ls mycharts/nginx/templates/
deployment.yaml  health-ingress.yaml  hpa.yaml                 ingress.yaml        NOTES.txt  prometheusrules.yaml         serviceaccount.yaml  stream-server-block-configmap.yaml  tls-secret.yaml
extra-list.yaml  _helpers.tpl         ingress-tls-secret.yaml  networkpolicy.yaml  pdb.yaml   server-block-configmap.yaml  servicemonitor.yaml  svc.yaml
```
* Modify the values.yml file to change the service type to "ClusterIp"
```bash
vi mycharts/nginx/values.yaml 
```
* In the command mode of the vi editor search the term /nodePorts
* Change the service type from LoadBalancer to ClusterIP and save the file 
```bash
service:
  ## @param service.type Service type
  ##
  type: ClusterIP
  ## @param service.ports.http Service HTTP port
  ## @param service.ports.https Service HTTPS port
  ##
  ports:
    http: 8081
    https: 443
```
* Release the application 
```bash
helm install mywebserver mycharts/nginx 
NAME: mywebserver
LAST DEPLOYED: Thu Jan 30 14:28:36 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 18.3.5
APP VERSION: 1.27.3

Did you know there are enterprise versions of the Bitnami catalog? For enhanced secure software supply chain features, unlimited pulls from Docker, LTS support, or application customization, see Bitnami Premium or Tanzu Application Catalog. See https://www.arrow.com/globalecs/na/vendors/bitnami for more information.

** Please be patient while the chart is being deployed **
NGINX can be accessed through the following DNS name from within your cluster:

    mywebserver-nginx.default.svc.cluster.local (port 8081)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

    export SERVICE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].port}" services mywebserver-nginx)
    kubectl port-forward --namespace default svc/mywebserver-nginx ${SERVICE_PORT}:${SERVICE_PORT} &
    echo "http://127.0.0.1:${SERVICE_PORT}"

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - cloneStaticSiteFromGit.gitSync.resources
  - resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
```
* List the release
```bash
helm list 
NAME       	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART       	APP VERSION
mywebserver	default  	1       	2025-01-30 14:28:36.061030294 +0530 IST	deployed	nginx-18.3.5	1.27.3     
```
* Verify the applciation status 
```bash
helm status mywebserver
```
* Verify the kubernetes resources 
```bash
kubectl get all
NAME                                     READY   STATUS    RESTARTS        AGE
pod/mywebserver-nginx-64445ccf85-vvz28   1/1     Running   0               18m

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)            AGE
service/mywebserver-nginx   ClusterIP   10.101.237.37   <none>        8081/TCP,443/TCP   18m

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mywebserver-nginx   1/1     1            1           18m

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/mywebserver-nginx-64445ccf85   1         1         1       18m
```
* Uninstall a release
```bnash
helm uninstall mywebserver
release "mywebserver" uninstalled
```
* List the resources in the K8s cluster 

#### Create your first chart 
* Navigate to the mycharts directory 
```bash
cd mycharts/ 
```
* Create a chart directory along with the common files and directories used in a chart
```bash
helm create app1
```
* Navigate the values.yml files in nginx and app1 chart repos
```bash
less mycharts/app1/values.yaml
less mycharts/nginx/values.yaml 
```
* Navigate the Chart.yml files in nginx and app1 chart repos
```bash
less mycharts/app1/Chart.yaml
less mycharts/nginx/Chart.yaml 
```

