---
layout: default
title: Kubernetes Architecture
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-archi/
nav_order: 10
---

# Kubernetes Architectur
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
Operating management in a few host of distributed computing environment is not too difficult even by hand. However in case of large distributed computing environment, it is very hard to do management such as start/stop containers, storage management, health checking and so on.

## Understanding of Kubernetes


Kubernetes can be a solution to overcome the problem. The followings are the summary of Kubernetes.

Container Orchestration
- Large containers are controlled under the distributed computing environment.
- Distribute the containers to lots of servers.
- Connet the services with service discovery.
- Increase the number of server under overload.
- Create new containers in case of failure.

Also large distributed environment can be handled by a few of engineers.
It is also referred to K8s since Kubernetes has long characters to speak.
- Three philosophy of Kubernetes
- Immutable Infrastructure
- Declarative Configuration
- Self Healing

## Kubernethes Structure
It is communicated with API server and repository (etcd) in Maser, and agent(kubelet) of each node.

![](/assets/images/k8s-archi2.png)

 

Master
- Manage K8s cluster
- Trace node status
- Select which node the container allocated
- Save cluster information on etcd repository

API server
- Handle all requests
- Communicate all components through API server
- Authentication and Permission

Scheduler
- Define which node the container created and run in
- Node selection by understanding status of nodes
- Controller Manager
- Check cluster status
- Real time check if any difference between current and requested information

etcd
- Distributed key-value repository
- Manage all setting value and status data
- Inquiry and update by scheduler and controller manager

Node
- Server which runs containers
- Configures an cluster with multiple nodes
- In case of cloud, the node is virtual machine like ec2.
- kubelet
- Create containers requested by master
- Send status of containers

kube proxy
- Network proxy executed by each node

## Kubernethes Resource
K8s has abstracted some element such as container, application and network as following.
- Application and Distribution - Pod, ReplicaSet, Deployment, DaemonSet, StatefulSet
- Network - Service, Ingress
- Setting management - ConfigMap, Secrets 

 

## Kubernethes Architecture
This is Kubernethes architecture, it is helpful to understand overall structure of K8s.


![](/assets/images/k8s-arch1.jpg)

*[Kubernetes Architecture]* 
{: .text-center }








