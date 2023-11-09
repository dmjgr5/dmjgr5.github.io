---
layout: default
title: Replicaset
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-replicaset/
nav_order: 15
---

# Replicaset
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---



Replicatset 에 관해 설명한다.

## YAML 포맷

- `kind : Replicaset`

- `spec > selector > matchLabels`

  - 여기에 정의된 라벨을 가진 파드에 대해서 먼저 확인한다.

- `spec > template > metadata > labels`

  - 실행할 파드의 라벨을 정한다.

- `spec > replicas`

  - 실행할 파드의 개수를 정한다.

## Replica 생성

### `kubectl apply -f deploy-nginx.yaml`

## Replica 변경

### `kubectl scale rs replicaset-nginx --replicas=4`

## Replica 삭제

`kubectl delete rs replicaset-nginx`
  - 레플리카셋 + 파드들 삭제

`kubectl delete rs replicaset-nginx --cascade=orphan`
  - 레플리카셋만 삭제된다.

```bash
terminal 2 # watch -n 1 kubectl get rs,pods -o wide

# cd /root/labfile/controller/replicaset
# cat rs-nginx.yaml 

# kubectl apply -f rs-nginx.yaml
# kubectl get pods --show-labels
# kubectl delete pod replicaset-nginx-p9bqp
        -->  레플리카셋에 의해서 생성된 파드 이름 확인 후 진행

# kubectl scale rs replicaset-nginx --replicas=4
# kubectl scale rs replicaset-nginx --replicas=3

# kubectl delete rs replicaset-nginx
    --> 레플리카셋 + 파드들 삭제

# kubectl apply -f rs-nginx.yaml
# kubectl delete rs replicaset-nginx --cascade=orphan

# kubectl apply -f rs-nginx.yaml
```

- `kubectl delete rs replicaset-nginx`

- `kubectl delete rs --all`



 
{: .important-title }
> 실습 정리 
>
> `kubectl delete rs replicaset-nginx`
> `kubectl delete rs --all`