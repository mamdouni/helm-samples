# Using existing helm charts

## Helm hub

Keep in mind that charts in https://charts.helm.sh/stable are deprecated and the repo is no more maintained.

Where can i find charts so ?

You can use one of these urls :
- https://hub.helm.sh/
- https://artifacthub.io/

to access the **helm hub repository**. This is a registry with a lot of third party helm repositories.
You can just search for a project and you can find a list of charts available for that project and their repositories.


<img src="images/helm_hub.PNG" data-canonical-src="images/helm_hub.PNG" width="700" height="550" />

You can install the chart maintained by the Bitnami organization via :

````shell
helm repo add my-repo https://charts.bitnami.com/bitnami
helm install my-release my-repo/mongodb
````

But you can find this and all the details in their nice documentation :
- https://artifacthub.io/packages/helm/bitnami/mongodb

## Command line

If you prefer the command line, you can search for the chart via :

````shell
helm search [hub|repo] chart_name
````

to look in your list of repo via :

````shell
helm search repo mongodb
````

````log
NAME                                    CHART VERSION   APP VERSION     DESCRIPTION
stable/mongodb                          7.8.10          4.2.4           DEPRECATED NoSQL document-oriented database tha...
stable/mongodb-replicaset               3.17.2          3.6             DEPRECATED - NoSQL document-oriented database t...
stable/prometheus-mongodb-exporter      2.8.1           v0.10.0         DEPRECATED A Prometheus exporter for MongoDB me...
chartmuseum/database                    1.2.0           3.6             A Helm chart for Guestbook Database Mongodb 3.6
stable/unifi                            0.10.2          5.12.35         DEPRECATED - Ubiquiti Network's Unifi Controller
````

or from the hub via :

````shell
helm search hub mongodb
````

````log
URL                                                     CHART VERSION   APP VERSION     DESCRIPTION
https://artifacthub.io/packages/helm/bitnami/mo...      13.9.3          6.0.5           MongoDB(R) is a relational open source NoSQL da...
https://artifacthub.io/packages/helm/bitnami-ak...      13.4.1          6.0.2           MongoDB(R) is a relational open source NoSQL da...
https://artifacthub.io/packages/helm/commongrou...      13.4.4          6.0.3           MongoDB(R) is a relational open source NoSQL da...
````

To check the chart documentation, you can use :

````shell
helm inspect [all|readme|chart|values] chart_name
````

but it's more simple to check the documentation on the hub or the official site than the raw documentation.

If you would like to download the chart without its dependencies (without the charts sub-folder), you can use :

````shell
helm fetch chart_name
````

to download the stable dependency in the charts sub-folder, you can run :

````shell
helm dependency update chart_name 
````

## Customizing charts

In the case you want to export the child values to the parent, you can check this video :
- https://app.pluralsight.com/course-player?clipId=339769ab-01f5-4bf6-a94a-bef94a815a62

This method is tricky and rarely used. You can achieve the same goal using global variables.

## Demos

### Mongodb Demo

What if you want to use a production mongodb database with an arbitary, primary and a replica database ?
In this case, you have to use existing charts. Check this demo which explains how to use the **bitnami** mongodb release in production :
- https://app.pluralsight.com/course-player?clipId=4afa6202-c4fc-4da2-b1bb-a50b4643058e

### Wordpress and MariaDB

How it will take to install a wordpress with a mariadb database ?
Well, less than one minute using Helm. Check the demo :
- https://app.pluralsight.com/course-player?clipId=9c0d5e21-50d7-4182-9488-c1f4864593ac
