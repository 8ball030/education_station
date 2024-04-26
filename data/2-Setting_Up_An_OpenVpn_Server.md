---
date: 04-26-24
id: 2
modified_at: 04/26/24 04:47
path: ''
tags: []
title: Setting Up An OpenVpn Server
type: note
---

Read this [guide](https://github.com/idlab-discover/easy-openvpn-server)

Install the snap on the server.

```bash
sudo snap install easy-openvpn-server
```
Copy the client config to your personal device.

# Run this on the _server_ to create the config file.
```
sudo easy-openvpn-server show-client default > default.ovpn
```
and from your device

# Run this on the _client_ to download the config file.
scp my-user@my-server:~/default.ovpn .

Import the .ovpn config file into the VPN application of your device.