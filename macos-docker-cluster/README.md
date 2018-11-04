
# Run a local kubernetes cluster on your MacBook

This is a quickstart guide to using kubernetes locally on your Mac.  Following  this guide will yield a lightweight (fast) kuberenetes cluster for local development, fun and profit.  

> I highly recommend it as an alternative to minikube.
> - _me_

## Why not minikube

I find minikube to be a bit overkill on a laptop, and don't really like the user experience.  It exposes to me (as a developer) a different interface/workflow/command line than a production cluster. Why use/learn/entertain a divergent set of commands?  I also find this to be much faster and architecturally more sound and futureproof.  The full rationale is beyond the scope of this quickstart. If interested to learn more and/or dig deeper the following is a good breadcrumb trail.

### History / Context

_(taken from [mobyproject.org post](https://blog.mobyproject.org/moby-and-kubernetes-bf888ab31e38))_

![containerd](img/containerd-timeline.png)

- <https://blog.docker.com/2017/10/docker-for-mac-and-windows-with-kubernetes-beta>
- <https://blog.docker.com/2017/10/kubernetes-docker-platform-and-moby-project>
- <https://blog.docker.com/2018/01/docker-mac-kubernetes>
- <https://blog.mobyproject.org/moby-and-kubernetes-bf888ab31e38>
- <https://blog.docker.com/2018/07/kubernetes-is-now-available-in-docker-desktop-stable-channel>

### The now (and future): K8s and containerd, CRI

_(taken from [Liu Lantao's talk](https://www.slideshare.net/Docker/kubernetes-cri-containerd-integration-by-lantao-liu-google))_

![liu-lantao-slide](img/liu-lantao-cri-containerd-architecture-slide.png)

- <https://mobyproject.org/kubernetes>
- <https://www.slideshare.net/Docker/kubernetes-cri-containerd-integration-by-lantao-liu-google>
- <https://github.com/containerd/containerd>
- <https://github.com/containerd/cri>

## Enable Kubernetes Support

This guide assumes you're already running Docker CE for Mac.

![dockerce_version](img/dockerce-version.png)

Enable Kubernetes support.  I prefer to allow the containers being leveraged to appear via normal docker commands.  

![dockerce_enable-k8s](img/dockerce-k8s-tab.png)

Depending on your use cases you might want to bump memory/disk, or setup additional storage.

![dockerce_resources](img/dockerce-resources.png)

[dashboard wiki]: https://github.com/kubernetes/dashboard/wiki

## Set your context

```bash
kubectl config use-context docker-for-desktop
```

## deploy the kubernetes dashboard

The official [dashboard wiki] is the source of truth.  There's a nice issue / thread ([kubernetes/dashboard: 2474](https://github.com/kubernetes/dashboard/issues/2474)for some background if you're really curious about why your normal kubeconfig file will likely not work.  TLDR: you need a token, not a cert.

Deploy directly from github HEAD...

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```

or

```bash
git clone git@github.com:kubernetes/dashboard.git && cd dashboard
kubectl create -f src/deploy/recommended/kubernetes-dashboard.yaml
```

Example:

```shell
(⎈ |docker-for-desktop:default)halcyondude@chezmoi:~/k8s$ kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
secret/kubernetes-dashboard-certs created
serviceaccount/kubernetes-dashboard created
role.rbac.authorization.k8s.io/kubernetes-dashboard-minimal created
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard-minimal created
deployment.apps/kubernetes-dashboard created
service/kubernetes-dashboard created
```

## access dashboard

There are a number of options and approaches regarding security that are out of scope for this quickstart.

(TODO: expand)