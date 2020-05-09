## HELM

Helm is a package manager and application management tool for Kubernetes that packages multiple Kubernetes resources

A __Chart__ is a Helm package. It contains all of the resource definitions necessary to run an application, tool, or service inside of a Kubernetes cluster.

A __Repository__ is the place where charts can be collected and shared. 

A __Release__ is an instance of a chart running in a Kubernetes cluster. One chart can often be installed many times into the same cluster. And each time it is installed, a new release is created.

__??Tiller__

### Install Helm

Mac OS 
```
brew install helm
helm version
```
To keep Helm’s local list updated with all these changes, we need to occasionally run the repository update command.

```
# first, add the default repository, then update

helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update
```
### Install chart

```
helm install mywebserver bitnami/nginx
```

### Cleanup

```
# Get List of app running with helm

helm list

#Uninstall

helm uninstall mywebserver
```