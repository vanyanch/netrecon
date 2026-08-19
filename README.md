# NetRecon

Asynchronous TCP port scanner with service detection and VNC/RFB support.

---

## Overview

NetRecon is a lightweight network reconnaissance tool written in Python. It uses `asyncio` for high-speed scanning with real-time progress feedback and service banner grabbing.

---

## Features

- **Asynchronous scanning** — Fast scanning with configurable concurrency
- **Service detection** — Grabs banners from SSH, FTP, SMTP, and other services
- **VNC/RFB support** — Parses VNC protocol versions
- **Colored output** — Clear visual feedback
- **Progress bar** — Real-time scan progress
- **Cross-platform** — Works on Windows and Linux

---

## Quick Start

```bash
git clone https://github.com/vanyanch/netrecon
cd netrecon
python netrecon.py scanme.nmap.org
```

---

## Usage

### Basic Scan
Scans common ports and VNC range (5900-5909):
```bash
python netrecon.py 192.168.1.1
```

### Full Port Scan
Scans all 65,535 TCP ports:
```bash
python netrecon.py scanme.nmap.org --all
```

### Adjust Concurrency
Control parallel connections (default: 1000):
```bash
python netrecon.py 192.168.1.1 --concurrency 500
```

---

## Command Reference

| Flag | Description |
| :--- | :--- |
| `target` | Target IP or hostname (required) |
| `--all` | Scan all 65,535 ports |
| `--concurrency N` | Max parallel connections (default: 1000) |
| `-h, --help` | Show help message |

---

## Output Example

```text
\$ python netrecon.py scanme.nmap.org

🚀 Launching scan for: scanme.nmap.org (45.33.32.156)
🔍 Mode: Popular ports & VNC range (24 ports)
⚙️  Socket concurrency limit: 1000
------------------------------------------------------------
[+] Port 22    [OPEN] -> SSH
    └── Banner: SSH-2.0-OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.13
[+] Port 80    [OPEN] -> HTTP
[+] Port 443   [OPEN] -> HTTPS
[*] Progress: |██████████████████████████████| 100.0% Complete
------------------------------------------------------------
🎯 Target scanning completed successfully.
```

---

## Banner Grabbing

### Supported Services
The scanner grabs banners from services that send an initial greeting:
- **SSH** — OpenSSH, Dropbear
- **FTP** — vsftpd, ProFTPD
- **SMTP** — Postfix, Sendmail
- **POP3/IMAP** — Dovecot
- **VNC** — RealVNC, TightVNC, TigerVNC

### VNC Detection
VNC servers are detected via RFB handshake parsing. The scanner looks for the `RFB` prefix in the initial response and displays the protocol version.

---

## Performance

The scanner uses `asyncio.Semaphore` to control concurrent connections. Default concurrency is 1000, which balances speed and resource usage.

### Recommended Settings

| Environment | Recommended Concurrency |
| :--- | :--- |
| Linux | 1000 - 2000 |
| Windows | 500 - 1000 |
| Low-spec machine | 200 - 500 |

---

## Technical Details

- **Concurrency Control:** The semaphore limits simultaneous socket connections to prevent file descriptor exhaustion. Each port scan runs as an independent task.
- **Windows Compatibility:** On Windows, the script uses `WindowsSelectorEventLoopPolicy` internally to ensure stable asyncio behavior.
- **Color Output:** ANSI escape codes provide colored output on compatible terminals.

---

## Dependencies

- Python 3.8+
- **No external dependencies** (Pure standard library)

---

## License

This project is licensed under the [GNU GPL v3 License](LICENSE).

---

## Author

Created by **vanyanch**