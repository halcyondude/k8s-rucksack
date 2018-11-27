# Kail (Kubernetes Tail)

## What is this

> Kubernetes tail. Streams logs from all containers of all matched pods. Match
> pods by service, replicaset, deployment, and others. Adjusts to a changing
> cluster - pods are added and removed from logging as they fall in or out of 
> the selection.

<https://github.com/boz/kail>

The project readme / docs are complete and easy to follow.

## TL;DR

```bash
brew tap boz/repo
brew install boz/repo/kail
```

## simple example

```bash
(⎈ |docker-for-desktop:kube-system)~ ᐅ k get deploy
NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
kube-dns               1         1         1            1           22d
kubernetes-dashboard   1         1         1            1           22d

(⎈ |docker-for-desktop:kube-system)~ ᐅ kail --deploy kubernetes-dashboard
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:15 Getting application global configuration
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:15 Application configuration {"serverTime":1543348875532}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Incoming HTTP/2.0 GET /api/v1/settings/global request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Incoming HTTP/2.0 GET /api/v1/systembanner request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Incoming HTTP/2.0 GET /api/v1/login/status request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Incoming HTTP/2.0 GET /api/v1/rbac/status request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Incoming HTTP/2.0 GET /api/v1/login/modes request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Incoming HTTP/2.0 GET /api/v1/login/skippable request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:16 [2018-11-27T20:01:16Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 [2018-11-27T20:01:19Z] Incoming HTTP/2.0 GET /api/v1/login/status request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 [2018-11-27T20:01:19Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 [2018-11-27T20:01:19Z] Incoming HTTP/2.0 GET /api/v1/login/status request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 [2018-11-27T20:01:19Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 [2018-11-27T20:01:19Z] Incoming HTTP/2.0 GET /api/v1/overview/default?filterBy=&itemsPerPage=10&name=&page=1&sortBy=d,creationTimestamp request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 Getting config category
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 Getting discovery and load balancing category
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 Getting lists of all workloads
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 No metric client provided. Skipping metrics.
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 Getting pod metrics
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 No metric client provided. Skipping metrics.
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:19 [2018-11-27T20:01:19Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:20 [2018-11-27T20:01:20Z] Incoming HTTP/2.0 GET /api/v1/systembanner request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:20 [2018-11-27T20:01:20Z] Incoming HTTP/2.0 GET /api/v1/login/status request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:20 [2018-11-27T20:01:20Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:20 [2018-11-27T20:01:20Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:20 [2018-11-27T20:01:20Z] Incoming HTTP/2.0 GET /api/v1/rbac/status request from 10.1.0.1:45960: {}
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:20 [2018-11-27T20:01:20Z] Outcoming response to 10.1.0.1:45960 with 200 status code
kube-system/kubernetes-dashboard-7b9c7bc8c9-7gh92[kubernetes-dashboard]: 2018/11/27 20:01:35 Metric client health check failed: the server could not find the requested resource (get services heapster). Retrying in 30 seconds.
```