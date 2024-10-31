### Working with helm 
* list the repos
```bash
helm repo list
Error: no repositories to show
```
* Note: You get error if there is no repos availble in the local system
#### Search app repo in bitami
* https://bitnami.com/stacks
* Add bitami repos
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories
```
* List the repos now
```bash
helm repo list
NAME   	URL                               
bitnami	https://charts.bitnami.com/bitnami
```
#### Remove repos
* use helm remove
```bash
helm repo remove bitnami
"bitnami" has been removed from your repositories
```
* Add the repos back
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories
```
#### Search a specific repo
```bash
helm search repo apache
NAME                    	CHART VERSION	APP VERSION	DESCRIPTION                                       
bitnami/apache          	11.2.14      	2.4.62     	Apache HTTP Server is an open-source HTTP serve...
bitnami/airflow         	19.0.1       	2.10.0     	Apache Airflow is a tool to express and execute...
bitnami/apisix          	3.3.9        	3.9.1      	Apache APISIX is high-performance, real-time AP...
bitnami/cassandra       	11.3.12      	4.1.5      	Apache Cassandra is an open source distributed ...
bitnami/dataplatform-bp2	12.0.5       	1.0.1      	DEPRECATED This Helm chart can be used for the ...
bitnami/flink           	1.3.12       	1.20.0     	Apache Flink is a framework and distributed pro...
bitnami/geode           	1.1.8        	1.15.1     	DEPRECATED Apache Geode is a data management pl...
bitnami/kafka           	30.0.4       	3.8.0      	Apache Kafka is a distributed streaming platfor...
bitnami/mxnet           	3.5.2        	1.9.1      	DEPRECATED Apache MXNet (Incubating) is a flexi...
bitnami/schema-registry 	21.0.0       	7.7.0      	Confluent Schema Registry provides a RESTful in...
bitnami/scylladb        	1.0.10       	6.0.2      	ScyllaDB is an open-source, distributed NoSQL w...
bitnami/solr            	9.4.0        	9.6.1      	Apache Solr is an extremely powerful, open sour...
bitnami/spark           	9.2.9        	3.5.2      	Apache Spark is a high-performance engine for l...
bitnami/tomcat          	11.2.16      	10.1.28    	Apache Tomcat is an open-source web server desi...
bitnami/zookeeper       	13.4.10      	3.9.2      	Apache ZooKeeper provides a reliable, centraliz...
```
```bash
helm search repo mysql
NAME                  	CHART VERSION	APP VERSION	DESCRIPTION                                       
bitnami/mysql         	11.1.15      	8.4.2      	MySQL is a fast, reliable, scalable, and easy t...
bitnami/phpmyadmin    	17.0.5       	5.2.1      	phpMyAdmin is a free software tool written in P...
bitnami/mariadb       	19.0.4       	11.4.3     	MariaDB is an open source, community-developed ...
bitnami/mariadb-galera	14.0.9       	11.4.3     	MariaDB Galera is a multi-primary database clus...
```
* Search the version of mysql
```bash
helm search repo mysql --versions
NAME                  	CHART VERSION	APP VERSION	DESCRIPTION                                       
bitnami/mysql         	11.1.15      	8.4.2      	MySQL is a fast, reliable, scalable, and easy t...
bitnami/mysql         	11.1.14      	8.4.2      	MySQL is a fast, reliable, scalable, and easy t...
bitnami/mysql         	11.1.13      	8.4.2      	MySQL is a fast, reliable, scalable, and easy t...
** Actual o/p is more
```
#### Install a database
```bash
helm install db1 bitnami/mysql
```
```bash
NAME: db1
LAST DEPLOYED: Sat Aug 17 10:05:16 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: mysql
CHART VERSION: 11.1.15
APP VERSION: 8.4.2

** Please be patient while the chart is being deployed **

Tip:

  Watch the deployment status using the command: kubectl get pods -w --namespace default

Services:

  echo Primary: db1-mysql.default.svc.cluster.local:3306

Execute the following to get the administrator credentials:

  echo Username: root
  MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default db1-mysql -o jsonpath="{.data.mysql-root-password}" | base64 -d)

To connect to your database:

  1. Run a pod that you can use as a client:

      kubectl run db1-mysql-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mysql:8.4.2-debian-12-r2 --namespace default --env MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD --command -- bash

  2. To connect to primary service (read/write):

      mysql -h db1-mysql.default.svc.cluster.local -uroot -p"$MYSQL_ROOT_PASSWORD"

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - primary.resources
  - secondary.resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
```
* List charts
  ```bash
  helm list
  ```
* Uninstall
```bash
helm uninstall db1
```
* Update helm charts in the local repo
```bash
helm repo update
```
* Upgrade the helm charts
```bash
helm status
```
* use the upgrade commands
* Reuse values during upgrade
```base
helm upgrade db1 bitnami/mysql --reuse-values
```
#### Ass
```bash
helm search repo tomcat
helm install myserver bitnami/tomcat
[root@kubeadm ~]# helm list
NAME    	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART        	APP VERSION
myserver	default  	1       	2024-10-31 15:55:35.218546206 +0000 UTC	deployed	tomcat-11.3.0	10.1.31    

vi ass.yml

tomcatPassword: test
service:
   type: NodePort
   nodePorts:
     http: 30007
     
helm status myserver

helm upgrade myserver bitnami/tomcat --values ass.yml 

[root@kubeadm ~]# kubectl get svc
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes        ClusterIP   10.96.0.1      <none>        443/TCP        40d
myserver-tomcat   NodePort    10.96.14.176   <none>        80:30007/TCP   9m5s
```



