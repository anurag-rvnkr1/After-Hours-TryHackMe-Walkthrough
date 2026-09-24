# 🌙 After Hours — Digital Forensics & Malware Analysis Report

> **TryHackMe — Hacker Holidays 2026 (Day 12)**
>
> **Category:** Digital Forensics / Malware Analysis
>
> **Difficulty:** Medium
>
> **Platform:** TryHackMe
>
> **Report Type:** Incident Investigation Walkthrough
>
> **Author:** Anurag Revankar 
>
> **Portfolio Project:** Windows DFIR & Reverse Engineering

---

> **Educational Disclaimer**
>
> This documentation was created for educational purposes as part of the TryHackMe learning platform. The investigation demonstrates digital forensic analysis and malware reverse engineering techniques inside an isolated laboratory environment. Challenge secrets, flags, passwords, and sensitive identifiers have been intentionally **redacted** to preserve the integrity of the room.

---

# Executive Summary

## Investigation Overview

The **After Hours** room from TryHackMe's **Hacker Holidays 2026** series simulates a Windows endpoint that appears clean during routine inspection but continues executing malicious activity during late-night hours. Traditional persistence locations—including **Startup folders**, **Scheduled Tasks**, and **Registry Run keys**—contain no suspicious entries, forcing the investigator to look deeper into Windows internals.

Rather than exploiting a vulnerable machine, this challenge focuses entirely on **Digital Forensics and Incident Response (DFIR)** methodology. The investigation follows a realistic analyst workflow:

1. Acquire forensic artifacts.
2. Enumerate evidence safely.
3. Identify suspicious encoded PowerShell activity.
4. Recover hidden malware configuration stored in Windows Management Instrumentation (WMI).
5. Decode an embedded executable payload.
6. Reverse engineer a .NET executable using ILSpy.
7. Understand malware behavior without executing the payload.

The investigation highlights several techniques commonly encountered during Windows incident response engagements, including **Base64 obfuscation**, **PowerShell fileless execution**, **Deflate compression**, **WMI abuse**, and **in-memory assembly loading**.

Unlike offensive CTF rooms, this challenge emphasizes the analytical mindset of a forensic investigator: collecting evidence, validating findings, documenting indicators, and reconstructing attacker behavior through static analysis.

---

## Skills Demonstrated

| Security Domain     | Practical Techniques                                       |
| ------------------- | ---------------------------------------------------------- |
| Windows Forensics   | Artifact collection, string extraction, WMI inspection     |
| Malware Analysis    | Static payload extraction, PE identification               |
| DFIR                | Timeline creation, IOC identification, persistence hunting |
| Reverse Engineering | .NET assembly analysis with ILSpy                          |
| PowerShell Security | Encoded script decoding and execution flow analysis        |
| Incident Response   | Evidence validation and malware behavior reconstruction    |

---

## Investigation Outcome

The forensic investigation successfully identified a concealed persistence mechanism leveraging Windows Management Instrumentation and recovered a malicious .NET executable through static analysis.

The recovered executable was analyzed without execution, revealing hidden process creation logic and embedded encoded configuration values.

> **Challenge flag and encoded secrets have been intentionally redacted throughout this documentation.**

---

# Table of Contents

## Report Sections

1. Executive Summary
2. Threat Scenario
3. Lab Environment
4. Evidence Acquisition
5. Initial Artifact Enumeration
6. PowerShell Artifact Discovery
7. Windows Management Instrumentation Analysis
8. Payload Recovery Methodology
9. Portable Executable Identification
10. Reverse Engineering with ILSpy
11. Malware Behavior Analysis
12. MITRE ATT&CK Mapping
13. Indicators of Compromise
14. Detection Engineering Opportunities
15. Incident Timeline
16. Defensive Recommendations
17. Lessons Learned
18. References

---

# Threat Scenario

## Concierge Briefing

Long after the resort's front desk closes, systems inside the Byte Lotus Hotel continue receiving unauthorized activity during late-night hours.

Initial investigations report:

* No suspicious startup entries.
* No malicious scheduled tasks.
* No registry persistence.
* No obvious autoruns.

Despite the clean appearance, unauthorized logins continue occurring after operational hours.

The objective becomes identifying **how persistence survives** outside locations commonly inspected during incident response.

---

## Investigation Objectives

The investigation was performed with four primary goals.

### Primary Objectives

<table><table-section header><table-row header><table-cell header width="52">Phase</table-cell><table-cell header>Objective</table-cell></table-row></table-section><table-row><table-cell>**1**</table-cell><table-cell>Locate hidden persistence artifacts inside supplied Windows forensic evidence.</table-cell></table-row><table-row><table-cell>**2**</table-cell><table-cell>Recover and decode malicious PowerShell execution logic.</table-cell></table-row><table-row><table-cell>**3**</table-cell><table-cell>Extract the concealed executable payload from encoded configuration storage.</table-cell></table-row><table-row><table-cell>**4**</table-cell><table-cell>Reverse engineer malware behavior using static analysis techniques.</table-cell></table-row></table>

---

# Lab Environment

## Investigation Platform

The challenge provides a prepared forensic environment inside **TryHackMe AttackBox**.

### Environment Details

| Component          | Value                    |
| ------------------ | ------------------------ |
| Platform           | TryHackMe                |
| Investigation Host | Linux AttackBox          |
| Challenge Category | Windows Forensics        |
| Archive Format     | Password Protected 7z    |
| Malware Type       | .NET Portable Executable |
| Analysis Mode      | Static Only              |

---

## Investigation Toolkit

<table><table-section header><table-row header><table-cell header width="220">Tool</table-cell><table-cell header>Purpose During Investigation</table-cell></table-row></table-section><table-row><table-cell>**7-Zip (7z)**</table-cell><table-cell>Extract password-protected evidence archive.</table-cell></table-row><table-row><table-cell>**strings**</table-cell><table-cell>Extract printable strings from forensic artifacts.</table-cell></table-row><table-row><table-cell>**grep**</table-cell><table-cell>Search encoded PowerShell and WMI indicators.</table-cell></table-row><table-row><table-cell>**CyberChef**</table-cell><table-cell>Decode Base64, remove null bytes, decompress payloads.</table-cell></table-row><table-row><table-cell>**ILSpy**</table-cell><table-cell>Decompile .NET executable into readable source code.</table-cell></table-row><table-row><table-cell>**Linux Terminal**</table-cell><table-cell>Evidence handling and artifact processing.</table-cell></table-row></table>

---

# Investigation Workflow

The investigation followed a structured DFIR methodology instead of trial-and-error exploitation.

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
Payload Extraction
        │
        ▼
Portable Executable Validation
        │
        ▼
.NET Reverse Engineering
        │
        ▼
IOC Documentation & Reporting
```

Every stage preserves forensic evidence while avoiding malware execution.

---

# Evidence Acquisition

## Accessing the Investigation Files

The room provides a password-protected archive containing Windows forensic artifacts.

The evidence package is located inside the AttackBox workspace.

### Working Directory

```bash
/root/Rooms/hacker-holidays-2026/after-hours/
```

This directory contains:

* Windows forensic artifacts.
* Investigation utilities.
* Reverse engineering tools.
* Malware sample components.

---

## Screenshot — Room Overview

<p align="center">
  <img src="../Screenshots/01_room-overview.png" width="100%" alt="TryHackMe After Hours Room Overview"/>
</p>

**Figure 1.** TryHackMe room overview showing the investigation scenario and forensic objectives.

---

## Screenshot — AttackBox Workspace

<p align="center">
  <img src="../Screenshots/02_workspace-access.png" width="100%" alt="AttackBox Investigation Workspace"/>
</p>

**Figure 2.** AttackBox environment prepared for forensic investigation.

---

# Extracting the Evidence Archive

The investigation begins by extracting the supplied archive using 7-Zip.

### Extraction Command

```bash
7z x after-hours.7z
```

The archive prompts for a challenge password supplied by the room.

### Why This Matters

Password-protected archives are commonly encountered during malware investigations because they preserve binary integrity and prevent accidental antivirus interference during distribution.

---

## Evidence Integrity

Extraction completed successfully.

Observed characteristics included:

* Archive Size approximately 50 MB.
* Multiple directories extracted.
* Seven evidence files recovered.
* No extraction errors observed.

### Analyst Observation

Evidence should remain unchanged throughout the investigation to preserve reproducibility.

---

# Initial Artifact Enumeration

After extraction, the next objective is identifying every artifact supplied within the investigation package.

---

## Evidence Inventory

The extracted directory contains several Windows-related artifacts.

<table><table-section header><table-row header><table-cell header width="220">Artifact</table-cell><table-cell header>Investigation Relevance</table-cell></table-row></table-section><table-row><table-cell>**mappings**</table-cell><table-cell>Configuration mapping information.</table-cell></table-row><table-row><table-cell>**index**</table-cell><table-cell>Metadata and lookup references.</table-cell></table-row><table-row><table-cell>**objects.data**</table-cell><table-cell>Primary artifact containing encoded configuration information.</table-cell></table-row><table-row><table-cell>**tools/**</table-cell><table-cell>Investigation utilities including ILSpy binaries.</table-cell></table-row></table>

---

## Screenshot — Artifact Inventory

<p align="center">
  <img src="../Screenshots/03_artifact-inventory.png" width="100%" alt="Artifact Enumeration"/>
</p>

**Figure 3.** Initial artifact inventory after extracting the forensic package.

---

# Investigation Strategy

Instead of opening artifacts individually, investigators enumerate **every printable string** across all files.

This approach quickly surfaces:

* Encoded commands.
* URLs.
* Registry paths.
* PowerShell scripts.
* WMI references.
* Malware configuration blobs.

---

## String Enumeration

### Command Used

```bash
strings -a * > strings-encoded.txt
```

### Purpose

The `strings` utility extracts printable ASCII and Unicode strings embedded inside binary artifacts.

Using `-a` ensures all files—including binary blobs—are scanned.

### Output File

A consolidated investigation file:

```text
strings-encoded.txt
```

This becomes the primary searchable evidence source.

---

# Evidence Searching Methodology

Rather than manually inspecting thousands of strings, targeted searches identify suspicious Windows behaviors.

Common hunting keywords include:

* powershell
* cmd
* Run
* Win32
* registry
* task
* startup
* telemetry

This mirrors real-world DFIR hunting methodology.

---

# Discovering Suspicious PowerShell Activity

PowerShell is frequently abused for:

* Fileless malware.
* Encoded payload execution.
* In-memory malware loading.
* Persistence.
* Lateral movement.

The investigation therefore prioritizes PowerShell-related strings.

### Enumeration Command

```bash
grep -i powershell strings-encoded.txt | sort -u
```

### Why `sort -u`?

* Removes duplicate strings.
* Produces a cleaner investigation dataset.
* Makes encoded commands easier to identify.

---

## Screenshot — Encoded PowerShell Discovery

<p align="center">
  <img src="../Screenshots/04_powershell-discovery.png" width="100%" alt="PowerShell Discovery"/>
</p>

**Figure 4.** Suspicious PowerShell command recovered during artifact enumeration.

---

# Initial Findings

PowerShell enumeration immediately reveals a suspicious command chain.

Observed behaviors include:

* Hidden execution window.
* Encoded command argument.
* PowerShell launched via `cmd.exe`.
* Non-interactive execution switches.

These characteristics are strong indicators of malicious scripting.

---

## Analyst Observation

Legitimate administrative PowerShell typically executes readable scripts.

Encoded PowerShell combined with hidden execution is frequently associated with:

* Malware loaders.
* Initial access payloads.
* Obfuscated persistence mechanisms.
* Red team tooling.

Further decoding becomes necessary.

---

# Understanding the Encoded Command

The recovered PowerShell command contains a long Base64 blob.

Rather than executing it, investigators decode it offline.

This preserves evidence while revealing malware logic.

---

# Safe Decoding Workflow

The encoded value is copied into CyberChef.

Two transformations are applied.

### Recipe Chain

```text
From Base64
        │
        ▼
Remove Null Bytes
```

---

## Why Remove Null Bytes?

PowerShell strings often use UTF-16 encoding.

Removing null bytes converts Unicode text into readable PowerShell syntax.

---

## Screenshot — Base64 Decoding

<p align="center">
  <img src="../Screenshots/05_base64-decoding.png" width="100%" alt="CyberChef Base64 Decoding"/>
</p>

**Figure 5.** CyberChef decoding workflow for the encoded PowerShell command.

---

# Decoded PowerShell Script Analysis

After decoding, the script becomes human readable.

High-level behavior observed:

1. Access Windows Management Instrumentation.
2. Read a configuration property.
3. Decode embedded Base64 content.
4. Inflate compressed data.
5. Load an assembly directly into memory.

No filesystem execution is required.

---

## Analyst Interpretation

This is a classic staged malware loader.

Rather than shipping a standalone executable, configuration data contains a compressed binary that is reconstructed dynamically.

Advantages for attackers include:

* Reduced static detection.
* Smaller payload storage.
* Memory-only execution.
* Minimal disk artifacts.

---

# Identifying the WMI Artifact

One string stands out during PowerShell analysis.

A WMI property references hidden configuration data.

The investigator searches specifically for this indicator.

### Investigation Command

```bash
grep -C 3 "Win32_HardwareTelemetry" strings-encoded.txt | sort -u
```

### Why Context (`-C 3`)?

Context reveals surrounding encoded configuration values stored near the WMI reference.

This exposes the payload location.

---

# Windows Management Instrumentation Analysis

Windows Management Instrumentation (WMI) provides a management database used throughout Windows.

Attackers sometimes abuse WMI to:

* Store configuration.
* Execute persistence.
* Launch scripts.
* Hide payloads.

This room demonstrates one such abuse case.

---

## Investigation Finding

The suspicious WMI property contains a very large Base64 blob.

Characteristics include:

* High entropy.
* Encoded binary.
* Stored inside configuration property.
* Retrieved dynamically through PowerShell.

This becomes the primary payload artifact.

---

# Why WMI Persistence Matters

Traditional persistence locations include:

* Startup folders.
* Run Registry Keys.
* Scheduled Tasks.
* Services.

WMI persistence is quieter because:

* Fewer analysts inspect it.
* Configuration can appear legitimate.
* Payload retrieval happens dynamically.

This explains why the concierge briefing mentions persistence hidden somewhere "most tools don't think to check."

---

# Analyst Notes

## Important Indicators Identified So Far

| Finding                    | Significance              |
| -------------------------- | ------------------------- |
| Hidden PowerShell          | Suspicious execution      |
| Encoded Base64 Blob        | Obfuscation               |
| UTF-16 Encoding            | PowerShell artifact       |
| WMI Configuration Property | Hidden storage            |
| Memory Assembly Loading    | Fileless malware behavior |

---

## Evidence Collected

* Windows forensic artifacts.
* Encoded PowerShell loader.
* WMI configuration reference.
* Embedded compressed payload.
* Initial persistence indicators.

The investigation now moves toward recovering the embedded executable hidden inside WMI configuration data.

---

# Phase 5 — Recovering the Embedded Payload

> **Objective:** Extract the hidden executable stored inside the malicious Windows Management Instrumentation (WMI) configuration property without executing any malicious code.

At this stage of the investigation, the encoded PowerShell loader has already revealed that the malware retrieves configuration data from a custom WMI property. The next objective is to recover that configuration safely and determine what it contains.

This phase represents one of the most important DFIR concepts in the challenge: **recovering malware artifacts without execution**.

---

## Why Static Extraction Matters

Executing unknown malware introduces several risks:

* Triggering malicious behavior.
* Altering forensic evidence.
* Generating additional persistence.
* Creating outbound network activity.
* Losing reproducibility of findings.

Instead, investigators extract and decode the payload offline.

---

## Investigation Strategy

The recovered PowerShell script references a configuration value stored inside WMI.

High-level workflow:

```text
WMI Property
      │
      ▼
Base64 Encoded Blob
      │
      ▼
Deflate Compression
      │
      ▼
Portable Executable
      │
      ▼
Static Reverse Engineering
```

Each transformation is performed independently.

---

# Extracting the Configuration Blob

After locating the WMI property inside the strings output, the encoded configuration is copied into a separate analysis workspace.

### Evidence Handling Notes

* Original artifact remains unchanged.
* Encoded payload copied only for analysis.
* No PowerShell execution performed.
* No Windows binary executed.

This preserves chain-of-custody style investigation methodology.

---

## Screenshot — Encoded Configuration Discovery

<p align="center">
  <img src="../Screenshots/04_powershell-discovery.png" width="100%" alt="Encoded WMI Configuration"/>
</p>

**Figure 6.** Encoded configuration data identified inside Windows Management Instrumentation artifact.

---

# CyberChef Analysis — Stage One

CyberChef provides a safe environment for decoding encoded artifacts without running malicious content.

### Recipe Used

```text
From Base64
```

The output initially appears unreadable because the decoded data is still compressed.

---

## Why Compression Is Used

Attackers frequently compress payloads before encoding because it:

* Reduces payload size.
* Obfuscates recognizable strings.
* Avoids simple signature matching.
* Makes static inspection more difficult.

---

# CyberChef Analysis — Stage Two

The decoded binary still appears as compressed bytes.

The next transformation is applied.

### Recipe Chain

```text
From Base64
      │
      ▼
Raw Inflate
```

---

## Screenshot — Deflate Payload Recovery

<p align="center">
  <img src="../Screenshots/06_deflate-payload.png" width="100%" alt="Deflate Payload Recovery"/>
</p>

**Figure 7.** CyberChef recovering decompressed payload using Raw Inflate.

---

# Understanding Deflate Compression

The PowerShell loader creates a `DeflateStream`, indicating that the embedded configuration stores compressed binary content.

### Analyst Observation

Recovered output immediately becomes recognizable as executable data rather than text.

Visible indicators include:

* DOS executable header.
* Binary structure.
* PE metadata.
* Windows executable signature.

---

# Portable Executable Identification

The recovered payload begins with recognizable executable markers.

### Initial Indicators

Investigators immediately observe the classic Windows executable signature.

```text
MZ
```

Further inspection reveals the familiar DOS stub message.

```text
This program cannot be run in DOS mode.
```

These indicators strongly suggest the payload is a Windows Portable Executable.

---

## Screenshot — Windows PE Detection

<p align="center">
  <img src="../Screenshots/07_pe-identification.png" width="100%" alt="Windows PE Detection"/>
</p>

**Figure 8.** Portable Executable signature identified during payload recovery.

---

# Validating File Type

CyberChef includes a built-in file type detector.

Instead of trusting visual inspection alone, investigators validate the recovered artifact.

### Validation Result

| Attribute        | Result                      |
| ---------------- | --------------------------- |
| File Signature   | MZ Header Present           |
| DOS Stub         | Present                     |
| PE Format        | Valid                       |
| Executable Type  | Windows Portable Executable |
| Managed Assembly | Yes (.NET)                  |

---

## Screenshot — File Type Validation

<p align="center">
  <img src="../Screenshots/08_pe-filetype-confirmed.png" width="100%" alt="PE File Type Validation"/>
</p>

**Figure 9.** CyberChef confirming recovered artifact as a Windows Portable Executable.

---

# Preserving the Payload

Rather than executing the executable, investigators export the recovered binary.

### Evidence Preservation

* Save payload as binary file.
* Preserve original bytes.
* Avoid execution.
* Prepare for static reverse engineering.

### Suggested Filename

```text
payload.exe
```

---

# Static Malware Analysis Workflow

Static malware analysis examines executable contents **without running the malware**.

Advantages include:

* Safe investigation.
* Repeatable results.
* No behavioral side effects.
* Full source reconstruction for .NET binaries.

---

# Why ILSpy?

The recovered executable is a **managed .NET assembly**.

ILSpy provides:

* C# decompilation.
* Namespace inspection.
* Class hierarchy.
* Method reconstruction.
* Embedded resource viewing.

This makes it ideal for DFIR investigations involving .NET malware.

---

# Preparing ILSpy

The challenge includes ILSpy inside the tools directory.

### Investigation Steps

1. Extract ILSpy.
2. Launch application.
3. Open recovered executable.
4. Navigate through namespaces.
5. Inspect entry point.

---

## Screenshot — ILSpy Workspace

<p align="center">
  <img src="../Screenshots/09_ilspy-analysis.png" width="100%" alt="ILSpy Reverse Engineering"/>
</p>

**Figure 10.** Static reverse engineering of recovered executable using ILSpy.

---

# Reverse Engineering the Assembly

After opening the executable, investigators inspect the assembly tree.

### Components Identified

| Component         | Purpose                            |
| ----------------- | ---------------------------------- |
| Assembly Metadata | Executable information             |
| Namespace         | Malware project namespace          |
| Program Class     | Main execution logic               |
| Entry Point       | Initial malware execution routine  |
| Resources         | Embedded strings and configuration |

---

# Entry Point Analysis

The `Main()` method becomes the primary focus because it controls malware execution.

### High-Level Behavior

The recovered executable performs several defensive checks before executing additional commands.

Observed logic includes:

* Machine environment validation.
* Conditional execution.
* Process creation.
* Hidden command invocation.

---

## Analyst Observation

Conditional execution indicates malware may avoid executing unless specific environmental conditions are met.

Possible attacker goals include:

* Sandbox avoidance.
* Target validation.
* Environment fingerprinting.
* Controlled payload activation.

---

# Environment Validation

The malware compares the current machine name against a predefined expected value.

### Why Attackers Do This

Environment validation helps malware avoid:

* Sandboxes.
* Malware analysis environments.
* Automated scanners.
* Incorrect deployment targets.

This technique is commonly observed during targeted malware campaigns.

---

# Hidden Process Creation

The malware constructs a `ProcessStartInfo` object.

### Observed Characteristics

| Behavior          | Observation |
| ----------------- | ----------- |
| Hidden Window     | Yes         |
| Visible Console   | No          |
| Command Processor | cmd.exe     |
| Window Creation   | Disabled    |
| Execution Style   | Background  |

---

## Defensive Interpretation

Hidden command execution is suspicious because legitimate administrative utilities rarely suppress every visible window during routine execution.

Security monitoring solutions frequently inspect hidden PowerShell and hidden command prompt executions.

---

# Embedded Encoded Configuration

Further inside the executable, investigators discover another encoded string.

Characteristics include:

* Base64 format.
* Embedded inside executable.
* Used during hidden command execution.

### Important Note

The value is **redacted** throughout this documentation.

---

# Understanding Embedded Strings

Malware authors often embed configuration values inside executables rather than retrieving them remotely.

Advantages include:

* Offline operation.
* Reduced network indicators.
* Smaller infrastructure footprint.
* Easier persistence.

---

# Decoding Embedded Values

The encoded string can be decoded safely using CyberChef.

### Safe Workflow

```text
Encoded String
      │
      ▼
Base64 Decode
      │
      ▼
Readable Output
```

---

## Screenshot — Encoded Value Investigation

<p align="center">
  <img src="../Screenshots/10_encoded-final-stage.png" width="100%" alt="Encoded String Analysis"/>
</p>

**Figure 11.** Embedded encoded configuration identified during reverse engineering.

---

# Sanitized Recovery Result

After decoding, investigators recover the challenge output.

For responsible publication, this repository intentionally hides the recovered flag.

### Portfolio Version

```text
THM{REDACTED_FOR_PORTFOLIO}
```

---

## Screenshot — Final Sanitized Result

<p align="center">
  <img src="../Screenshots/11_flag-redacted.png" width="100%" alt="Sanitized Final Result"/>
</p>

**Figure 12.** Final investigation result with challenge flag intentionally redacted.

---

# Malware Behaviour Analysis

This executable behaves similarly to lightweight Windows malware loaders.

---

## Execution Flow Reconstruction

```text
PowerShell Loader
       │
       ▼
Read WMI Configuration
       │
       ▼
Decode Base64 Blob
       │
       ▼
Inflate Binary Payload
       │
       ▼
Load .NET Assembly
       │
       ▼
Validate Machine Environment
       │
       ▼
Launch Hidden Command
```

---

# Malware Capabilities Observed

<table><table-section header><table-row header><table-cell header width="260">Capability</table-cell><table-cell header>Assessment</table-cell></table-row></table-section><table-row><table-cell>Encoded PowerShell Loader</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>Base64 Obfuscation</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>Deflate Compression</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>Memory Assembly Loading</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>WMI Configuration Retrieval</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>Hidden Process Creation</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>Environment Validation</table-cell><table-cell>Observed</table-cell></table-row><table-row><table-cell>Network Communication</table-cell><table-cell>Not Required</table-cell></table-row><table-row><table-cell>Persistence via Registry</table-cell><table-cell>Not Observed</table-cell></table-row><table-row><table-cell>Scheduled Task Persistence</table-cell><table-cell>Not Observed</table-cell></table-row></table>

---

# Static vs Dynamic Analysis

## Static Analysis Performed

* Binary inspection.
* Decompiled source review.
* Embedded configuration analysis.
* Control flow reconstruction.
* String extraction.

## Dynamic Analysis Avoided

The payload was **never executed**.

Reasons include:

* Preserve evidence integrity.
* Avoid malware execution.
* Maintain safe investigation workflow.
* Follow DFIR best practices.

---

# Indicators Identified During Reverse Engineering

## PowerShell Indicators

* Hidden execution.
* Encoded commands.
* Base64 payload retrieval.
* Deflate decompression.

---

## Windows Indicators

* WMI configuration property.
* Managed .NET executable.
* Hidden process creation.
* In-memory assembly loading.

---

## Malware Characteristics

| Characteristic  | Evidence          |
| --------------- | ----------------- |
| Loader          | PowerShell        |
| Payload Storage | WMI Configuration |
| Obfuscation     | Base64            |
| Compression     | Deflate           |
| Execution       | .NET Assembly     |
| Analysis Tool   | ILSpy             |

---

# Evidence Summary

## Artifacts Collected

<table><table-section header><table-row header><table-cell header width="220">Artifact</table-cell><table-cell header>Purpose</table-cell></table-row></table-section><table-row><table-cell>`strings-encoded.txt`</table-cell><table-cell>Primary searchable evidence.</table-cell></table-row><table-row><table-cell>Encoded PowerShell Command</table-cell><table-cell>Loader recovery.</table-cell></table-row><table-row><table-cell>WMI Configuration Blob</table-cell><table-cell>Embedded payload storage.</table-cell></table-row><table-row><table-cell>`payload.exe`</table-cell><table-cell>Recovered executable sample.</table-cell></table-row><table-row><table-cell>ILSpy Decompiled Source</table-cell><table-cell>Static malware analysis evidence.</table-cell></table-row></table>

---

# Phase Completion Summary

## Objectives Completed

* Encoded PowerShell recovered.
* WMI configuration identified.
* Base64 blob decoded.
* Deflate payload extracted.
* Portable Executable validated.
* Malware decompiled safely.
* Hidden execution logic reconstructed.
* Final challenge output recovered and redacted.

---

# Phase 11 — Threat Intelligence & MITRE ATT&CK Analysis

After reconstructing the malware execution flow through static analysis, the next step is mapping observed behavior to the **MITRE ATT&CK Framework**. This helps defenders understand attacker tactics, techniques, and possible detection opportunities.

The malware in **After Hours** demonstrates several techniques frequently seen in Windows intrusions involving PowerShell loaders, fileless malware, and WMI-based persistence.

---

## MITRE ATT&CK Mapping

| ATT&CK ID     | Technique                                 | Evidence Observed                                                        |
| ------------- | ----------------------------------------- | ------------------------------------------------------------------------ |
| **T1059.001** | PowerShell                                | Encoded PowerShell execution used to retrieve hidden configuration data. |
| **T1027**     | Obfuscated / Encoded Files or Information | Base64-encoded payload embedded inside WMI configuration.                |
| **T1140**     | Deobfuscate / Decode Files or Information | PowerShell decodes Base64 content before decompression.                  |
| **T1562**     | Impair Defenses / Obfuscated Execution    | Hidden PowerShell execution with suppressed window visibility.           |
| **T1047**     | Windows Management Instrumentation        | WMI class abused for configuration storage.                              |
| **T1620**     | Reflective / In-Memory Loading            | Assembly loaded directly into memory through PowerShell.                 |
| **T1106**     | Native API / Process Execution            | Process creation through Windows command processor.                      |

---

## ATT&CK Kill Chain Visualization

```text id="qjxd8l"
Reconnaissance
      │
      ▼
Persistence (WMI Configuration)
      │
      ▼
PowerShell Loader
      │
      ▼
Base64 Decoding
      │
      ▼
Deflate Decompression
      │
      ▼
Memory Assembly Loading
      │
      ▼
Hidden Process Execution
```

---

# Threat Behaviour Summary

## Malware Execution Characteristics

<table><table-section header><table-row header><table-cell header width="220">Behavior</table-cell><table-cell header>Investigation Finding</table-cell></table-row></table-section><table-row><table-cell>Persistence Mechanism</table-cell><table-cell>Configuration hidden inside WMI property.</table-cell></table-row><table-row><table-cell>Execution Method</table-cell><table-cell>PowerShell executed through `cmd.exe` with hidden window.</table-cell></table-row><table-row><table-cell>Payload Storage</table-cell><table-cell>Base64 + Deflate compressed executable.</table-cell></table-row><table-row><table-cell>Payload Type</table-cell><table-cell>Managed .NET Portable Executable.</table-cell></table-row><table-row><table-cell>Execution Style</table-cell><table-cell>Memory assembly loading without traditional file execution.</table-cell></table-row><table-row><table-cell>Defense Evasion</table-cell><table-cell>Encoded command and hidden process creation.</table-cell></table-row></table>

---

# Phase 12 — Indicators of Compromise (IOCs)

The investigation identified several behavioral indicators useful during Windows threat hunting.

All sensitive challenge-specific values have been sanitized.

---

## PowerShell Indicators

<table><table-section header><table-row header><table-cell header width="260">Indicator</table-cell><table-cell header>Description</table-cell></table-row></table-section><table-row><table-cell>Hidden PowerShell execution</table-cell><table-cell>PowerShell launched without visible console window.</table-cell></table-row><table-row><table-cell>Encoded PowerShell (`-enc`)</table-cell><table-cell>PowerShell executed using Base64-encoded command argument.</table-cell></table-row><table-row><table-cell>Memory assembly loading</table-cell><table-cell>.NET assembly loaded directly into memory.</table-cell></table-row><table-row><table-cell>Compressed configuration blob</table-cell><table-cell>Base64 content inflated using Deflate compression.</table-cell></table-row></table>

---

## Windows Management Instrumentation Indicators

| IOC                           | Hunting Opportunity                             |
| ----------------------------- | ----------------------------------------------- |
| Custom WMI property           | Investigate non-standard WMI namespaces.        |
| Configuration blob inside WMI | Search unusually large encoded property values. |
| WMI persistence               | Audit custom classes and consumers.             |

---

## Executable Indicators

| IOC                     | Observation               |
| ----------------------- | ------------------------- |
| PE Header               | Valid Windows executable. |
| DOS Stub                | Present.                  |
| .NET Metadata           | Managed assembly.         |
| Entry Point             | Custom execution routine. |
| Hidden Process Creation | Observed.                 |

---

## Sanitized Indicators Table

| Category            | Portfolio Version             |
| ------------------- | ----------------------------- |
| Flag                | `THM{REDACTED_FOR_PORTFOLIO}` |
| Base64 Payload      | `BASE64_REDACTED...`          |
| WMI Property Value  | `REDACTED_CONFIGURATION_DATA` |
| Embedded Credential | `REDACTED`                    |

---

# Phase 13 — Detection Engineering

A DFIR report should include defensive detection opportunities.

The following examples are educational hunting concepts.

---

## Sigma Rule — Encoded PowerShell Execution

```yaml id="rj6qv1"
title: Suspicious Encoded PowerShell Execution
id: after-hours-powershell-loader
status: experimental

logsource:
  product: windows
  category: process_creation

detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains:
      - '-enc'
      - '-EncodedCommand'

  condition: selection

level: high

falsepositives:
  - Administrative automation
  - Enterprise deployment scripts
```

### Detection Goal

Identify PowerShell processes launched using encoded commands.

---

## Sigma Rule — Hidden PowerShell Window

```yaml id="uijjdz"
title: Hidden PowerShell Window Execution

logsource:
  product: windows
  category: process_creation

detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains:
      - '-Window Hidden'
      - '-nop'
      - '-sta'

condition: selection

level: medium
```

---

## Sigma Rule — WMI Configuration Retrieval

```yaml id="nn6gsy"
title: Suspicious WMI Configuration Access

logsource:
  product: windows
  category: wmi_event

detection:
  selection:
    Query|contains:
      - 'Win32_'
      - 'ConfigData'

condition: selection

level: medium
```

---

# Sysmon Hunting Opportunities

Windows Sysmon provides rich telemetry useful for hunting similar malware.

---

## Event ID 1 — Process Creation

Hunt for:

```text id="rslm8y"
powershell.exe
cmd.exe
```

Interesting command line indicators include:

* `-EncodedCommand`
* `-Window Hidden`
* `-NoProfile`
* `-Sta`

---

## Event ID 7 — Image Loaded

Monitor:

* Managed .NET assemblies.
* Unusual DLL loading from temporary locations.
* Memory-loaded executables.

---

## Event ID 19–21 — WMI Activity

Investigate:

* New WMI classes.
* WMI event consumers.
* WMI filters.
* Custom namespaces.

---

## Event ID 11 — File Creation

Potential hunting opportunities include:

* Newly written executables.
* Temporary payload extraction.
* Suspicious PowerShell artifacts.

---

# Microsoft Defender Hunting Ideas

Defenders can search Microsoft Defender for Endpoint telemetry.

### Example Hunting Concepts

<table><table-section header><table-row header><table-cell header width="240">Telemetry</table-cell><table-cell header>Hunt For</table-cell></table-row></table-section><table-row><table-cell>DeviceProcessEvents</table-cell><table-cell>Encoded PowerShell execution.</table-cell></table-row><table-row><table-cell>DeviceEvents</table-cell><table-cell>WMI configuration activity.</table-cell></table-row><table-row><table-cell>DeviceImageLoadEvents</table-cell><table-cell>Suspicious managed assemblies.</table-cell></table-row><table-row><table-cell>DeviceRegistryEvents</table-cell><table-cell>Unexpected persistence attempts.</table-cell></table-row></table>

---

# Phase 14 — Incident Response Timeline

## Investigation Timeline

<table><table-section header><table-row header><table-cell header width="140">Time</table-cell><table-cell header>Investigation Activity</table-cell></table-row></table-section><table-row><table-cell>**T+00**</table-cell><table-cell>Evidence package extracted.</table-cell></table-row><table-row><table-cell>**T+05**</table-cell><table-cell>Artifacts enumerated using `strings`.</table-cell></table-row><table-row><table-cell>**T+10**</table-cell><table-cell>Encoded PowerShell identified.</table-cell></table-row><table-row><table-cell>**T+15**</table-cell><table-cell>PowerShell decoded safely in CyberChef.</table-cell></table-row><table-row><table-cell>**T+20**</table-cell><table-cell>WMI configuration property isolated.</table-cell></table-row><table-row><table-cell>**T+30**</table-cell><table-cell>Deflate payload decompressed.</table-cell></table-row><table-row><table-cell>**T+40**</table-cell><table-cell>Portable Executable recovered.</table-cell></table-row><table-row><table-cell>**T+50**</table-cell><table-cell>Executable analyzed using ILSpy.</table-cell></table-row><table-row><table-cell>**T+60**</table-cell><table-cell>Malware behavior reconstructed and documented.</table-cell></table-row></table>

---

## Investigation Flow Summary

```text id="5pxzth"
Evidence Collection
        │
        ▼
Artifact Enumeration
        │
        ▼
PowerShell Discovery
        │
        ▼
CyberChef Decoding
        │
        ▼
Payload Recovery
        │
        ▼
PE Validation
        │
        ▼
ILSpy Analysis
        │
        ▼
IOC Documentation
        │
        ▼
Defensive Recommendations
```

---

# Phase 15 — Evidence Summary

## Evidence Collected During Investigation

<table><table-section header><table-row header><table-cell header width="260">Evidence Artifact</table-cell><table-cell header>Investigation Purpose</table-cell></table-row></table-section><table-row><table-cell>Encoded PowerShell Command</table-cell><table-cell>Identify malware execution chain.</table-cell></table-row><table-row><table-cell>WMI Configuration Blob</table-cell><table-cell>Recover hidden payload.</table-cell></table-row><table-row><table-cell>Base64 Configuration</table-cell><table-cell>Decode embedded malware data.</table-cell></table-row><table-row><table-cell>Deflate Payload</table-cell><table-cell>Recover executable bytes.</table-cell></table-row><table-row><table-cell>Portable Executable</table-cell><table-cell>Validate malware format.</table-cell></table-row><table-row><table-cell>ILSpy Decompiled Code</table-cell><table-cell>Understand malware behavior.</table-cell></table-row><table-row><table-cell>Screenshot Timeline</table-cell><table-cell>Visual forensic evidence.</table-cell></table-row></table>

---

# Phase 16 — Defensive Recommendations

## Windows Hardening Recommendations

### PowerShell Logging

Enable:

* Script Block Logging.
* Module Logging.
* Transcription Logging.

Benefits include visibility into encoded PowerShell execution.

---

### WMI Monitoring

Monitor:

* WMI Event Consumers.
* WMI Filters.
* Custom WMI Classes.
* Permanent Event Subscriptions.

Unexpected custom namespaces deserve investigation.

---

### Process Creation Monitoring

Alert on:

* Hidden PowerShell windows.
* Encoded PowerShell.
* PowerShell launched by `cmd.exe`.
* Office spawning PowerShell.
* WMI spawning PowerShell.

---

### Memory-Based Malware Detection

Use:

* Microsoft Defender.
* Sysmon.
* AMSI.
* EDR telemetry.

Focus on reflective assembly loading behaviors.

---

### Incident Response Recommendations

| Recommendation                      | Reason                       |
| ----------------------------------- | ---------------------------- |
| Preserve artifacts before execution | Maintain evidence integrity. |
| Decode offline using CyberChef      | Avoid malware execution.     |
| Reverse engineer statically first   | Understand behavior safely.  |
| Validate executable signatures      | Confirm payload type.        |
| Document every IOC discovered       | Improve repeatability.       |

---

# Phase 17 — Lessons Learned

## Technical Lessons

This room reinforces several important DFIR concepts.

### Windows Persistence Isn't Always Registry-Based

Persistence can exist in:

* WMI.
* Scheduled Consumers.
* COM objects.
* Services.
* Event subscriptions.

---

### Encoded PowerShell Doesn't Mean Encryption

Base64 often hides commands rather than protecting them.

Investigators should always decode safely before analysis.

---

### Compression Is an Obfuscation Layer

Malware frequently combines:

1. Encoding.
2. Compression.
3. Memory loading.

Understanding each layer simplifies investigation.

---

### Static Analysis Can Reveal Complete Malware Logic

Because this payload is a managed .NET assembly, ILSpy reconstructs readable source code without executing malware.

---

## DFIR Skills Practiced

* Evidence acquisition.
* Artifact triage.
* IOC extraction.
* WMI hunting.
* PowerShell decoding.
* Malware configuration recovery.
* Static reverse engineering.
* Threat documentation.

---

# Phase 18 — Tools Used

| Tool                | Investigation Purpose            |
| ------------------- | -------------------------------- |
| TryHackMe AttackBox | Secure investigation environment |
| Linux CLI           | Evidence handling                |
| `strings`           | String extraction                |
| `grep`              | IOC discovery                    |
| CyberChef           | Base64 & Deflate decoding        |
| ILSpy               | Reverse engineering              |
| WMI Knowledge       | Persistence analysis             |

---

# Screenshot Timeline

| Screenshot                     | Investigation Stage            |
| ------------------------------ | ------------------------------ |
| `01_room-overview.png`         | Room overview                  |
| `02_workspace-access.png`      | Investigation environment      |
| `03_artifact-inventory.png`    | Evidence collection            |
| `04_powershell-discovery.png`  | PowerShell discovery           |
| `05_base64-decoding.png`       | CyberChef decoding             |
| `06_deflate-payload.png`       | Payload extraction             |
| `07_pe-identification.png`     | PE detection                   |
| `08_pe-filetype-confirmed.png` | File validation                |
| `09_ilspy-analysis.png`        | Reverse engineering            |
| `10_encoded-final-stage.png`   | Encoded payload analysis       |
| `11_flag-redacted.png`         | Sanitized investigation result |

---

# Portfolio Takeaways

This investigation demonstrates practical skills expected from a **SOC Analyst**, **DFIR Analyst**, or **Malware Analyst**.

## Competencies Demonstrated

<table><table-section header><table-row header><table-cell header width="260">Area</table-cell><table-cell header>Evidence in This Report</table-cell></table-row></table-section><table-row><table-cell>Windows Forensics</table-cell><table-cell>Artifact parsing and WMI investigation.</table-cell></table-row><table-row><table-cell>PowerShell Security</table-cell><table-cell>Encoded command recovery and decoding.</table-cell></table-row><table-row><table-cell>Reverse Engineering</table-cell><table-cell>.NET malware inspection using ILSpy.</table-cell></table-row><table-row><table-cell>Malware Analysis</table-cell><table-cell>Payload extraction and execution flow reconstruction.</table-cell></table-row><table-row><table-cell>Incident Documentation</table-cell><table-cell>Professional forensic report suitable for portfolio publication.</table-cell></table-row><table-row><table-cell>Detection Engineering</table-cell><table-cell>Sigma rules, Sysmon hunting ideas, IOC documentation.</table-cell></table-row></table>

---

# Conclusion

The **After Hours** TryHackMe room provides an excellent introduction to **Windows Digital Forensics and Malware Analysis** through a realistic investigation workflow instead of exploitation.

During this investigation, encoded PowerShell artifacts were identified, WMI configuration data was recovered, a compressed Portable Executable payload was extracted, and the malware execution chain was reconstructed through static reverse engineering with ILSpy.

Rather than focusing on obtaining the challenge flag, this documentation emphasizes the complete DFIR methodology:

* Preserve evidence.
* Analyze artifacts safely.
* Decode malicious configuration.
* Recover embedded payloads.
* Reverse engineer malware behavior.
* Document indicators and defensive recommendations.

This report is designed as a **professional cybersecurity portfolio artifact** demonstrating practical Windows forensic investigation, malware analysis, and incident response documentation skills.

---

## Responsible Disclosure Statement

All challenge secrets have been intentionally **redacted**.

This repository publishes:

* Investigation methodology.
* Forensic analysis process.
* Detection opportunities.
* Defensive lessons.

It does **not** publish challenge flags, passwords, embedded secrets, or reusable malicious payload values.

---

<div align="center">

# 🌙 After Hours — Investigation Completed

### Windows DFIR • Malware Analysis • Reverse Engineering • Threat Hunting

**Cybersecurity Portfolio Project — Anurag Revankar**

*TryHackMe Hacker Holidays 2026 • Day 12*

</div>

