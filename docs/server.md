# K3S-Server deployed on Raspberry Pi 4B

The Raspberry PI 4B is used as the server device. Here are some of its technical specifications:

- Processor: Broadcom BCM2711, quad-core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz
- RAM: 4GB LPDDR4-3200 SDRAM
- Ethernet: Gigabit-Ethernet

To set up the server, the pre-existing script from "get.k3s.io" was used, and executed with the following command: 

```shell
    curl -sfL https://get.k3s.io | sh -
```

After starting the server (including a possible restart of the Raspberry), the token is obtained using the following command: 

```shell
    sudo cat /var/lib/rancher/k3s/server/node-token
```

This token is used on every agent and must therefore be cached.

---

## Dashboard

<p>In order to provide the user with the possibilities of integration, a dashboard is implemented on the Raspberry Pi. The installation of the dashboard followed the tutorial found at https://docs.k3s.io/installation/kube-dashboard.</p>

---

## ${\color{#ff0000}Needed - TODO}$

Executing the command:

```shell
    kubectl -n kubernetes-dashboard edit service kubernetes-dashboard
```

will display the yaml file of the service. Take note of the line NodePort: <Port> and change the line **type: ClusterIP** to **type: NodePort** and save the file.

Afterwards, the dashboard should be accessible at:

```shell
    https://<IP-address-Raspberry-Pi>:<port>
```
Note that using **https** is important.

By executing the following command, you will receive the token - you will need to it access the dashboard.
```shell
    sudo cat /var/lib/rancher/k3s/server/node-token
```
