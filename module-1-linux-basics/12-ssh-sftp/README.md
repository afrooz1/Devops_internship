# Task 12 — SSH, SCP and SFTP

## Objective

Understand and demonstrate secure remote access and file transfer between two Linux environments using:

* SSH
* SCP
* SFTP
* Password authentication
* SSH key-based authentication

---

# 1. Environment

Two Linux environments were used.

### SSH Client

```text
Environment: WSL Ubuntu
```

### SSH Server

```text
Environment: Ubuntu Server
```

The connection used in this task was:


---

# 2. SSH Server Verification

The SSH server executable was verified with:

```bash
which sshd
```

Output:

```text
/usr/sbin/sshd
```

The SSH service was checked using:

```bash
systemctl status ssh
```

The service was active and running:

```text
Active: active (running)
```


Therefore, the SSH server was successfully configured and listening for connections.

---

# 3. Network Connectivity

The Ubuntu Server IP address was:

```text
10.0.110.100
```

Connectivity from the WSL client was tested using:

```bash
ping -c 4 10.0.110.100
```

The test returned:

```text
4 packets transmitted, 4 received, 0% packet loss
```

This confirmed that the WSL client could reach the Ubuntu Server.

---

# 4. Password-Based SSH Login

A password-based SSH connection was established using:

```bash
ssh root@10.0.110.100
```

After authentication, the remote server prompt was displayed:

```text
root@scp-test:~#
```

The remote system was verified with:

```bash
whoami
hostname
```

Expected results:

```text
root
scp-test
```

This demonstrated successful password-based SSH authentication.

---

# 5. SSH Key Generation

The SSH client initially contained `known_hosts` but did not have an SSH key pair.

An  key pair was generated using:

```bash
ssh-keygen -t ed25519
```

The key pair consists of:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

### Private Key

```text
~/.ssh/id_ed25519
```

The private key must remain secret and must never be uploaded to GitHub or included in a project submission.

### Public Key

```text
~/.ssh/id_ed25519.pub
```

The public key can safely be installed on the remote server.

---

# 6. Installing the Public Key

The public key was copied to the Ubuntu Server using:

```bash
ssh-copy-id root@10.0.110.100
```

The public key was added to:

```text
/root/.ssh/authorized_keys
```

The private key was **not** copied to the server.


---

# 7. Key-Based SSH Authentication

After installing the public key, SSH access was tested again:

```bash
ssh root@10.0.110.100
```

The connection successfully authenticated using the SSH key.

The remote system was verified using:

```bash
whoami
hostname
```

Results confirmed:

```text
root
scp-test
```

This demonstrated successful public-key authentication.

---

# 8. SSH Key Files

The important SSH files used in this task are:

| File              | Location         | Purpose                                    |
| ----------------- | ---------------- | ------------------------------------------ |
| `id_ed25519`      | Client `~/.ssh/` | Private key                                |
| `id_ed25519.pub`  | Client `~/.ssh/` | Public key                                 |
| `known_hosts`     | Client `~/.ssh/` | Stores known SSH server identities         |
| `authorized_keys` | Server `~/.ssh/` | Stores public keys allowed to authenticate |

### Important Security Rule

Never share:

```text
~/.ssh/id_ed25519
```

Only the public key:

```text
~/.ssh/id_ed25519.pub
```

should be copied to a remote server.

---

# 9. `known_hosts`

The client stores information about previously contacted SSH servers in:

```text
~/.ssh/known_hosts
```

When connecting to a server for the first time, SSH checks the server's host key.

If the server's host key changes unexpectedly later, SSH can warn the user about a possible security problem.

---

# 10. `authorized_keys`

The Ubuntu Server stores allowed public keys in:

```text
~/.ssh/authorized_keys
```

For the root account used in this task:

```text
/root/.ssh/authorized_keys
```

The server uses this file to determine which public keys are allowed to authenticate to the account.

---

# 11. SCP

SCP stands for **Secure Copy Protocol**.

It uses SSH to securely copy files between systems.

## SCP Upload

A test file was created on the WSL client:

```bash
echo "SCP test from WSL client" > scp-test.txt
```

The file was uploaded to the Ubuntu Server using:

```bash
scp scp-test.txt root@10.0.110.100:/root/
```

The file was then verified on the server.

## SCP Download

A test file was created on the Ubuntu Server:

```bash
echo "File created on Ubuntu Server" > /root/server-file.txt
```

It was downloaded to the WSL client using:

```bash
scp root@10.0.110.100:/root/server-file.txt .
```

The downloaded file was verified using:

```bash
cat server-file.txt
```

Both SCP upload and download operations were successfully demonstrated.

---

# 12. SFTP

SFTP stands for **SSH File Transfer Protocol**.

It provides secure file transfer and remote file management through an SSH connection.

An SFTP session was started using:

```bash
sftp root@10.0.110.100
```

After connecting, the following commands were demonstrated.

### List Remote Files

```text
ls
```

### Show Remote Working Directory

```text
pwd
```

### Change Remote Directory

```text
cd /tmp
```

Return to the root user's directory:

```text
cd /root
```

### Upload a File

A local test file was created:

```bash
echo "SFTP upload test" > sftp-upload.txt
```

Inside SFTP:

```text
put sftp-upload.txt
```

### Download a File

Inside SFTP:

```text
get server-file.txt
```

### Exit SFTP

```text
exit
```

The SFTP connection, navigation, upload, and download operations were successfully demonstrated.

---

# 13. SSH, SCP and SFTP

| Tool | Purpose                             | Default Port |
| ---- | ----------------------------------- | ------------ |
| SSH  | Remote shell/access                 | 22           |
| SCP  | Secure file copying                 | 22           |
| SFTP | Secure file transfer and management | 22           |

All three can use SSH for secure communication.

---

# 14. SFTP vs FTP

| Feature               | FTP                                             | SFTP                       |
| --------------------- | ----------------------------------------------- | -------------------------- |
| Full Name             | File Transfer Protocol                          | SSH File Transfer Protocol |
| Encryption            | No encryption by default                        | Encrypted through SSH      |
| Default port          | 21                                              | 22                         |
| Authentication        | Username/password                               | Password or SSH keys       |
| Security              | Weak for sensitive data over untrusted networks | Secure                     |
| Underlying technology | FTP                                             | SSH                        |

Traditional FTP does not encrypt communication by default.

As a result, usernames, passwords, and transferred files may be exposed to someone monitoring an untrusted network.

SFTP uses SSH encryption to protect the communication between the client and server.

Therefore, SFTP is generally preferred over plain FTP when transferring sensitive information over untrusted networks.

---

# 15. How SSH Works

A simplified SSH connection can be represented as:

```text
SSH Client
    |
    | Connection request
    v
SSH Server
    |
    | Server host key
    v
Client verifies server
    |
    | Secure encrypted session
    v
Authentication
    |
    | Password or SSH key
    v
Remote Shell
```

SSH provides:

* Encrypted communication
* Server identification through host keys
* Password authentication
* Public/private key authentication
* Secure remote command execution

---

# 16. Security Notes

* Never share an SSH private key.
* Never commit private SSH keys to GitHub.
* Verify the host key when connecting to a server for the first time.
* Investigate unexpected SSH host-key changes.
* Use SSH keys where appropriate for secure authentication.
* Avoid plain FTP for sensitive information over untrusted networks.

---

# 17. Task Completion

The following requirements were completed:

*  SSH client/server identified
*  SSH server verified
*  Port 22 verified
*  Network connectivity verified
*  Password-based SSH login
*  SSH key generation
*  Public key installation
*  Key-based SSH login
*  Private/public key identified
*  `known_hosts` identified
*  `authorized_keys` identified
*  SCP upload
*  SCP download
*  SFTP connection
*  SFTP remote listing
*  SFTP directory navigation
*  SFTP upload
*  SFTP download
*  SFTP vs FTP comparison
*  SSH security concepts documented

## Conclusion

Task 12 demonstrated secure remote access and file transfer between the WSL Ubuntu client and Ubuntu Server.

SSH was used for remote authentication, SCP was used for secure file copying, and SFTP was used for interactive secure file management.

Both password-based and public-key SSH authentication were demonstrated, including the use of `known_hosts` and `authorized_keys`. The practical exercises confirmed that SSH, SCP, and SFTP can securely connect to and transfer files between Linux systems.
