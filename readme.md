# Helm 3 Tutorial

All the sources of this project has been taken from the pluralsight tutorial **Packaging Applications with Helm for Kubernetes** :
- https://app.pluralsight.com/library/courses/kubernetes-packaging-applications-helm/table-of-contents

You can find the author repository here :
- https://github.com/phcollignon/helm3

Helm official site :
- https://helm.sh/

The order of the examples are as below :
- [GuestBook Frontend](guestbook_frontend)
- [GuestBook Full Platform](guestbook_full/)
- [Helm Templates](helm_templates)
- [Helm Functions and Pipeline](helm_functions_pipeline)
- [Publishing Charts](publishing_charts)

## Installing Helm

Well, with Helm 3 the architecture becames more simple. We don't use anymore a tiller service, helm will use the kube rest api to manage charts.

To install the helm on you client host, follow this :
- https://helm.sh/docs/intro/install/#from-the-binary-releases

Just download the binary and place it under **/usr/local/bin/helm**.

All the helm configuration related to helm will be stored on secrets that contains the *helm* word.