# Task 8 — SSL/TLS Certificates

## Objective

The objective of this task was to understand **SSL/TLS certificates**, their purpose, major components, different certificate validation types, and how certificates help establish secure HTTPS communication.

> **Note:** The practical certificate generation and web server configuration will be completed after learning about Web Servers, as required by the task instructions.

---

# 1. What is an SSL/TLS Certificate?

An **SSL/TLS certificate** is a digital certificate used to authenticate a server and associate a public key with a domain name.

Modern websites use **TLS (Transport Layer Security)** rather than the older SSL protocol. However, the term **SSL certificate** is still commonly used.

A TLS certificate helps:

* Authenticate the website.
* Bind the website's domain name to a public key.
* Support secure TLS communication.
* Establish trust through a Certificate Authority (CA).

When a website uses HTTPS, the connection can be accessed using:

```text
https://example.com
```

instead of an unencrypted HTTP connection:

```text
http://example.com
```

> **Important:** The certificate itself does not encrypt all website traffic. The **TLS protocol** uses the certificate and cryptographic mechanisms during the TLS handshake to authenticate the server and establish keys for encrypted communication.

---

# 2. Purpose of an SSL/TLS Certificate

## 2.1 Authentication

A certificate helps verify that a server is authorized to represent a particular domain.

For example:

```text
example.com
     ↓
TLS Certificate
     ↓
Public Key
```

The certificate is digitally signed by a trusted Certificate Authority.

This helps protect against certain **Man-in-the-Middle (MITM)** attacks by allowing the client to verify the server's identity.

---

## 2.2 Supporting Encryption

TLS certificates contain the server's public key and provide information required during the TLS handshake.

The TLS protocol then establishes cryptographic session keys that are used to encrypt the communication.

```text
Browser
   |
   | TLS Handshake
   |
   ↓
Certificate + Key Exchange
   |
   ↓
Encrypted HTTPS Session
   |
   ↓
Web Server
```

This protects sensitive information such as:

* Passwords
* Login credentials
* Personal information
* Payment information

---

## 2.3 Data Integrity

TLS provides mechanisms that help detect whether transmitted data has been modified during communication.

This helps ensure that the data received by the client is the same data sent by the server.

---

## 2.4 User Trust

A valid and trusted certificate allows browsers to establish HTTPS connections without certificate security warnings.

Browsers can warn users when problems are detected, such as:

* Expired certificates
* Untrusted Certificate Authorities
* Domain name mismatches
* Invalid certificate chains

---

# 3. Components of an SSL/TLS Certificate

An SSL/TLS certificate contains several important pieces of information.

## Certificate Authority (CA)

A **Certificate Authority (CA)** is a trusted organization that issues and digitally signs certificates.

Examples include:

* Let's Encrypt
* DigiCert
* Sectigo

Operating systems and browsers maintain lists of trusted root Certificate Authorities.

---

## Certificate Subject

The certificate contains information identifying the certificate's subject.

For a website certificate, this is associated with the domain or domains covered by the certificate.

Example:

```text
Subject: CN = example.com
```

However, modern hostname validation primarily uses the **Subject Alternative Name (SAN)** extension.

---

## Subject Alternative Name (SAN)

**SAN (Subject Alternative Name)** specifies the domain names and other identities for which the certificate is valid.

Example:

```text
DNS: example.com
DNS: www.example.com
```

Modern browsers generally use SAN entries when checking whether a certificate matches the requested hostname.

---

## Validity Period

A certificate contains the period during which it is valid.

It includes:

```text
Not Before
Not After
```

Example:

```text
Not Before: 2026-07-29
Not After:  2026-10-27
```

Certificates must be renewed before they expire.

---

## Public Key

The certificate contains the server's **public key**.

The public key is part of an asymmetric cryptographic key pair:

```text
Public Key  → Included in the certificate
Private Key → Kept secret by the server
```

The private key must be carefully protected because unauthorized access to it can allow an attacker to impersonate the server.

---

## Signature Algorithm

The certificate contains information about the algorithm used for its digital signature.

Common modern algorithms include:

```text
RSA
ECDSA
```

Hash functions such as SHA-256 can also be used as part of certificate signature schemes.

The digital signature allows clients to verify that the certificate was issued and signed by the claimed CA and that its signed contents have not been altered.

---

## Certificate Fingerprint

A **certificate fingerprint** is a hash-based identifier derived from a certificate.

A commonly used fingerprint algorithm is:

```text
SHA-256
```

Fingerprints can be useful for identifying or comparing a particular certificate.

---

## Certificate Chain

Certificates normally form a **chain of trust**.

The conceptual structure is:

```text
Trusted Root CA
       ↓
Intermediate CA
       ↓
Server / Leaf Certificate
       ↓
Website
```

The browser or operating system trusts the root CA. The intermediate CA provides a link between the trusted root and the server certificate.

---

# 4. Types of SSL/TLS Certificates

Certificate validation is commonly categorized as **DV, OV, or EV**.

## Domain Validation (DV)

DV certificates verify control over the domain.

### Common use cases

* Personal websites
* Blogs
* Small projects
* Development websites

DV certificates are widely used for HTTPS.

---

## Organization Validation (OV)

OV certificates involve additional validation of the organization associated with the domain.

### Common use cases

* Business websites
* Organizations
* Companies

---

## Extended Validation (EV)

EV certificates involve a more extensive validation process for the organization.

They may be used by organizations that want a higher level of identity verification.

> **Note:** Modern browsers generally no longer display the historical "green address bar" treatment that was once associated with EV certificates.

---

# 5. How a Browser Verifies a Certificate

When a user visits an HTTPS website, the browser performs several checks as part of establishing trust.

A simplified process is:

```text
User visits HTTPS website
          ↓
Server provides certificate
          ↓
Browser checks certificate
          ↓
Checks hostname / SAN
          ↓
Checks validity period
          ↓
Builds certificate chain
          ↓
Verifies digital signatures
          ↓
Checks trusted root
          ↓
TLS connection established
```

The browser may display a security warning if problems are detected, such as:

* Certificate expiration
* Hostname mismatch
* Untrusted CA
* Invalid certificate chain
* Other TLS configuration problems

---

# 6. Certificate Verification with OpenSSL

SSL/TLS certificates can be inspected using tools such as a web browser or OpenSSL.

## Browser

A browser can display certificate and connection information through its security or certificate interface.

## OpenSSL

The certificate presented by an HTTPS server can be inspected from the command line:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

Certificate information can also be extracted directly:

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

This can display information such as:

* Certificate subject
* Certificate issuer
* Certificate validity dates

Additional certificate information can be displayed with:

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -text
```

---

# 7. Let's Encrypt

**Let's Encrypt** is a free, automated Certificate Authority that provides SSL/TLS certificates.

It allows website owners to obtain certificates without purchasing them from a traditional commercial CA.

Let's Encrypt certificates are commonly used with web servers such as:

* NGINX
* Apache

Certificate generation and web server configuration will be practiced later after completing the **Web Servers** topic.

---

# 8. Practical Work Status

| Task Requirement                        | Status            |
| --------------------------------------- | ----------------- |
| Research SSL/TLS certificate purpose    | Completed         |
| Research certificate components         | Completed         |
| Understand certificate validation types | Completed         |
| Understand certificate verification     | Completed         |
| Generate Let's Encrypt certificate      | After Web Servers |
| Configure NGINX/Apache                  | After Web Servers |
| Force HTTPS                             | After Web Servers |
| Verify configured certificate           | After Web Servers |

---

# 9. Security Considerations

When working with TLS certificates:

* Keep the certificate's **private key secret**.
* Never commit private keys such as `.key` or `.pem` files containing private key material to a public GitHub repository.
* Renew certificates before they expire.
* Use trusted Certificate Authorities.
* Use modern TLS versions such as TLS 1.2 or TLS 1.3.
* Disable obsolete protocols such as SSL 2.0 and SSL 3.0.
* Verify that certificate hostnames match the requested domain.
* Protect server private keys with appropriate file permissions.

---

# 10. Key Learnings

* SSL certificates are commonly used as a term for **TLS certificates**.
* TLS certificates help provide **server authentication** and bind a public key to a domain.
* TLS provides encrypted communication and data integrity.
* A certificate contains information such as the domain name, public key, validity period, issuer, and digital signature.
* Modern certificates commonly use **Subject Alternative Name (SAN)** for hostname validation.
* Certificate Authorities issue and sign certificates.
* Certificates use a **chain of trust** involving root and intermediate CAs.
* DV, OV, and EV represent different certificate validation approaches.
* Browsers verify certificates before establishing trusted HTTPS communication.
* OpenSSL can be used to inspect certificates and TLS connections.
* Let's Encrypt provides free, automated SSL/TLS certificates.
* Private keys must be protected and should never be exposed publicly.

---

# Conclusion

In this task, I learned the purpose and structure of **SSL/TLS certificates** and how they contribute to secure HTTPS communication.

I studied important certificate components including the **Certificate Authority, subject, Subject Alternative Name, validity period, public key, signature algorithm, fingerprint, and certificate chain**.

I also learned how browsers verify certificates and the differences between **DV, OV, and EV** certificate validation.

The practical steps involving **Let's Encrypt, NGINX/Apache configuration, and forcing HTTPS** will be completed after learning the Web Servers topic.
