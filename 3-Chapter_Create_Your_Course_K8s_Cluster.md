# Chapter 3: Create Your Course K8s Cluster

## Introduction

In this chapter, we will guide you through the process of creating a Kubernetes (K8s) cluster that you will use throughout this course. Setting up your own cluster is essential for practicing the hands-on exercises and examples provided in this book. By the end of this chapter, you will have a fully functional Kubernetes cluster ready for use.

## Main Concepts

In this chapter, we'll cover the following topics:
- Choosing a Kubernetes distribution
- Setting up a local Kubernetes cluster
- Configuring kubectl
- Verifying the cluster setup

### Choosing a Kubernetes Distribution

There are several Kubernetes distributions available, each with its own features and benefits. Some popular options include:
- Minikube: A lightweight Kubernetes distribution that runs on your local machine.
- Kind (Kubernetes IN Docker): A tool for running local Kubernetes clusters using Docker containers.
- K3s: A lightweight Kubernetes distribution designed for resource-constrained environments.

For this course, we recommend using Minikube or Kind, as they are easy to set up and provide a good learning experience.

### Setting up a Local Kubernetes Cluster

#### Using Minikube

1. Install Minikube:
   ```sh
   curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   chmod +x minikube
   sudo mv minikube /usr/local/bin/
   ```

2. Start Minikube:
   ```sh
   minikube start
   ```

#### Using Kind

1. Install Kind:
   ```sh
   curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
   chmod +x ./kind
   sudo mv ./kind /usr/local/bin/kind
   ```

2. Create a Kind cluster:
   ```sh
   kind create cluster
   ```

### Configuring kubectl

kubectl is the command-line tool for interacting with your Kubernetes cluster. To configure kubectl:

1. Install kubectl:
   ```sh
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x kubectl
   sudo mv kubectl /usr/local/bin/
   ```

2. Verify the installation:
   ```sh
   kubectl version --client
   ```

3. Configure kubectl to use your cluster:
   - For Minikube:
     ```sh
     kubectl config use-context minikube
     ```
   - For Kind:
     ```sh
     kubectl cluster-info --context kind-kind
     ```

### Verifying the Cluster Setup

To ensure your cluster is set up correctly, run the following commands:

1. Check the cluster nodes:
   ```sh
   kubectl get nodes
   ```

2. Deploy a test application:
   ```sh
   kubectl create deployment hello-world --image=k8s.gcr.io/echoserver:1.4
   kubectl expose deployment hello-world --type=NodePort --port=8080
   ```

3. Access the test application:
   - For Minikube:
     ```sh
     minikube service hello-world
     ```
   - For Kind:
     ```sh
     kubectl port-forward service/hello-world 8080:8080
     ```

## CKS-Specific Tips and Tricks

- Familiarize yourself with the process of setting up and configuring a Kubernetes cluster.
- Practice using kubectl commands to interact with your cluster.
- Focus on understanding the different components and configurations of a Kubernetes cluster.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the process of creating a Kubernetes cluster using Minikube or Kind, configuring kubectl, and verifying the cluster setup. By following these steps, you will have a fully functional Kubernetes cluster ready for use throughout this course.

## Mini-Quiz

1. What are some popular Kubernetes distributions for local development?
   - Minikube, Kind, K3s.

2. How do you start a Minikube cluster?
   - Run the command `minikube start`.

3. How do you create a Kind cluster?
   - Run the command `kind create cluster`.

4. What is kubectl used for in Kubernetes?
   - kubectl is the command-line tool for interacting with your Kubernetes cluster.

5. How can you verify that your Kubernetes cluster is set up correctly?
   - Check the cluster nodes with `kubectl get nodes` and deploy a test application.

## Answers

1. Minikube, Kind, K3s.
2. Run the command `minikube start`.
3. Run the command `kind create cluster`.
4. kubectl is the command-line tool for interacting with your Kubernetes cluster.
5. Check the cluster nodes with `kubectl get nodes` and deploy a test application.
