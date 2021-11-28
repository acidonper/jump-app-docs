# CI/CD

"Continuous Integration (CI) is a development practice where developers integrate code into a shared repository frequently, preferably several times a day. Each integration can then be verified by an automated build and automated tests. While automated testing is not strictly part of CI it is typically implied".

"Continuous deployment (CD) is an extension of continuous integration since it automatically deploys all code changes to a testing and/or production environment after the build stage".

In *Jump App*, CI/CD strategy allows test and deploy new microservices releases in Kubernetes through a full automated procedure and them between all environments (PRE, PRE and PRO)

## Tekton

"Tekton is an open-source project for providing a set of shared and standard components for building Kubernetes-style CI/CD systems". In *Jump App*, Tekton supports CI/CD strategy managing new microservices' releases:

- Execute unit tests
- Build container images
- Force these container images deployments via ArgoCD
- Execute functional tests
- container images between environments (PRE -> PRO) via ArgoCD

### Requirements

- Red Hat Openshift Pipelines Operator installed ([jump-app-gitops](https://github.com/acidonper/jump-app-gitops) provides an automated method to install this operator automatically which is referred in *Quick Start* section)

### Installation

- Go to Openshift Operator Hub Dashboard
- Install *Red Hat Openshift Pipelines Operator*
- Create respective Tekton Objects

*NOTE*: [jump-app-gitops](https://github.com/acidonper/jump-app-gitops) provides an automated method to install Tekton operator automatically which is referred in *Quick Start* section

## Versions Workflow

It is important to understand how a new *Jump App* microservice release works. The following list explains microservices' version details:

- Each microservice has 1 version at least, named *v1*.
- Each microservice has an imageStream and a couple of BuildConfig to build images for DEV and PRE/PRO environments.
- Each microservice image is tagged with the respective commit ID (By default, each environment has a default tag created with the first build process). For example:
  - back-golang:develop -> DEV
  - back-golang:master -> PRE
  - back-golang:production -> PRO
- Each microservice version has a specific Deployment, Service and Route.
- Exists a general Route which point v1 or v2 when it exists.
- Tekton Listeners are configured to automatically integrate code commits in v1.
- Tekton has pipelines to deploy and microservices changes for the different versions.

*NOTE*: The main idea is to have a blue-green strategy based on versions which supports multi microservices versions deployment. For this model, it is required to deploy v1 and v2 versions.

## Code Changes (Commits)

When a new commit is released, a set of actions are triggered automatically depends on the repository branch:

- DEVELOP
  - Tekton Listener triggers *pipeline-\<tech\>-build*
  - Tekton *pipeline-\<tech\>-build* downloads the new code in develop and performs the tests
  - Tekton *pipeline-\<tech\>-build* launches buildConfig v1 in order to build a new container image for DEV environment
  - Tekton *pipeline-\<tech\>-build* changes Deployment v1 settings to deploy the new image in DEV namespace via ArgoCD

- MASTER
    - Tekton Listener triggers *pipeline-\<tech\>-build*
    - Tekton *pipeline-\<tech\>-build* downloads the new code in master and performs the tests
    - Tekton *pipeline-\<tech\>-build* launches buildConfig v1 in order to build a new container image for PRE environment
    - Tekton *pipeline-\<tech\>-build* changes Deployment v1 settings to deploy the new image in PRE namespace via ArgoCD
    - Tekton *pipeline-\<tech\>-build* execute the functional tests
    - Tekton *pipeline-\<tech\>-build* create a new imageStream v1 tag with production reference for PRO environment
    - Tekton *pipeline-\<tech\>-build* changes Deployment v1 settings to deploy the new image in PRO namespace via ArgoCD

*NOTE:* Automated changes are defined to modify v1 version and it is required execute promotion pipelines to v2 manually

## Author Information

Asier Cidon @Red Hat

asier.cidon@gmail.com
