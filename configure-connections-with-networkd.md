# **configure network devices with systemd-networkd**

based on [this page](https://www.xmodulo.com/switch-from-networkmanager-to-systemd-networkd.html)

To configure network devices with systemd-networkd, you must specify configuration information in text files with **.network** extension.

These network configuration files are then stored and loaded from `/etc/systemd/network`.

When there are multiple files, systemd-networkd loads and processes them one by one in lexical order.

Create a folder `/etc/systemd/network`:
```bash
sudo mkdir /etc/systemd/network
```

## DHCP Networking

To set up a DHCP network, create the following configuration file (The name of a file can be arbitrary, but remember that files are processed in lexical order.)

```Bash
sudo nano /etc/systemd/network/name-of-file.network
```

in the file enter the following:

```toml
[Match]
#name if the network interface
Name=enp3*

[Network]
#if you want to have dhcp then uncomment this line
DHCP=yes
```

each network configuration file contains one or more sections with each section preceded by `[XXX]` heading.  
Each section contains one or more key/value pairs.

The `[Match]` section determine which network device(s) are configured by this configuration file.  
For example, this file matches any network interface whose name starts with `ens3` (e.g., `enp3s0`, `enp3s1`, `enp3s2`, etc.) for matched interface(s), it then applies DHCP network configuration specified under [Network] section.

## Static IP Networking

to assign a static IP address to a network interface, create the following configuration file:

```Bash
sudo nano /etc/systemd/network/10-static-enp3s0.network
```

enter the following:
```toml
[Match]
Name=wlan0 

[Network]
# to set dynamic IPv4, uncomment this line
# and comment out "Adress", "Gatway", and "DNS"
# DHCP=ipv4 

# to set manual IPv4, uncomment these lines
# and comment out "DHCP"
Address=192.168.1.30/24 
Gateway=192.168.1.1
DNS=192.168.1.1 8.8.8.8 8.8.4.4.
```