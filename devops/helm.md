# Helm

### What is Helm?

[Helm](https://helm.sh/) is widely known as **"the package manager for Kubernetes"**.
Although it presents itself like this, its scope goes way beyond that of a simple package manager.

The original goal of Helm was to provide users with a better way to manage all the Kubernetes YAML files we create on Kubernetes projects.

The path Helm took to solve this issue was to create **Helm Charts**.
Each chart is a bundle with one or more Kubernetes manifests – a chart can have child charts and dependent charts as well.

Helm supports Kubernetes natively, which means you don't have to write any complex syntax files or anything to start using Helm.
Just drop your template files into a new chart and you're good to go.

### Why Should You Use Helm?

Helm really shines where Kubernetes didn't go. For instance, templating.
The scope of the Kubernetes project is to deal with your containers for you, not your template files.

This makes it overly difficult to create truly generic files to be used across a large team or a large organization with many different parameters that need to be set for each file.

### What are Helm Charts?

Helm uses a packaging format called _charts_. A chart is a collection of files that describe a related set of Kubernetes resources.

When you create a Chart, we have a **specific directory tree** that we must follow so Helm understands what we want to do.

### The Chart File Structure

A [chart](https://helm.sh/docs/topics/charts/) is organized as a collection of files inside of a directory.
The directory name is the name of the chart (without versioning information). Thus, a chart describing WordPress would be stored in a `wordpress/` directory.

```
wordpress/
  Chart.yaml          # A YAML file containing information about the chart
  LICENSE             # OPTIONAL: A plain text file containing the license for the chart
  README.md           # OPTIONAL: A human-readable README file
  values.yaml         # The default configuration values for this chart
  values.schema.json  # OPTIONAL: A JSON Schema for imposing a structure on the values.yaml file
  charts/             # A directory containing any charts upon which this chart depends.
  crds/               # Custom Resource Definitions
  templates/          # A directory of templates that, when combined with values,
                      # will generate valid Kubernetes manifest files.
  templates/NOTES.txt # OPTIONAL: A plain text file containing short usage notes
```

Inside the `templates` directory, we can add our manifest files, with **native go templating**, like this:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.name }}
  namespace: {{ default .Release.Namespace .Values.namespace }}
  labels:
    app: {{ .Values.name }}
    version: {{ .Values.image.tag }}
    env: {{ .Values.env }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Values.name }}
      env: {{ .Values.env }}
  template:
    metadata:
      labels:
        app: {{ .Values.name }}
        version: {{ .Values.image.tag }}
        env: {{ .Values.env }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "khaosdoctor/zaqar:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          env:
            - name: SENDGRID_APIKEY
              value: {{ required "You must set a valid Sendgrid API key" .Values.environment.SENDGRID_APIKEY | quote }}
            - name: DEFAULT_FROM_ADDRESS
              value: {{ required "You must set a default from address" .Values.environment.DEFAULT_FROM_ADDRESS | quote }}
            - name: DEFAULT_FROM_NAME
              value: {{ required "You must set a default from name" .Values.environment.DEFAULT_FROM_NAME | quote }}
          ports:
            - name: http
              containerPort: 3000
              protocol: TCP
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

All those values can be obtained from a `Values.yaml` file (for default values), or you can set them in the CLI using the `--set <path> value` flag.