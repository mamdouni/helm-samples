# Helm 3 Templates

You can find in this video an explanation about how helm templating works :
- https://app.pluralsight.com/course-player?clipId=a9d7bfb0-ea8a-4c87-be7d-4236ce2057b3

## Dry run

A useful to dry run before deplying your chart :

| Static                | Dynamic                                          |
| --------------------- | ------------------------------------------------ |
| helm template [chart] | helm install [release] [chart] --dry-run --debug |

The Static command will run it locally.
The dynamic will run it via the k8s api without commiting any changes.

## Define the template Data

Well, there are a lot of details for this part but keep in mind that :
- You can define the template date in the **values.yaml** file
- You have to define a json-schema for your values file (a yaml can be converted easily to json)
- You can access the values via **.Values** keywork in the template
- You can access some other data like the chart values via **.Chart** , the release via **.Release**
- You can access kubernets data via **.Capabilities** and more
- You can override a value via a commande line (--set property=value)
- For sub-charts, you can define a **values.yaml** file per sub-chart
- You can define a **values.yaml** file in the parent chart (and you can override what defined in a sub-chart)
- You can define a **global** metadata in the parent char that can be accessible by all the sub-charts

This video explains in details all of these concepts :
- https://app.pluralsight.com/course-player?clipId=13f773d7-db56-466e-9628-115982116203

## Install the new Chart

Check the new Chart in the **guestbook** folder that contains the the data of templates.
You can also find the template here :
- https://github.com/phcollignon/helm3/tree/master/lab7_helm_template_final/chart/guestbook

From the root directory of this module, run :

````shell
helm template guestbook
````

````log
    matchLabels:
      app: release-name-frontend
  template:
    metadata:
      labels:
        app: release-name-frontend
    spec:
      containers:
      - image: phico/frontend:2.0
        imagePullPolicy: Always
        name: release-name-frontend
````

This shows the manifest built by the template engine.
This is a static template rendering, not calling the kubernetes api. That's why we don't have the **release-name** in **release-name-frontend** for example. 

And now with the server side tests :

````shell
helm install guestbook guestbook --dry-run --debug
````

````log
spec:
  replicas: 1
  selector:
    matchLabels:
      app: guestbook-frontend
  template:
    metadata:
      labels:
        app: guestbook-frontend
    spec:
      containers:
      - image: phico/frontend:2.0
        imagePullPolicy: Always
        name: guestbook-frontend
        ports:
````

Please note the rendering of the computed values (the result of merging of all the values) at the start of the command result :

````log
COMPUTED VALUES:
backend:
  global: {}
  image:
    repository: phico/backend
    tag: "2.0"
  ingress:
    host: backend.minikube.local
  replicaCount: 1
  secret:
    mongodb_uri: bW9uZ29kYjovL2FkbWluOnBhc3N3b3JkQG1vbmdvZGI6MjcwMTcvZ3Vlc3Rib29rP2F1dGhTb3VyY2U9YWRtaW4=
  service:
    port: 80
    type: ClusterIP
database:
  global: {}
  secret:
    mongodb_password: cGFzc3dvcmQ=
    mongodb_username: YWRtaW4=
  service:
    port: 80
    type: NodePort
    ...
````

Well, there are no bugs. Let's install the release :

````shell
helm install guestbook guestbook
````

Well, if you run this example you'll notice an issue in the backend pod.
Actually, the backend retrieves the mongodb connection url from its secrets :

````log
secret:
  mongodb_uri: "bW9uZ29kYjovL2FkbWluOnBhc3N3b3JkQG1vbmdvZGI6MjcwMTcvZ3Vlc3Rib29rP2F1dGhTb3VyY2U9YWRtaW4="
#              "mongodb://admin:password@mongodb:27017/guestbook?authSource=admin"
````

But the **mongodb** is no more the service name of the mongo database. Actually, it's :

````log
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}
  ...
````

which is **guestbook-database** in our case. but how can we inform the backend that the url has changed and how to retrieve it after the database deployment ?

That's what we gonna see in the next module **Functions and pipelines**.

