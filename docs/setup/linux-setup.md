# 🐧 Linux Setup Guide

> **C!p#3r Club IIST** | Get your hacking environment ready in 3 options

---

## Choose Your Setup Path

```
Option A: Virtual Machine (VirtualBox/VMware) ← Recommended for beginners
Option B: WSL2 on Windows                     ← Quick start on Windows
Option C: Dual Boot / Bare Metal               ← Best performance
```

---

## Option A — Virtual Machine (Recommended)

### Step 1: Install VirtualBox (Free)

1. Go to [virtualbox.org](https://www.virtualbox.org/wiki/Downloads)
2. Download **VirtualBox for your OS** (Windows/macOS/Linux)
3. Download the **VirtualBox Extension Pack** (same page)
4. Install VirtualBox, then install the Extension Pack

### Step 2: Download Kali Linux ISO

1. Go to [kali.org/get-kali](https://www.kali.org/get-kali/)
2. Select **Installer Images → 64-bit**
3. Download the `.iso` file (~3–4 GB)

> **Alternatively:** Download the pre-built VirtualBox image (`.ova` file) from the same page — fastest option!

### Step 3: Create the Virtual Machine

**If using .ova (pre-built image):**
```
VirtualBox → File → Import Appliance → Select .ova → Import
```

**If using .iso (manual install):**
```
1. VirtualBox → Machine → New
2. Name: Kali Linux | Type: Linux | Version: Debian (64-bit)
3. RAM: 4096 MB (minimum 2048)
4. Hard Disk: Create new → VDI → Dynamically allocated → 50 GB
5. Settings → Storage → Add ISO as optical drive
6. Start the VM → Follow Kali installer
```

### Step 4: Kali Linux Initial Setup

```bash
# Update system (run in Kali terminal)
sudo apt update && sudo apt upgrade -y

# Install Guest Additions for better performance
sudo apt install -y virtualbox-guest-utils virtualbox-guest-x11
sudo reboot

# Enable clipboard sharing:
# VirtualBox → Devices → Shared Clipboard → Bidirectional
```

### Recommended VM Settings

| Setting | Recommended |
|---|---|
| RAM | 4–8 GB |
| CPU cores | 2–4 |
| Video memory | 128 MB |
| Storage | 50–80 GB |
| Network | NAT (for internet) + Host-Only (for lab) |

---

## Option B — WSL2 on Windows (Quick Start)

WSL2 (Windows Subsystem for Linux) lets you run Linux inside Windows without a VM.

### Install WSL2 with Kali Linux

```powershell
# Open PowerShell as Administrator:
wsl --install -d kali-linux

# After reboot, update Kali:
sudo apt update && sudo apt upgrade -y

# Install the full Kali toolset:
sudo apt install -y kali-linux-default

# Install Win-KeX (Kali desktop in Windows):
sudo apt install -y kali-win-kex
kex --win -s
```

### WSL2 Limitations

- ❌ No raw socket support (nmap SYN scan won't work)
- ❌ No full kernel control
- ❌ Some forensics tools won't work properly
- ✅ Most CTF tools work fine
- ✅ Fast startup and easy file sharing with Windows

---

## Option C — Dual Boot Kali Linux

> ⚠️ Advanced — risk of data loss. Back up your data first!

### Steps

1. Download Kali Linux ISO from [kali.org](https://kali.org/get-kali/)
2. Create bootable USB with [Balena Etcher](https://etcher.balena.io/)
3. Shrink Windows partition (Disk Management → right-click C: → Shrink)
4. Boot from USB → Install Kali alongside Windows
5. GRUB bootloader will let you choose OS at startup

---

## Kali Linux Post-Installation Setup

After any installation method, run these:

```bash
# 1. Update everything
sudo apt update && sudo apt full-upgrade -y

# 2. Install essential tools
sudo apt install -y \
    curl wget git vim nano htop \
    nmap wireshark tshark \
    burpsuite gobuster ffuf \
    john hashcat hydra \
    ghidra gdb radare2 \
    binwalk exiftool steghide \
    seclists wordlists \
    python3-pip python3-venv

# 3. Install Python packages
pip3 install pwntools pycryptodome gmpy2 requests

# 4. Extract rockyou wordlist
sudo gunzip /usr/share/wordlists/rockyou.txt.gz

# 5. Set up GDB with PEDA
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit

# 6. Verify installations
nmap --version
python3 -c "import pwn; print('pwntools OK')"
```

---

## 🔧 Useful Kali Linux Tips

### Customize Your Terminal

```bash
# Install Zsh + Oh My Zsh (better terminal)
sudo apt install -y zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Useful aliases to add to ~/.zshrc or ~/.bashrc
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade -y'
alias myip='ip a | grep "inet " | grep -v 127'
alias ports='ss -tulpn'
alias py='python3'
```

### Shared Folder Between VM and Host

```bash
# VirtualBox: Devices → Shared Folders → Add
# Mount name: "shared"

# In Kali:
sudo mkdir /mnt/shared
sudo mount -t vboxsf shared /mnt/shared
```

### Taking Snapshots

> **Always take a snapshot before trying anything risky!**

```
VirtualBox → Machine → Take Snapshot
```

---

## Networking Lab Setup

For practicing network attacks, set up a local lab:

```
Your Host (Windows/Mac)
    │
    ├── Kali Linux VM (attacker)  → Host-Only Adapter
    │
    └── Target VM (vulnerable)   → Host-Only Adapter
        Examples:
        - Metasploitable2 (VulnHub)
        - DVWA (Damn Vulnerable Web App)
        - VulnHub machines
```

### Download Practice Targets

- **Metasploitable2:** [sourceforge.net/projects/metasploitable](https://sourceforge.net/projects/metasploitable/)
- **DVWA:** [github.com/digininja/DVWA](https://github.com/digininja/DVWA)
- **VulnHub machines:** [vulnhub.com](https://vulnhub.com)

---

<div align="center">

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

</div>
