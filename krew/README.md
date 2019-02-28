# Krew

## What is this

<https://github.com/GoogleContainerTools/krew#what-is-krew>

> krew is a tool that makes it easy to use [kubectl
> plugins](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/). krew
> helps you discover plugins, install and manage them on your machine. It is
> similar to tools like apt, dnf or [brew](http://brew.sh).

## TL;DR

```shell
kubectl krew search               # show all plugins
kubectl krew install view-secret  # install a plugin named "view-secret"
kubectl view-secret               # use the plugin
kubectl krew upgrade              # upgrade installed plugins
kubectl krew remove view-secret   # uninstall a plugin
```

## install on mac...

```bash
(
  set -x; cd "$(mktemp -d)" &&
  curl -fsSLO "https://storage.googleapis.com/krew/v0.2.1/krew.{tar.gz,yaml}" &&
  tar zxvf krew.tar.gz &&
  ./krew-"$(uname | tr '[:upper:]' '[:lower:]')_amd64" install \
    --manifest=krew.yaml --archive=krew.tar.gz
)
```

add to .bashrc / .zshrc

```bash
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
```
