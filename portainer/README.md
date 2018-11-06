# Portainer

Portainer a a lightweight web UI that surfaces stats and info about docker images, containers, networks, and other basic info from docker instances (local or remote).

It's handy as an alternative to raw docker commands.

* [portainer github] ***(start here, live demo)***
* [portainer docs]
* [portainer install]

[portainer docs]: https://portainer.readthedocs.io/en/latest/deployment.html
[portainer github]: https://github.com/portainer/portainer
[portainer install]: https://portainer.io/install.html

## Portainer Quickstart

```bash

docker volume create portainer_data
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer

# (macos specific)
open http://localhost:9000

```

or to manage a remote docker endpoint...

```bash

docker run -d -p 9000:9000 --name portainer --restart always -v portainer_data:/data portainer/portainer -H tcp://<REMOTE_HOST>:<REMOTE_PORT>

```