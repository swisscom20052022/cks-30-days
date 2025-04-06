# Chapter 10: Cluster Setup - RBAC

## Introduction

In this chapter, we will explore the implementation and management of Role-Based Access Control (RBAC) in a Kubernetes (K8s) cluster. RBAC is crucial for defining and enforcing access controls, ensuring that users and service accounts have the appropriate permissions to interact with cluster resources. By the end of this chapter, you will understand how to create and apply RBAC policies to secure your Kubernetes environment.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of RBAC
- Creating RBAC policies
- Applying RBAC policies
- Testing RBAC policies
- Best practices for RBAC

### Overview of RBAC

Role-Based Access Control (RBAC) is a method for regulating access to resources based on the roles of individual users or service accounts within an organization. In Kubernetes, RBAC is implemented using four main resources:
- **Role**: Defines a set of permissions within a specific namespace.
- **ClusterRole**: Defines a set of permissions cluster-wide.
- **RoleBinding**: Grants a Role's permissions to a user or service account within a specific namespace.
- **ClusterRoleBinding**: Grants a ClusterRole's permissions to a user or service account cluster-wide.

### Creating RBAC Policies

RBAC policies are defined using YAML manifests. Here is an example of a simple RBAC policy that grants read-only access to pods within a namespace:

1. Create a Role that defines the permissions:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

2. Create a RoleBinding that grants the Role's permissions to a user or service account:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: <username>
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

### Applying RBAC Policies

To apply an RBAC policy, use the `kubectl apply` command:

```sh
kubectl apply -f role.yaml
kubectl apply -f rolebinding.yaml
```

You can verify that the RBAC policy has been applied using the `kubectl get roles` and `kubectl get rolebindings` commands:

```sh
kubectl get roles -n default
kubectl get rolebindings -n default
```

### Testing RBAC Policies

To test RBAC policies, you can attempt to perform actions as the user or service account to which the Role or ClusterRole has been bound. Here is an example of how to test an RBAC policy:

1. Attempt to list pods as the user:

```sh
kubectl auth can-i list pods --as=<username> -n default
```

If the RBAC policy is correctly applied, the command should return "yes".

2. Attempt to create a pod as the user:

```sh
kubectl auth can-i create pods --as=<username> -n default
```

If the RBAC policy is correctly applied, the command should return "no".

### Best Practices for RBAC

- Define Roles and ClusterRoles that align with your organization's security requirements.
- Use the principle of least privilege when granting permissions.
- Regularly review and update RBAC policies to ensure they meet your security requirements.
- Use RoleBindings and ClusterRoleBindings to apply RBAC policies to the appropriate users and service accounts.
- Monitor user and service account activities to detect and respond to potential security incidents.

## CKS-Specific Tips and Tricks

- Familiarize yourself with the syntax and structure of RBAC YAML manifests.
- Practice creating and applying RBAC policies in a test environment.
- Understand how to use RoleBindings and ClusterRoleBindings to apply RBAC policies.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the implementation and management of Role-Based Access Control (RBAC) in a Kubernetes cluster, including an overview of RBAC, creating and applying RBAC policies, testing RBAC policies, and best practices. By understanding how to use RBAC, you can enhance the security and compliance of your Kubernetes environment.

## Mini-Quiz

1. What is Role-Based Access Control (RBAC) used for in Kubernetes?
   - RBAC is used to regulate access to resources based on the roles of individual users or service accounts.

2. How do you create a Role in Kubernetes?
   - Define the Role using a YAML manifest.

3. What is the purpose of a RoleBinding in RBAC?
   - A RoleBinding grants a Role's permissions to a user or service account within a specific namespace.

4. How can you test an RBAC policy in Kubernetes?
   - Attempt to perform actions as the user or service account to which the Role or ClusterRole has been bound.

5. What are some best practices for RBAC?
   - Define Roles and ClusterRoles that align with security requirements, use the principle of least privilege, regularly review and update policies, use RoleBindings and ClusterRoleBindings, monitor user and service account activities.

## Answers

1. RBAC is used to regulate access to resources based on the roles of individual users or service accounts.
2. Define the Role using a YAML manifest.
3. A RoleBinding grants a Role's permissions to a user or service account within a specific namespace.
4. Attempt to perform actions as the user or service account to which the Role or ClusterRole has been bound.
5. Define Roles and ClusterRoles that align with security requirements, use the principle of least privilege, regularly review and update policies, use RoleBindings and ClusterRoleBindings, monitor user and service account activities.
