![kube-vip.png](kube-vip.png)

## Overview

Kube-Vip provides Kubernetes clusters with a virtual IP and load balancer for both the control plane (for building a highly-available cluster) and Kubernetes Services of type `LoadBalancer` without relying on any external hardware or software.

Current solutions for building a highly-available Kubernetes cluster, when not in the cloud, often involve external hardware, which can be expensive, or external software, which can be complex and difficult to manage. For Kubernetes Service resources of type `LoadBalancer`, you are once again on your own if not in a PaaS environment. Kube-Vip addresses both of these by packaging them together and running them inside the same Kubernetes cluster they service, simplifying complexity, reducing cost as well as build times.

The idea behind `kube-vip` is a small, self-contained, highly-available option for all environments, especially:

- Bare metal
- On-Premises
- Edge (ARM / Raspberry Pi)
- Virtualisation
- Pretty much anywhere else :)

## Features

Kube-Vip was originally created to provide a HA solution for the Kubernetes control plane, but over time it has evolved to incorporate that same functionality for Kubernetes Services of type [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer). Some of the features include:

- VIP addresses can be IPv4, IPv6, or DNS name
- Control plane load balancing:
  - Floating IP in ARP (Layer 2) or BGP (Layer 3) modes
  - Uses [leader election](https://godoc.org/k8s.io/client-go/tools/leaderelection)
  - Support for `kubeadm`-provisioned clusters (via static Pods)
  - Support for K3s and others (via DaemonSets)
  - [IPVS](https://en.wikipedia.org/wiki/IP_Virtual_Server) mode for true load balancing (kube-vip ≥ 0.4)
- Service of type `LoadBalancer`:
  - Uses [leader election](https://godoc.org/k8s.io/client-go/tools/leaderelection) for ARP (Layer 2)
  - Multiple nodes with BGP
  - Address pools scoped per Namespace or global
  - Addresses from CIDR blocks or IP ranges
  - DHCP support
  - Exposure to gateways via UPnP
- Automated manifest generation, vendor API integrations, and much more...

## Why?

The "original" purpose of `kube-vip` was to simplify the building of highly-available (HA) Kubernetes clusters, which at the time involved a few components and configurations that all needed to be managed. This was blogged about in detail by [thebsdbox](https://twitter.com/thebsdbox/) [here](https://thebsdbox.co.uk/2020/01/02/Designing-Building-HA-bare-metal-Kubernetes-cluster/#Networking-load-balancing). Since the project has evolved, it can now use those same technologies to provide load balancing capabilities for Kubernetes Service resources of type `LoadBalancer`.

## Architecture

The architecture for `kube-vip` (and associated Kubernetes components) is covered in detail [here](/architecture/).

## Installation

There are two main routes for deploying `kube-vip`: either through a [static Pod](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/) when bringing up a Kubernetes cluster with [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) or as a [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) (typically with distributions like [K3s](https://k3s.io)).

- [Static Pod](/install_static)
- [DaemonSet](/install_daemonset)

## Usage

- [On-Prem with the kube-vip cloud controller](/usage/on-prem)
- [KinD](/usage/kind)
- [Equinix Metal](/usage/EquinixMetal)
- [k3s](/usage/k3s)

## Flags/Environment Variables

- [Flags and Environment variables](/flags/)

## Links

- [Kube-Vip Cloud Provider Repository](https://github.com/kube-vip/kube-vip-cloud-provider)
- [Kube-Vip Repository](https://github.com/kube-vip/kube-vip)
- [Kube-Vip RBAC manifest (required for the DaemonSet)](https://kube-vip.io/manifests/rbac.yaml)

## Copyright

© 2021 [The Linux Foundation](https://www.linuxfoundation.org/). All rights reserved.

The Linux Foundation has registered trademarks and uses trademarks.

For a list trademarks of The Linux Foundation, please see our [Trademark Usage page](https://www.linuxfoundation.org/en/trademark-usage).
