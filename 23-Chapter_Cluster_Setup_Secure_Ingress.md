# Chapter 23: Cluster Setup - Secure Ingress

## Introduction

In this chapter, we will explore the implementation and management of secure ingress in a Kubernetes (K8s) cluster. Secure ingress is essential for controlling and securing external access to services running within the cluster. By the end of this chapter, you will understand how to set up, configure, and manage secure ingress to enhance the security and accessibility of your Kubernetes environment.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of ingress
- Setting up ingress controllers
- Configuring secure ingress
- Managing ingress resources
- Best practices for secure ingress

### Overview of Ingress

Ingress is a Kubernetes resource that manages external access to services within a cluster, typically HTTP and HTTPS traffic. Ingress allows you to define rules for routing traffic to different services based on the request's host and path. Ingress controllers are responsible for implementing these rules and managing the ingress traffic.

### Setting up Ingress Controllers

To set up ingress in a Kubernetes cluster, you need to deploy an ingress controller. Here is an example of how to set up the NGINX ingress controller:

1. Deploy the NGINX ingress controller using a YAML manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-ingress-controller
  namespace: ingress-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-ingress
  template:
    metadata:
      labels:
        app: nginx-ingress
    spec:
      containers:
      - name: nginx-ingress-controller
        image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.32.0
        args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration
        env:
        - name: POD_NAME
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        ports:
        - name: http
          containerPort: 80
        - name: https
          containerPort: 443
```

2. Create a service to expose the ingress controller:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-ingress
  namespace: ingress-nginx
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
  - port: 443
    targetPort: 443
  selector:
    app: nginx-ingress
```

3. Apply the YAML manifests to deploy the ingress controller and service:

```sh
kubectl apply -f nginx-ingress-controller.yaml
kubectl apply -f nginx-ingress-service.yaml
```

### Configuring Secure Ingress

To configure secure ingress, you need to create ingress resources that define the routing rules and TLS settings. Here is an example of how to configure secure ingress for a service:

1. Create a TLS secret to store the SSL certificate and key:

```sh
kubectl create secret tls my-tls-secret --cert=path/to/tls.crt --key=path/to/tls.key
```

2. Create an ingress resource with TLS settings:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  namespace: default
spec:
  tls:
  - hosts:
    - example.com
    secretName: my-tls-secret
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

3. Apply the ingress resource:

```sh
kubectl apply -f my-ingress.yaml
```

### Managing Ingress Resources

Managing ingress resources involves monitoring and maintaining the ingress rules and configurations. Here are some best practices for managing ingress resources:

- Regularly review and update ingress rules to ensure they meet your security and accessibility requirements.
- Use TLS to encrypt traffic and secure communication between clients and services.
- Monitor ingress traffic to detect and respond to potential security incidents.
- Use annotations and labels to organize and manage ingress resources.

### Best Practices for Secure Ingress

- Use ingress controllers to manage external access to services.
- Configure TLS to encrypt traffic and secure communication.
- Regularly review and update ingress rules to ensure they meet your security and accessibility requirements.
- Monitor ingress traffic to detect and respond to potential security incidents.
- Use annotations and labels to organize and manage ingress resources.
- Implement rate limiting and access control to protect services from abuse and unauthorized access.

## CKS-Specific Tips and Tricks

- Familiarize yourself with the setup and configuration of ingress controllers like NGINX.
- Practice creating and applying ingress resources with TLS settings in a test environment.
- Understand how to use annotations and labels to manage ingress resources.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the implementation and management of secure ingress in a Kubernetes cluster, including an overview of ingress, setting up ingress controllers, configuring secure ingress, managing ingress resources, and best practices. By understanding how to use secure ingress, you can enhance the security and accessibility of your Kubernetes environment.

## Mini-Quiz

1. What is ingress used for in Kubernetes?
   - Ingress manages external access to services within a cluster, typically HTTP and HTTPS traffic.

2. How do you set up an ingress controller in Kubernetes?
   - Deploy an ingress controller using a YAML manifest and create a service to expose the ingress controller.

3. How do you configure secure ingress in Kubernetes?
   - Create ingress resources with TLS settings and define routing rules.

4. What are some best practices for managing ingress resources?
   - Regularly review and update ingress rules, use TLS to encrypt traffic, monitor ingress traffic, use annotations and labels to organize resources.

5. Why is it important to use secure ingress in Kubernetes?
   - To control and secure external access to services, ensuring secure communication and protecting services from unauthorized access.

## Answers

1. Ingress manages external access to services within a cluster, typically HTTP and HTTPS traffic.
2. Deploy an ingress controller using a YAML manifest and create a service to expose the ingress controller.
3. Create ingress resources with TLS settings and define routing rules.
4. Regularly review and update ingress rules, use TLS to encrypt traffic, monitor ingress traffic, use annotations and labels to organize resources.
5. To control and secure external access to services, ensuring secure communication and protecting services from unauthorized access.
