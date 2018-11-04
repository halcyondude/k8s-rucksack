# Portainer

Portainer a a lightweight web UI that surfaces stats/info about docker images, containers, networks, and other basic info from docker instances (local or remote).  It's handy as an alternative to raw docker commands.

## RTFM Links

* [portainer docs]
* [portainer github]
* [portainer install]

[portainer docs]: https://portainer.readthedocs.io/en/latest/deployment.html
[portainer github]: https://github.com/portainer/portainer
[portainer install]: https://portainer.io/install.html

## Portainer Quickstart (mac, because `open`)

```bash
docker volume create portainer_data
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
open http://localhost:9000
```

or to manage a remote docker endpoint...

```bash
docker run -d -p 9000:9000 --name portainer --restart always -v portainer_data:/data portainer/portainer -H tcp://<REMOTE_HOST>:<REMOTE_PORT>
```