# Jump App Documentation Repository

*Jump App* is a microservice-based application created to emulate an enterprise application complex architecture with multi environments. This app allows users to configure a set of jumps between components and generate a continuous traffic flow defining the number of retries and their span of time.

The objectives of this multi microservices application from a technical prospective are enumerated in the following list:

- Create a set of microservices in different programing languages (**Javascript**, **Java**, **Golang**, **Python** and **Quarkus**)
- Automate these microservices deployment in **Kubernetes** including their lifecycle management strategies:
  - Implement a GitOps solution based on **ArgoCD** and **Helm** in order to manage and automate the microservices settings changes
  - Create a CI/CD strategy based on **Tekton** in order to manage *Jump App* release lifecycle
- Design a Service Mesh architecture based on **Istio** to manage and monitoring the traffic between *Jump App* components
- Integrate Knative solution in order to become aware of scale to 0 application deployment benefits 

In Summary, the main idea of this project is to generate a test tool to analyze microservices complex architectures networking and support multi hands-on and webinars around microservices, CI/CD, Service Mesh and so on in Kubernetes.

## Requirements

*Jump App* was born to be deployed on Kubernetes, more specially **Red Hat Openshift Container Platform**. For this reason, it is required the following requirements:

- A Red Hat Openshift Container Platform +4.7 Cluster
- Internet Access from the previous cluster

## Get Started

- Review the information included in this document and documents added in *Specific Documentation* section
- Go to [jump-app-gitops](https://github.com/acidonper/jump-app-gitops) & Follow *Get Started* instructions

## Specific Documentation

- [Microservices](./Microservices.md)
- [GitOps - ArgoCD & Helm](./GitOps.md)
- [CI/CD - Tekton](./CICD.md)
- [Service Mesh - Istio](./ServiceMesh.md)

## Repositories Summary

- Microservices
  - [jump-app-typescrypt-react](https://github.com/acidonper/jump-app-typescrypt-react)
  - [jump-app-java-springboot](https://github.com/acidonper/jump-app-java-springboot)
  - [jump-app-golang](https://github.com/acidonper/jump-app-golang)
  - [jump-app-python](https://github.com/acidonper/jump-app-python)
  - [jump-app-quarkus](https://github.com/acidonper/jump-app-quarkus)
- Microservices gRPC
  - [jump-app-typescrypt-react](https://github.com/acidonper/jump-app-typescrypt-react) (*Same Frontend)
  - [jump-app-java-springboot-grpc](https://github.com/acidonper/jump-app-java-springboot-grpc)
  - [jump-app-golang-grpc](https://github.com/acidonper/jump-app-golang-grpc)
  - [jump-app-python-grpc](https://github.com/acidonper/jump-app-python-grpc)
- Automation tools
  - [jump-app-gitops](https://github.com/acidonper/jump-app-gitops)
  - [jump-app-helm-charts](https://github.com/acidonper/jump-app-helm-charts)

## Author Information

Asier Cidon @Red Hat

asier.cidon@gmail.com
