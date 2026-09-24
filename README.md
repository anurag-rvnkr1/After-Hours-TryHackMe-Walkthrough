# 🌙 After Hours — TryHackMe Walkthrough

<div align="center">

# Hacker Holidays 2026 • Day 12

### Professional Digital Forensics & Malware Analysis Walkthrough

[![TryHackMe](https://img.shields.io/badge/TryHackMe-After_Hours-red?style=for-the-badge\&logo=tryhackme)](https://tryhackme.com/)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Forensics-blue?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Windows_Forensics-0078D6?style=for-the-badge\&logo=windows)
![Reverse Engineering](https://img.shields.io/badge/Focus-.NET_Reverse_Engineering-6f42c1?style=for-the-badge)
![Status](https://img.shields.io/badge/Writeup-Completed-success?style=for-the-badge)

*A complete DFIR investigation documenting hidden Windows persistence, PowerShell artifact analysis, Base64 & Deflate decoding, Portable Executable recovery, and .NET malware reverse engineering using ILSpy.*

---

**Author:** Anurag Ravikumar

*Cybersecurity Portfolio • Digital Forensics • Malware Analysis • Reverse Engineering*

</div>

---

## 📌 Repository Overview

This repository contains a **professional incident investigation walkthrough** for the **After Hours** room from **TryHackMe Hacker Holidays 2026 – Day 12**.

The challenge simulates a compromised Windows environment where malicious persistence is intentionally hidden outside common startup locations. Rather than exploiting a live system, the objective is to perform **artifact-driven forensic analysis**, identify an encoded PowerShell loader, recover an embedded executable payload, reverse engineer the malware using **ILSpy**, and understand how the persistence mechanism operates.

> **This repository is intended for educational purposes, defensive security research, malware analysis training, and DFIR portfolio demonstrations.**

---

# 🎯 Learning Objectives

After completing this room, you will understand how to:

* Investigate Windows forensic artifacts.
* Enumerate hidden persistence locations.
* Parse suspicious strings from system files.
* Identify encoded PowerShell payloads.
* Decode Base64-encoded commands.
* Decompress Deflate-compressed payloads.
* Identify Windows Portable Executable (PE) files.
* Reverse engineer a .NET executable using ILSpy.
* Analyze malicious process execution logic.
* Recover hidden indicators without exposing challenge flags.

---

# 🧠 Skills Demonstrated

| Domain              | Techniques                                                    |
| ------------------- | ------------------------------------------------------------- |
| Windows Forensics   | Strings analysis, WMI artifact discovery, persistence hunting |
| DFIR                | Artifact parsing, encoded script investigation                |
| Malware Analysis    | Static malware analysis, payload extraction                   |
| Reverse Engineering | ILSpy, .NET assembly inspection                               |
| PowerShell          | Base64 decoding, hidden execution analysis                    |
| Incident Response   | IOC extraction, execution flow analysis                       |

---

# 🗂 Repository Structure

```text
After-Hours-TryHackMe-Walkthrough/
│
├── README.md
├── SECURITY.md
├── CONTRIBUTING.md
├── LICENSE
├── .gitignore
│
├── Documentation/
│   ├── Documentation.md
│   └── Documentation.docx
│
├── Resources/
│   └── notes.md
│
├── Screenshots/
│   ├── 01_room-overview.png
│   ├── 02_workspace-access.png
│   ├── 03_artifact-inventory.png
│   ├── 04_powershell-discovery.png
│   ├── 05_base64-decoding.png
│   ├── 06_deflate-payload.png
│   ├── 07_pe-identification.png
│   ├── 08_pe-filetype-confirmed.png
│   ├── 09_ilspy-analysis.png
│   ├── 10_encoded-final-stage.png
│   └── 11_flag-redacted.png
│
└── docs/
    ├── index.md
    └── assets/
        └── css/
            └── custom.scss
```

---

# 🧪 Investigation Workflow

The walkthrough follows a realistic DFIR methodology.

| Phase                   | Description                                                   |
| ----------------------- | ------------------------------------------------------------- |
| Environment Preparation | Extract investigation package and inspect provided artifacts. |
| Artifact Enumeration    | Parse Windows artifact files for suspicious encoded content.  |
| PowerShell Analysis     | Recover hidden PowerShell execution chain.                    |
| Payload Recovery        | Decode embedded Base64 + Deflate payload.                     |
| PE Validation           | Verify recovered binary as Windows Portable Executable.       |
| Reverse Engineering     | Inspect .NET executable using ILSpy.                          |
| IOC Analysis            | Understand malware execution logic and persistence behavior.  |
| Defensive Findings      | Document indicators and mitigation recommendations.           |

---

# 🔍 Investigation Highlights

### Stage 1 — Windows Artifact Enumeration

* Extracted forensic package inside the TryHackMe AttackBox.
* Enumerated provided Windows system artifacts.
* Performed comprehensive string extraction across files.
* Identified suspicious PowerShell references.

---

### Stage 2 — Encoded PowerShell Discovery

The investigation uncovered a hidden PowerShell execution chain referencing Windows Management Instrumentation (WMI).

Analysis revealed:

* Hidden PowerShell execution.
* Encoded Base64 payload.
* Non-interactive execution.
* Suspicious WMI configuration access.

---

### Stage 3 — Payload Recovery

Recovered PowerShell script performs:

* Retrieval of configuration data.
* Base64 decoding.
* Deflate decompression.
* In-memory assembly loading.

This behavior closely resembles fileless malware techniques used in Windows environments.

---

### Stage 4 — Windows PE Extraction

Recovered binary identified as:

* Windows Portable Executable.
* .NET managed assembly.
* Executable payload hidden inside configuration data.

No execution was required during the investigation.

---

### Stage 5 — Reverse Engineering (.NET)

Using **ILSpy**, the payload was statically analyzed.

Key observations include:

* Hidden execution logic.
* Machine name validation.
* Process creation through `cmd.exe`.
* Embedded encoded value used by malware.

The challenge flag has been **fully redacted** from this repository.

---

# 📸 Investigation Screenshots

This repository includes a curated evidence timeline.

| Screenshot                     | Description                                   |
| ------------------------------ | --------------------------------------------- |
| `01_room-overview.png`         | TryHackMe room overview                       |
| `02_workspace-access.png`      | AttackBox environment and artifact extraction |
| `03_artifact-inventory.png`    | Initial forensic artifact inventory           |
| `04_powershell-discovery.png`  | Encoded PowerShell discovery                  |
| `05_base64-decoding.png`       | CyberChef Base64 decoding workflow            |
| `06_deflate-payload.png`       | Raw Deflate payload extraction                |
| `07_pe-identification.png`     | Portable Executable detection                 |
| `08_pe-filetype-confirmed.png` | PE validation results                         |
| `09_ilspy-analysis.png`        | ILSpy reverse engineering                     |
| `10_encoded-final-stage.png`   | Encoded payload investigation                 |
| `11_flag-redacted.png`         | Final investigation result (flag hidden)      |

> All screenshots have been recreated and sanitized for portfolio publication.

---

# 🛠 Tools Used

| Tool                  | Purpose                          |
| --------------------- | -------------------------------- |
| TryHackMe AttackBox   | Investigation environment        |
| Linux CLI             | Artifact extraction and analysis |
| `strings`             | Artifact enumeration             |
| `grep`                | IOC discovery                    |
| `7z`                  | Archive extraction               |
| CyberChef             | Base64 & Deflate decoding        |
| ILSpy                 | .NET reverse engineering         |
| Windows WMI Knowledge | Configuration artifact analysis  |

---

# 💻 Technologies Covered

```text
Windows Forensics
PowerShell
WMI
Base64 Encoding
Deflate Compression
Portable Executable (PE)
.NET Assemblies
ILSpy
Digital Forensics
Malware Analysis
DFIR
Incident Response
```

---

# 🛡 Indicators of Compromise (Sanitized)

The walkthrough documents indicators **without revealing challenge secrets**.

Included:

* Suspicious PowerShell execution behavior.
* WMI configuration access.
* Hidden persistence technique.
* In-memory assembly loading.
* Encoded malware configuration.

Excluded:

* Challenge flag.
* Final decoded value.
* Sensitive challenge solution strings.

---

# 📚 Documentation Included

| File                               | Purpose                                |
| ---------------------------------- | -------------------------------------- |
| `README.md`                        | Professional GitHub landing page       |
| `Documentation/Documentation.md`   | Complete forensic investigation report |
| `Documentation/Documentation.docx` | Printable report version               |
| `Resources/notes.md`               | Quick investigation notes and commands |
| `docs/index.md`                    | GitHub Pages portfolio documentation   |

---

# 🌐 GitHub Pages Preview

The repository includes a dedicated documentation website.

Features include:

* Professional landing page.
* Investigation timeline.
* Interactive navigation.
* DFIR styling.
* Responsive GitHub Pages layout.
* Cybersecurity-themed design.

---

# 🎓 Knowledge Gained

This room reinforces practical understanding of:

* Windows persistence mechanisms.
* Hidden PowerShell execution.
* WMI abuse techniques.
* Fileless malware behavior.
* Static malware analysis methodology.
* Safe reverse engineering workflows.
* IOC documentation for incident reports.

---

# ⚠ Responsible Disclosure

This repository intentionally omits sensitive challenge answers.

Redacted items include:

* TryHackMe flag.
* Embedded challenge secrets.
* Exact decoded credential values.

The goal is to demonstrate the investigation methodology while respecting the integrity of the learning platform.

---

# 📖 Related Portfolio Projects

| Repository              | Category                    |
| ----------------------- | --------------------------- |
| Infinity Pool           | Linux Privilege Escalation  |
| CryptoCabana            | Web Exploitation            |
| Do Not Disturb          | Linux Enumeration & PrivEsc |
| SuperSecretTip          | Web Security                |
| Valenfind               | Local File Inclusion        |
| Corp Website            | Web Application Security    |
| Active Directory Basics | Windows AD Fundamentals     |

---

# 🤝 Contributing

Suggestions, corrections, and educational improvements are welcome.

Please open an Issue or submit a Pull Request.

See **CONTRIBUTING.md** for contribution guidelines.

---

# 🔐 Security Policy

This repository is published for:

* Security education.
* Malware analysis training.
* DFIR methodology.
* Portfolio demonstration.

See **SECURITY.md** for responsible usage guidelines.

---

# ⭐ Support

If this repository helped you learn Digital Forensics or Malware Analysis:

Give the repository a ⭐ on GitHub and explore the rest of the cybersecurity portfolio.

---

<div align="center">

### 🌙 Hacker Holidays 2026 — Day 12 Completed

**Windows Forensics • Malware Analysis • DFIR • Reverse Engineering**

*Built for cybersecurity learning and professional portfolio presentation.*

</div>
