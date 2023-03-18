# GuestBook Full Application

In this example, we will build a chart based on a :
- frontend (the one from the last tutorial)
- backend
- database

You can find the dependencies charts under the **charts** directory. each subdirectory is a Chart.
this called **umbrella** chart.

## Install the gustbook application

From the current directory, you can upgrade to the version 2 via :

````shell
helm upgrade guestbook guestbook
````

To get the releases :

````shell
helm list
````

````log
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
guestbook       default         4               2023-03-18 17:41:00.6840797 +0100 CET   deployed        guestbook-1.1.0 2.0
````

You can get the kubernetes manifest files via :

````shell
helm get manifest guestbook
````

````log
# Source: guestbook/charts/backend/templates/backend.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
---
# Source: guestbook/charts/database/templates/mongodb.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  replicas: 1
  selector:
````

````shell
k get deployments.apps
````

````log
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
backend    1/1     1            1           2m51s
frontend   1/1     1            1           102m
mongodb    1/1     1            1           2m51s
````