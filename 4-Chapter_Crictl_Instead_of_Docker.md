# Chapter 4: Crictl Instead of Docker

## Introduction

In this chapter, we will explore the use of `crictl` as an alternative to Docker for managing containers in Kubernetes. With the deprecation of Docker as a container runtime in Kubernetes, it is essential to understand how to use `crictl` to interact with container runtimes that comply with the Container Runtime Interface (CRI). By the end of this chapter, you will be familiar with `crictl` commands and how to use them effectively.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of `crictl`
- Installing `crictl`
- Basic `crictl` commands
- Using `crictl` with different container runtimes
- Practical examples

### Overview of `crictl`

`crictl` is a command-line interface for interacting with container runtimes that implement the Kubernetes Container Runtime Interface (CRI). It provides a consistent way to manage containers, images, and other resources, regardless of the underlying container runtime.

### Installing `crictl`

To install `crictl`, follow these steps:

1. Download the latest release of `crictl`:
   ```sh
   VERSION="v1.24.0"
   curl -LO https://github.com/kubernetes-sigs/cri-tools/releases/download/$VERSION/crictl-$VERSION-linux-amd64.tar.gz
   ```

2. Extract the binary and move it to a directory in your PATH:
   ```sh
   tar -C /usr/local/bin -xzf crictl-$VERSION-linux-amd64.tar.gz
   ```

3. Verify the installation:
   ```sh
   crictl --version
   ```

### Basic `crictl` Commands

Here are some basic `crictl` commands that you should be familiar with:

- List all containers:
  ```sh
  crictl ps
  ```

- List all images:
  ```sh
  crictl images
  ```

- Pull an image:
  ```sh
  crictl pull <image>
  ```

- Run a container:
  ```sh
  crictl run <pod-config> <container-config>
  ```

- Stop a container:
  ```sh
  crictl stop <container-id>
  ```

- Remove a container:
  ```sh
  crictl rm <container-id>
  ```

### Using `crictl` with Different Container Runtimes

`crictl` can be used with various container runtimes that support the CRI, such as containerd and CRI-O. To configure `crictl` to use a specific runtime, you need to create a configuration file:

1. Create a configuration file at `/etc/crictl.yaml`:
   ```yaml
   runtime-endpoint: unix:///var/run/containerd/containerd.sock
   ```

2. Update the `runtime-endpoint` to point to the socket file of your container runtime. For example, for CRI-O:
   ```yaml
   runtime-endpoint: unix:///var/run/crio/crio.sock
   ```

### Practical Examples

#### Example 1: Pulling and Running a Container

1. Pull an image:
   ```sh
   crictl pull nginx:latest
   ```

2. Create a pod configuration file (`pod-config.json`):
   ```json
   {
     "metadata": {
       "name": "nginx-pod",
       "namespace": "default",
       "attempt": 1
     },
     "logDirectory": "/var/log/pods",
     "dnsConfig": {}
   }
   ```

3. Create a container configuration file (`container-config.json`):
   ```json
   {
     "metadata": {
       "name": "nginx-container",
       "attempt": 1
     },
     "image": {
       "image": "nginx:latest"
     },
     "command": [
       "nginx",
       "-g",
       "daemon off;"
     ],
     "logPath": "nginx.log",
     "stdin": true,
     "stdinOnce": true,
     "tty": true
   }
   ```

4. Run the container:
   ```sh
   crictl run pod-config.json container-config.json
   ```

#### Example 2: Stopping and Removing a Container

1. List all running containers and get the container ID:
   ```sh
   crictl ps
   ```

2. Stop the container:
   ```sh
   crictl stop <container-id>
   ```

3. Remove the container:
   ```sh
   crictl rm <container-id>
   ```

## CKS-Specific Tips and Tricks

- Familiarize yourself with the `crictl` commands and their usage.
- Practice using `crictl` to manage containers and images.
- Understand how to configure `crictl` to work with different container runtimes.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the use of `crictl` as an alternative to Docker for managing containers in Kubernetes. We discussed the installation of `crictl`, basic commands, using `crictl` with different container runtimes, and provided practical examples. By mastering `crictl`, you will be well-prepared to manage containers in a Kubernetes environment.

## Mini-Quiz

1. What is `crictl` used for in Kubernetes?
   - `crictl` is a command-line interface for interacting with container runtimes that implement the Kubernetes Container Runtime Interface (CRI).

2. How do you install `crictl`?
   - Download the latest release, extract the binary, and move it to a directory in your PATH.

3. How can you list all running containers using `crictl`?
   - Run the command `crictl ps`.

4. How do you configure `crictl` to use a specific container runtime?
   - Create a configuration file at `/etc/crictl.yaml` and update the `runtime-endpoint` to point to the socket file of your container runtime.

5. What command is used to pull an image using `crictl`?
   - `crictl pull <image>`.

## Answers

1. `crictl` is a command-line interface for interacting with container runtimes that implement the Kubernetes Container Runtime Interface (CRI).
2. Download the latest release, extract the binary, and move it to a directory in your PATH.
3. Run the command `crictl ps`.
4. Create a configuration file at `/etc/crictl.yaml` and update the `runtime-endpoint` to point to the socket file of your container runtime.
5. `crictl pull <image>`.
