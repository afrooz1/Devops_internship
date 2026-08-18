# Task 1: Linux History and Boot Process

## Part A: Linux History

Linux is a popular open-source operating system that was initially created by **Linus Torvalds in 1991**. At that time, Torvalds was a Computer Science student at the **University of Helsinki, Finland**.

In early 1991, Torvalds wanted to work with a UNIX-like operating system on his personal computer. However, available free UNIX-like systems had limitations, while commercial UNIX systems were expensive. Therefore, he decided to develop his own operating system kernel.

In **August 1991**, Torvalds announced his project to the online community and invited other developers to contribute. Linux was later released under the **GNU General Public License (GPL)**, which allowed developers to freely use, modify, and distribute the software.

Technically, **Linux is the kernel**, which manages hardware and system resources. A Linux distribution combines the Linux kernel with GNU tools, libraries, utilities, and other software to provide a complete operating-system environment.

---

# The Linux Boot Process

The Linux boot process is the sequence of steps a computer follows from powering on until the operating system is fully loaded and the user reaches the login screen.

## 1. BIOS/UEFI

The boot process begins with **BIOS (Basic Input/Output System)** or **UEFI (Unified Extensible Firmware Interface)**.

When the computer is powered on, BIOS/UEFI performs an initial hardware check called **POST (Power-On Self-Test)**. It checks important components such as:

* CPU
* RAM
* Keyboard
* Storage devices

After completing these checks, BIOS/UEFI identifies a suitable boot device and transfers control to the bootloader.

## 2. Bootloader (GRUB)

After BIOS/UEFI finishes, it loads the **bootloader**.

Most Linux systems use **GRUB (GRand Unified Bootloader)**. Its main responsibilities are:

* Locate the Linux kernel
* Load the kernel into memory
* Pass boot parameters to the kernel
* Provide a boot menu when multiple operating systems or kernels are installed

## 3. Linux Kernel

The **Linux kernel** is the core component of the operating system.

Once GRUB loads the kernel into memory, the kernel takes control of the computer. It initializes and manages:

* CPU
* Memory
* Storage
* Other hardware devices

The kernel also prepares the system for the userspace environment.

## 4. Initramfs

During early boot, the kernel may need additional drivers and tools to access the actual root filesystem.

**Initramfs (Initial RAM Filesystem)** is a small temporary filesystem loaded into RAM along with the kernel.

It contains:

* Essential drivers
* Utilities
* Boot scripts

These help the kernel locate, access, and mount the real root filesystem.

**In simple terms:** Initramfs acts as a temporary toolbox that helps the kernel access the main filesystem.

## 5. systemd (PID 1)

After the kernel has initialized the system and the real root filesystem is available, it starts the first userspace process.

On most modern Linux distributions, this process is **systemd**, which normally has **Process ID 1 (PID 1)**.

systemd is responsible for:

* Starting services
* Managing dependencies
* Mounting filesystems
* Managing system processes
* Bringing the system into its required operating state

## 6. Starting System Services

After systemd starts, it launches the services required by the operating system.

Examples include:

* Networking
* SSH
* System logging
* Scheduled tasks
* Device management
* Other background services

systemd manages the order of services based on their dependencies.

## 7. Reaching a systemd Target

systemd works toward a specific **target**, which represents a particular system state.

### multi-user.target

Provides a fully functional multi-user environment without requiring a graphical desktop. It is commonly used on servers.

### graphical.target

Provides the services required for a graphical desktop environment.

Targets help systemd determine which services and components should be running.

## 8. Login Prompt

Finally, after the required services have started and the system reaches its target, the computer is ready for the user.

On a server, the user may see a **text-based login prompt**.

On a desktop Linux system, a **graphical login screen** is normally displayed.

Successfully reaching the login screen indicates that the Linux boot process has completed.

---

# Part B: Investigate Your System

The system investigated for this task is **Linux running inside WSL2**.

## 1. Kernel Initialization

### Command

```bash
dmesg | head -30
```

### Observation

The `dmesg` output provides evidence that the Linux kernel running inside WSL2 initialized and processed CPU, memory, and virtualization information during early startup.

---

## 2. Storage / Device Detection

### Command

```bash
dmesg | grep -Ei "disk|block|storage|nvme|scsi"
```

### Observation

The Linux kernel initializes the SCSI subsystem and detects Microsoft virtual disks in the WSL2 environment.

These virtual disks are attached as Linux storage devices, such as `sd*`, allowing Linux to access and use the storage devices.

---

## 3. Network Initialization

### Command

```bash
dmesg | grep -Ei "network|eth|net|veth"
```

### Observation

The kernel initializes networking functionality and registers network-related drivers and protocols, including IPv4 and IPv6 networking.

WSL2 uses the **Hyper-V networking driver (`hv_netvsc`)** for its virtual network interface.

### Checking Network Interfaces

```bash
ip link
```

### Observation

The `ip link` command shows that the WSL2 system has an **eth0** network interface, and the interface is currently **UP**.

This confirms that networking has been initialized successfully.

---

## 4. Services Starting

### Command

```bash
journalctl -b | grep -E "Started|Starting" | head -30
```

### Observation

`journalctl` is used to read logs collected by the **systemd journal**.

The `-b` option displays logs from the current boot.

The command filters the logs to show services that were **starting** or **started** during the boot process.

This provides evidence that systemd is starting and managing system services.

---

## 5. Reaching the Final Boot Target

### Command

```bash
systemctl status graphical.target
```

### Observation

This command checks the status of the `graphical.target`.

It helps determine whether the system has reached the graphical system target and whether the services associated with that target are active.

In a WSL2 environment, a graphical target may not be active because WSL2 normally provides a Linux command-line environment rather than a traditional desktop login screen.

---


# Commands Used

| Purpose                      | Command                                                |
| ---------------------------- | ------------------------------------------------------ |
| View kernel messages         | `dmesg \| head -30`                                    |
| Check storage devices        | `dmesg \| grep -Ei "disk\|block\|storage\|nvme\|scsi"` |
| Check network initialization | `dmesg \| grep -Ei "network\|eth\|net\|veth"`          |
| View network interfaces      | `ip link`                                              |
| View current boot logs       | `journalctl -b`                                        |
| Check graphical target       | `systemctl status graphical.target`                    |

---

# Conclusion

This task provided an overview of **Linux history and the Linux boot process**. The investigation of the WSL2 environment demonstrated how the Linux kernel initializes hardware and networking, detects virtual storage devices, and works with systemd to start system services.

