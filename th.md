### Template Functions and Pipelines in Helm
* Helm's templating engine, based on Go templates, provides powerful features to dynamically generate Kubernetes manifests. Two key components of this are template functions and pipelines.
#### Template Functions
* Built-in Functions: Helm provides a rich set of built-in functions that can be used within templates. These functions cover various aspects, including:
  * default:	Provides a default value if the variable is empty	`{{ .Values.appName
  * String Manipulation: quote, upper, lower, trim, replace. 
  * Mathematical Operations: add, sub, mul, div.
  * Data Structures: list, dict, index, range.
  * Conditional Logic: if, else, and, or, not.
  * YAML and JSON: toYaml, toJson, fromYaml, fromJson.
  * Helm-Specific Functions: include, required, tpl.
  * Custom Functions: You can define custom functions within your Helm charts using Go templates.
 
#### Common Helm Template Functions:


#### Pipelines
* Chaining Functions: Pipelines allow you to chain multiple template functions together, passing the output of one function as the input to the next.
* Syntax: Pipelines use the | (pipe) character to separate functions.
* Example:
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: {{ .Values.myvalue | quote | upper }}
```
* In this example:
 * Values.myvalue retrieves the value from the values.yaml file.
 * quote function adds quotes around the value.
 * upper function converts the value to uppercase.
#### How They Work Together
* Values Retrieval: Helm templates use dot notation (.) to access values from the values.yaml file and other context variables.
* Function Application: Template functions are applied to these values to perform transformations or calculations.
* Pipeline Execution: Pipelines chain multiple functions, executing them in sequence.
* Manifest Generation: The resulting values are inserted into the Kubernetes manifest, generating the final YAML output.

#### Deleting a default key
* If you need to delete a key from the default values, you may override the value of the key to be null, in which case Helm will remove the key from the overridden values merge.
* For example, the stable Drupal chart allows configuring the liveness probe, in case you configure a custom image. Here are the default values:
```bash
livenessProbe:
  httpGet:
    path: /user/login
    port: http
  initialDelaySeconds: 120
```
* If you try to override the livenessProbe handler to exec instead of httpGet using --set livenessProbe.exec.command=[cat,docroot/CHANGELOG.txt], Helm will coalesce the default and overridden keys together, resulting in the following YAML:
```bash
livenessProbe:
  httpGet:
    path: /user/login
    port: http
  exec:
    command:
    - cat
    - docroot/CHANGELOG.txt
  initialDelaySeconds: 120
```
#### Template function list
* https://helm.sh/docs/chart_template_guide/function_list/

### Flow Control in Helm Templates
* Helm templates, provide powerful flow control mechanisms to generate Kubernetes manifests.
* These mechanisms allow you to conditionally include or exclude parts of your YAML based on values, variables, and other conditions.
 * if/else for creating conditional blocks
 * with to specify a scope
 * range, which provides a "for each"-style loop
* In addition to these, it provides a few actions for declaring and using named template segments:
 * define declares a new named template inside of your template
 * template imports a named template
 * block declares a special kind of fillable template area

#### if Statements
```bash
{{- if CONDITION }}
# YAML content to include if CONDITION is true
{{- else if CONDITION }}
# YAML content to include if CONDITION is true
{{ else }}
# Default content
{{- end }}
```
#### Looping with Range 
* Loops allow you to iterate over a range of values, creating resources and making decisions based on those values.
* Creating multiple replicas of a deployment: One of the most common use cases for loops in Helm charts is to create multiple replicas of a deployment. This is useful when you want to scale your application horizontally across multiple nodes. The .Range function can be used to loop over a range of values, creating a replica for each value.
 ```bash
{{- range $i := .Values.replicaCount }}
- name: replica-{{ $i }}
  image: myorg/myapp:{{ .Values.image.tag }}
{{- end }}
```
* Creating multiple resources of the same type: Another common use case for loops in Helm charts is to create multiple resources of the same type. For example, you might use a loop to create multiple ConfigMaps, each with different configuration values. The .Range function can be used to loop over a range of values, creating a resource for each value.
 ```bash
{{- range $i, $value := .Values.configmaps }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap-{{ $i }}
data:
  {{ $value.key }}: {{ $value.value }}
{{- end }}
```
* Waiting for a specific condition: Sometimes, you may need to wait for a certain condition to be met before proceeding with the next step in your chart. For example, you may need to wait for a deployment to be ready before proceeding with creating additional resources. The .Until function can be used to loop over a range until a certain condition is met.
```bash
{{- $name := "mydeployment" -}}
{{- $ready := false -}}
{{- until $ready }}
{{- $status := kubectl get deploy $name -o jsonpath='{.status.conditions[?(@.type=="Ready")].status}' -}}
{{- if eq $status "True" }}
{{- $ready = true -}}
{{- else }}
waiting for deployment to be ready...
{{- end }}
{{- end }}
```
* Creating multiple resources based on a list: Sometimes, you may need to create multiple resources based on a list of values. For example, you may have a list of ports that need to be exposed on a service. The .Range function can be used to loop over the list of values, creating a resource for each value.
```bash
{{- range $i, $value := .Values.ports }}
- name: port-{{ $value }}
  port: {{ $value }}
{{- end }}
```

#### Modifying scope using with
* This controls variable scoping. Recall that . is a reference to the current scope. So .Values tells the template to find the Values object in the current scope.
```bash
{{ with PIPELINE }}
  # restricted scope
{{ end }}
```
* Scopes can be changed. with can allow you to set the current scope (.) to a particular object.
* 
* 

