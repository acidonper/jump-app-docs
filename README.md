# Jump App Documentation Repository

*Jump App* is a microservice-based application created to emulate an enterprise application complex architecture in production. The  objectives of this multi microservices application is enumerated in the following list:

- Create a set of microservices in different programing languages (Javascript, Java, Golang and Python).
- Automate these microservices deployment in Kubernetes including testing, container image build, etc.
- Implement a GitOps solution in order to manage and automate the microservices settings changes.
- Create a CI/CD strategy in order to manage *Jump App* release lifecycle.
- Design a Service Mesh architecture, based on Istio, to manage the flow inside the *Jump App*.

In Summary, the main idea of this project is to generate a test tool to analyze microservices complex architectures communications and support multi hands-on and webinars around microservices, CI/CD, Service Mesh and so on in Kubernetes.

## Requirements

*Jump App* was born to be deployed on Kubernetes, more specially Red Hat Openshift. For this reason, it is required the following requirements:

- A Red Hat Openshift Container Platform +4.5 Cluster
- Internet Access from the previous cluster
- Requirements included in the following sections

## Get Started

- Review requirements sections included in this document
- Go to https://github.com/acidonper/jump-app-helm.git
- Follow *Get Started* instructions defined in the previous repository

## Microservices

As mentioned before, *Jump App* is a multi microservices application based on different programing languages. Javascript is the technology selected for the Frontend solution and the other languages implement the Backend solution.

### Fronted

The frontend is an application based on React which implements an interface to generate flow between the backend microservices.

### Backend

The main objective of these applications are to implement an API named *jump*, included in each repository, designed to receive GET and POST requests.

- GET - Return a JSON object 

```$json
{
    "code": 200,
    "message": "/jump - Greetings from XXXXX!"
}
```

- POST - Receive a JSON Object, called Jump, and send a POST or GET request to the next *jump* based on the information included in jumps field in the object

```$bash
$ curl -XPOST -H "Content-type: application/json" -v -d '{
    "message": "Hello",
    "last_path": "/jump",
    "jump_path": "/jump",
    "jumps": [
        "http://localhost:8080" # Perform a GET to the next jump
    ]
}' 'localhost:8080/jump'
{
    "code": 200,
    "message": "/jump - Greetings from Python!"
}
```

*NOTE*: A part of that, it is possible to find other features in each microservice in order to manage databases or similar.


## GitOps 

GitOps is a paradigm or a set of practices that empowers developers to perform tasks which typically fall under the purview of IT operations. Regarding *Jump App* solutions, GitOps includes a set of automated procedures to manage Kubernetes Objects:

- Deployments
- Services
- BuildConfig
- RoleBindings
- ...

### ArgoCD

"ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes and it is the solution selected to manage GitOps automated procedures which controls Kubernetes' objects". 

It is the technology selected to deploy automatically all staff around *Jump App*, including:

- CI/CD Objects in cicd namespace (ImageStreams, Tekton Objects, etc)
- *Jump App* Objects in dev, pre and pro namespaces (Deployments, Services, Routes, Service Mesh Objects, etc)

#### Requirements

- ArgoCD Operator Installed in Openshift 4 Cluster (Please visit https://argoproj.github.io/argo-cd/ for more information )

#### Operator Installation

- Create a new project named **gitops**

- Go to Openshift Operator Hub Dashboard
- Install *ArgoCD Operator*

- Create an ArgoCD Server (*This task is performed automatically by *setup.sh* in helm repository)

#### Access ArgoCD Console

Once ArgoCD Server is installed, it is possible access ArgoCD Web UI follow next procedure:

- Obtain admin password and ArgoCD Server URL

```$bash
$ oc login 
$ oc get secret argocd-cluster -o jsonpath='{.data.admin\.password}' -n gitops | base64 -d
$ oc get route argocd -n gitops
argocd-gitops.apps.<mydomain>
```

- Review Application created (cicd, jump-app-dev, jump-app-pre and jump-app-pro)

### Helm

"Helm is the best way to find, share, and use software built for Kubernetes"

In *Jump App*, Helm is the automation solution to create and modify all Kubernetes objects required to deploy *Jump App* application. 

#### Requirements

- Helm client installed in the local machine (Please follow https://helm.sh/docs/intro/install/ for more information)

## CI/CD

"Continuous Integration (CI) is a development practice where developers integrate code into a shared repository frequently, preferably several times a day. Each integration can then be verified by an automated build and automated tests. While automated testing is not strictly part of CI it is typically implied".

"Continuous delivery (CD)  is an extension of continuous integration since it automatically deploys all code changes to a testing and/or production environment after the build stage".

In *Jump App*, CI/CD strategy allows test and deploy new microservices releases in Kubernetes through a full automated procedure.

### Tekton

"Tekton is an open-source project for providing a  set of shared and standard components for building Kubernetes-style CI/CD systems"

In *Jump App*, Tekton supports CI/CD strategy managing new microservices' releases.

#### Requirements

- Red Hat Openshift Pipelines Operator installed

#### Installation

- Go to Openshift Operator Hub Dashboard
- Install *Red Hat Openshift Pipelines Operator*

### Versions Workflow

It is important to understand how a new *Jump App* microservice release works. The following list explains microservices' version details:

- Each microservice has 1 version at least, named *v1*.
- Each microservice version has a specific Deployment, Service, Route, ImageStream, BuildConfig, etc.
- Exists a general Route which point v1 or v2, when it exists.
- Tekton Listeners are configured to automatically integrate code commits in v1.
- Tekton has pipelines to deploy and promote microservices' changes for the different versions.

*NOTE*: The main idea is to have a blue-green strategy based on versions which supports multi microservices versions deployment. For this model, it is required to deploy v1 and v2 versions.

### Code Changes (Commits)

When a new commit is released, a set of actions are triggered automatically depends on the repository branch:

- DEVELOP
    - Tekton Listener triggers *Deploy Pipeline*
    - Tekton *Deploy Pipeline* downloads the new code in develop and performs the tests
    - Tekton *Deploy Pipeline* launches buildConfig v1 in order to build a new container image for DEV environment
    - Tekton *Deploy Pipeline* changes Deployment v1 settings to deploy the new image in DEV namespace

- MASTER
    - Tekton Listener triggers *Deploy and Promote Pipeline*
    - Tekton *Deploy and Promote Pipeline* downloads the new code in master and performs the tests
    - Tekton *Deploy and Promote Pipeline* launches buildConfig v1 in order to build a new container image for PRE environment
    - Tekton *Deploy and Promote Pipeline* changes Deployment v1 settings to deploy the new image in PRE namespace
    - Tekton *Deploy and Promote Pipeline* create a new buildConfig tag with production reference for PRO environment
    - Tekton *Deploy and Promote Pipeline* changes Deployment v1 settings to deploy the new image in PRO namespace

## Service Mesh

A service mesh is a dedicated network for service-to-service communications. When using a service mesh, the supporting services for distributed applications are provided via the container platform and service mesh. 

Based on the open source Istio project, Red Hat OpenShift Service Mesh provides a platform for behavioral insight and operational control over your networked microservices in a service mesh.

In *Jump App*, Service Mesh administrates microservices' connections in order to provide traffic management, observability features, flow control, etc.

#### Requirements

- Red Hat Openshift Service Mesh Operator installed
- Kiali Operator installed provided by Red Hat
- Red Hat Openshift Jager Operator installed 
- Service Mesh Control Plane Object with default gateways (ingress and egress) configured
- Service Mesh Member Roll Object configured

#### Installation

- Create a new project named **istio-system**

Once the new project is created, it is required install a set of operators:

- Go to Openshift Operator Hub Dashboard
- Install *Kiali Operator provided by Red Hat* 
- Install *Red Hat Openshift Jager* 
- Install *Red Hat Openshift Service Mesh Operator*

On the other hand, it is required to create Service Mesh Control Plane:

- Go to Installed Operator Dashboard
- Select *istio-system* namespace
- Click on "Istio Service Mesh Control Plane"
- Click on "Create ServiceMeshControlPlane"
- Enable to TRUE in *Egress Gateway and Ingress Gateway*
- Enable to FALSE in *Openshift Route*
- Click "Create"

Lastly, it is required to create Service Mesh Member Roll:

- Go to Installed Operator Dashboard
- Select *istio-system* namespace
- Click on "Istio Service Mesh Member Roll"
- Click on "Create ServiceMeshMemberRoll"
- Include jump-app-dev, jump-app-pre and jump-app-pro members
- Click "Create"