# Chapter 8: Cluster Setup - Network Policies

## Introduction

In this chapter, we will explore the implementation and management of network policies in a Kubernetes (K8s) cluster. Network policies are crucial for controlling the flow of traffic between pods and services, enhancing the security and isolation of your applications. By the end of this chapter, you will understand how to create and apply network policies to secure your Kubernetes environment.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of network policies
- Creating network policies
- Applying network policies
- Testing network policies
- Best practices for network policies

### Overview of Network Policies

Network policies are Kubernetes resources that define how pods are allowed to communicate with each other and with other network endpoints. They use labels to select pods and specify rules for ingress (incoming) and egress (outgoing) traffic.

Network policies are implemented by network plugins, such as Calico, Cilium, and Weave Net. These plugins enforce the rules defined in the network policies to control traffic flow within the cluster.

### Creating Network Policies

Network policies are defined using YAML manifests. Here is an example of a simple network policy that allows incoming traffic to a pod from other pods with a specific label:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-traffic-from-app
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: my-app
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: my-app
```

This policy allows incoming traffic to pods with the label `app: my-app` from other pods with the same label.

### Applying Network Policies

To apply a network policy, use the `kubectl apply` command:

```sh
kubectl apply -f network-policy.yaml
```

You can verify that the network policy has been applied using the `kubectl get networkpolicies` command:

```sh
kubectl get networkpolicies
```

### Testing Network Policies

To test network policies, you can create pods with different labels and observe their communication behavior. Here is an example of how to test a network policy:

1. Create a pod with the label `app: my-app`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
  labels:
    app: my-app
spec:
  containers:
  - name: my-app-container
    image: nginx
```

2. Create another pod with a different label:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: other-app-pod
  labels:
    app: other-app
spec:
  containers:
  - name: other-app-container
    image: nginx
```

3. Test communication between the pods using `kubectl exec`:

```sh
kubectl exec -it my-app-pod -- curl other-app-pod
```

If the network policy is correctly applied, the communication should be blocked.

### Best Practices for Network Policies

- Use labels to select pods and define network policies.
- Start with a default deny-all policy and then create specific allow policies.
- Regularly review and update network policies to ensure they meet your security requirements.
- Use network policy tools, such as `kubectl`, to test and validate network policies.
- Monitor network traffic to detect and respond to potential security incidents.

## CKS-Specific Tips and Tricks

- Familiarize yourself with the syntax and structure of network policy YAML manifests.
- Practice creating and applying network policies in a test environment.
- Understand how to use labels to select pods and define traffic rules.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the implementation and management of network policies in a Kubernetes cluster, including an overview of network policies, creating and applying network policies, testing network policies, and best practices. By understanding how to use network policies, you can enhance the security and isolation of your Kubernetes environment.

## Mini-Quiz

1. What are network policies used for in Kubernetes?
   - Network policies control the flow of traffic between pods and services.

2. How do you create a network policy in Kubernetes?
   - Define the network policy using a YAML manifest and apply it using `kubectl apply`.

3. What is the purpose of the `podSelector` field in a network policy?
   - The `podSelector` field is used to select the pods to which the network policy applies.

4. How can you test a network policy in Kubernetes?
   - Create pods with different labels and observe their communication behavior using `kubectl exec`.

5. What are some best practices for network policies?
   - Use labels to select pods, start with a default deny-all policy, regularly review and update policies, use network policy tools, and monitor network traffic.

## Answers

1. Network policies control the flow of traffic between pods and services.
2. Define the network policy using a YAML manifest and apply it using `kubectl apply`.
3. The `podSelector` field is used to select the pods to which the network policy applies.
4. Create pods with different labels and observe their communication behavior using `kubectl exec`.
5. Use labels to select pods, start with a default deny-all policy, regularly review and update policies, use network policy tools, and monitor network traffic.
