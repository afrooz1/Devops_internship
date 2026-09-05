# Task 4 — DHCP Configuration and IP Addressing

This task focuses on understanding **DHCP (Dynamic Host Configuration Protocol)**, checking IP configuration on Linux and Windows, identifying DHCP-related settings, and understanding how DHCP leases can be released and renewed.

---

## 🎯 Objective

The objectives of this task were to:

* Understand DHCP and its purpose.
* Learn how DHCP automatically assigns IP addresses.
* Check IP configuration on Linux and Windows.
* Identify DHCP-related network information.
* Understand DHCP lease release and renewal.
* Learn the **DORA** DHCP process.
* Understand the relationship between DHCP and IP addressing.

---

# 1. What is DHCP?

**DHCP (Dynamic Host Configuration Protocol)** is a network management protocol that automatically provides devices with network configuration information.

A DHCP server can provide:

* IP address
* Subnet mask
* Default gateway
* DNS server
* Lease duration

DHCP eliminates the need to manually configure network settings on every device.

---

# 2. Purpose of DHCP

### Automated IP Assignment

DHCP automatically assigns an available IP address to devices when they connect to a network.

### Efficient IP Management

DHCP manages a pool of IP addresses and helps reduce the possibility of address conflicts.

### Centralized Configuration

Network settings such as the subnet mask, default gateway, and DNS servers can be configured centrally through DHCP.

### Support for Mobile Devices

Devices can automatically obtain appropriate network configuration when connecting to different networks.

---

# 3. Checking IP Address on Linux

On modern Linux systems, the recommended command for viewing network configuration is:

```bash
ip addr
```

Example:

```bash
ip addr
```

The output can be used to identify:

* Network interfaces
* IPv4 and IPv6 addresses
* MAC addresses
* Interface state

Another command that may be available on some systems is:

```bash
ifconfig
```

> **Note:** `ifconfig` is considered a legacy tool on many modern Linux distributions. The `ip` command from the `iproute2` package is generally preferred.

---

# 4. Checking IP Address on Windows

On Windows, use:

```cmd
ipconfig
```

For more detailed information:

```cmd
ipconfig /all
```

The `ipconfig /all` command can display:

* IPv4 address
* Subnet mask
* Default gateway
* DHCP enabled status
* DHCP server
* DNS servers
* Lease information

---

# 5. Release and Renew IP Address

Windows provides commands to release and renew DHCP-assigned network configuration.

## Release IP Address

```cmd
ipconfig /release
```

This releases the current DHCP configuration for the relevant network interfaces.

## Renew IP Address

```cmd
ipconfig /renew
```

This requests a new DHCP configuration from the DHCP server.

> **Note:** Renewing a DHCP lease does not necessarily result in a different IP address. The DHCP server may assign the same address again.

---

# 6. DHCP Commands on Linux

Some Linux systems use a DHCP client such as `dhclient`.

To release a DHCP lease:

```bash
sudo dhclient -r
```

To request a DHCP lease:

```bash
sudo dhclient
```

The assigned address can then be checked using:

```bash
ip addr
```

> **Note:** `dhclient` may not be installed or used by every modern Linux distribution. DHCP may instead be managed by **NetworkManager**, **systemd-networkd**, or another network management service.

---

# 7. DHCP Workflow

The basic DHCP process is commonly known as **DORA**:

```text
Client                         DHCP Server
  |                                 |
  | -------- DHCP Discover -------> |
  |                                 |
  | <--------- DHCP Offer --------- |
  |                                 |
  | -------- DHCP Request --------> |
  |                                 |
  | <--------- DHCP ACK ----------- |
  |                                 |
  |       IP Configuration          |
  |          Assigned               |
```

## DORA

### 1. Discover

The client broadcasts a **DHCP Discover** message to find available DHCP servers.

### 2. Offer

A DHCP server responds with a **DHCP Offer** containing an available IP configuration.

### 3. Request

The client sends a **DHCP Request** to indicate that it wants to use the offered configuration.

### 4. ACK

The DHCP server sends a **DHCP Acknowledgment (ACK)** confirming the IP configuration.

---

# 8. Checking DHCP Status

On Windows, run:

```cmd
ipconfig /all
```

Look for information such as:

```text
DHCP Enabled
DHCP Server
IPv4 Address
Default Gateway
DNS Servers
```

If **DHCP Enabled** shows `Yes`, the interface is configured to obtain its IP configuration automatically through DHCP.

---

# 9. Practical Commands

## Linux

### View network configuration

```bash
ip addr
```

### Legacy alternative

```bash
ifconfig
```

### Release DHCP lease

```bash
sudo dhclient -r
```

### Request DHCP lease

```bash
sudo dhclient
```

### Check IP configuration again

```bash
ip addr
```

---

## Windows

### Check IP configuration

```cmd
ipconfig
```

### View detailed configuration

```cmd
ipconfig /all
```

### Release DHCP configuration

```cmd
ipconfig /release
```

### Renew DHCP configuration

```cmd
ipconfig /renew
```

### Check configuration again

```cmd
ipconfig
```

---

# 🔄 10. DHCP Configuration Flow

A simplified DHCP configuration process looks like this:

```text
Device connects to network
          |
          v
DHCP Discover
          |
          v
DHCP Offer
          |
          v
DHCP Request
          |
          v
DHCP ACK
          |
          v
IP configuration received
          |
          v
Device communicates on network
```

DHCP therefore allows a device to obtain the configuration it needs to communicate on the network without requiring every setting to be manually entered.

---

# 🔐 11. Security Considerations

DHCP simplifies network management, but it should be properly controlled in managed environments.

Important considerations include:

* Preventing unauthorized DHCP servers.
* Using network controls such as **DHCP snooping** where appropriate.
* Monitoring unexpected DHCP activity.
* Protecting network infrastructure from rogue devices.
* Using appropriate network segmentation.

DHCP itself does not provide encryption for normal DHCP communication. Its primary purpose is automated network configuration.

---

# 📚 12. Key Learnings

Through this task, I learned:

* DHCP automatically provides network configuration to devices.
* DHCP can provide an IP address, subnet mask, gateway, DNS information, and lease details.
* `ip addr` is commonly used to inspect network configuration on Linux.
* `ifconfig` is a legacy alternative available on some Linux systems.
* `ipconfig` is commonly used on Windows.
* `ipconfig /all` provides detailed DHCP and network configuration information.
* `ipconfig /release` releases DHCP configuration on Windows.
* `ipconfig /renew` requests DHCP configuration again.
* DHCP uses the **DORA** process:

  * Discover
  * Offer
  * Request
  * ACK
* Renewing a DHCP lease may result in the same IP address being assigned again.
* DHCP is important for automated network configuration and troubleshooting.

---

# 🛠️ Tools and Technologies

* **Linux / Ubuntu**
* **Windows**
* **Terminal**
* **Command Prompt**
* **DHCP**
* **IPv4 / IPv6**
* **iproute2**
* **dhclient**

---

# 📌 Conclusion

DHCP simplifies network configuration by automatically assigning IP addresses and other network settings to devices.

Commands such as `ip addr` and `ipconfig` can be used to inspect network configuration, while Windows provides `ipconfig /release` and `ipconfig /renew` for managing DHCP configuration.

The **DORA process — Discover, Offer, Request, and ACK —** explains how a DHCP client obtains its network configuration.

Understanding DHCP and IP addressing is important for **network troubleshooting, Linux administration, system administration, and DevOps environments**.
