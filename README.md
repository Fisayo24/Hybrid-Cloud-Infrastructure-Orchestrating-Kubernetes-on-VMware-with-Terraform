# Multi-Node Kubernetes Cluster on Ubuntu 24.04
## 
This project demonstrates the manual bootstrap of a 2-node Kubernetes cluster using **Kubeadm** on Ubuntu 24.04 (Noble Numbat). It features a High-Availability (HA) Nginx deployment exposed via a NodePort Service.

## Architecture
* **Control Plane (Master):** 192.168.58.130 (Ubuntu 24.04)
* **Worker Node:** 192.168.58.131 (Ubuntu 24.04)
* **Runtime:** Containerd with SystemdCgroup driver
* **Networking:** Flannel CNI (Pod Network: 10.244.0.0/16)

## Technical Deep-Dive & Troubleshooting
During the initialization, I encountered and resolved several critical "Day 0" issues:

1. **Cgroup Driver Alignment:** Identified a `CrashLoopBackOff` in the API Server caused by a mismatch between the Kubelet and Containerd. I Resolved this by manually reconfiguring `config.toml` to use the `SystemdCgroup` driver.
2. **Kernel Hardening:** Permanently disabled swap via `/etc/fstab` and configured kernel modules (`overlay`, `br_netfilter`) to ensure networking persistence across reboots.
3. **Service Discovery:** Implemented a **NodePort Service** to load-balance traffic across 4 Nginx replicas, ensuring the application remained reachable even during pod restarts.

## How to Run
1. Initialize the cluster: `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`
2. Deploy the web farm: `kubectl create deployment nginx-ha --image=nginx --replicas=4`
3. Expose the service: `kubectl expose deployment nginx-ha --type=NodePort --port=80`

## 📊 Visual Validation
### 1. Successful <img width="960" height="504" alt="k8s-nginx-browser-validation" src="https://github.com/user-attachments/assets/957b6fe3-ad30-4180-9775-1df42e090138" />
Application Delivery
(./images/k8s-nginx-browser-validation.png)
*The Nginx welcome page is accessible via the Worker IP on Port 32006.*

### 2. Cluster Health Status
(./images/k8s-ubuntu2404-cluster-success.png)<img width="960" height="504" alt="k8s-ubuntu2404-cluster-success" src="https://github.com/user-attachments/assets/a6a852f0-d7e7-4d32-8257-5b8dda82d67c" />

*Verification of all nodes in a 'Ready' state with the API Server listening on 6443.*
