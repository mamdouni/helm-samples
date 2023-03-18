# GuestBook Fontend

## Helm commands

Some of the useful commands are :

| Action                        | Command                         |
| ----------------------------- | ------------------------------- |
| Install a release             | helm install [release] [chart]  |
| Upgrade a release version     | helm upgrade [release] [chart]  |
| Rollback to a release version | helm rollback [release] [chart] |
| Print release history         | helm history [release]          |
| Display release status        | helm status [release]           |
| Show details of a release     | helm get all [release]          |
| Uninstall a release           | helm uninstall [release]        |
| List releases                 | helm list                       |

You can find cheat sheets on the net.

## Install the gustbook application

For now, we have only the frontend part of the guestbook application. To install it, run this command (from the root of this project) :

````shell
helm install guestbook guestbook
````

guestbook is just the release name. If you'll install multipe releases of the same chart on the same cluster, you have to add a criteria to diffirentiate them like guestbook-demo, guestbook-staging, guestbook-production ...

````log
NAME: guestbook
LAST DEPLOYED: Sat Mar 18 16:01:11 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
````

To get the releases (you can also add --short as option) :

````shell
helm list
````

````log
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
guestbook       default         1               2023-03-18 16:01:11.4970888 +0100 CET   deployed        guestbook-0.1.0 1.0
````

You can get the kubernetes manifest files via :

````shell
helm get manifest guestbook
````

````log
 ports:
        - name: frontend
          containerPort: 4200
---
# Source: guestbook/templates/ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: guestbook-ingress
spec:
  rules:
````

## How devops upgrade a release

Well, it's simple.
Go to the **Chart.yaml** file and upgrade the :
- frontend.yaml file : upgrade the docker image tag release
- Chart.yaml file : upgrade the appVersion

Then, you can run :
````shell
helm upgrade guestbook guestbook
````

````shell
helm list
````

````log
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
guestbook       default         2               2023-03-18 16:17:25.7699982 +0100 CET   deployed        guestbook-0.1.0 2.0
````

To check the status of the revision :

````shell
helm status guestbook
````

## How devops upgrade a release

To rollback the any revision (in our case, we will rollback to the version 1), you can run :

````shell
helm rollback guestbook 1
````

````log
Rollback was a success! Happy Helming!
````

To get the history of all the changes, you can run :

````shell
helm history guestbook
````

````log
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Sat Mar 18 16:01:11 2023        superseded      guestbook-0.1.0 1.0             Install complete
2               Sat Mar 18 16:17:25 2023        superseded      guestbook-0.1.0 2.0             Upgrade complete
3               Sat Mar 18 16:21:54 2023        deployed        guestbook-0.1.0 1.0             Rollback to 1
````

## How to uninstall the release

Well, to delete all the objects created via the helm chart, you can run :

````shell
helm uninstall guestbook
````