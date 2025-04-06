# Chapter 9: Cluster Setup - Pod Security Policies

## Introduction

In this chapter, we will explore the implementation and management of Pod Security Policies (PSPs) in a Kubernetes (K8s) cluster. PSPs are crucial for defining and enforcing security requirements for pods, ensuring that they adhere to best practices and organizational policies. By the end of this chapter, you will understand how to create and apply Pod Security Policies to secure your Kubernetes environment.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of Pod Security Policies
- Creating Pod Security Policies
- Applying Pod Security Policies
- Testing Pod Security Policies
- Best practices for Pod Security Policies

### Overview of Pod Security Policies

Pod Security Policies are Kubernetes resources that define a set of conditions that a pod must meet to be accepted into the cluster. These conditions can include restrictions on the use of privileged containers, host networking, volume types, and more. PSPs help ensure that pods adhere to security best practices and organizational policies.

### Creating Pod Security Policies

Pod Security Policies are defined using YAML manifests. Here is an example of a simple Pod Security Policy that restricts the use of privileged containers:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted-psp
spec:
  privileged: false
  allowPrivilegeEscalation: false
  requiredDropCapabilities:
  - ALL
  volumes:
  - 'configMap'
  - 'emptyDir'
  - 'projected'
  - 'secret'
  - 'downwardAPI'
  - 'persistentVolumeClaim'
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

This policy restricts the use of privileged containers, requires containers to run as non-root users, and limits the types of volumes that can be used.

### Applying Pod Security Policies

To apply a Pod Security Policy, you need to create a Role or ClusterRole that grants access to the PSP and bind it to the appropriate users or service accounts using RoleBindings or ClusterRoleBindings.

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

2. Create a ClusterRoleBinding that binds the ClusterRole to a user or service account:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: use-restricted-psp-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: use-restricted-psp
subjects:
- kind: User
  name: <username>
  apiGroup: rbac.authorization.k8s.io
```

### Testing Pod Security Policies

To test Pod Security Policies, you can create pods that violate the policy and observe their behavior. Here is an example of how to test a Pod Security Policy:

1. Create a pod that violates the policy by running as a root user:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: root-pod
spec:
  containers:
  - name: root-container
    image: nginx
    securityContext:
      runAsUser: 0
```

2. Attempt to create the pod using `kubectl apply`:

```sh
kubectl apply -f root-pod.yaml
```

If the Pod Security Policy is correctly applied, the pod creation should be denied.

### Best Practices for Pod Security Policies

- Define Pod Security Policies that align with your organization's security requirements.
- Start with restrictive policies and gradually relax them as needed.
- Regularly review and update Pod Security Policies to ensure they meet your security requirements.
- Use RoleBindings and ClusterRoleBindings to apply Pod Security Policies to the appropriate users and service accounts.
- Monitor pod creation and behavior to detect and respond to potential security incidents.

## CKS-Specific Tips and Tricks

- Familiarize yourself with the syntax and structure of Pod Security Policy YAML manifests.
- Practice creating and applying Pod Security Policies in a test environment.
- Understand how to use RoleBindings and ClusterRoleBindings to apply Pod Security Policies.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the implementation and management of Pod Security Policies in a Kubernetes cluster, including an overview of Pod Security Policies, creating and applying Pod Security Policies, testing Pod Security Policies, and best practices. By understanding how to use Pod Security Policies, you can enhance the security and compliance of your Kubernetes environment.

## Mini-Quiz

1. What are Pod Security Policies used for in Kubernetes?
   - Pod Security Policies define a set of conditions that a pod must meet to be accepted into the cluster.

2. How do you create a Pod Security Policy in Kubernetes?
   - Define the Pod Security Policy using a YAML manifest.

3. What is the purpose of a ClusterRole in applying Pod Security Policies?
   - A ClusterRole grants access to the Pod Security Policy.

4. How can you test a Pod Security Policy in Kubernetes?
   - Create pods that violate the policy and observe their behavior.

5. What are some best practices for Pod Security Policies?
   - Define policies that align with security requirements, start with restrictive policies, regularly review and update policies, use RoleBindings and ClusterRoleBindings, monitor pod creation and behavior.

## Answers

1. Pod Security Policies define a set of conditions that a pod must meet to be accepted into the cluster.
2. Define the Pod Security Policy using a YAML manifest.
3. A ClusterRole grants access to the Pod Security Policy.
4. Create pods that violate the policy and observe their behavior.
5. Define policies that align with security requirements, start with restrictive policies, regularly review and update policies, use RoleBindings and ClusterRoleBindings, monitor pod creation and behavior.
