# 🏗️ PrinterReaper v2.4.2 - Complete Architecture Diagram

**Version**: 2.4.2  
**Date**: October 4, 2025  
**Type**: System Architecture & Data Flow

---

## 📊 ASCII Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                         PrinterReaper v2.4.2 - Complete Architecture                     │
│                              109 Commands | 3 Languages | 4 Protocols                    │
└─────────────────────────────────────────────────────────────────────────────────────────┘
                                              │
                                              ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  USER INTERFACE                                                                           │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  User Command:  ls /etc/passwd                                                            │
│  Mode Selected: pjl | ps | pcl | auto                                                     │
│  Protocol:      RAW(9100) | LPD(515) | IPP(631) | SMB(445)                               │
└──────────────┬────────────────────────────────────────────────────────────┬──────────────┘
               │                                                              │
               ▼                                                              ▼
┌──────────────────────────────┐                              ┌──────────────────────────┐
│   ENTRY POINT                │                              │   DISCOVERY ENGINE       │
├──────────────────────────────┤                              ├──────────────────────────┤
│  printer-reaper.py           │                              │  discovery.py            │
│  └─> main.py                 │                              │  ├─> SNMP Scanning       │
│      ├─> Arg Parsing         │                              │  ├─> 17 OID Queries      │
│      ├─> OS Detection        │◄─────────────────────────────┤  ├─> Network Enum        │
│      ├─> Capability Check    │                              │  └─> Printer List        │
│      └─> Shell Selection     │                              └──────────────────────────┘
└──────────────┬───────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  LANGUAGE SELECTOR (Auto-detect or Manual)                                                │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  capabilities.py: IPP + HTTP + HTTPS + SNMP → Detect: PJL / PostScript / PCL            │
│  Priority: PJL > PostScript > PCL                                                         │
└──────────────┬───────────────┬───────────────────────────────────────┬───────────────────┘
               │               │                                       │
               ▼               ▼                                       ▼
┌───────────────────┐  ┌──────────────────────┐  ┌─────────────────────────────┐
│  PJL MODULE       │  │  POSTSCRIPT MODULE   │  │  PCL MODULE                 │
├───────────────────┤  ├──────────────────────┤  ├─────────────────────────────┤
│  pjl.py           │  │  ps.py               │  │  pcl.py                     │
│  54 Commands      │  │  40 Commands         │  │  15 Commands                │
├───────────────────┤  ├──────────────────────┤  ├─────────────────────────────┤
│ 📁 Filesystem(12) │  │ ℹ️ Information(8)     │  │ 📊 Virtual FS(3)            │
│ ℹ️ Info(8)        │  │ 📁 Filesystem(4)     │  │ ℹ️ Info(2)                  │
│ ⚙️ Control(8)     │  │ 🔒 Security(5)       │  │ ⚙️ Control(3)               │
│ 🔒 Security(4)    │  │ 💥 Attacks(6)        │  │ 💥 Attacks(2)               │
│ 💥 Attacks(8)     │  │ 🔧 Advanced(11)      │  │ ⚡ Execute(5)               │
│ 🌐 Network(3)     │  │ 💣 Payloads(6)       │  │                             │
│ 📊 Monitor(2)     │  │                      │  │                             │
└────────┬──────────┘  └──────────┬───────────┘  └──────────┬──────────────────┘
         │                        │                          │
         │                        ├──────────────────────────┘
         │                        │
         ▼                        ▼
┌────────────────────────────────────────────────┐
│  TRANSLATOR ENGINE                             │
├────────────────────────────────────────────────┤
│  printer.py (Base Class)                       │
│  ├─> Command Translation                       │
│  ├─> Protocol Wrapping                         │
│  ├─> Error Handling                            │
│  └─> Response Parsing                          │
└──────────────┬─────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  AUXILIARY ENGINES                                                                        │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐  ┌────────────────┐ │
│  │  CODEBOOK ENGINE │  │  FUZZER ENGINE   │  │  OPERATORS DB    │  │  PAYLOAD SYS   │ │
│  ├──────────────────┤  ├──────────────────┤  ├──────────────────┤  ├────────────────┤ │
│  │ codebook.py      │  │ fuzzer.py        │  │ operators.py     │  │ payloads/      │ │
│  │ 378 Error Codes  │  │ 381 Vectors      │  │ 371 Operators    │  │ 5 Payloads     │ │
│  │                  │  │                  │  │                  │  │                │ │
│  │ 10xxx-50xxx      │  │ • fuzz_paths()   │  │ • File ops       │  │ • banner.ps    │ │
│  │ PJL Errors       │  │ • fuzz_names()   │  │ • Control ops    │  │ • loop.ps      │ │
│  │ Status Codes     │  │ • fuzz_data()    │  │ • Dict ops       │  │ • erase.ps     │ │
│  │ Hardware Errors  │  │ • traversal()    │  │ • Security ops   │  │ • storm.ps     │ │
│  │                  │  │                  │  │                  │  │ • exfil.ps     │ │
│  └──────────────────┘  └──────────────────┘  └──────────────────┘  └────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  PROTOCOL LAYER (Multi-Protocol Support)                                                  │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │
│  │ RAW Protocol │  │ LPD Protocol │  │ IPP Protocol │  │ SMB Protocol │               │
│  ├──────────────┤  ├──────────────┤  ├──────────────┤  ├──────────────┤               │
│  │ raw.py       │  │ lpd.py       │  │ ipp.py       │  │ smb.py       │               │
│  │ Port: 9100   │  │ Port: 515    │  │ Port: 631    │  │ Port: 445    │               │
│  │              │  │              │  │              │  │              │               │
│  │ DEFAULT      │  │ Queue-based  │  │ HTTP-based   │  │ Windows      │               │
│  │ AppSocket/   │  │ RFC 1179     │  │ RFC 2910     │  │ Network      │               │
│  │ JetDirect    │  │ Legacy       │  │ Modern       │  │ Printing     │               │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘               │
└─────────┼──────────────────┼──────────────────┼──────────────────┼───────────────────────┘
          │                  │                  │                  │
          └──────────────────┴──────────────────┴──────────────────┘
                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  CONNECTION MANAGER (helper.py - conn class)                                              │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ├─> Socket Management (TCP/IP)                                                           │
│  ├─> Timeout Control (30s default)                                                        │
│  ├─> Send/Receive with Error Handling                                                     │
│  ├─> Delimiter Detection                                                                  │
│  ├─> Retry Logic (3 attempts)                                                             │
│  └─> Connection Pooling (ready)                                                           │
└──────────────┬────────────────────────────────────────────────────────────────────────────┘
               │
               ▼
         [NETWORK LAYER]
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              TARGET PRINTER                                               │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐               │
│  │  PJL Engine  │  │  PS Engine   │  │  PCL Engine  │  │  Filesystem  │               │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘               │
└─────────────────────────────────────────────────────────────────────────────────────────┘
               │
               ▼
    [Response: Files, Status, Data]
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  RESPONSE PROCESSING PIPELINE                                                             │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  Connection Manager → Parser → Error Handler → Result Formatter → User Display          │
│                                                                                           │
│  ├─> PJL Error Codes (codebook.py) → User-friendly messages                             │
│  ├─> PostScript Errors (PS_ERROR regex) → Detailed errors                               │
│  ├─> PCL Responses → Macro tracking                                                      │
│  └─> File Data → Local storage (exfiltrated/, downloads/)                               │
└─────────────────────────────────────────────────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│  OUTPUT & LOGGING                                                                         │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ├─> Terminal Output (helper.py - output class)                                          │
│  │   ├─> Colored messages (colorama)                                                     │
│  │   ├─> Status indicators                                                               │
│  │   └─> Error highlighting                                                              │
│  │                                                                                        │
│  ├─> File Logging (optional -o flag)                                                     │
│  │   └─> Raw command log                                                                 │
│  │                                                                                        │
│  └─> Debug Output (optional -d flag)                                                     │
│      ├─> Sent data (cyan background)                                                     │
│      └─> Received data (magenta background)                                              │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Detailed Component Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                    PRINTERREAPER v2.4.2                                   │
│                                    LAYERED ARCHITECTURE                                   │
└─────────────────────────────────────────────────────────────────────────────────────────┘

LAYER 1: PRESENTATION LAYER
═══════════════════════════════════════════════════════════════════════════════════════════
┌───────────────────────────────────────────────────────────────────────────────────────┐
│  CLI Interface (cmd.Cmd based)                                                          │
│  ├─> Command Line Parsing (argparse)                                                    │
│  ├─> Interactive Shell (cmd.Cmd)                                                        │
│  ├─> Help System (100% coverage)                                                        │
│  ├─> Tab Completion                                                                     │
│  └─> Command History                                                                    │
└───────────────────────────────────────────────────────────────────────────────────────┘

LAYER 2: APPLICATION LAYER
═══════════════════════════════════════════════════════════════════════════════════════════
┌───────────────────────────────────────────────────────────────────────────────────────┐
│  Language Modules (Inherits from printer.py)                                            │
│  ┌─────────────────────┐  ┌──────────────────────┐  ┌────────────────────────┐       │
│  │  PJL Module         │  │  PostScript Module   │  │  PCL Module            │       │
│  │  pjl.py (2,840L)    │  │  ps.py (962L)        │  │  pcl.py (411L)         │       │
│  ├─────────────────────┤  ├──────────────────────┤  ├────────────────────────┤       │
│  │ • 54 Commands       │  │ • 40 Commands        │  │ • 15 Commands          │       │
│  │ • Filesystem        │  │ • Dictionary Ops     │  │ • Virtual FS (Macros)  │       │
│  │ • NVRAM Access      │  │ • Operator Enum      │  │ • Basic Control        │       │
│  │ • Job Control       │  │ • Print Job Manip    │  │ • Legacy Support       │       │
│  │ • Lock/Unlock       │  │ • Overlay/Cross      │  │ • Macro Management     │       │
│  └─────────────────────┘  └──────────────────────┘  └────────────────────────┘       │
└───────────────────────────────────────────────────────────────────────────────────────┘

LAYER 3: BUSINESS LOGIC LAYER
═══════════════════════════════════════════════════════════════════════════════════════════
┌───────────────────────────────────────────────────────────────────────────────────────┐
│  Base Printer Class (printer.py - 1,304 lines)                                          │
│  ┌─────────────────────────────────────────────────────────────────────────────────┐  │
│  │  CORE FUNCTIONS                                                                   │  │
│  │  ├─> Connection Management                                                        │  │
│  │  ├─> File Operations (upload, download, append, delete, cat, edit)              │  │
│  │  ├─> Filesystem Navigation (cd, pwd, chvol, traversal)                           │  │
│  │  ├─> Command Execution Framework                                                 │  │
│  │  ├─> Error Handling & Retry Logic                                                │  │
│  │  ├─> Signal Handling (Ctrl+C graceful)                                           │  │
│  │  ├─> Timeout Management (configurable)                                           │  │
│  │  ├─> Fuzzing Support                                                              │  │
│  │  └─> Mirror/Backup Functions                                                     │  │
│  └─────────────────────────────────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────────────────────────────────┘

LAYER 4: UTILITY LAYER
═══════════════════════════════════════════════════════════════════════════════════════════
┌───────────────────────────────────────────────────────────────────────────────────────┐
│  Utility Modules                                                                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ helper.py    │  │ codebook.py  │  │ fuzzer.py    │  │ operators.py │             │
│  │ (705 lines)  │  │ (451 lines)  │  │ (216 lines)  │  │ (447 lines)  │             │
│  ├──────────────┤  ├──────────────┤  ├──────────────┤  ├──────────────┤             │
│  │ • output()   │  │ • 378 codes  │  │ • 254 paths  │  │ • 371 ops    │             │
│  │ • conv()     │  │ • get_errors │  │ • 31 names   │  │ • 16 cats    │             │
│  │ • file()     │  │ • PJL codes  │  │ • fuzz_data  │  │ • File ops   │             │
│  │ • conn()     │  │ • 10-50xxx   │  │ • traversal  │  │ • Dict ops   │             │
│  │ • log()      │  │              │  │              │  │ • Security   │             │
│  │ • const()    │  │              │  │              │  │              │             │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘             │
└───────────────────────────────────────────────────────────────────────────────────────┘

LAYER 5: PROTOCOL LAYER
═══════════════════════════════════════════════════════════════════════════════════════════
┌───────────────────────────────────────────────────────────────────────────────────────┐
│  Network Protocol Implementations                                                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ RAW          │  │ LPD          │  │ IPP          │  │ SMB          │             │
│  │ raw.py       │  │ lpd.py       │  │ ipp.py       │  │ smb.py       │             │
│  │ (70 lines)   │  │ (180 lines)  │  │ (200 lines)  │  │ (120 lines)  │             │
│  ├──────────────┤  ├──────────────┤  ├──────────────┤  ├──────────────┤             │
│  │ Port: 9100   │  │ Port: 515    │  │ Port: 631    │  │ Port: 445    │             │
│  │ DEFAULT      │  │ Queue-based  │  │ HTTP/IPP     │  │ Windows SMB  │             │
│  │ Direct TCP   │  │ RFC 1179     │  │ RFC 2910     │  │ CIFS/SMB     │             │
│  │ Fastest      │  │ Legacy Unix  │  │ CUPS/Modern  │  │ via smbcli   │             │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘             │
└──────────────┬────────────────────────────────────────────────────────────────────────┘
               │
               ▼
LAYER 6: TRANSPORT LAYER
═══════════════════════════════════════════════════════════════════════════════════════════
┌───────────────────────────────────────────────────────────────────────────────────────┐
│  TCP/IP Stack (Python socket module)                                                    │
│  ├─> Socket Creation & Management                                                       │
│  ├─> Connection Establishment                                                           │
│  ├─> Data Transmission                                                                  │
│  ├─> Timeout Handling                                                                   │
│  └─> Error Recovery                                                                     │
└──────────────┬────────────────────────────────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                   PRINTER DEVICE                                          │
│  IP: xxx.xxx.xxx.xxx                                                                      │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐       │
│  │ PJL Interpreter│  │ PS Interpreter │  │ PCL Interpreter│  │  Filesystem    │       │
│  └────────────────┘  └────────────────┘  └────────────────┘  └────────────────┘       │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐       │
│  │  NVRAM         │  │  Job Queue     │  │  Network Stack │  │  Control Panel │       │
│  └────────────────┘  └────────────────┘  └────────────────┘  └────────────────┘       │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Complete Data Flow Diagram

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                            COMMAND EXECUTION FLOW                                        │
└────────────────────────────────────────────────────────────────────────────────────────┘

1. USER INPUT
   │
   │  User types: "download /etc/passwd"
   │  Mode: pjl
   │
   ▼

2. COMMAND PARSING (main.py → pjl.py)
   │
   │  cmd.Cmd.onecmd("download /etc/passwd")
   │  └─> pjl.do_download("/etc/passwd")
   │
   ▼

3. ARGUMENT PROCESSING (pjl.py)
   │
   │  Parse arguments:
   │  ├─> remote_file = "/etc/passwd"
   │  └─> local_path = "passwd" (basename)
   │
   ▼

4. PJL COMMAND CONSTRUCTION (pjl.py - cmd method)
   │
   │  Build PJL command:
   │  ┌────────────────────────────────────────┐
   │  │ %-12345X                               │ ← UEL (Universal Exit Language)
   │  │ @PJL FSDOWNLOAD NAME="/etc/passwd"     │ ← PJL Command
   │  │ @PJL ECHO DELIMITER42                  │ ← Response delimiter
   │  │ %-12345X                               │ ← UEL
   │  └────────────────────────────────────────┘
   │
   ▼

5. PROTOCOL WRAPPING (conn.send - helper.py)
   │
   │  Select protocol:
   │  ├─> RAW (default) → Direct TCP send
   │  ├─> LPD → Queue + control file
   │  ├─> IPP → HTTP POST request
   │  └─> SMB → smbclient call
   │
   ▼

6. NETWORK TRANSMISSION (socket)
   │
   │  TCP/IP Stack:
   │  ├─> Connect to printer:9100
   │  ├─> Send payload
   │  ├─> Set 30s timeout
   │  └─> Wait for response
   │
   ▼

7. PRINTER PROCESSING
   │
   │  Printer receives:
   │  ├─> Parse PJL command
   │  ├─> Execute FSDOWNLOAD
   │  ├─> Read /etc/passwd
   │  └─> Send file data
   │
   ▼

8. RESPONSE RECEPTION (conn.recv_until)
   │
   │  Receive data until:
   │  ├─> DELIMITER42 found
   │  ├─> Timeout (30s)
   │  └─> Connection closed
   │
   │  Watchdog protection:
   │  ├─> Monitor bytes received
   │  ├─> Visual feedback (large files)
   │  └─> Timeout detection
   │
   ▼

9. ERROR HANDLING (pjl_err method)
   │
   │  Check for errors:
   │  ├─> File errors (NONEXISTENT, permission denied)
   │  ├─> Status codes (CODE=xxxxx)
   │  └─> Display messages (DISPLAY="...")
   │
   │  Error lookup:
   │  └─> codebook.get_errors(code) → "READY (online)"
   │
   ▼

10. DATA PROCESSING
    │
    │  Process received data:
    │  ├─> Strip delimiters
    │  ├─> Remove UEL sequences
    │  ├─> Decode content
    │  └─> Validate size
    │
    ▼

11. FILE STORAGE (file.write - helper.py)
    │
    │  Save to local file:
    │  ├─> Path: ./passwd
    │  ├─> Mode: binary write
    │  └─> Size validation
    │
    ▼

12. USER FEEDBACK (output.message)
    │
    │  Display result:
    │  └─> "Downloaded /etc/passwd to ./passwd"
    │
    ▼

13. READY FOR NEXT COMMAND
    │
    └─> Prompt: "192.168.1.100:/>"
```

---

## 🎨 Module Interaction Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                         MODULE INTERACTION & DEPENDENCIES                                 │
└─────────────────────────────────────────────────────────────────────────────────────────┘

                                    main.py
                                       │
                    ┌──────────────────┼──────────────────┐
                    │                  │                  │
                    ▼                  ▼                  ▼
              osdetect.py       discovery.py      capabilities.py
                    │                  │                  │
                    │                  ├─> helper.py      ├─> helper.py
                    │                  └─> osdetect.py    └─> requests
                    │
                    ▼
            ┌───────┴───────┬───────────────┬────────────┐
            │               │               │            │
            ▼               ▼               ▼            ▼
         pjl.py          ps.py          pcl.py      printer.py (BASE)
            │               │               │            │
            │               │               │            ├─> helper.py (all)
            │               │               │            ├─> discovery.py
            │               │               │            └─> fuzzer.py
            │               │               │
            ├─> codebook.py │               │
            │   (378 codes) │               │
            │               ├─> operators.py│
            │               │   (371 ops)   │
            │               │               │
            │               ├─> payloads/   │
            │               │   (5 payloads)│
            │               │               │
            └───────────────┴───────────────┴─> helper.py
                                                ├─> output() - Display
                                                ├─> conv() - Conversion
                                                ├─> file() - File I/O
                                                ├─> conn() - Connection
                                                ├─> log() - Logging
                                                └─> const() - Constants

                    Network Layer (protocols/)
                                │
                    ┌───────────┼───────────┬───────────┐
                    │           │           │           │
                    ▼           ▼           ▼           ▼
                 raw.py      lpd.py      ipp.py      smb.py
                (Port 9100) (Port 515)  (Port 631)  (Port 445)
                    │           │           │           │
                    └───────────┴───────────┴───────────┘
                                │
                                ▼
                          Python socket
                                │
                                ▼
                          TARGET PRINTER
```

---

## 🔧 Engine Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                             PRINTERREAPER ENGINES                                         │
└─────────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 1. DISCOVERY ENGINE (discovery.py)                                                      │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  User runs without target                                                       │
│  Process:                                                                                │
│    ├─> Detect OS (Linux, Windows, WSL, macOS, BSD)                                     │
│    ├─> Enumerate network interfaces                                                    │
│    ├─> Build network list (192.168.x.0/24, 10.x.x.0/24)                               │
│    ├─> User selects networks to scan                                                   │
│    ├─> For each IP in selected networks:                                               │
│    │   ├─> SNMP GET OID_HRDEV_TYPE (is it a printer?)                                 │
│    │   ├─> If printer, collect 17 OIDs:                                                │
│    │   │   ├─> Device description                                                      │
│    │   │   ├─> System uptime                                                           │
│    │   │   ├─> Printer status                                                          │
│    │   │   ├─> Supply levels                                                           │
│    │   │   ├─> Network info                                                            │
│    │   │   └─> Entity information                                                      │
│    │   └─> Display results                                                             │
│    └─> Generate printer list                                                            │
│  Output: Discovered printer table                                                       │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 2. CAPABILITY ENGINE (capabilities.py)                                                  │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  Target IP + Mode                                                               │
│  Process:                                                                                │
│    ├─> Try IPP (Port 631):                                                             │
│    │   └─> Parse MDL: and CMD: from response                                           │
│    ├─> Try HTTP (Port 80):                                                             │
│    │   └─> Extract <title> from HTML                                                   │
│    ├─> Try HTTPS (Port 443):                                                           │
│    │   └─> Extract <title> from HTML (verify=False)                                    │
│    ├─> Try SNMP (Port 161):                                                            │
│    │   └─> Query hrDeviceDescr + prtInterpreterDescription                            │
│    ├─> Model database lookup (pjl.dat):                                                │
│    │   └─> Match model string against known printers                                   │
│    └─> Determine language support (PJL/PS/PCL)                                         │
│  Output: Language support status                                                        │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 3. TRANSLATION ENGINE (pjl.py, ps.py, pcl.py)                                          │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  User command ("download /etc/passwd")                                          │
│  Process:                                                                                │
│    PJL Translation:                                                                      │
│    ├─> Command: download → @PJL FSDOWNLOAD NAME="..."                                  │
│    ├─> Add UEL wrapper                                                                  │
│    ├─> Add echo token for response detection                                            │
│    └─> Add status request (if enabled)                                                  │
│                                                                                          │
│    PostScript Translation:                                                               │
│    ├─> Command: get → (file) (r) file ... print                                        │
│    ├─> Add PS header (%!)                                                               │
│    ├─> Add delimiter token                                                              │
│    └─> Wrap in UEL                                                                      │
│                                                                                          │
│    PCL Translation:                                                                      │
│    ├─> Command: get → Retrieve macro                                                    │
│    ├─> Add PCL header (ESC sequence)                                                    │
│    └─> Macro ID tracking                                                                │
│  Output: Protocol-specific command                                                       │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 4. FUZZING ENGINE (fuzzer.py)                                                           │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  Fuzz command                                                                    │
│  Process:                                                                                │
│    ├─> fuzz_paths() generates:                                                         │
│    │   ├─> Volume paths (0:, 1:, C:, D:)                                               │
│    │   ├─> Traversal paths (../, ../../, ../../../)                                    │
│    │   ├─> Environment vars ($HOME, %WINDIR%)                                          │
│    │   ├─> SMB paths (\\\\server\\share)                                              │
│    │   └─> Filesystem hierarchy (/etc, /bin, /var)                                    │
│    │   Total: 254 paths                                                                 │
│    │                                                                                     │
│    ├─> fuzz_names() generates:                                                         │
│    │   ├─> Hidden files (.htaccess, .ssh)                                              │
│    │   ├─> Sensitive files (passwd, shadow, config.xml)                                │
│    │   ├─> Special chars (spaces, semicolons, pipes)                                   │
│    │   └─> Long names (255 chars, 1000 chars)                                          │
│    │   Total: 31 names                                                                  │
│    │                                                                                     │
│    ├─> fuzz_data() generates:                                                          │
│    │   ├─> Buffer overflow (AAAA... x1000)                                             │
│    │   ├─> Null bytes (\x00...)                                                        │
│    │   ├─> Format strings (%s, %x, %n)                                                 │
│    │   └─> SQL injection attempts                                                       │
│    │                                                                                     │
│    └─> fuzz_traversal_vectors() generates:                                             │
│        ├─> Unix traversal (../../../etc/passwd)                                         │
│        ├─> Windows traversal (..\\..\\..)                                              │
│        └─> Volume-based (0:/../../../etc/passwd)                                       │
│        Total: 96 vectors                                                                │
│  Output: Attack vectors for testing                                                      │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 5. PAYLOAD ENGINE (payloads/__init__.py)                                                │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  payload banner "HACKED!"                                                        │
│  Process:                                                                                │
│    ├─> Load template: banner.ps                                                         │
│    ├─> Find variables: {{msg}}                                                          │
│    ├─> Substitute: {{msg}} → "HACKED!"                                                  │
│    ├─> Validate: All variables filled                                                   │
│    └─> Return: Complete PostScript code                                                 │
│  Output:                                                                                 │
│    %!PS                                                                                  │
│    /Helvetica findfont 16 scalefont setfont                                             │
│    100 700 moveto                                                                        │
│    (HACKED!) show                                                                        │
│    showpage                                                                              │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 6. ERROR HANDLING ENGINE (codebook.py + error handlers)                                 │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  PJL Response with CODE=10001 DISPLAY="READY"                                   │
│  Process:                                                                                │
│    ├─> Extract error codes: CODE=10001                                                  │
│    ├─> Extract messages: DISPLAY="READY"                                                │
│    ├─> Lookup in codebook:                                                              │
│    │   └─> codebook.get_errors('10001') → "READY (online)"                             │
│    ├─> Format output:                                                                   │
│    │   └─> "CODE 10001: READY (READY (online))"                                        │
│    └─> Display with appropriate color (green for success)                               │
│  Output: User-friendly error message                                                     │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│ 7. OPERATORS ENGINE (operators.py - PostScript only)                                    │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Input:  enumerate_operators command                                                     │
│  Process:                                                                                │
│    ├─> Load operator database (371 operators, 16 categories)                           │
│    ├─> For each category:                                                               │
│    │   ├─> For each operator in category:                                              │
│    │   │   ├─> Build test: /operator where {pop (1)} {(0)} ifelse print               │
│    │   │   ├─> Send to printer                                                         │
│    │   │   ├─> Parse response: '1' = supported, '0' = not                             │
│    │   │   └─> Record result                                                           │
│    │   └─> Display category results                                                     │
│    └─> Generate support matrix                                                          │
│  Output:                                                                                 │
│    01. Operand Stack Manipulation: 11/11 supported                                      │
│    02. Arithmetic Operators: 21/21 supported                                            │
│    10. File Operators: 28/30 supported (file, deletefile supported!)                   │
│    ...                                                                                   │
└───────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🌐 Protocol Stack Diagram

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                          MULTI-PROTOCOL ARCHITECTURE                                      │
└─────────────────────────────────────────────────────────────────────────────────────────┘

                            USER COMMAND
                                 │
                                 ▼
                        ┌─────────────────┐
                        │ Protocol Router │
                        │  (conn.open)    │
                        └────────┬────────┘
                                 │
           ┌─────────────────────┼─────────────────────┬─────────────────────┐
           │                     │                     │                     │
           ▼                     ▼                     ▼                     ▼
    ┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
    │ RAW(9100)   │      │ LPD(515)    │      │ IPP(631)    │      │ SMB(445)    │
    ├─────────────┤      ├─────────────┤      ├─────────────┤      ├─────────────┤
    │ DEFAULT     │      │ LEGACY      │      │ MODERN      │      │ WINDOWS     │
    │             │      │             │      │             │      │             │
    │ 1. Connect  │      │ 1. Connect  │      │ 1. HTTP GET │      │ 1. SMB Nego │
    │ 2. Send raw │      │ 2. Queue cmd│      │ 2. IPP POST │      │ 2. Tree Conn│
    │ 3. Recv raw │      │ 3. Ctrl file│      │ 3. Parse    │      │ 3. File ops │
    │ 4. Close    │      │ 4. Data file│      │ 4. Response │      │ 4. Print    │
    │             │      │ 5. Confirm  │      │ 5. Close    │      │ 5. Close    │
    └──────┬──────┘      └──────┬──────┘      └──────┬──────┘      └──────┬──────┘
           │                    │                     │                     │
           └────────────────────┴─────────────────────┴─────────────────────┘
                                          │
                                          ▼
                                   TCP/IP STACK
                                          │
                                          ▼
                                   TARGET PRINTER
```

---

## 💾 Data Storage Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                            DATA PERSISTENCE & STORAGE                                     │
└─────────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│  STATIC DATA (Read-Only)                                                                 │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  src/core/db/pjl.dat         - Printer models database                                  │
│  src/utils/codebook.py       - 378 PJL error codes                                      │
│  src/utils/fuzzer.py         - 381 attack vectors                                       │
│  src/utils/operators.py      - 371 PostScript operators                                 │
│  src/payloads/*.ps           - 5 attack payload templates                               │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│  RUNTIME DATA (Created During Execution)                                                 │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  exfiltrated/                - Downloaded/captured files                                │
│  ├─> passwd, shadow, config.xml (from printer)                                          │
│  ├─> captured_job_*.ps (print jobs)                                                     │
│  └─> mirrored filesystem structure                                                       │
│                                                                                          │
│  Session Files:                                                                          │
│  ├─> session.log (-o flag) - Raw command log                                            │
│  ├─> config.backup - Configuration backups                                              │
│  └─> *.pcap - Network captures (tcpdump)                                                │
└───────────────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────────────────────────────┐
│  STATE MANAGEMENT (In-Memory)                                                            │
├───────────────────────────────────────────────────────────────────────────────────────┤
│  Connection State:                                                                       │
│  ├─> self.conn - Active connection object                                               │
│  ├─> self.target - Target IP/hostname                                                   │
│  └─> self.mode - Current language (pjl/ps/pcl)                                          │
│                                                                                          │
│  Filesystem State:                                                                       │
│  ├─> self.vol - Current volume (0:, 1:, etc.)                                           │
│  ├─> self.cwd - Current working directory                                               │
│  └─> self.traversal - Traversal root                                                    │
│                                                                                          │
│  Configuration:                                                                          │
│  ├─> self.timeout - Command timeout (30s)                                               │
│  ├─> self.debug - Debug mode flag                                                       │
│  ├─> self.quiet - Quiet mode flag                                                       │
│  └─> self.logfile - Log file handle                                                     │
│                                                                                          │
│  PCL Specific:                                                                           │
│  └─> self.macros - Virtual filesystem (macro ID → file info)                            │
└───────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔐 Security Testing Flow

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                       SECURITY TESTING WORKFLOW ENGINE                                    │
└─────────────────────────────────────────────────────────────────────────────────────────┘

PHASE 1: RECONNAISSANCE
═══════════════════════════════════════════════════════════════════════════════════════════
User → discovery → SNMP scan → Printer list
       ├─> id → Device identification
       ├─> network → Network configuration
       ├─> variables → Environment variables
       └─> info config → Firmware and capabilities

PHASE 2: VULNERABILITY ASSESSMENT
═══════════════════════════════════════════════════════════════════════════════════════════
User → Test filesystem access
       ├─> ls /etc → Check directory listing
       ├─> permissions /etc/passwd → Test file access
       ├─> download ../../../etc/passwd → Path traversal
       └─> traverse → Automated traversal testing

PHASE 3: EXPLOITATION
═══════════════════════════════════════════════════════════════════════════════════════════
User → Execute attacks
       ├─> download /etc/passwd → Exfiltrate files
       ├─> upload backdoor.ps → Install persistence
       ├─> lock 12345 → Lock control panel
       ├─> capture → Intercept print jobs
       ├─> payload storm 100 → DoS attack
       └─> nvram dump → Extract credentials

PHASE 4: POST-EXPLOITATION
═══════════════════════════════════════════════════════════════════════════════════════════
User → Maintain access & cleanup
       ├─> mirror → Full filesystem backup
       ├─> backup config.bak → Config backup
       ├─> backdoor_remove → Remove traces
       └─> unlock 0 → Restore access

PHASE 5: REPORTING
═══════════════════════════════════════════════════════════════════════════════════════════
Data → QA Report Generator
       ├─> Findings documentation
       ├─> Evidence collection
       ├─> Risk assessment
       └─> Remediation recommendations
```

---

## 📋 Component Responsibility Matrix

| Component | Lines | Responsibility | Dependencies |
|-----------|-------|----------------|--------------|
| **main.py** | 212 | Entry point, arg parsing, shell selection | osdetect, discovery, capabilities, pjl, ps, pcl |
| **printer.py** | 1,304 | Base class, connection mgmt, file ops | helper, discovery, fuzzer |
| **pjl.py** | 2,840 | PJL commands, filesystem, NVRAM | printer, codebook, helper |
| **ps.py** | 962 | PostScript commands, operators, payloads | printer, operators, payloads, helper |
| **pcl.py** | 411 | PCL commands, virtual filesystem | printer, helper |
| **capabilities.py** | 208 | Language detection | helper, requests, pysnmp |
| **discovery.py** | 247 | Network scanning | osdetect, helper, subprocess |
| **osdetect.py** | 42 | OS detection with caching | platform |
| **helper.py** | 705 | Core utilities (5 classes) | colorama, socket |
| **codebook.py** | 451 | 378 PJL error codes | re |
| **fuzzer.py** | 216 | 381 attack vectors | None |
| **operators.py** | 447 | 371 PS operators | None |
| **raw.py** | 70 | RAW protocol (Port 9100) | socket |
| **lpd.py** | 180 | LPD protocol (Port 515) | socket |
| **ipp.py** | 200 | IPP protocol (Port 631) | socket, struct |
| **smb.py** | 120 | SMB protocol (Port 445) | socket, subprocess |
| **payloads/** | 301 | 5 attack payloads | re, os |

**Total**: 9,716 lines across 17 core modules

---

## 🎯 Attack Surface Map

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                         PRINTERREAPER ATTACK SURFACE                                      │
└─────────────────────────────────────────────────────────────────────────────────────────┘

ATTACK VECTOR 1: INFORMATION DISCLOSURE
════════════════════════════════════════════════════════════════════════════════════════
┌─> Filesystem Enumeration
│   ├─> ls, find, mirror (all languages)
│   └─> Path traversal (PJL, PS)
│
├─> Configuration Extraction
│   ├─> download /etc/passwd, config.xml
│   ├─> nvram dump (PJL)
│   └─> dump systemdict (PostScript)
│
├─> Credential Disclosure
│   ├─> NVRAM passwords
│   ├─> Configuration files
│   └─> Network credentials
│
└─> Network Information
    ├─> network command
    └─> SNMP enumeration

ATTACK VECTOR 2: AUTHENTICATION BYPASS
════════════════════════════════════════════════════════════════════════════════════════
┌─> Lock Bypass
│   ├─> unlock 0 (default PIN)
│   ├─> unlock_bruteforce (1-65535)
│   └─> Factory reset (destructive)
│
└─> Password Testing
    ├─> PostScript: systemparamspassword
    └─> PJL: LOCKPIN manipulation

ATTACK VECTOR 3: DENIAL OF SERVICE
════════════════════════════════════════════════════════════════════════════════════════
┌─> Resource Exhaustion
│   ├─> flood (buffer overflow test)
│   ├─> payload storm (print many pages)
│   ├─> dos_jobs (job queue flood)
│   └─> dos_connections (connection exhaustion)
│
├─> Service Disruption
│   ├─> offline (take printer offline)
│   ├─> disable (reject jobs)
│   └─> display spam (dos_display)
│
└─> System Crash
    ├─> hang (infinite loop - PostScript)
    ├─> payload loop (DoS payload)
    └─> Buffer overflow attempts

ATTACK VECTOR 4: DATA MANIPULATION
════════════════════════════════════════════════════════════════════════════════════════
┌─> Print Job Manipulation
│   ├─> overlay (add watermark)
│   ├─> cross (add text)
│   ├─> replace (text substitution)
│   └─> capture (job interception)
│
├─> Configuration Tampering
│   ├─> set (change variables)
│   ├─> config (page device settings)
│   └─> Upload modified configs
│
└─> Page Counter Fraud
    └─> pagecount (reset/modify counter)

ATTACK VECTOR 5: CODE EXECUTION
════════════════════════════════════════════════════════════════════════════════════════
┌─> PostScript Execution
│   ├─> exec_ps (arbitrary PS code)
│   ├─> payload (pre-built attacks)
│   └─> File operations (file, run, exec)
│
└─> PJL Execution
    └─> execute (raw PJL commands)

ATTACK VECTOR 6: PERSISTENCE
════════════════════════════════════════════════════════════════════════════════════════
┌─> File Upload
│   ├─> upload backdoor.ps
│   └─> Verify across reboots
│
├─> Configuration Modification
│   └─> Persistent settings in NVRAM
│
└─> Operator Redefinition (PostScript)
    └─> Redefine showpage, print, etc.

ATTACK VECTOR 7: PHYSICAL DAMAGE
════════════════════════════════════════════════════════════════════════════════════════
┌─> NVRAM Destruction
│   ├─> destroy (PJL) - @PJL SET NVRAM=0
│   └─> destroy (PS) - Repeated erasepage
│
└─> Filesystem Destruction
    └─> format - Erase all data
```

---

## 📊 Performance & Optimization Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                        PERFORMANCE OPTIMIZATION LAYERS                                    │
└─────────────────────────────────────────────────────────────────────────────────────────┘

CACHING LAYER:
══════════════════════════════════════════════════════════════════════════════════════════
├─> OS Detection Cache (osdetect.py)
│   └─> _cached_os - Prevents repeated OS checks
│
└─> Future: Command Response Cache
    └─> Cache static info (id, network) - Planned v2.5.0

TIMEOUT MANAGEMENT:
══════════════════════════════════════════════════════════════════════════════════════════
├─> Global Timeout: 30 seconds (configurable)
├─> Socket Timeout: Set per operation
├─> Watchdog: Monitor stalled connections
└─> Visual Feedback: Progress for large transfers

RETRY LOGIC:
══════════════════════════════════════════════════════════════════════════════════════════
├─> cmd_with_retry() - Up to 3 attempts
├─> 0.5s delay between retries
└─> Graceful degradation on failure

CONNECTION POOLING (Ready for v2.5.0):
══════════════════════════════════════════════════════════════════════════════════════════
└─> Reuse connections for multiple commands
```

---

## 🎨 Visual Architecture (Simplified)

```
                    ┌─────────────────────────────┐
                    │         USER                │
                    │   (Security Professional)   │
                    └─────────────┬───────────────┘
                                  │
                    Command: ls /etc/passwd
                    Mode: auto
                                  │
                                  ▼
        ┌─────────────────────────────────────────────────┐
        │            PRINTERREAPER v2.4.2                 │
        │                                                 │
        │  ┌─────────────────────────────────────────┐  │
        │  │  ENTRY: main.py                         │  │
        │  │  ├─> Parse args                         │  │
        │  │  ├─> Detect OS (osdetect.py)           │  │
        │  │  ├─> Detect capabilities               │  │
        │  │  └─> Select shell (pjl/ps/pcl)         │  │
        │  └─────────────────────────────────────────┘  │
        │                    │                           │
        │                    ▼                           │
        │  ┌─────────────────────────────────────────┐  │
        │  │  LANGUAGE MODULES                       │  │
        │  │  ┌──────┐  ┌──────┐  ┌──────┐          │  │
        │  │  │ PJL  │  │  PS  │  │ PCL  │          │  │
        │  │  │  54  │  │  40  │  │  15  │          │  │
        │  │  └──────┘  └──────┘  └──────┘          │  │
        │  └─────────────────────────────────────────┘  │
        │                    │                           │
        │  ┌─────────────────┴──────────────────────┐  │
        │  │  ENGINES                               │  │
        │  │  • Translator  • Fuzzer                │  │
        │  │  • Codebook    • Operators             │  │
        │  │  • Payloads                            │  │
        │  └─────────────────┬──────────────────────┘  │
        │                    │                           │
        │  ┌─────────────────┴──────────────────────┐  │
        │  │  PROTOCOLS                             │  │
        │  │  [RAW] [LPD] [IPP] [SMB]              │  │
        │  └─────────────────┬──────────────────────┘  │
        │                    │                           │
        │  ┌─────────────────┴──────────────────────┐  │
        │  │  CONNECTION (socket)                   │  │
        │  └─────────────────┬──────────────────────┘  │
        └────────────────────┼──────────────────────────┘
                             │
                ┌────────────┴────────────┐
                │                         │
        Send PJL Request          Receive Response
                │                         │
                ▼                         ▼
        ┌───────────────────────────────────────┐
        │         TARGET PRINTER                │
        │  ┌─────────────────────────────────┐  │
        │  │  Languages: PJL, PS, PCL        │  │
        │  │  Filesystem: 0:, 1:, /etc       │  │
        │  │  NVRAM: Passwords, Config       │  │
        │  │  Jobs: Queue, Retention         │  │
        │  └─────────────────────────────────┘  │
        └───────────────────────────────────────┘
                             │
                             │ Response
                             ▼
        ┌───────────────────────────────────────┐
        │  RESULT PROCESSING                    │
        │  ├─> Error handling (codebook)        │
        │  ├─> Data parsing                     │
        │  ├─> File storage                     │
        │  └─> User display (colored)           │
        └───────────────────────────────────────┘
                             │
                             ▼
        ┌───────────────────────────────────────┐
        │  USER SEES RESULT                     │
        │  "Downloaded /etc/passwd to ./passwd" │
        └───────────────────────────────────────┘
```

---

**Generated by**: PrinterReaper Architecture Team  
**Date**: October 4, 2025  
**Version**: 2.4.2

