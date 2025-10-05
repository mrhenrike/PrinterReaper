# 🔥 Welcome to PrinterReaper Wiki

**Advanced Printer Penetration Testing Toolkit**

> *"Is your printer safe from the void? Find out before someone else does…"*

> **📍 Official Website**: [www.uniaogeek.com.br/printer-reaper](https://www.uniaogeek.com.br/printer-reaper/)

---

## 🎯 What is PrinterReaper?

PrinterReaper v2.5.3 is a **complete, powerful toolkit** for **penetration testing network printers**, supporting **PJL, PostScript, and PCL** with **4 network protocols**. It enables security professionals to discover, assess, and exploit printer vulnerabilities through an intuitive command-line interface.

### Key Features

- **🎯 PJL-Focused** - Specialized in Printer Job Language exploitation
- **📋 54+ Commands** - Complete PJL command coverage across 7 categories
- **🔧 File Operations** - Upload, download, manipulate printer files
- **🔒 Security Testing** - Lock, unlock, backup, restore, NVRAM access
- **💥 Attack Vectors** - Physical damage, DoS, job retention, formatting
- **📚 Complete Documentation** - Every command has detailed help
- **⚡ High Performance** - Optimized for speed and reliability
- **🛡️ Error Handling** - Robust error handling with user-friendly messages

---

## 🚀 Quick Links

### Getting Started
- **[Installation Guide](Installation)** - Set up PrinterReaper in minutes
- **[Quick Start](Quick-Start)** - Your first printer test in 5 minutes
- **[Commands Reference](Commands-Reference)** - Complete command list

### Advanced Usage
- **[PJL Commands](PJL-Commands)** - Detailed PJL command reference
- **[Security Testing](Security-Testing)** - Security assessment workflows
- **[Attack Vectors](Attack-Vectors)** - Exploitation techniques
- **[Examples](Examples)** - Real-world usage examples

### Help & Support
- **[FAQ](FAQ)** - Frequently asked questions
- **[Troubleshooting](Troubleshooting)** - Common issues and solutions
- **[Architecture](Architecture)** - Technical architecture overview

---

## 📊 PrinterReaper at a Glance

```
┌─────────────────────────────────────────────────────────────┐
│                    PrinterReaper v2.4.2                     │
├─────────────────────────────────────────────────────────────┤
│  📁 Filesystem     │  12 commands │ Upload, download, copy │
│  ℹ️  Information    │   8 commands │ ID, version, network   │
│  ⚙️  Control        │   8 commands │ Restart, reset, backup │
│  🔒 Security       │   4 commands │ Lock, unlock, NVRAM    │
│  💥 Attacks        │   8 commands │ Destroy, flood, format │
│  🌐 Network        │   3 commands │ Direct, execute, load  │
│  📊 Monitoring     │   2 commands │ Pagecount, status      │
├─────────────────────────────────────────────────────────────┤
│  Total             │  54 commands │ 100% documented        │
└─────────────────────────────────────────────────────────────┘
```

---

## 🎓 What You Can Do

### File System Access
- Browse printer's file system (folders, files)
- Download configuration files and logs
- Upload arbitrary files to printer
- Mirror entire filesystem locally
- Manipulate file permissions

### Information Gathering
- Identify printer model and firmware
- Extract network configuration
- Dump NVRAM contents
- List environment variables
- Get system uptime and page count

### Control & Configuration
- Change printer settings
- Display custom messages on panel
- Take printer offline
- Backup and restore configuration
- Perform self-tests

### Security Testing
- Test for authentication bypass
- Verify file system restrictions
- Check NVRAM security
- Test buffer overflows
- Path traversal testing

### Exploitation
- Capture print jobs
- Inject malicious data
- Cause denial of service
- Physical NVRAM damage
- Job retention attacks

---

## ⚠️ Legal Notice

**IMPORTANT**: PrinterReaper is intended **solely for authorized security testing**. 

- ✅ Run only against devices you own or have written permission to test
- ❌ Unauthorized use may violate laws and regulations
- ⚖️ The authors disclaim all liability for misuse or damage

**By using PrinterReaper, you accept full responsibility for your actions.**

---

## 📚 Documentation Structure

This wiki is organized into the following sections:

### 🚀 Getting Started
1. [Installation](Installation) - Install and configure
2. [Quick Start](Quick-Start) - First steps tutorial
3. [Commands Reference](Commands-Reference) - All commands overview

### 📖 Command Details
4. [PJL Commands](PJL-Commands) - PJL-specific commands
5. [Filesystem Commands](Filesystem-Commands) - File operations
6. [Security Commands](Security-Commands) - Security testing
7. [Attack Commands](Attack-Commands) - Exploitation techniques

### 🔬 Advanced Topics
8. [Security Testing](Security-Testing) - Testing workflows
9. [Attack Vectors](Attack-Vectors) - Exploitation methods
10. [Examples](Examples) - Real-world scenarios
11. [Architecture](Architecture) - System design

### 🆘 Support
12. [FAQ](FAQ) - Common questions
13. [Troubleshooting](Troubleshooting) - Problem solving
14. [Contributing](Contributing) - How to contribute

---

## 🌟 Key Highlights

### Version 2.4.2 Features

**✨ New in 2.4.2:**
- HTML wiki for website deployment
- QA tested with 100% pass rate
- Complete toolkit: 109 commands
- 3 printer languages (PJL, PS, PCL)
- 4 network protocols (RAW, LPD, IPP, SMB)
- 5 attack payloads
- Comprehensive documentation

**⚡ Performance:**
- 30-second timeout configuration
- Robust retry logic
- Connection pooling ready
- Optimized for reliability

**🛡️ Security:**
- Safe mode verification
- Destructive command warnings
- Comprehensive error handling
- No sensitive data leakage

---

## 🎯 Typical Workflow

```
1. Discovery
   └─> ./printer-reaper.py        # Find printers on network

2. Connect
   └─> ./printer-reaper.py <target> pjl

3. Reconnaissance
   ├─> id                          # Identify printer
   ├─> ls                          # Browse filesystem
   ├─> network                     # Get network info
   └─> info config                 # Get configuration

4. Exploitation
   ├─> download /etc/passwd        # Exfiltrate sensitive files
   ├─> upload backdoor.ps          # Upload malicious files
   ├─> lock 12345                  # Lock control panel
   └─> capture                     # Capture print jobs

5. Cleanup
   ├─> delete backdoor.ps          # Remove evidence
   └─> exit                        # Disconnect
```

---

## 📞 Need Help?

- **Wiki Navigation**: Use the sidebar to browse all topics
- **Command Help**: Type `help <command>` in the shell
- **Search**: Use GitHub's wiki search feature
- **Issues**: Report bugs on GitHub Issues
- **Contact**: X / LinkedIn @mrhenrike

---

## 🚀 Ready to Start?

**→ [Install PrinterReaper Now](Installation)**

**→ [Quick Start Guide](Quick-Start)**

---

<div align="center">

**PrinterReaper v2.5.3**  
*Complete Printer Penetration Testing Toolkit*

**109 Commands | 3 Languages | 4 Protocols | 5 Payloads**

Made with ❤️ for the security community

---

### Powered by União Geek

[![União Geek](../img/logotype-uniaogeek-2.png)](https://www.uniaogeek.com.br)

**[www.uniaogeek.com.br](https://www.uniaogeek.com.br)** | **[Blog](https://www.uniaogeek.com.br/blog)**

</div>

