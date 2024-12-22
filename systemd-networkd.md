# **systemd-networkd**

**systemd-networkd** is a system service that manages network configurations in Linux systems. It's part of the **systemd** suite of system and service management tools.

**Key Features:**

* **Dynamic Configuration:** `systemd-networkd` can dynamically detect and configure network devices as they appear or disappear.
* **Virtual Network Devices:** It can create virtual network devices, which are useful for setting up complex network configurations for containers or virtual machines.
* **Configuration Files:** Network configurations are typically defined in files within the `/etc/systemd/network/` directory. These files use a declarative syntax to specify network settings.
* **Integration with systemd:** `systemd-networkd` is tightly integrated with other systemd components, allowing for seamless management of network services alongside other system services.

**How it Works:**

1. **Configuration Files:** You create configuration files in the `/etc/systemd/network/` directory. These files describe the desired network settings for each interface (e.g., IP address, subnet mask, gateway).
2. **Detection and Configuration:** `systemd-networkd` monitors for changes in network devices. When a new device appears, it reads the corresponding configuration file and applies the specified settings.
3. **Management:** `systemd-networkd` manages the network interfaces, ensuring they are properly configured and operational.

**Advantages:**

* **Flexibility:** Offers a flexible and powerful way to manage complex network configurations.
* **Dynamic:** Can adapt to changing network conditions.
* **Integration:** Seamlessly integrates with other systemd components.

**Example Configuration:**

```bat
[Match]
Name=enp0s3

[Network]
Address=192.168.1.100/24
Gateway=192.168.1.1
DNS=8.8.8.8 8.8.4.4
```

**Another example Configuration:**

```Bash
[Match]
Name=wlan0 

[Network]
DHCP=ipv4 
# Optional: Set static IP if needed
# Address=192.168.1.100/24 
# Gateway=192.168.1.1

[WiFi]
Scan=yes
# Replace with your actual SSID
SSID="Your_WiFi_Network_Name"
# Replace with your actual password
Password="Your_WiFi_Password"
```

This configuration file specifies that the `enp0s3` interface should have a static IP address of 192.168.1.100, use the 192.168.1.1 gateway, and use Google Public DNS servers.

**Note:** While `systemd-networkd` is a powerful tool, some distributions may use other network management tools like `Netplan` as a frontend for configuring `systemd-networkd`.




-----


