# Task 2 — FTP and SFTP Basics

This task focuses on understanding **FTP (File Transfer Protocol)** and **SFTP (SSH File Transfer Protocol)**, their differences, common use cases, and basic file-transfer commands.

Since a separate remote/test server was not available, the practical commands and workflows were studied and documented for future testing.

---

## 🎯 Objective

The objectives of this task were to:

* Understand FTP and SFTP.
* Learn how FTP and SFTP transfer files.
* Understand the security differences between FTP and SFTP.
* Learn common FTP commands.
* Learn common SFTP commands.
* Understand the ports used by FTP and SFTP.
* Identify common use cases for both protocols.

---

## 1. FTP

**FTP (File Transfer Protocol)** is a standard network protocol used to transfer files between a client and a server.

### Key Characteristics

* Control connection normally uses **port 21**.
* Does not encrypt communication by default.
* Supports file upload and download.
* Supports remote directory management.
* Authentication commonly uses a username and password.
* Suitable for environments where encryption is not required or where a secured alternative is provided separately.

### FTP Connection

```bash
ftp <server-address>
```

### Important FTP Commands

```text
ls                  # List remote files
pwd                 # Show remote working directory
cd <directory>      # Change remote directory
put test.txt        # Upload a file
get test.txt        # Download a file
bye                 # Exit FTP
```

---

## 2. SFTP

**SFTP (SSH File Transfer Protocol)** is a secure file-transfer protocol that operates over **SSH (Secure Shell)**.

It is a separate protocol from traditional FTP and provides secure file transfer through an SSH connection.

### Key Characteristics

* Normally uses **port 22**.
* Encrypts communication through SSH.
* Protects authentication credentials during transmission.
* Supports secure file upload and download.
* Supports remote directory management.
* Supports password and SSH-key-based authentication.
* Commonly used for Linux server administration and secure file transfers.

### SFTP Connection

```bash
sftp username@server-address
```

### Important SFTP Commands

```text
ls                  # List remote files
pwd                 # Show remote working directory
cd <directory>      # Change remote directory

lpwd                # Show local working directory
lls                 # List local files
lcd <directory>     # Change local directory

put test.txt        # Upload a file
get test.txt        # Download a file

mkdir <directory>   # Create a remote directory
rm test.txt         # Delete a remote file

exit                # Exit SFTP
```

---

## 3. FTP vs SFTP

| Feature                        | FTP                             | SFTP                       |
| ------------------------------ | ------------------------------- | -------------------------- |
| Full Name                      | File Transfer Protocol          | SSH File Transfer Protocol |
| Default Port                   | 21                              | 22                         |
| Encryption                     | No encryption by default        | Encrypted through SSH      |
| Security                       | Lower                           | Higher                     |
| Authentication                 | Username/password commonly used | Password or SSH key        |
| Underlying Technology          | FTP                             | SSH                        |
| File Transfer                  | Yes                             | Yes                        |
| Remote Directory Management    | Yes                             | Yes                        |
| Recommended for Sensitive Data | Generally no                    | Yes                        |

> **Note:** FTP and SFTP are different protocols. SFTP is not simply FTP running over SSH.

---

## 4. Advantages of SFTP

SFTP is commonly preferred for secure file transfers because it provides:

* **Encrypted communication**
* **Secure authentication**
* **Data integrity protection**
* **Protection of credentials during transmission**
* **A single SSH-based connection**
* **Secure remote file and directory management**

---

## 5. Practical Commands Studied

### FTP

```bash
ftp <server-address>
```

Example workflow:

```text
ls
pwd
cd <directory>
put test.txt
get test.txt
bye
```

### SFTP

```bash
sftp username@server-address
```

Example workflow:

```text
ls
pwd
cd /remote/directory
put test.txt
get test.txt
exit
```

These commands demonstrate the basic process of:

1. Connecting to a remote server.
2. Viewing remote files.
3. Navigating directories.
4. Uploading files.
5. Downloading files.
6. Closing the connection.

### Practical Note

Actual FTP/SFTP connections were not performed because a separate test/remote server was not available.

The commands and workflow were studied theoretically and documented for future practical testing.

---

## 6. Common Use Cases

### FTP Use Cases

FTP may be encountered in:

* Legacy file-transfer systems.
* Internal networks where additional security controls are already in place.
* Public or anonymous file distribution.
* Systems that specifically require traditional FTP.

### SFTP Use Cases

SFTP is commonly used for:

* Secure server administration.
* Transferring configuration files.
* Uploading application files to Linux servers.
* Downloading logs and backups.
* Secure DevOps file transfers.
* Moving sensitive files between systems.

---

## 🔐 7. Security Considerations

Traditional FTP does **not provide encryption by default**. This means usernames, passwords, and transferred data can potentially be exposed when transmitted over an untrusted network.

SFTP provides encryption through SSH, making it a much safer choice for transferring sensitive information.

For modern systems, secure alternatives such as **SFTP** should generally be preferred when encrypted file transfer is required.

---

## 📚 Key Learnings

Through this task, I learned:

* FTP is a protocol used for transferring files between clients and servers.
* Traditional FTP does not encrypt communication by default.
* SFTP is a separate secure file-transfer protocol that operates over SSH.
* FTP normally uses **port 21** for its control connection.
* SFTP normally uses **port 22**.
* The `put` command is used to upload files.
* The `get` command is used to download files.
* SFTP supports secure authentication using passwords or SSH keys.
* SFTP is generally preferred when secure file transfer is required.
* FTP and SFTP are different protocols despite having similar purposes.

---

## 🛠️ Tools and Technologies

* **Linux / Ubuntu**
* **Terminal**
* **FTP**
* **SFTP**
* **SSH**
* **File Transfer**

---

## 📌 Conclusion

FTP and SFTP both provide file-transfer functionality, but they differ significantly in security.

**FTP normally uses port 21 and does not encrypt communication by default**, while **SFTP normally uses port 22 and provides encrypted file transfer through SSH**.

Understanding SFTP is particularly useful for **Linux administration, DevOps, server management, backups, and secure file transfers**.
