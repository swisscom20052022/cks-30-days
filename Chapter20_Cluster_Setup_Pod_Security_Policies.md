# Chapter 20: Cluster Setup - Pod Security Policies

## Introduction

In this chapter, we will explore the implementation and management of Pod Security Policies (PSPs) in a Kubernetes (K8s) cluster. Pod Security Policies are a critical security feature that allows you to control the security settings of pods, ensuring that they adhere to your organization's security standards. By the end of this chapter, you will understand how to create, apply, and manage Pod Security Policies to enhance the security of your Kubernetes environment.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of Pod Security Policies
- Creating Pod Security Policies
- Applying Pod Security Policies
- Managing Pod Security Policies
- Best practices for Pod Security Policies

### Overview of Pod Security Policies

Pod Security Policies (PSPs) are cluster-level resources that control the security settings of pods. They define a set of conditions that a pod must meet to be accepted into the cluster, such as restricting the use of privileged containers, controlling access to host namespaces, and enforcing read-only root file systems. PSPs help ensure that pods adhere to your organization's security standards and reduce the risk of security vulnerabilities.

### Creating Pod Security Policies

Pod Security Policies are defined using YAML manifests. Here is an example of a simple Pod Security Policy that restricts the use of privileged containers and enforces a read-only root file system:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted-psp
spec:
  privileged: false
  readOnlyRootFilesystem: true
  allowedCapabilities:
  - NET_ADMIN
  volumes:
  - 'configMap'
  - 'emptyDir'
  - 'secret'
  hostNetwork: false
  hostIPC: false
  hostPID: false
  runAsUser:
    rule: 'MustRunAsNonRoot'
  seLinux:
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'MustRunAs'
    ranges:
    - min: 1
      max: 65535
  fsGroup:
    rule: 'MustRunAs'
    ranges:
    - min: 1
      max: 65535
```

This Pod Security Policy restricts the use of privileged containers, enforces a read-only root file system, and requires that containers run as non-root users.

### Applying Pod Security Policies

To apply a Pod Security Policy, you need to create a Role or ClusterRole that grants access to the PSP and bind it to a user or group using a RoleBinding or ClusterRoleBinding. Here is an example of how to apply a Pod Security Policy:

1. Create a ClusterRole that grants access to the PSP:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: use-restricted-psp
rules:
- apiGroups:
  - policy
  resources:
  - podsecuritypolicies
  resourceNames:
  - restricted-psp
  verbs:
  - use
```

2. Create a ClusterRoleBinding that binds the ClusterRole to a user or group:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: use-restricted-psp-binding
subjects:
- kind: User
  name: "example-user"
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: use-restricted-psp
  apiGroup: rbac.authorization.k8s.io
```

3. Apply the ClusterRole and ClusterRoleBinding:

```sh
kubectl apply -f clusterrole.yaml
kubectl apply -f clusterrolebinding.yaml
```

### Managing Pod Security Policies

Managing Pod Security Policies involves monitoring and maintaining the policies to ensure they meet your security requirements. Here are some best practices for managing Pod Security Policies:

- Regularly review and update Pod Security Policies to ensure they meet your security standards.
- Use PSPs to enforce security best practices, such as restricting the use of privileged containers and requiring non-root users.
- Monitor pod deployments to ensure they comply with the defined PSPs.
- Use RoleBindings and ClusterRoleBindings to control access to PSPs and ensure that only authorized users can create or modify pods.

### Best Practices for Pod Security Policies

- Define Pod Security Policies that align with your organization's security standards.
- Use PSPs to enforce security best practices, such as restricting the use of privileged containers and requiring non-root users.
- Regularly review and update Pod Security Policies to ensure they meet your security requirements.
- Monitor pod deployments to ensure they comply with the defined PSPs.
- Use RoleBindings and ClusterRoleBindings to control access to PSPs and ensure that only authorized users can create or modify pods.

## CKS-Specific Tips and Tricks

- Familiarize yourself with the syntax and structure of Pod Security Policy YAML manifests.
- Practice creating and applying Pod Security Policies in a test environment.
- Understand how to use RoleBindings and ClusterRoleBindings to control access to PSPs.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the implementation and management of Pod Security Policies in a Kubernetes cluster, including an overview of PSPs, creating and applying PSPs, managing PSPs, and best practices. By understanding how to use Pod Security Policies, you can enhance the security of your Kubernetes environment.

## Mini-Quiz

1. What are Pod Security Policies used for in Kubernetes?
   - Pod Security Policies control the security settings of pods, ensuring they adhere to your organization's security standards.

2. How do you create a Pod Security Policy in Kubernetes?
   - Define the Pod Security Policy using a YAML manifest.

3. How do you apply a Pod Security Policy in Kubernetes?
   - Create a Role or ClusterRole that grants access to the PSP and bind it to a user or group using a RoleBinding or ClusterRoleBinding.

4. What are some best practices for managing Pod Security Policies?
   - Regularly review and update PSPs, use PSPs to enforce security best practices, monitor pod deployments, use RoleBindings and ClusterRoleBindings to control access.

5. Why is it important to use Pod Security Policies in Kubernetes?
   - To control the security settings of pods, ensuring they adhere to your organization's security standards and reducing the risk of security vulnerabilities.

## Answers

1. Pod Security Policies control the security settings of pods, ensuring they adhere to your organization's security standards.
2. Define the Pod Security Policy using a YAML manifest.
3. Create a Role or ClusterRole that grants access to the PSP and bind it to a user or group using a RoleBinding or ClusterRoleBinding.
4. Regularly review and update PSPs, use PSPs to enforce security best practices, monitor pod deployments, use RoleBindings and ClusterRoleBindings to control access.
5. To control the security settings of pods, ensuring they adhere to your organization's security standards and reducing the risk of security vulnerabilities.
