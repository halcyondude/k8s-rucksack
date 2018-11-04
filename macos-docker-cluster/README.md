
# Run a local kubernetes cluster on MacOS

The official [dashboard wiki] is the source of truth.  There's a nice issue / thread ([kubernetes/dashboard: 2474](https://github.com/kubernetes/dashboard/issues/2474)for some background if you're really curious about why your normal kubeconfig file will likely not work.  TLDR: you need a token, not a cert.

[dashboard wiki]: https://github.com/kubernetes/dashboard/wiki

## Set your context

```bash
# kctx docker-for-desktop
kubectl config use-context docker-for-desktop
```

## deploy the kubernetes dashboard

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

