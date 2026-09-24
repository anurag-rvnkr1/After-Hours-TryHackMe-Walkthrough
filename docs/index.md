---
layout: default
title: "🌙 After Hours — Windows DFIR Investigation"
description: "Professional TryHackMe Hacker Holidays 2026 Day 12 digital forensics and malware analysis case study by Anurag Ravikumar."
permalink: /
---

<div align="center">

# 🌙 AFTER HOURS

### Hacker Holidays 2026 — Day 12

<img src="assets/banner-after-hours.png" width="100%">

<br>

# Windows Digital Forensics & Malware Analysis Investigation

### PowerShell • WMI Persistence • Malware Reverse Engineering • ILSpy • DFIR

<br>

![TryHackMe](https://img.shields.io/badge/TryHackMe-Hacker_Holidays-red?style=for-the-badge&logo=tryhackme)
![Room](https://img.shields.io/badge/Room-After_Hours-darkred?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Digital_Forensics-blue?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Medium-orange?style=for-the-badge)
![Windows](https://img.shields.io/badge/Platform-Windows_Artifacts-0078D6?style=for-the-badge&logo=windows)
![Reverse Engineering](https://img.shields.io/badge/Focus-.NET_Reverse_Engineering-6f42c1?style=for-the-badge)

<br>

> *A professional Digital Forensics and Incident Response (DFIR) case study documenting the investigation of a hidden Windows persistence mechanism using PowerShell artifact analysis, WMI configuration recovery, payload extraction, and .NET malware reverse engineering.*

<br>

**Cybersecurity Portfolio Project**

**Author:** **Anurag Ravikumar**

</div>

---

# 👨‍💻 About This Investigation

> This GitHub Pages documentation is a **portfolio-focused forensic investigation report**, not a CTF solution dump.

The **After Hours** room simulates a Windows endpoint compromised by malware that executes **after business hours** while leaving almost no traces in traditional persistence locations.

Instead of exploiting vulnerabilities, the investigation focuses on reconstructing attacker activity using Windows forensic artifacts, encoded PowerShell analysis, malware extraction, and static reverse engineering.

---

<div class="security">

## 🔐 Investigation Summary

**Scenario**

A suspicious Windows system continues performing unauthorized activity despite appearing clean during normal persistence checks.

**Mission**

Recover hidden malware configuration data, identify the persistence mechanism, extract the embedded payload, reverse engineer the executable, and document indicators of compromise without executing malware.

</div>

---

# 🎯 Investigation Dashboard

| Investigation Attribute | Details |
|------------------------|---------|
| **Platform** | TryHackMe |
| **Room** | Hacker Holidays 2026 — Day 12 |
| **Category** | Windows Digital Forensics |
| **Difficulty** | Medium |
| **Operating System** | Windows Artifacts (Analyzed from Linux AttackBox) |
| **Malware Type** | Managed .NET Portable Executable |
| **Analysis Type** | Static Malware Analysis |
| **Persistence Technique** | Windows Management Instrumentation (WMI) |
| **Primary Scripting Language** | PowerShell |
| **Reverse Engineering Tool** | ILSpy |
| **Status** | Investigation Completed ✅ |

---

# ⚡ Skills Demonstrated

<div class="success">

### Core Cybersecurity Skills

- Windows Digital Forensics
- Malware Analysis
- Reverse Engineering
- DFIR Methodology
- WMI Persistence Hunting
- PowerShell Analysis
- CyberChef Payload Decoding
- Portable Executable Analysis
- Threat Hunting
- Detection Engineering

</div>

---

# 🛠 Investigation Toolkit

| Tool | Purpose |
|------|---------|
| 🐧 Linux AttackBox | Investigation Environment |
| 📦 7-Zip | Archive Extraction |
| 🔍 strings | Artifact Enumeration |
| 🎯 grep | IOC Discovery |
| 🍳 CyberChef | Base64 + Deflate Decoding |
| 🧬 ILSpy | .NET Reverse Engineering |
| 🖥 Windows Knowledge | WMI Persistence Investigation |

---

# 📚 Documentation Navigation

<div class="note">

### Repository Documentation

| File | Description |
|------|-------------|
| 📘 **README.md** | Repository landing page and project overview. |
| 📖 **Documentation.md** | Complete forensic investigation report. |
| 📝 **notes.md** | Analyst notebook containing commands, observations, and IOC references. |
| 🔒 **SECURITY.md** | Responsible disclosure and repository security policy. |
| 🤝 **CONTRIBUTING.md** | Contribution guidelines for educational improvements. |

</div>

---

# 🧭 Investigation Roadmap

```text
Evidence Acquisition
        │
        ▼
Artifact Enumeration
        │
        ▼
PowerShell Discovery
        │
        ▼
WMI Configuration Analysis
        │
        ▼
Payload Recovery
        │
        ▼
Portable Executable Validation
        │
        ▼
Reverse Engineering (ILSpy)
        │
        ▼
IOC Extraction
        │
        ▼
Detection Engineering
        │
        ▼
Incident Report
```

---

# 🚨 Threat Scenario

<div class="warning">

## Incident Brief

The concierge team reports suspicious administrative activity occurring **only after operational hours**.

Routine checks reveal:

- No malicious Startup folder entries.
- No Scheduled Tasks.
- No Registry Run persistence.
- No obvious autoruns.

Yet unauthorized activity continues.

The investigation shifts toward **hidden Windows persistence mechanisms** outside traditional locations.

</div>

---

# 🕵️ Investigation Objectives

<table>
<tr>
<th width="60">Phase</th>
<th>Objective</th>
</tr>

<tr>
<td><strong>01</strong></td>
<td>Acquire forensic artifacts from the investigation package.</td>
</tr>

<tr>
<td><strong>02</strong></td>
<td>Enumerate Windows artifacts for suspicious indicators.</td>
</tr>

<tr>
<td><strong>03</strong></td>
<td>Recover encoded PowerShell execution chain.</td>
</tr>

<tr>
<td><strong>04</strong></td>
<td>Identify hidden WMI configuration storage.</td>
</tr>

<tr>
<td><strong>05</strong></td>
<td>Extract Base64 and Deflate compressed payload.</td>
</tr>

<tr>
<td><strong>06</strong></td>
<td>Validate Windows Portable Executable.</td>
</tr>

<tr>
<td><strong>07</strong></td>
<td>Reverse engineer malware with ILSpy.</td>
</tr>

<tr>
<td><strong>08</strong></td>
<td>Document Indicators of Compromise and defensive detections.</td>
</tr>

</table>

---

# 🗺 Investigation Timeline

| Stage | Investigation Activity |
|-------|------------------------|
| 🟢 Stage 1 | Evidence Acquisition |
| 🔵 Stage 2 | Artifact Enumeration |
| 🟡 Stage 3 | PowerShell Discovery |
| 🟣 Stage 4 | WMI Persistence Analysis |
| 🟠 Stage 5 | Payload Recovery |
| 🔴 Stage 6 | Portable Executable Validation |
| ⚫ Stage 7 | Reverse Engineering with ILSpy |
| 🟢 Stage 8 | IOC Documentation |
| 🔵 Stage 9 | Detection Engineering |
| 🟣 Stage 10 | Incident Response Report |

---

# 🧱 Malware Investigation Architecture

```text
             Windows System Artifacts
                      │
                      ▼
             Hidden WMI Configuration
                      │
                      ▼
        Encoded PowerShell Loader (-enc)
                      │
                      ▼
          Base64 Configuration Blob
                      │
                      ▼
          Deflate Compressed Payload
                      │
                      ▼
        Windows Portable Executable (.NET)
                      │
                      ▼
            Static Reverse Engineering
                      │
                      ▼
       Malware Behaviour Reconstruction
                      │
                      ▼
       IOC Extraction & Detection Rules
```

---

# 📂 Investigation Environment

## Working Directory

```bash
/root/Rooms/hacker-holidays-2026/after-hours/
```

### Environment Characteristics

- TryHackMe AttackBox
- Linux Investigation Host
- Windows Forensic Evidence
- Offline Malware Analysis
- Static Reverse Engineering Only

---

# 📸 Investigation Preview

## Phase 1 — Room Overview

![Room Overview](../Screenshots/01_room-overview.png)

*Figure 1 — TryHackMe room overview introducing the forensic investigation.*

---

## Phase 2 — Investigation Workspace

![Workspace](../Screenshots/02_workspace-access.png)

*Figure 2 — AttackBox workspace containing forensic artifacts and investigation tools.*

---

## Phase 3 — Evidence Inventory

![Inventory](../Screenshots/03_artifact-inventory.png)

*Figure 3 — Initial evidence collection after extracting the investigation archive.*

---

# 📌 Evidence Collection Summary

| Artifact | Purpose |
|----------|---------|
| `mappings` | Configuration mapping artifact. |
| `index` | Metadata reference artifact. |
| `objects.data` | Hidden malware configuration storage. |
| `tools/` | Investigation utilities including ILSpy. |

---

# 💡 Why This Investigation Matters

Unlike many introductory malware challenges, **After Hours** teaches investigators how attackers abuse legitimate Windows features instead of relying on obvious persistence.

The room demonstrates:

- Hidden PowerShell execution.
- Windows Management Instrumentation abuse.
- Fileless malware concepts.
- Encoded payload recovery.
- Static reverse engineering methodology.
- Incident response documentation.

---

<div align="center">

---

# 🔍 Phase 2 — Evidence Analysis & Artifact Hunting

<div class="security">

## Digital Forensics Investigation

With the evidence package extracted, the next objective is to enumerate every supplied Windows artifact and identify hidden persistence indicators.

Unlike traditional malware investigations, the malicious activity is **not immediately visible** through Startup folders, Scheduled Tasks, or Registry Run keys.

The investigation therefore begins with **artifact triage** and **string enumeration**.

</div>

---

# 📊 Investigation Progress Dashboard

| Investigation Phase | Status |
|--------------------|--------|
| Evidence Acquisition | ✅ Completed |
| Artifact Enumeration | ✅ Completed |
| PowerShell Discovery | 🔄 In Progress |
| WMI Persistence Hunting | 🔄 In Progress |
| Payload Recovery | ⏳ Pending |
| Reverse Engineering | ⏳ Pending |

---

# 📂 Evidence Inventory

## Forensic Artifacts Recovered

| Artifact | Description |
|----------|-------------|
| **mappings** | Configuration mapping information recovered from Windows artifacts. |
| **index** | Metadata references used during forensic parsing. |
| **objects.data** | Primary evidence containing hidden malware configuration. |
| **tools/** | Investigation utilities including ILSpy for .NET reverse engineering. |

---

## 📸 Evidence Inventory

<img src="../Screenshots/03_artifact-inventory.png" width="100%">

*Figure 4 — Windows forensic artifacts extracted inside the investigation workspace.*

---

# 🧰 Evidence Enumeration Strategy

Rather than opening every artifact manually, the investigation uses a **bulk enumeration** strategy.

### Goals

- Extract printable strings.
- Recover hidden PowerShell commands.
- Locate Windows paths.
- Identify encoded payloads.
- Search WMI references.

This mirrors real-world **DFIR artifact triage** performed during incident response investigations.

---

# ⚙ String Enumeration

### Command Executed

```bash
strings -a * > strings-encoded.txt
```

### Why `strings`?

The Linux `strings` utility extracts printable ASCII and Unicode strings embedded inside binary files.

Advantages include:

- Works against binary artifacts.
- Quickly exposes hidden configuration.
- Safe offline analysis.
- No malware execution required.

---

## Investigation Output

A consolidated evidence file is created.

```text
strings-encoded.txt
```

This becomes the primary searchable artifact for the remainder of the investigation.

---

# 🎯 PowerShell Hunting

PowerShell is commonly abused for:

- Malware loaders
- Fileless persistence
- Obfuscation
- Lateral movement
- Memory execution

The investigation therefore prioritizes PowerShell-related artifacts.

---

## IOC Hunting Command

```bash
grep -i powershell strings-encoded.txt | sort -u
```

### Investigation Goal

- Remove duplicate entries.
- Surface encoded PowerShell commands.
- Recover suspicious execution chains.

---

## 📸 PowerShell Discovery

<img src="../Screenshots/04_powershell-discovery.png" width="100%">

*Figure 5 — Encoded PowerShell command recovered during artifact enumeration.*

---

# 🚨 Initial Suspicious Findings

The recovered command immediately raises several DFIR indicators.

<div class="warning">

### Suspicious Behaviours Observed

- Hidden PowerShell execution.
- Encoded Base64 payload.
- PowerShell launched from `cmd.exe`.
- No visible PowerShell console.
- Non-interactive execution.

</div>

---

# 🧠 Analyst Observation

These execution switches are frequently associated with:

| Technique | Reason |
|-----------|--------|
| `-enc` | Base64 encoded command execution. |
| `-nop` | Skip PowerShell profile loading. |
| `-Window Hidden` | Hide console window from users. |
| `-Sta` | Execute in single-threaded apartment mode. |

### Defensive Insight

Encoded PowerShell alone is **not always malicious**, but encoded commands combined with hidden execution warrant immediate investigation.

---

# 🧬 Understanding the PowerShell Loader

The recovered script contains a very long Base64 value.

Instead of executing it, investigators decode it offline.

### Safe Workflow

```text
Encoded PowerShell
        │
        ▼
Base64 Decode
        │
        ▼
Readable PowerShell Script
        │
        ▼
Static Behaviour Analysis
```

---

# 🍳 CyberChef Investigation — Stage One

CyberChef is used to safely decode the PowerShell command.

### Recipe

```text
From Base64
      │
      ▼
Remove Null Bytes
```

---

## Why Remove Null Bytes?

PowerShell often stores encoded commands as UTF-16.

Removing null bytes converts the Unicode stream into readable PowerShell syntax.

---

## 📸 CyberChef — Base64 Decoding

<img src="../Screenshots/05_base64-decoding.png" width="100%">

*Figure 6 — Decoding UTF-16 PowerShell into readable script using CyberChef.*

---

# 🔎 Decoded Script Analysis

The recovered script performs several important operations.

## High-Level Execution Flow

```text
Read WMI Configuration
        │
        ▼
Retrieve Encoded Payload
        │
        ▼
Decode Base64
        │
        ▼
Inflate Compressed Data
        │
        ▼
Load .NET Assembly into Memory
```

---

# 💥 Why This Behaviour Matters

<div class="security">

### Fileless Malware Behaviour

Instead of downloading malware from the internet, the script reconstructs an executable entirely from configuration data stored locally.

Advantages for attackers include:

- Minimal disk artifacts.
- Reduced antivirus detection.
- Memory-based execution.
- Smaller malicious footprint.

</div>

---

# 🛰 Windows Management Instrumentation Investigation

One keyword recovered from the script becomes the next hunting target.

## WMI Indicator

The decoded PowerShell references a custom Windows Management Instrumentation property containing configuration data.

### Investigation Command

```bash
grep -C 3 "Win32_HardwareTelemetry" strings-encoded.txt | sort -u
```

---

## Why Investigate WMI?

Windows Management Instrumentation can be abused to:

- Store payload configuration.
- Launch scripts.
- Maintain persistence.
- Hide attacker configuration.

---

# 📌 WMI Persistence Hunting

### Investigation Findings

| Finding | Analyst Interpretation |
|---------|------------------------|
| Custom WMI property | Suspicious configuration storage. |
| Large encoded blob | Hidden payload data. |
| Retrieved dynamically | Loader reconstructs executable at runtime. |

---

## WMI Abuse Overview

<div class="note">

### Windows Persistence Beyond Registry Keys

Traditional persistence locations include:

- Registry Run Keys.
- Startup Folder.
- Scheduled Tasks.
- Windows Services.

This challenge instead demonstrates persistence hidden inside **Windows Management Instrumentation**, a location frequently overlooked during basic investigations.

</div>

---

# 🧠 WMI Threat Hunting Notes

Security teams should investigate:

- Unknown namespaces.
- Custom event consumers.
- Permanent event subscriptions.
- Large Base64 configuration properties.

---

# 🧩 Payload Extraction Workflow

Once the encoded WMI property is recovered, investigators extract the embedded payload.

### Recovery Pipeline

```text
WMI Configuration Property
          │
          ▼
Base64 Encoded Blob
          │
          ▼
Raw Inflate
          │
          ▼
Recovered Executable Bytes
```

---

# 🍳 CyberChef Investigation — Stage Two

### Recipe Used

```text
From Base64
      │
      ▼
Raw Inflate
```

---

## 📸 Payload Recovery

<img src="../Screenshots/06_deflate-payload.png" width="100%">

*Figure 7 — Recovering compressed payload stored inside WMI configuration.*

---

# 📦 Understanding Deflate Compression

The PowerShell loader creates a **DeflateStream** object.

### Purpose

- Compress executable.
- Hide recognizable strings.
- Reduce payload size.
- Obfuscate binary content.

---

# 🧬 Portable Executable Recovery

Immediately after decompression, recognizable executable markers appear.

## Evidence Observed

```text
MZ
```

and

```text
This program cannot be run in DOS mode.
```

These indicate the payload is a Windows executable.

---

## 📸 Portable Executable Detection

<img src="../Screenshots/07_pe-identification.png" width="100%">

*Figure 8 — Windows Portable Executable signature recovered during forensic analysis.*

---

# ✔ PE Validation

CyberChef validates the recovered file.

| Property | Result |
|----------|--------|
| DOS Header | Present |
| PE Signature | Present |
| Managed Assembly | Yes |
| Executable Type | Windows PE |

---

## 📸 File Type Validation

<img src="../Screenshots/08_pe-filetype-confirmed.png" width="100%">

*Figure 9 — Validation confirms recovered artifact is a Windows Portable Executable.*

---

# 💾 Evidence Preservation

The executable is exported as:

```text
payload.exe
```

### Evidence Handling Best Practices

- Preserve original bytes.
- Never execute unknown malware.
- Analyze copy only.
- Maintain investigation reproducibility.

---

---

# 🧬 Phase 3 — Reverse Engineering the Malware Payload

<div class="security">

## Static Malware Analysis

After recovering the Windows Portable Executable from the encoded WMI configuration, the next phase focuses on **reverse engineering the malware without executing it**.

The executable is analyzed using **ILSpy**, allowing investigators to reconstruct the source code, inspect execution logic, identify persistence behavior, and recover embedded configuration values safely.

</div>

---

# 🛠 Reverse Engineering Toolkit

| Tool | Purpose |
|------|---------|
| **ILSpy** | .NET decompiler and assembly browser |
| **CyberChef** | Decode embedded Base64 values |
| **Windows PE Knowledge** | Validate executable structure |
| **Static Analysis Methodology** | Analyze malware safely without execution |

---

# 🧩 Reverse Engineering Workflow

```text
Recovered payload.exe
        │
        ▼
ILSpy Assembly Browser
        │
        ▼
Program.Main()
        │
        ▼
Execution Logic Review
        │
        ▼
Process Creation Analysis
        │
        ▼
Embedded Configuration Recovery
        │
        ▼
IOC Documentation
```

---

# 📸 ILSpy Workspace

<img src="../Screenshots/09_ilspy-analysis.png" width="100%">

*Figure 10 — Static reverse engineering of the recovered .NET executable inside ILSpy.*

---

# 🔍 Assembly Investigation

The recovered executable contains a standard .NET assembly structure.

### Components Investigated

| Assembly Component | Investigation Purpose |
|-------------------|-----------------------|
| Manifest | Assembly metadata |
| Namespace | Malware project namespace |
| Program Class | Main execution logic |
| Entry Point | Initial malware routine |
| Resources | Embedded strings and configuration |

---

# 🧠 Main Execution Logic

The `Main()` method contains the malware's startup routine.

### Analyst Findings

The executable performs:

- Environment validation.
- Machine name comparison.
- Hidden command construction.
- Background process execution.
- Encoded value handling.

> The actual encoded challenge value is intentionally **redacted** throughout this repository.

---

# 🛰 Environment Validation

The malware checks whether it is running on a specific Windows machine before continuing execution.

### Why Attackers Use Environment Checks

| Purpose | Explanation |
|---------|-------------|
| Sandbox Detection | Avoid automated malware analysis systems. |
| Target Validation | Execute only on intended victim machines. |
| Defense Evasion | Reduce unwanted execution during analysis. |
| Campaign Control | Restrict malware deployment scope. |

---

# 💻 Hidden Process Execution

The malware constructs a Windows `ProcessStartInfo` object.

### Observed Behaviour

| Behaviour | Finding |
|-----------|---------|
| `cmd.exe` launched | Yes |
| Visible console | No |
| Window Style | Hidden |
| Create Window | Disabled |
| Background execution | Yes |

---

<div class="warning">

### Why This Matters

Hidden command execution is a strong behavioural indicator frequently monitored by EDR and Sysmon deployments.

Combined with encoded PowerShell, this behaviour often indicates:

- Malware loaders.
- Persistence scripts.
- Post-exploitation tooling.
- Defense evasion techniques.

</div>

---

# 🔐 Embedded Configuration Analysis

During reverse engineering, investigators discover another encoded value embedded directly inside the executable.

### Investigation Workflow

```text
Embedded Base64 String
          │
          ▼
CyberChef Decode
          │
          ▼
Recovered Configuration
          │
          ▼
Challenge Output (Sanitized)
```

---

# 📸 Embedded Configuration Discovery

<img src="../Screenshots/10_encoded-final-stage.png" width="100%">

*Figure 11 — Embedded Base64 configuration identified inside the malware source code.*

---

# 🛡 Responsible Disclosure

Instead of publishing the recovered TryHackMe flag, this portfolio intentionally sanitizes sensitive challenge content.

### Portfolio Version

```text
THM{REDACTED_FOR_PORTFOLIO}
```

---

# 📸 Sanitized Investigation Result

<img src="../Screenshots/11_flag-redacted.png" width="100%">

*Figure 12 — Final investigation result published without exposing challenge secrets.*

---

# ☣ Malware Behaviour Dashboard

<div class="security">

## Behaviour Reconstruction

The malware demonstrates a staged execution chain combining PowerShell, Windows Management Instrumentation, compression, and in-memory execution.

</div>

| Behaviour | Status |
|-----------|--------|
| Encoded PowerShell Loader | ✅ |
| WMI Configuration Storage | ✅ |
| Base64 Obfuscation | ✅ |
| Deflate Compression | ✅ |
| Managed .NET Payload | ✅ |
| Hidden Process Creation | ✅ |
| Environment Validation | ✅ |
| Memory Assembly Loading | ✅ |
| Static Payload Recovery | ✅ |
| Dynamic Execution Avoided | ✅ |

---

# ⚔ Malware Kill Chain

```text
Windows Artifact
      │
      ▼
Encoded PowerShell Loader
      │
      ▼
Retrieve WMI Configuration
      │
      ▼
Decode Base64 Payload
      │
      ▼
Inflate Binary
      │
      ▼
Load .NET Assembly
      │
      ▼
Validate Environment
      │
      ▼
Hidden Windows Command
```

---

# 🎯 MITRE ATT&CK Matrix

<div class="note">

## ATT&CK Techniques Observed

</div>

| ATT&CK Technique | Description |
|-----------------|-------------|
| **T1059.001** | PowerShell execution. |
| **T1027** | Encoded configuration data. |
| **T1140** | Base64 decoding and decompression. |
| **T1047** | Windows Management Instrumentation. |
| **T1620** | Memory assembly loading. |
| **T1106** | Native Windows process execution. |
| **T1562** | Hidden execution / defense evasion behaviour. |

---

# 🛡 MITRE ATT&CK Dashboard

| Tactic | Coverage |
|--------|----------|
| Execution | ██████████ |
| Persistence | ████████ |
| Defense Evasion | █████████ |
| Discovery | ███ |
| Collection | ██ |
| Impact | █ |

---

# 🚨 Indicators of Compromise Dashboard

<div class="warning">

## Sanitized IOC Summary

These indicators describe malware behaviour **without exposing challenge secrets**.

</div>

### PowerShell Indicators

| IOC | Description |
|-----|-------------|
| Encoded PowerShell (`-enc`) | Obfuscated command execution. |
| Hidden Window | Invisible PowerShell session. |
| `cmd.exe` Launch | Hidden child process creation. |
| Base64 Payload | Embedded configuration blob. |

---

### WMI Indicators

| IOC | Description |
|-----|-------------|
| Custom WMI Property | Configuration storage. |
| Encoded Blob | Embedded malware payload. |
| WMI Configuration Retrieval | Loader behaviour. |

---

### Executable Indicators

| IOC | Description |
|-----|-------------|
| Windows PE Header | Managed executable. |
| DOS Stub | Portable executable validation. |
| ILSpy Decompiled Source | Malware logic reconstruction. |

---

# 🔍 Detection Engineering

<div class="success">

## Defender Hunting Opportunities

Security teams can detect similar behaviour through PowerShell, Sysmon, Windows Event Logs, and WMI monitoring.

</div>

### Detection Surface

| Telemetry Source | Hunt For |
|-----------------|----------|
| Windows Event Logs | Encoded PowerShell execution. |
| PowerShell Operational Logs | Hidden execution parameters. |
| Sysmon Event ID 1 | Encoded PowerShell processes. |
| Sysmon Event IDs 19–21 | WMI event consumers. |
| Defender DeviceEvents | Suspicious WMI activity. |
| Defender ProcessEvents | Hidden PowerShell child processes. |

---

# 🛰 Sysmon Hunting Cheatsheet

### Interesting Process Arguments

```text
-EncodedCommand
-Window Hidden
-NoProfile
-Sta
```

### Interesting Parent / Child Relationships

```text
cmd.exe
    │
    └── powershell.exe
```

### Interesting WMI Activity

- Unknown namespaces.
- Custom event consumers.
- Permanent event subscriptions.
- Large Base64 property values.

---

# 🧾 DFIR Investigation Timeline

| Time | Investigation Activity |
|------|------------------------|
| **T+00** | Evidence archive acquired. |
| **T+05** | Windows artifacts enumerated. |
| **T+10** | Encoded PowerShell identified. |
| **T+20** | PowerShell decoded safely. |
| **T+30** | WMI configuration recovered. |
| **T+40** | Deflate payload extracted. |
| **T+50** | Portable Executable validated. |
| **T+60** | ILSpy reverse engineering completed. |
| **T+70** | IOC extraction completed. |
| **T+80** | Detection engineering documented. |

---

# 📚 Digital Forensics Evidence Timeline

## Investigation Progress

| Stage | Evidence |
|-------|----------|
| 🟢 Acquisition | Archive extraction |
| 🔵 Enumeration | Artifact inventory |
| 🟡 Discovery | PowerShell IOC |
| 🟣 Persistence | WMI analysis |
| 🟠 Payload Recovery | Base64 + Deflate |
| 🔴 Reverse Engineering | ILSpy |
| ⚫ IOC Documentation | Sigma & Sysmon |

---

# 📖 Skills Demonstrated

<div class="success">

## Blue Team Skill Matrix

</div>

| Domain | Practical Skills |
|--------|------------------|
| Windows DFIR | Artifact collection, string extraction, WMI investigation |
| Malware Analysis | PE recovery, static payload analysis |
| Reverse Engineering | ILSpy, .NET assembly inspection |
| Threat Hunting | IOC identification, PowerShell hunting |
| Detection Engineering | Sigma rules, Sysmon hunting opportunities |
| Incident Response | Documentation and timeline reconstruction |

---

# 🧰 Technical Knowledge Gained

### Windows Security Concepts

- Windows Management Instrumentation
- Fileless malware
- Encoded PowerShell execution
- In-memory assemblies
- Portable Executables
- .NET assemblies

### DFIR Concepts

- Evidence preservation
- Offline malware analysis
- IOC extraction
- Timeline reconstruction
- Threat documentation

---

# 🖼 Investigation Screenshot Gallery

<div align="center">

## Complete Investigation Timeline

</div>

| Screenshot | Description |
|------------|-------------|
| `01_room-overview.png` | Room overview |
| `02_workspace-access.png` | AttackBox investigation workspace |
| `03_artifact-inventory.png` | Evidence collection |
| `04_powershell-discovery.png` | Encoded PowerShell discovery |
| `05_base64-decoding.png` | CyberChef decoding workflow |
| `06_deflate-payload.png` | Payload recovery |
| `07_pe-identification.png` | Windows PE detection |
| `08_pe-filetype-confirmed.png` | PE validation |
| `09_ilspy-analysis.png` | Static reverse engineering |
| `10_encoded-final-stage.png` | Embedded configuration analysis |
| `11_flag-redacted.png` | Sanitized final investigation result |

---

# 📂 Repository Resources

<div class="note">

## Project Documentation

| Resource | Purpose |
|----------|---------|
| **README.md** | Project overview and repository landing page. |
| **Documentation/Documentation.md** | Complete DFIR investigation report. |
| **Documentation/Documentation.docx** | Printable investigation report. |
| **Resources/notes.md** | Investigation notebook and commands. |
| **SECURITY.md** | Responsible disclosure policy. |
| **CONTRIBUTING.md** | Contribution guidelines. |

</div>

---

# 🧠 Lessons Learned

<div class="success">

## Key Takeaways

- Hidden Windows persistence is not limited to Registry Run Keys.
- PowerShell encoding is an obfuscation technique, not encryption.
- WMI can store attacker configuration.
- Static analysis often reveals complete malware behaviour.
- Memory-loaded assemblies leave different forensic artifacts than traditional executables.

</div>

---

# 🛡 Defensive Recommendations

| Recommendation | Why It Matters |
|---------------|----------------|
| Enable PowerShell Script Block Logging | Capture decoded PowerShell scripts. |
| Monitor WMI Activity | Detect hidden persistence mechanisms. |
| Deploy Sysmon | Improve Windows telemetry visibility. |
| Alert on Encoded PowerShell | Detect suspicious PowerShell loaders. |
| Investigate Hidden Process Creation | Identify stealth execution behaviour. |
| Review Custom WMI Namespaces | Hunt advanced persistence. |

---

# 🌐 Explore More Portfolio Projects

<div class="note">

## Related TryHackMe Investigations

- 🌊 Infinity Pool — Linux Privilege Escalation
- 🏝 CryptoCabana — Web Exploitation & SSRF
- 🌙 Do Not Disturb — Linux Enumeration & PrivEsc
- 💬 SuperSecretTip — Web Security Investigation
- 🔎 Valenfind — Local File Inclusion Investigation
- 🏢 Corp Website — Web Application Security Assessment
- 🏛 Active Directory Basics — Windows Domain Fundamentals

</div>

---

# 👨‍💻 About the Author

<div align="center">

## Anurag Revankar

**Cybersecurity • DFIR • SOC • Malware Analysis • Reverse Engineering**

Passionate about building practical cybersecurity portfolio projects focused on:

- Digital Forensics
- Threat Hunting
- Blue Team Operations
- Malware Analysis
- Detection Engineering
- Active Directory Security
- Web Application Security

</div>

---

# ⭐ Portfolio Project Summary

<div class="security">

## After Hours — Windows DFIR Investigation

A complete forensic investigation demonstrating:

- Windows artifact analysis.
- PowerShell threat hunting.
- WMI persistence investigation.
- Malware payload recovery.
- Portable Executable validation.
- ILSpy reverse engineering.
- IOC documentation.
- MITRE ATT&CK mapping.
- Detection engineering recommendations.

This project is published as part of a professional cybersecurity portfolio showcasing DFIR investigation methodology instead of simply revealing a CTF solution.

</div>

---

<div align="center">

# 🌙 Investigation Completed Successfully

### Windows Digital Forensics • Malware Analysis • Reverse Engineering • Threat Hunting

**TryHackMe Hacker Holidays 2026 — Day 12**

---

**Built for GitHub Pages • Jekyll Hacker Theme • Cybersecurity Portfolio**

⭐ If you found this project useful, consider exploring the rest of the cybersecurity portfolio.

</div>
