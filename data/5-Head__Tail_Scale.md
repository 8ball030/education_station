---
date: 05-01-24
id: 5
modified_at: 05/02/24 07:52
path: ''
tags: []
title: Head + Tail Scale
type: note
---

# Head + Tailscale
Used to allow for quick and easy management for for vpns.

First need to create a server.



at present make sure you have

- docker
- docker-compose
```bash
docker run \
                                   --name headscale \
                                   --detach \
                                   --volume $(pwd)/config:/etc/headscale/ \
                                   --publish 0.0.0.0:8080:8080 \
                                   --publish 0.0.0.0:9090:9090 \
                                   headscale/headscale:v0.23.0-alpha10 \
                                   serve
```

```bash
git clone REPO
cd REPO
docker compose up
```

The on a client, we are able to connecto to the network as so;


## Client

### Install

We first need to install tailscale.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```
Once We have tailsale


We make a user;

```bash
docker exec headscale \
    headscale users create eightballer
```

```bash
sudo tailscale up --login-server http://rae.cloud:8080

```
This prints out a token which must be passed to the tailscale server as so;

```
 docker exec headscale \
                                                                                              headscale nodes register --user eightballer --key mkey:SECRET_KEY

```


## Once tailscale is setup,

It is possible to create a controller in the local cluster.

k3sup install --ip $SERVER_IP --user tom --tls-san $SERVER_IP --k3s-extra-args '--node-ip=100.64.0.4  --node-external-ip=100.64.0.4 --disable traefik --flannel-iface=tailscale0'


It is then possible to join to the cluster.

```
 k3sup join --ip $AGENT_IP --server-ip $SERVER_IP --user $USER --k3s-extra-args '--flannel-iface=tailscale0'
```

Couple things to note;
```
100.64.0.2      gefion               eightballer  linux   -
100.64.0.3      cloud-ubuntu         eightballer  linux   -
100.64.0.5      lakshmi              eightballer  linux   active; direct 192.168.1.83:41641, tx 19048 rx 15784
100.64.0.4      zeus                 eightballer  linux   idle, tx 12788 rx 44108

# Health check:
#     - TLS connection error for "": certificate is self-signed
[I] ➜  private-infrastructure git:(master) ✗ kubectl get ingress --all^C
[I] ➜  private-infrastructure git:(master) ✗ export AGENT_IP=100.64.0.5

                                             export SERVER_IP=100.64.0.4
                                             export USER=tom
```