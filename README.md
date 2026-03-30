# Multi-Node Kubernetes Cluster on Ubuntu 24.04

## Project Overview
This repository documents the successful deployment of a 2-node Kubernetes cluster built on Ubuntu 24.04 (Noble Numbat). The project demonstrates core Systems Administration skills, including container runtime configuration, Linux kernel tuning, and multi-node networking.

## Architecture & Infrastructure
* **Control Plane (Master):** 192.168.58.130
* **Worker Node:** 192.168.58.131
* **Runtime:** Containerd (SystemdCgroup driver)
* **Networking:** Flannel CNI

### 1. Cluster Health Status
Verification of the control plane and worker node in a 'Ready' state using `kubectl get nodes`.
![Cluster Success](./images/k8s-ubuntu2404-cluster-success.png)

---
## Application Delivery (Nginx)
I deployed a 4-replica Nginx "Web Farm" to test the cluster's orchestration and load-balancing capabilities.

### 2. Service Configuration & NodePort
The deployment was exposed via a NodePort Service, mapping the internal container port to an external port (**32006**) for host-machine access.
![Service Success](./images/k8s-nginx-service-success.png)

### 3. Browser Validation (End-to-End Test)
This final validation proves that the networking, firewall rules, and pod routing are functioning correctly. The Nginx welcome page is reachable from the external browser.
![Browser Validation](./images/k8s-nginx-browser-validation.png)

---

## 🔧 Troubleshooting Log
* **Cgroup Mismatch:** Resolved a `CrashLoopBackOff` by reconfiguring `containerd` to use the `SystemdCgroup` driver to align with Ubuntu 24.04.
* **Kernel Stability:** Configured permanent swap disablement and loaded `br_netfilter` modules to ensure networking persistence across reboots.
