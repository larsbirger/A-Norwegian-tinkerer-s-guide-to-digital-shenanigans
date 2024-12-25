# change from Network_Manager to networkd

based on [this page](https://www.xmodulo.com/switch-from-networkmanager-to-systemd-networkd.html)

## Requirements

[systemd-networkd](/systemd-networkd.md) is available in **systemd version 210** and higher. Thus distributions like Debian 8 Jessie (**systemd 215**), Fedora 21 (**systemd 217**), Ubuntu 15.04 (**systemd 219**) or later are compatible with systemd-networkd.

run the command to check you version:
```bash
systemctl --version
```

## Switch from Network Manager to systemd-networkd

### **1. enabling systemd-networkd**

Disable NetworkManager and enable systemd-networkd by running the commands:

```Bash
sudo systemctl disable NetworkManager
sudo systemctl enable systemd-networkd
```

### **2. Enabling systemd-resolved**

[systemd-resolved](/systemd-resolved.md) is a key component of the systemd suite in Linux systems, responsible for resolving domain names to IP addresses

- **Note:**  i recommend you back up the file `/etc/resolv.conf`. if you have it already. you can do by running the file:

    ```Bash
    sudo mv /etc/resolv.conf /etc/resolv.conf.bak
    ```

Enable systemd-resolved:

```bash
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
```

- Once started, systemd-resolved will create its own resolv.conf somewhere under /run/systemd directory. 

    However, it is a common practice to store DNS resolver information in /etc/resolv.conf, and many applications still rely on /etc/resolv.conf. 

    For compatibility create a symlink to /etc/resolv.conf:
    ```Bash
    sudo rm /etc/resolv.conf
    sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
    ```


this should be it. if you want to set up network configurations you can [read how to do so here:](/configure-connections-with-networkd.md)