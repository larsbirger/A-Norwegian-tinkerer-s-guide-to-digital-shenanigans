# setting up wlan networkd with wpa_supplicant

**1. Identify Your Wireless Interface:**

- **Use `ip a` command:**
   ```bash
   ip a 
   ```
   Look for the interface name of your Wi-Fi card (e.g., `wlan0`, `wlp2s0`).



**2. Create a Network Configuration File:**

- Create a file in the `/etc/systemd/network/` directory (e.g., `/etc/systemd/network/25-wlan.network`).
- **Example Configuration (WPA2/WPA3):**

    ```ini
    [Match]
    Name=wlan0 

    [Network]
    DHCP=ipv4 
    # Optional: Set static IP if needed
    # Address=192.168.1.123/24 
    # Gateway=192.168.1.1

    # it is generally not reccomeneded to include this section
    # as it may make wifi password and security easier to locate.
    [WiFi]
    Scan=yes
    # Replace with your actual SSID
    SSID="Your_WiFi_Network_Name"
    # Replace with your actual password
    Password="Your_WiFi_Password" 
    ```

    Specifying the SSID and password in both the `systemd-networkd` configuration file and the `wpa_supplicant` configuration file is **redundant and generally not recommended.**

    **Here's why:**

    - **`systemd-networkd` is not designed to handle Wi-Fi security directly.** It's primarily concerned with network device configuration (IP addresses, routing, etc.). 
    - **`wpa_supplicant` is the specialized tool for Wi-Fi security.** It handles authentication, encryption, and key management. 

    **Best Practice:**

    - **Only specify the SSID and password in the `wpa_supplicant` configuration file.** This is where this information belongs.
    - **In your `systemd-networkd` configuration file, focus on general network settings:**
        - Interface name (`Match`)
        - DHCP or static IP configuration (`Network`)

    **Example:**

    `/etc/systemd/network/wlan0.network`

    **`systemd-networkd` configuration (wlan0.network):**

    ```ini
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
    
    **`wpa_supplicant` configuration (wpa_supplicant-wlan0.conf):**

    * Create a configuration file for `wpa_supplicant`:
        
        * Create a directory: `sudo mkdir /etc/wpa_supplicant`
        (if it does not already exist)

        * Create a file: `sudo touch /etc/wpa_supplicant/wpa_supplicant-wlan0.conf`

    `/etc/wpa_supplicant/wpa_supplicant-wlan0.conf`

    ```ini
    ctrl_interface=/var/run/wpa_supplicant
    ctrl_interface_group=0
    update_config=1
    network={
        ssid="<NETWORK_SSID>" 
        psk="<NETWORK_PASSWORD>" 
        key_mgmt=WPA-PSK 
        proto=WPA2 
        pairwise=CCMP TKIP 
        group=CCMP TKIP 
        scan_ssid=1 
    } 
    ```

    By following this approach, you maintain a cleaner and more maintainable configuration, avoiding potential conflicts or inconsistencies.

**4. Enable and Start Services:**

* Enable the `wpa_supplicant` service for your interface:
   ```bash
   sudo systemctl enable wpa_supplicant@wlan0.service 
   ```
* Restart `systemd-networkd` and `wpa_supplicant`:
   ```bash
   sudo systemctl restart systemd-networkd.service 
   sudo systemctl restart wpa_supplicant@wlan0.service 
   ```

**5. Verify Connection:**

* Check the interface status:
   ```bash
   ip a 
   ```
   You should see the Wi-Fi interface with an IP address assigned.
* Check connectivity:
   ```bash
   ping -c 4 8.8.8.8 
   ```

**Important Notes:**

* **Replace placeholders:** 
    * `<NETWORK_SSID>` with the actual SSID of your Wi-Fi network.
    * `<NETWORK_PASSWORD>` with the actual Wi-Fi password.
    * `wlan0` with the actual name of your Wi-Fi interface.
* **Security:** 
    * Store Wi-Fi passwords securely. Consider using a secrets management tool.
* **Troubleshooting:** 
    * Check `journalctl -u systemd-networkd` and `journalctl -u wpa_supplicant@wlan0` for any error messages.
    * If you encounter issues, try disabling any other network managers (like NetworkManager).

This guide provides a basic setup. You can further customize the configuration based on your specific needs (e.g., static IP, VLANs, 802.1x authentication).

Remember to consult the official systemd-networkd documentation for the most up-to-date information and advanced configuration options.
