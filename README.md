# WPS Audit Tool

A lightweight Python orchestrator for auditing WPS (Wi-Fi Protected Setup) security on wireless networks. It automates **Pixie Dust** and **PIN brute-force** attacks by wrapping established, industry-standard tools — it does not reimplement any cryptographic attacks itself.

> ⚠️ **Legal notice**
> Only use this tool against networks **you own** or have **explicit written authorization** to test. Accessing or attempting to access a wireless network without authorization is illegal in most jurisdictions (e.g. the U.S. Computer Fraud and Abuse Act, UK Computer Misuse Act, and equivalents elsewhere). You are solely responsible for how you use this software. See [LICENSE](./LICENSE) for the full disclaimer of liability.

## What it does

1. Puts a wireless adapter into monitor mode (`airmon-ng`)
2. Scans for WPS-enabled access points and their lock status (`wash`)
3. Lets you select a target from the discovered list
4. Runs Pixie Dust (`reaver -K`), PIN brute-force (`bully`), or both
5. Restores the adapter to managed mode when finished

## Requirements

**Hardware**
- A wireless adapter that supports **monitor mode and packet injection**. Most built-in laptop Wi-Fi chips do not support this — a USB adapter such as the Alfa AWUS036ACH/ACS is commonly used.

**OS**
- Linux (tested conceptually for Debian/Ubuntu/Kali-style distros)

**System packages**
```bash
sudo apt update
sudo apt install -y aircrack-ng reaver bully pixiewps
```

**Python**
- Python 3.8+
- No pip packages required — see [requirements.txt](./requirements.txt)

## Usage

```bash
sudo python3 wps_audit.py
```

You'll be prompted to:
1. Choose your wireless interface (e.g. `wlan0`)
2. Wait while it scans for WPS-enabled networks
3. Select a target index from the list
4. Choose an attack mode: Pixie Dust, PIN brute-force, or both

The script must run as root because it needs to control the wireless interface directly (enabling monitor mode, injecting packets).

## Notes on effectiveness

- **Pixie Dust** only succeeds against routers with a vulnerable WPS chipset/implementation. Many modern routers have patched the underlying weakness, so it will fail cleanly against those.
- **PIN brute-force** is slow (can take hours) and many routers lock WPS after a handful of failed attempts. The tool checks and reports lock status (`bully -d`) before and during the attack.
- Results (recovered PIN / PSK) are printed by the underlying tool (`reaver`/`bully`) to the terminal — this wrapper does not currently persist them to a file.

## Disclaimer

This tool is provided for **authorized security testing and educational purposes only**. The author(s) accept no responsibility for misuse or for any damage caused by this software. See [LICENSE](./LICENSE).
