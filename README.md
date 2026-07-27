## IoT Security Project - Secure Key Exchange between IoT Device and Server

## Overview

This project demonstrates how an IoT device securely exchanges a session key with a server using Elliptic Curve Diffie–Hellman (ECDH). After the key exchange, both parties derive the same session key and use it to encrypt sensor data.

This project is developed for the IoT Security course.

---

## Objectives

- Understand TLS Handshake
- Understand ECDH Key Exchange
- Generate Shared Secret
- Derive Session Key
- Encrypt Sensor Data using AES-GCM
- Analyze Security Risks (MITM, Static Keys)

---

## Project Structure

secure-key-exchange-iot-server/
│
├── 📁 src/                        # Python source code
│   ├── client.py                  # IoT Device (Client)
│   ├── server.py                  # Server
│   ├── crypto_utils.py            # Cryptographic utilities
│   ├── config.py                  # Configuration settings
│   ├── gui.py                     # Tkinter GUI
│   └── iot_tls_demo.py           # Entry point
│
├── 📁 results/                    # Results and output
│   ├── images/                    # System diagrams
│   │   ├── system_architecture.png
│   │   ├── tls_handshake.png
│   │   ├── ecdh_process.png
│   │   └── gui/                   # GUI screenshots
│   ├── demo_output.txt            # Demo execution log
│   └── session_keys.log           # Key generation logs
│
├── 📁 reports/                    # Documentation
│   ├── report.docx                # Full report (Vietnamese)
│   └── report.pdf                 # PDF version
│
├── 📁 references/                 # Reference materials
│   └── references.md              # References list
│
├── 📄 README.md                   # This file
├── 📄 LICENSE                     # MIT License
└── 📄 .gitignore                  # Git ignore rules

---

## Technologies

- Python 3
- cryptography
- AES-GCM
- ECDH
- TLS Concepts

---

## References

- Mbed TLS
- OWASP ISVS

---

## Author

Student: Huynh Hong Ngoc Thach

Course: Bao Mat IOT - HK3

University: Van Hien University