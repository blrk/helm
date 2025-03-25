### Template Functions and Pipelines
* Create a directory
```bash
mkdir -p mycharts/app2/templates
```
* Create a configmap.yaml in the template directory of app2
```bash
vi mycharts/app2/templates/configmap.yaml
```
* Add the following config
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: {{ .Values.myvalue }}
```
* Create values.yaml in app2 directory
```bash
vi mycharts/app2/values.yaml
```
* Add the following config to it
```bash
config:
  myvalue: "axess"
  serverPort: "8080"
  datasourceUrl: "jdbc:mysql://mysql-service:3306/mydb"
  datasourceUsername: "user"
  datasourcePassword: "password"
  loggingLevel: "INFO"
```
* 
