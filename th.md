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
* 
 

