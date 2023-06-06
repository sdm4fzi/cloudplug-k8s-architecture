# K3S-Agent deployed on Cloudplug Edge

The Cloudplug Edge is used as a agent device. Here are some of its technical specifications:

- **CPU**: iMX7
- **STORAGE/RAM**: 8G eMMC-Flash / 1G RAM
- **Ethernet**: Gigabit-Ethernet

For the SDM4FZI projekt a Cloudplug Edge system build was developed. It does no longer contain Docker and is extended with a K3S - Agent. This allows a Cloudplug Edge to integrate into a Kubernetes cluster as a so-called **node**. Since the Cloudplugs Edge's persistent storage is located in the /data directory, we need to map our K3S information onto it. 

```shell
    systemctl stop k3s
    systemctl stop k3s-agent
    k3s-killall

    mv /run/k3s/ /data/k3s/
    mv /var/lib/kubelet/pods/ /data/k3s-pods/
    mv /var/lib/rancher/ /data/k3s-rancher/

    ln -s /data/k3s/ /run/k3s
    ln -s /data/k3s-pods/ /var/lib/kubelet/pods
    ln -s /data/k3s-rancher/ /var/lib/rancher

    systemctl start k3s
    systemctl start k3s-agent
```
---

## Start agent

```shell
k3s agent --server https://<ip-addr>:6443 --token our/token --data-dir /data/rancher/k3s
```

- **Server**: The IP address of the Raspberry Pi must be entered here.
- **Token**: Token of the server (see above)
- **Data-Dir**: Storage space/storage options, therefore working in the data directory.

---