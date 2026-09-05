# Task 3 — Email Protocols (SMTP, POP3, IMAP)

This task focuses on understanding the three major email protocols used in email communication:

* **SMTP**
* **POP3**
* **IMAP**

The task covers their purposes, common ports, security considerations, use cases, and the differences between POP3 and IMAP.

---

## 🎯 Objective

The objectives of this task were to:

* Understand SMTP, POP3, and IMAP.
* Learn the purpose of each email protocol.
* Understand the common ports used by email protocols.
* Compare POP3 and IMAP.
* Understand how email messages are sent and retrieved.
* Learn about TLS-secured email connections.
* Study basic SMTP testing using a test server.

---

## 1. SMTP

**SMTP (Simple Mail Transfer Protocol)** is primarily used for **sending and transferring email messages**.

SMTP is used for:

* Sending email from an email client to a mail server.
* Transferring email between mail servers.
* Sending automated application notifications.
* Sending system-generated emails.

### Common SMTP Ports

|    Port | Purpose                                                          |
| ------: | ---------------------------------------------------------------- |
|  **25** | Traditional SMTP, mainly used for server-to-server mail transfer |
| **587** | Message submission, commonly used with STARTTLS                  |
| **465** | SMTP submission using implicit TLS                               |

### Important Point

SMTP is primarily a **sending/submission protocol**. It is not normally used to retrieve messages from a user's mailbox.

---

## 2. POP3

**POP3 (Post Office Protocol Version 3)** is used to retrieve email messages from a mail server.

POP3 generally downloads messages to the local device. Depending on the email client's configuration, messages may be deleted from the server or retained after downloading.

### Common POP3 Ports

|    Port | Purpose                |
| ------: | ---------------------- |
| **110** | POP3 without TLS       |
| **995** | POP3 over implicit TLS |

### Use Cases

POP3 can be useful when:

* Email is primarily accessed from one device.
* Local storage of messages is preferred.
* Offline access is important.
* A simple download-based email workflow is required.

---

## 3. IMAP

**IMAP (Internet Message Access Protocol)** is used to access and manage email messages stored on a mail server.

Unlike traditional POP3 workflows, IMAP is designed around server-side mailbox storage and synchronization.

### Common IMAP Ports

|    Port | Purpose                                 |
| ------: | --------------------------------------- |
| **143** | IMAP, optionally secured using STARTTLS |
| **993** | IMAP over implicit TLS                  |

### Use Cases

IMAP is commonly used for:

* Accessing email from multiple devices.
* Synchronizing messages.
* Managing server-side folders.
* Keeping messages stored on the mail server.
* Synchronizing read/unread and other mailbox changes.

---

# 4. SMTP vs POP3 vs IMAP

| Feature            | SMTP                        | POP3                 | IMAP                         |
| ------------------ | --------------------------- | -------------------- | ---------------------------- |
| Main Purpose       | Send/transfer email         | Retrieve email       | Access and synchronize email |
| Primary Direction  | Outgoing / server-to-server | Incoming             | Incoming                     |
| Main Role          | Sending messages            | Downloading messages | Managing messages on server  |
| Multiple Devices   | Not applicable              | Limited              | Excellent                    |
| Synchronization    | Not applicable              | Limited              | Yes                          |
| Common Secure Port | 587 / 465                   | 995                  | 993                          |

---

# 5. POP3 vs IMAP

The main difference between POP3 and IMAP is how they handle **email storage, access, and synchronization**.

| Feature              | POP3                                                 | IMAP                                                                             |
| -------------------- | ---------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Storage**          | Messages are commonly downloaded to the local device | Messages remain primarily on the mail server                                     |
| **Retrieval**        | Downloads messages from the server                   | Accesses and synchronizes messages on the server                                 |
| **Multiple Devices** | Limited                                              | Designed for multiple devices                                                    |
| **Synchronization**  | Limited                                              | Strong synchronization                                                           |
| **Folders**          | Limited mailbox management                           | Supports server-side folders                                                     |
| **Offline Access**   | Good because messages are downloaded locally         | Available through cached messages in supported clients                           |
| **Deletion**         | Depends on client/server configuration               | Changes can be synchronized with the server                                      |
| **Bandwidth**        | Often lower after messages are downloaded            | Can use more bandwidth due to synchronization                                    |
| **Backup**           | Local copies require separate backup                 | Server storage may provide an additional copy, but provider backups are separate |
| **Best Use**         | Simple or primarily single-device access             | Multi-device and synchronized access                                             |

---

# 6. When to Use Each Protocol

## SMTP

Use SMTP when you need to:

* Send emails.
* Send application notifications.
* Send automated system messages.
* Transfer email between mail servers.

## POP3

Use POP3 when you:

* Mainly use one device.
* Prefer downloading emails locally.
* Need strong offline access.
* Do not require extensive synchronization between devices.

## IMAP

Use IMAP when you:

* Use multiple devices.
* Want email synchronized between devices.
* Want messages stored on the server.
* Need server-side folder management.
* Want read/unread and mailbox changes synchronized.

---

# 🔐 7. Security

Email protocols can be protected using **TLS (Transport Layer Security)**.

Common TLS-secured ports include:

| Protocol | Secure Port |
| -------- | ----------: |
| **SMTP** |   587 / 465 |
| **POP3** |         995 |
| **IMAP** |         993 |

Using TLS helps protect credentials and email data while they are being transmitted between the client and server.

> **Note:** Port numbers alone do not guarantee security. The server and client must actually negotiate and use TLS correctly.

---

# 8. Email Communication Flow

A simplified email workflow can be represented as:

```text
Sender
   |
   | SMTP
   v
Sender Mail Server
   |
   | SMTP
   v
Recipient Mail Server
   |
   | IMAP / POP3
   v
Recipient Email Client
```

### Explanation

1. The sender's email client submits the message using **SMTP**.
2. SMTP is used to transfer the message between mail servers.
3. The recipient's mail server stores the message.
4. The recipient accesses the message using **IMAP or POP3**.

---

# 9. Practical Part

The task required using `telnet` or a similar tool to connect to a **test SMTP server** and simulate basic SMTP commands.

Since a dedicated test SMTP server was not available, the SMTP simulation was not performed.

### Practical Status

* ✅ SMTP theory completed
* ✅ POP3 theory completed
* ✅ IMAP theory completed
* ✅ POP3 vs IMAP comparison completed
* ⏳ SMTP practical simulation — Pending

### Example SMTP Testing Workflow

A controlled test server could be accessed using a command such as:

```bash
telnet <smtp-server> 25
```

The exact commands and authentication requirements depend on the SMTP server being used.

> **Security Note:** Never test SMTP authentication using real passwords or credentials in an unsecured connection. Use a controlled lab server and appropriate TLS protection.

---

# 📚 10. Key Learnings

Through this task, I learned:

* **SMTP** is primarily used for sending and transferring email.
* **POP3** is used to retrieve and commonly download emails.
* **IMAP** provides server-side email access and synchronization.
* SMTP commonly uses **port 25** for server-to-server transfer.
* SMTP submission commonly uses **port 587**.
* SMTP can also use **port 465** for implicit TLS.
* POP3 commonly uses **port 995** for TLS-secured access.
* IMAP commonly uses **port 993** for TLS-secured access.
* POP3 is suitable for simpler, download-oriented email access.
* IMAP is better suited to multi-device synchronized email access.
* TLS helps protect email communication and credentials during transmission.

---

# 🛠️ Tools and Technologies

* **Linux / Ubuntu**
* **Terminal**
* **SMTP**
* **POP3**
* **IMAP**
* **TLS**
* **Telnet**
* **Email Networking**

---

# 📌 Conclusion

SMTP, POP3, and IMAP are important protocols used in modern email systems.

**SMTP is primarily responsible for sending and transferring email**, while **POP3 and IMAP are used to retrieve or access messages from a mail server**.

POP3 generally follows a download-oriented approach, while IMAP is designed for server-side mailbox access and synchronization across multiple devices.

For modern users who access email from phones, laptops, and other devices, **IMAP is generally the more suitable choice** because it keeps mailbox state synchronized across devices.

Understanding these protocols provides a strong foundation for **network administration, Linux administration, DevOps, and cybersecurity**.
