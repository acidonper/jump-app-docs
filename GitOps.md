# GitOps

GitOps is a paradigm or a set of practices that empowers developers to perform tasks which typically fall under the purview of IT operations. Regarding *Jump App* solution, GitOps includes a set of automated procedures to manage all Kubernetes Objects:

- Deployments
- Services
- BuildConfig
- RoleBindings
- ...

## ArgoCD

"ArgoCD is a declarative continuous delivery tool for Kubernetes which standardizes workflows centered around Git and provides an unique visibility for operations. In *Jump App*, it is the solution selected to manage GitOps automated procedures which controls *Jump App* Kubernetes objects:

- CI/CD Objects in the respective namespace (ImageStreams, BuildConfigs, Tekton Pipelines, etc)
- *Jump App* Objects in dev, pre and pro namespaces (Deployments, Services, Routes, Service Mesh Objects, etc)

### Requirements

- ArgoCD Operator Installed in Openshift 4 Cluster ([jump-app-gitops](https://github.com/acidonper/jump-app-gitops) provides an automated method to install this operator automatically which is referred in *Quick Start* section)

*NOTE*: Please visit https://argoproj.github.io/argo-cd/ for more information about ArgoCD

### ArgoCD Installation

ArgoCD installation includes a set of steps which have to be completed in order to start making use of ArgoCD:

- Create a new kubernetes namespace
- Go to Openshift Operator Hub Dashboard
- Install *ArgoCD Openshift*
- Create an ArgoCD Server
- Create an ArgoCD Project
- Create required ArgoCD Applications

*NOTE*: [jump-app-gitops](https://github.com/acidonper/jump-app-gitops) provides an automated method to install this ArgoCD server automatically which is referred in *Quick Start* section

### Access ArgoCD Console

Once ArgoCD Server is installed, it is possible access ArgoCD Web UI follow next procedure:

- Obtain admin password and ArgoCD Server URL

```$bash
$ oc login 
$ oc get secret argocd-cluster -o jsonpath='{.data.admin\.password}' -n gitops-argocd | base64 -d
$ oc get route argocd -n gitops-argocd
```

## Helm

"Helm is the best way to find, share, and use software built for Kubernetes" as described in the official web page. In *Jump App*, Helm is the automation solution to create and modify all Kubernetes objects required to deploy *Jump App* application.

### Requirements

- Helm client installed in the local machine

*NOTE*: Please follow https://helm.sh/docs/intro/install/ for more information)


## Author Information

Asier Cidon @Red Hat

asier.cidon@gmail.com
