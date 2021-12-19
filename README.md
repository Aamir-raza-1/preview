# Preview environment based on pull request using ArgoCD
Dynamic environments based on pull requests are probably the best example of a need for a much higher level of dynamism.
There is no need for anything, especially not environments to be permanent, except for production.
By adopting GitOps best practices and Argo CD in a way that each pull request (PR) creates a new environment that is destroyed when a PR is closed. By doing that, we can improve efficiency while, at the same time, reducing the costs


Before directly jumping into technical details here are some presuppositions that we need to mentioned here
1. Familiarity with GitOps, Kind, kubectl, Kustomize, ArgoCD and its related concepts of Applicationset, generators.
2. Directory Structure

## [Gitops](https://www.weave.works/technologies/gitops/)
GitOps works by using Git as a single source of truth for declarative infrastructure and applications.
## [Kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/)
The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters
## [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
kind is a tool built for running local Kubernetes clusters using Docker containers as nodes.Its quite useful for creating a Kubernetes environment for local development, QA, or CI/CD.

```This tool requires Docker should be installed```
## [Kustomize](https://kustomize.io/)
Kustomize is a template-free declarative approach to Kubernetes configuration management and customization.
## [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)
Argo CD is a declarative, GitOps continuous delivery tool, which allows developers to define and control deployment of Kubernetes application resources from within their existing Git workflow
## [ApplicationSet Controller](https://argocd-applicationset.readthedocs.io/en/stable/)
Automating the generation of Argo CD Applications with the ApplicationSet Controller.
The ApplicationSet controller is a sub-project of Argo CD which adds Application automation, and seeks to improve multi-cluster support and cluster multitenant.

```Evolution of App of apps pattern and expanded it to be more flexible and deal with a wide range of use cases```
## [Generator](https://argocd-applicationset.readthedocs.io/en/stable/Generators/)
Generators are responsible for generating parameters, which are then rendered into the template: fields of the ApplicationSet resource.
### [Git Generator](https://argocd-applicationset.readthedocs.io/en/stable/Generators-Git/)
The Git generator allows you to create Applications based on files within a Git repository, or based on the directory structure of a Git repository.
### [Pull Request Generator](https://argocd-applicationset.readthedocs.io/en/stable/Generators-Pull-Request/)
The Pull Request generator uses the API of an SCMaaS provider (eg GitHub) to automatically discover open pull requests within an repository.
### [Matrix Generator](https://argocd-applicationset.readthedocs.io/en/stable/Generators-Matrix/)
The Matrix generator may be used to combine the generated parameters of two separate generators.

# Directory Structure
Using the power of gitOps and kustomize concepts our directory structure looks like
```

```
| Directory | Internal | Description
| --- | --- | --- |
| Clusters-config | | Contains configuration related to all k8s clusters. |
|  | dev-cluster.yaml | Contains configuration related to `dev` cluster. |
| preview-app | | Contains sample preview application based on `kustomize base and overlays` |
|  | base | Base layer contains sample service and deployment of an app  |
| | sandbox | Overlay layer named as `sandbox` having replicas and image tags  |
| | pipeline | Contains configuration related to ArgoCD `applicationset` and `project`  |
