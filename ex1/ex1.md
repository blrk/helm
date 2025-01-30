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
* Finding databases
```bash
helm search repo database
NAME                   	CHART VERSION	APP VERSION 	DESCRIPTION                                       
bitnami/cassandra      	12.1.2       	5.0.2       	Apache Cassandra is an open source distributed ...
bitnami/clickhouse     	7.2.0        	24.12.3     	ClickHouse is an open-source column-oriented OL...
bitnami/dokuwiki       	16.2.11      	20240206.1.0	DEPRECATED DokuWiki is a standards-compliant wi...
bitnami/gitea          	3.1.7        	1.23.1      	Gitea is a lightweight code hosting solution. W...
bitnami/influxdb       	6.5.3        	2.7.11      	InfluxDB(TM) is an open source time-series data...
bitnami/janusgraph     	1.2.3        	1.1.0       	JanusGraph is a scalable graph database optimiz...
bitnami/mariadb        	20.2.2       	11.4.4      	MariaDB is an open source, community-developed ...
bitnami/mariadb-galera 	14.1.2       	11.4.4      	MariaDB Galera is a multi-primary database clus...
bitnami/memcached      	7.6.1        	1.6.34      	Memcached is an high-performance, distributed m...
bitnami/milvus         	11.0.0       	2.5.3       	Milvus is a cloud-native, open-source vector da...
bitnami/mongodb        	16.4.2       	8.0.4       	MongoDB(R) is a relational open source NoSQL da...
bitnami/mongodb-sharded	9.1.1        	8.0.4       	MongoDB(R) is an open source NoSQL database tha...
bitnami/mysql          	12.2.2       	8.4.4       	MySQL is a fast, reliable, scalable, and easy t...
bitnami/neo4j          	0.2.2        	5.26.1      	Neo4j is a high performance graph store with al...
bitnami/postgresql     	16.4.5       	17.2.0      	PostgreSQL (Postgres) is an open source object-...
bitnami/valkey         	2.2.3        	8.0.2       	Valkey is an open source (BSD) high-performance...
bitnami/valkey-cluster 	2.1.1        	8.0.2       	Valkey is an open source (BSD) high-performance...
bitnami/etcd           	11.0.4       	3.5.18      	etcd is a distributed key-value store designed ...
bitnami/geode          	1.1.8        	1.15.1      	DEPRECATED Apache Geode is a data management pl...
bitnami/keydb          	0.3.2        	6.3.4       	KeyDB is a high performance fork of Redis with ...
bitnami/nessie         	1.1.7        	0.102.2     	Nessie is an open-source version control system...
bitnami/postgresql-ha  	15.1.6       	17.2.0      	This PostgreSQL cluster solution includes the P...
bitnami/redis          	20.6.3       	7.4.2       	Redis(R) is an open source, advanced key-value ...
bitnami/redis-cluster  	11.4.1       	7.4.2       	Redis(R) is an open source, scalable, distribut...
bitnami/scylladb       	3.1.7        	6.2.2       	ScyllaDB is an open-source, distributed NoSQL w...
```
* Find versions of db
```bash
helm search repo database --versions
```
