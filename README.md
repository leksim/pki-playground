# Certificate Forge

Certificate Forge is an interactive, browser-based playground for learning how
digital certificates are issued. It walks through the complete certificate
request flow, from generating a key pair to approving or rejecting a request as
a Certificate Authority (CA) officer.

The cryptographic artifacts are real: the application uses the Web Crypto API
to generate RSA keys, sign a PKCS#10 Certificate Signing Request (CSR), and
issue an X.509 certificate. Everything runs locally in the browser with no
backend and no build step.

## What You Can Explore

1. Generate an RSA 2048-bit public/private key pair.
2. Enter the certificate subject identity.
3. Build and sign a PKCS#10 CSR.
4. Simulate submitting the CSR to a CA.
5. Approve or reject the request as a CA registration officer.
6. Issue and download a signed X.509 certificate.

You can also download the generated CSR, certificate, and unencrypted PKCS#8
private key in PEM format.

## Run Locally

Clone the repository and serve it with any static HTTP server:

```bash
git clone git@github.com:leksim/pki-playground.git
cd pki-playground
python3 -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000).

There are no dependencies to install. Opening `index.html` directly may work,
but using `localhost` is recommended because browser security policies can
restrict Web Crypto and clipboard features for local files.

## Deploy to GitHub Pages

This repository is ready to serve as a static GitHub Pages site:

1. Open the repository's **Settings**.
2. Select **Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select the `main` branch and `/ (root)` folder, then save.

## Verify Downloaded Artifacts

After completing the flow and downloading the files, inspect them with
OpenSSL:

```bash
openssl req -in example.csr -text -noout -verify
openssl x509 -in example.crt -text -noout
openssl pkey -in example.key -text -noout
```

Replace the filenames with the Common Name used in the playground.

## Implementation

Certificate Forge is implemented in a single [`index.html`](index.html) file
using:

- HTML, CSS, and vanilla JavaScript
- Web Crypto API for RSA key generation and signing
- A small built-in DER/ASN.1 encoder for PKCS#10 and X.509 structures
- No external JavaScript libraries or server-side components

The generated keys and CA key exist only in the current page's memory. Reloading
or restarting the playground discards them.

## Security and Scope

This project is for education and experimentation only. Do not use its output
for production systems.

- The CA and identity-verification process are simulated.
- The generated root CA is ephemeral and is not exported as a trust anchor.
- Private keys are exportable and downloaded without encryption.
- Certificates contain a minimal X.509 structure and omit production extensions
  such as Subject Alternative Name, Key Usage, and Basic Constraints.
- No certificate revocation, chain validation, secure key storage, or hardware
  key protection is implemented.

For production PKI, use established tooling and protect private keys with an
appropriate secrets-management or hardware-security solution.
