# Wildcard SSL CSR Private Key Creation Guide

### Step-by-step guide to generate a secure private key and CSR for a GlobalSign Wildcard SSL certificate using OpenSSL

### 🔐 1. Generate a Secure Private Key
GlobalSign typically recommends a 2048-bit or 4096-bit RSA key (4096-bit is more secure).
```shell
openssl genrsa -out server.key 4096
```
### CSR with SAN support

```shell
nano wildcard.cnf
```
```shell
[ req ]
default_bits       = 4096
prompt             = no
default_md         = sha256
req_extensions     = req_ext
distinguished_name = dn

[ dn ]
C  = US
ST = State
L  = City
O  = CompanyName
OU = IT
CN = *.yourdomain.com

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = *.yourdomain.com
DNS.2 = yourdomain.com

```
### Generate CSR:
```shell
openssl req -new -key wildcard.key -out wildcard.csr -config wildcard.cnf
```
### ✔ Verify CSR before uploading to GlobalSign
```shell
openssl req -text -noout -verify -in wildcard.csr
```
* Look for:
```shell
  Subject: CN=*.yourdomain.com
  X509v3 Subject Alternative Name:
    DNS:*.yourdomain.com, DNS:yourdomain.com  (if SAN used)
```
Next step: upload wildcard.csr to GlobalSign
🔑 Keep wildcard.key safe — never share it.
