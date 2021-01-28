# Service Mesh

A service mesh is a dedicated network for service-to-service communications. When using a service mesh, the supporting services for distributed applications are provided via the container platform and service mesh.

Based on the open source Istio project, Red Hat OpenShift Service Mesh provides a platform for behavioral insight and operational control over your networked microservices in a service mesh.

In *Jump App*, Service Mesh manages microservices' traffic in order to provide traffic management, observability features, flow control, etc.

## Requirements

- Red Hat Openshift Service Mesh Operator installed
- Kiali Operator installed provided by Red Hat
- Red Hat Openshift Jager Operator installed
- Service Mesh Control Plane Object with default gateways (ingress and egress) configured
- Service Mesh Member Roll Object configured

*NOTE*: [jump-app-gitops](https://github.com/acidonper/jump-app-gitops) provides an automated method to install Service Mesh operator, Control Plane and Member Roll automatically which is referred in *Quick Start* section

## Installation

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


## Author Information

Asier Cidon @Red Hat

asier.cidon@gmail.com
