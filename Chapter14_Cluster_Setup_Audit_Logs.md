# Chapter 14: Cluster Setup - Audit Logs

## Introduction

In this chapter, we will explore the implementation and management of audit logs in a Kubernetes (K8s) cluster. Audit logs are crucial for tracking and monitoring activities within the cluster, providing valuable insights for security and compliance purposes. By the end of this chapter, you will understand how to configure, manage, and analyze audit logs to enhance the security and observability of your Kubernetes environment.

## Main Concepts

In this chapter, we'll cover the following topics:
- Overview of audit logs
- Configuring audit logs
- Managing audit logs
- Analyzing audit logs
- Best practices for audit logs

### Overview of Audit Logs

Audit logs are records of events that occur within a Kubernetes cluster. They capture information about actions taken by users, service accounts, and system components, providing a detailed history of activities. Audit logs are essential for detecting and investigating security incidents, ensuring compliance with regulatory requirements, and maintaining operational visibility.

### Configuring Audit Logs

To configure audit logs in a Kubernetes cluster, you need to define an audit policy and specify the location where the logs will be stored. Here is an example of how to configure audit logs:

1. Create an audit policy file (`audit-policy.yaml`):

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
  resources:
  - group: ""
    resources: ["pods"]
- level: RequestResponse
  resources:
  - group: "rbac.authorization.k8s.io"
    resources: ["roles", "rolebindings"]
- level: None
  users: ["system:kube-proxy"]
  verbs: ["watch"]
```

This policy captures metadata for pod-related events, request and response data for RBAC-related events, and excludes watch events from the kube-proxy user.

2. Update the API server configuration to use the audit policy:

```yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
apiServer:
  extraArgs:
    audit-policy-file: /etc/kubernetes/audit-policy.yaml
    audit-log-path: /var/log/kubernetes/audit.log
    audit-log-maxage: "30"
    audit-log-maxbackup: "10"
    audit-log-maxsize: "100"
```

3. Restart the API server to apply the changes:

```sh
sudo systemctl restart kube-apiserver
```

### Managing Audit Logs

Managing audit logs involves monitoring their size, rotation, and retention. Here are some best practices for managing audit logs:

- Configure log rotation to prevent audit logs from consuming excessive disk space.
- Set appropriate retention periods to ensure that logs are available for analysis when needed.
- Securely store audit logs to prevent unauthorized access and tampering.
- Regularly review and archive old audit logs to maintain a manageable log size.

### Analyzing Audit Logs

Analyzing audit logs is essential for detecting and investigating security incidents. Here are some tools and techniques for analyzing audit logs:

- Use `kubectl` commands to view and search audit logs:

```sh
kubectl logs -n kube-system kube-apiserver | grep "pods"
```

- Use log analysis tools like Elasticsearch, Fluentd, and Kibana (EFK stack) to collect, store, and visualize audit logs.
- Set up alerts and notifications for suspicious activities, such as unauthorized access attempts or changes to critical resources.
- Regularly review audit logs to identify patterns and trends that may indicate security issues.

### Best Practices for Audit Logs

- Define a comprehensive audit policy that captures relevant events and excludes unnecessary ones.
- Configure log rotation and retention to manage log size and ensure availability.
- Securely store audit logs to prevent unauthorized access and tampering.
- Use log analysis tools to collect, store, and visualize audit logs.
- Set up alerts and notifications for suspicious activities.
- Regularly review and analyze audit logs to detect and investigate security incidents.

## CKS-Specific Tips and Tricks

- Familiarize yourself with the syntax and structure of audit policy YAML files.
- Practice configuring and managing audit logs in a test environment.
- Understand how to use log analysis tools to collect, store, and visualize audit logs.
- Memorize key commands and concepts that are frequently tested in the exam.

## Summary

In this chapter, we covered the implementation and management of audit logs in a Kubernetes cluster, including an overview of audit logs, configuring and managing audit logs, analyzing audit logs, and best practices. By understanding how to use audit logs, you can enhance the security and observability of your Kubernetes environment.

## Mini-Quiz

1. What are audit logs used for in Kubernetes?
   - Audit logs are used to track and monitor activities within the cluster, providing valuable insights for security and compliance purposes.

2. How do you configure audit logs in Kubernetes?
   - Define an audit policy, update the API server configuration, and restart the API server.

3. What are some best practices for managing audit logs?
   - Configure log rotation and retention, securely store audit logs, regularly review and archive old logs.

4. How can you analyze audit logs in Kubernetes?
   - Use `kubectl` commands, log analysis tools like the EFK stack, set up alerts and notifications, regularly review logs.

5. Why is it important to use audit logs in Kubernetes?
   - To detect and investigate security incidents, ensure compliance with regulatory requirements, and maintain operational visibility.

## Answers

1. Audit logs are used to track and monitor activities within the cluster, providing valuable insights for security and compliance purposes.
2. Define an audit policy, update the API server configuration, and restart the API server.
3. Configure log rotation and retention, securely store audit logs, regularly review and archive old logs.
4. Use `kubectl` commands, log analysis tools like the EFK stack, set up alerts and notifications, regularly review logs.
5. To detect and investigate security incidents, ensure compliance with regulatory requirements, and maintain operational visibility.
