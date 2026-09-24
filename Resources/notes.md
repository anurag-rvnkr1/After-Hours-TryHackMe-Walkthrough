# 📝 Investigation Notes — After Hours (TryHackMe)

> **Room:** Hacker Holidays 2026 — Day 12: After Hours
>
> **Category:** Digital Forensics / Malware Analysis
>
> **Difficulty:** Medium
>
> **Platform:** TryHackMe
>
> **Author:** Anurag Revankar 

---

## 📌 Overview

This document contains the **investigation notes**, commands, observations, forensic methodology, and defensive findings collected while solving the **After Hours** TryHackMe room.

Unlike the complete walkthrough in `Documentation.md`, these notes act as a **field notebook** containing important commands, artifacts, indicators, and analyst observations recorded during the investigation.

> **Note:** Challenge flags, passwords, secrets, and sensitive challenge values have been intentionally **redacted** for responsible publication.

---

# 🎯 Investigation Objectives

The forensic investigation focused on four major objectives.

* Identify hidden persistence outside traditional startup locations.
* Recover encoded PowerShell execution artifacts.
* Extract the embedded executable payload.
* Perform static reverse engineering without executing malware.

---

# 🧰 Investigation Environment

| Component                | Details                         |
| ------------------------ | ------------------------------- |
| Platform                 | TryHackMe AttackBox             |
| Operating System         | Linux Investigation Environment |
| Challenge Files          | Password Protected 7z Archive   |
| Malware Type             | .NET Managed Executable         |
| Reverse Engineering Tool | ILSpy                           |
| Decoder                  | CyberChef                       |
| Analysis Method          | Static Analysis Only            |

---

# 📁 Initial Evidence Collection

After extracting the challenge archive, several forensic artifacts were available for investigation.

## Evidence Directory

```text
/root/Rooms/hacker-holidays-2026/after-hours/
```

### Primary Evidence

| Artifact     | Purpose                                 |
| ------------ | --------------------------------------- |
| mappings     | Configuration mapping artifact          |
| index        | Index information                       |
| objects.data | Hidden configuration storage            |
| tools/       | Investigation utilities including ILSpy |

---

# 🔍 Investigation Timeline

| Stage   | Goal                                 |
| ------- | ------------------------------------ |
| Stage 1 | Extract investigation package        |
| Stage 2 | Enumerate Windows artifacts          |
| Stage 3 | Search encoded PowerShell strings    |
| Stage 4 | Decode hidden PowerShell script      |
| Stage 5 | Recover compressed payload           |
| Stage 6 | Identify executable type             |
| Stage 7 | Reverse engineer payload using ILSpy |
| Stage 8 | Document persistence behavior        |

---

# ⚙️ Commands Used During Investigation

## Archive Extraction

```bash
7z x after-hours.7z
```

Purpose:

* Extract investigation files.
* Preserve original evidence structure.

---

## String Extraction

```bash
strings -a * > strings-encoded.txt
```

Purpose:

* Extract printable strings from every forensic artifact.
* Include encoded values and embedded configuration data.

---

## PowerShell Enumeration

```bash
grep -i powershell strings-encoded.txt | sort -u
```

Purpose:

* Identify suspicious PowerShell references.
* Remove duplicate entries.
* Locate encoded PowerShell execution chain.

---

## WMI Configuration Search

```bash
grep -C 3 "Win32_HardwareTelemetry" strings-encoded.txt | sort -u
```

Purpose:

* Locate hidden WMI configuration property.
* Recover embedded Base64 payload.
* Identify persistence storage location.

---

# 📖 Key Analyst Observations

## Observation 1 — Hidden PowerShell Execution

A hidden PowerShell execution chain was discovered.

Characteristics included:

* Non-interactive execution.
* Hidden window.
* Encoded command.
* PowerShell launched through `cmd.exe`.

### Analyst Note

This resembles common **fileless malware loader** behavior frequently observed during Windows incident response investigations.

---

## Observation 2 — WMI Used as Storage

A suspicious WMI class was referenced by the PowerShell script.

Analyst findings:

* Custom configuration stored inside WMI.
* Payload retrieved dynamically.
* Avoids common startup locations.
* Helps persistence remain less visible.

### Defensive Insight

Investigating unusual WMI properties is an important DFIR technique when startup folders and registry keys appear clean.

---

## Observation 3 — In-Memory Payload Loading

Recovered PowerShell logic performs:

1. Read configuration value.
2. Decode Base64.
3. Inflate compressed bytes.
4. Load .NET assembly directly into memory.

### Why It Matters

* Leaves fewer filesystem artifacts.
* Avoids writing executable immediately.
* Common technique used by PowerShell loaders.

---

# 🍳 CyberChef Workflow Notes

## Recipe Chain Used

### Phase 1

```text
From Base64
↓
Remove Null Bytes
```

Purpose:

Decode UTF-16 encoded PowerShell command into readable script.

---

### Phase 2

```text
From Base64
↓
Raw Inflate
```

Purpose:

Recover compressed executable payload stored inside WMI configuration data.

---

## Validation Result

Recovered binary displayed:

```text
MZ
This program cannot be run in DOS mode.
```

### Analyst Conclusion

Recovered payload is a **Windows Portable Executable**.

---

# 🧬 Portable Executable Identification

## Validation Methods

| Method       | Result                      |
| ------------ | --------------------------- |
| DOS Header   | Present                     |
| MZ Signature | Present                     |
| PE Detection | Successful                  |
| Magic Recipe | Windows Portable Executable |

### Analyst Notes

The payload appears to be:

* Managed .NET executable.
* Suitable for static decompilation.
* No runtime execution required.

---

# 🔧 ILSpy Investigation Notes

## Tool Purpose

ILSpy was used for:

* Static reverse engineering.
* Assembly inspection.
* Source code reconstruction.
* Entry point analysis.

---

## Important Components Reviewed

| Component        | Reason                       |
| ---------------- | ---------------------------- |
| Program Class    | Main execution logic         |
| Entry Point      | Malware startup routine      |
| ProcessStartInfo | Hidden command execution     |
| Encoded Variable | Embedded configuration value |

---

## Static Analysis Findings

Observed behavior includes:

* Environment validation.
* Machine name comparison.
* Hidden command execution.
* User creation logic.
* Encoded configuration embedded inside executable.

> Exact encoded values have been removed from this repository.

---

# 🧠 Malware Behaviour Summary

| Behavior                    | Observation   |
| --------------------------- | ------------- |
| PowerShell Loader           | Yes           |
| Hidden Execution            | Yes           |
| WMI Configuration Retrieval | Yes           |
| Memory Loading              | Yes           |
| .NET Assembly               | Yes           |
| Static Payload Recovery     | Yes           |
| Network Activity            | Not Required  |
| Dynamic Execution           | Not Performed |

---

# 🛡 Persistence Analysis Notes

Traditional persistence locations inspected conceptually include:

* Startup Folder
* Scheduled Tasks
* Registry Run Keys
* Services

### Investigation Result

Persistence instead relied on:

* Encoded PowerShell.
* WMI configuration storage.
* In-memory assembly execution.

---

# 🔍 Indicators of Compromise (Sanitized)

## PowerShell Indicators

* Hidden PowerShell execution.
* Encoded Base64 command.
* WMI property retrieval.
* Memory-based assembly loading.

---

## WMI Indicators

* Custom configuration property.
* Encoded binary data.
* Persistence through WMI namespace.

---

## Malware Indicators

* Windows PE payload.
* Managed .NET executable.
* Hidden command execution logic.

---

# 🚨 Defensive Hunting Opportunities

Security teams can hunt for:

### Windows Event Logs

* PowerShell Operational Logs.
* Script Block Logging.
* WMI Activity Logs.

### Process Creation

* Hidden `powershell.exe`.
* `cmd.exe` launching PowerShell.
* Unusual encoded command arguments.

### WMI Hunting

* Suspicious custom classes.
* Non-standard WMI properties.
* Encoded configuration blobs.

---

# 📚 DFIR Knowledge Reinforced

This room demonstrates practical investigation techniques including:

* Windows artifact parsing.
* PowerShell forensic analysis.
* WMI persistence hunting.
* Base64 decoding.
* Deflate decompression.
* Portable Executable validation.
* Static .NET reverse engineering.
* Malware configuration extraction.

---

# 🖥 Tools Reference

| Tool      | Purpose                            |
| --------- | ---------------------------------- |
| `7z`      | Extract password-protected archive |
| `strings` | Enumerate embedded strings         |
| `grep`    | Search suspicious artifacts        |
| CyberChef | Decode and decompress payloads     |
| ILSpy     | Reverse engineer .NET executable   |
| Linux CLI | Artifact analysis workflow         |

---

# 📁 Screenshot Reference

The screenshots included in this repository correspond to each investigation stage.

| Screenshot                     | Investigation Phase                  |
| ------------------------------ | ------------------------------------ |
| `01_room-overview.png`         | Room overview                        |
| `02_workspace-access.png`      | AttackBox preparation                |
| `03_artifact-inventory.png`    | Artifact enumeration                 |
| `04_powershell-discovery.png`  | Encoded PowerShell discovery         |
| `05_base64-decoding.png`       | Base64 decoding workflow             |
| `06_deflate-payload.png`       | Payload recovery                     |
| `07_pe-identification.png`     | PE detection                         |
| `08_pe-filetype-confirmed.png` | Executable validation                |
| `09_ilspy-analysis.png`        | ILSpy reverse engineering            |
| `10_encoded-final-stage.png`   | Embedded configuration analysis      |
| `11_flag-redacted.png`         | Final sanitized investigation result |

---

# 📖 Related Documentation

| File                               | Description                          |
| ---------------------------------- | ------------------------------------ |
| `README.md`                        | GitHub landing page                  |
| `Documentation/Documentation.md`   | Complete investigation report        |
| `Documentation/Documentation.docx` | Printable report                     |
| `docs/index.md`                    | GitHub Pages portfolio documentation |

---

# ⚠ Responsible Publication

This repository intentionally **redacts challenge-specific secrets**.

Hidden from publication:

* TryHackMe flag.
* Decoded flag value.
* Embedded challenge credentials.
* Sensitive challenge payload values.

The focus of this repository is the **forensic investigation methodology**, not reproducing or exposing challenge answers.

---

<div align="center">

### 🌙 After Hours — Investigation Notes Complete

**Digital Forensics • Malware Analysis • Reverse Engineering • Windows DFIR**

Maintained as part of my cybersecurity portfolio and incident response learning journey.

</div>
