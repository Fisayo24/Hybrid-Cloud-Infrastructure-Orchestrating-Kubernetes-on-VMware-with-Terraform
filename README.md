# Multi-Node Kubernetes Cluster on Ubuntu 24.04

## Project Overview
This repository documents the successful deployment of a 2-node Kubernetes cluster built on Ubuntu 24.04 (Noble Numbat). The project demonstrates core Systems Administration skills, including container runtime configuration, Linux kernel tuning, and multi-node networking.

## Architecture & Infrastructure
* **Control Plane (Master):** 192.168.58.130
* **Worker Node:** 192.168.58.131
* **Runtime:** Containerd (SystemdCgroup driver)
* **Networking:** Flannel CNI

### 1. Cluster Health Status
Verification of the control plane and worker <img width="960" height="504" alt="k8s-ubuntu2404-cluster-success" src="https://github.com/user-attachments/assets/49be5cf8-4092-4f53-9629-72d86c797dab" />
node in a 'Ready' state using `kubectl get nodes`.
![Cluster Success]![Success](./images/k8s-ubuntu2404-cluster-success.png)

---
## Application Delivery (Nginx)
I deployed a 4-replica Nginx "Web Farm" to test the cluster's orchestration and load-balancing capabilities.

### 2. Service Configuration & NodePort
The deployment was exposed via a NodePort Service, mapping the internal container port to an external port (**32006**) for host-machine access.
![Service Success](./images/k8s<img width="960" height="504" alt="k8s-service-deployment-verification" src="https://github.com/user-attachments/assets/4393c1ae-48d8-47ac-8e3c-3ac07fc81ddb" />
-nginx-service-success.png)

### 3. Browser Validation (End-to-End Test)
This final validation proves that the networking, firewall rules, and pod routing are functioning correctly. The Nginx welcome page is reachable from the external browser.
![Browser Validation](./images/k<img width="960" height="504" alt="k8s-nginx-browser-validation png" src="https://github.com/user-attachments/assets/45feaf4e-32e7-4cf1-b826-b9a9ed74952a" />
8s-nginx-browser-validation.png)

---

## Troubleshooting Log
* **Cgroup Mismatch:** Resolved a `CrashLoopBackOff` by reconfiguring `containerd` to use the `SystemdCgroup` driver to align with Ubuntu 24.04.
* **Kernel Stability:** Configured permanent swap disablement and loaded `br_netfilter` modules to ensure networking persistence across reboots.
