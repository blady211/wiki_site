# Project 1 — Kubernetes Private Registry + Ansible + App Deployment

## Overview

This project builds a complete container delivery pipeline:

Docker → Private Registry → Kubernetes → Web App → GitHub Markdown UI

It simulates a real DevOps workflow with mixed Linux nodes (Rocky + Ubuntu).

---

# Architecture

Mac (kubectl)
   ↓
Kubernetes Cluster
   ↓
Nodes: Rocky + Ubuntu
   ↓
containerd runtime
   ↓
Private Registry (192.168.0.75:5000)
   ↓
Images: k8s-runtime:X.Y

---

# Goals

- Configure private Docker registry
- Configure containerd to trust HTTP registry
- Deploy application in Kubernetes
- Version images and rollout updates
- Automate registry config with Ansible
- Build knowledge hub web UI
- Load Markdown content from GitHub

---

# Environment

## Nodes

- rocky-ansible (control-plane)
- rocky-node
- rocky-node1
- ubuntu-node
- registry-node

## Kubernetes

- Version: 1.29
- Runtime: containerd
- Network: flannel
- Service: NodePort

---

# Registry Setup

Registry container:

```bash
docker run -d -p 5000:5000 --name registry registry:2
