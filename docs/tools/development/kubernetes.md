# Kubernetes

Kubernetes is a container orchestration platform. In very simple terms, this allows developers to control sets of individual instances of a container to accomplish deployments, scale (and descale) rapidly, and other tasks involving large sets of machines.

## Terminology

A few terms and technologies, briefly explained.

- POD - The smallest unit of execution. A pod is an individual "machine", predominantly a Docker container. It has a unique IP address. Can be anything from a whole LAMP stack to a purpose-built application.
- SERVICE - something that routes traffic to a logical set of pods.
- VOLUMES - Storage associated with a pod. Can be Persistent, but by default it is ephemeral.
- NAMESPACES - Used to organize application clusters within the same logical group.
- CONTROLLERS - Manage the state of the cluster. Exists on the control plane.
- CONTROL PLANE - One or more individual pods that control the activity of the other pods.
- KUBECTL - The Kubernetes command line tool, used for a variety of functionality. The primary way to interface with a Kubernetes instance.
- KUBECONFIG - A file that defines the Kubernetes cluster(s) that you wish to control from your local machine. Located by default at `./root/.kube/config`.
- HELM - A command line tool for software installation of Kubernetes installations, think package manager.
- EKS - Amazon Web Service's purpose-built Kubernetes hosting service - stands for Elastic Kubernetes Service.

### Advanced Terminology

More advanced concepts, not necessarily exclusive to Kubernetes.

- RBAC - Role-Based Access Control. This is a methodology for controlling access by assigning users to groups, and giving those groups permissions.
- Service Account - In Kubernetes, a service account is a special object that allows assignment of a Kubernetes RBAC role to a specific pod. Each namespace receives a service account by default.
- OIDC - OpenID Connect. An authentication protocol supported within Kubernetes clusters.
- Kubernetes RoleBindings - A way to assign a user to an RBAC group within Kubernetes. A RoleBinding is constrained to a specific namespace.
- Kubernetes ClusterRoleBindings - A way to assign a user to an RBAC group within Kubernetes. A ClusterRoleBinding is scoped to the entire cluster, rather than a specific namespace. 
!!! note
    RoleBindings and ClusterRoleBindings "are similar to IAM Roles in that they define a set of actions (verbs) that can be performed against a collection of Kubernetes resources (objects)." [Source](https://aws.github.io/aws-eks-best-practices/security/docs/iam/#update-the-aws-node-daemonset-to-use-irsa)
- [IRSA](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts-technical-overview.html) - IAM Roles for Service Accounts. Rather than simply using the default service account, IRSA creates a role assumption within the cluster, validates a provided token and then assigns a temporary AWS role credential.