# After Hours — TryHackMe Walkthrough

[![TryHackMe](https://img.shields.io/badge/TryHackMe-Hacker%20Holidays%202026-212C42?logo=tryhackme&logoColor=white)](#)
[![Category](https://img.shields.io/badge/Category-Forensics-7C3AED)](#)
[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-F59E0B)](#)
[![Documentation](https://img.shields.io/badge/Docs-GitHub%20Pages-2563EB)](#)

> **Hacker Holidays 2026 · Day 12 — After Hours**

A portfolio-focused investigation of a Windows persistence/forensics challenge involving custom system artifacts, an encoded PowerShell loader, an embedded .NET payload, and static analysis with ILSpy.

## What this repository demonstrates

- Evidence-first triage of filesystem artifacts
- String extraction and targeted searching
- Base64 and Deflate decoding
- Windows PE identification
- Static .NET reverse engineering with ILSpy
- Traceability from artifact → loader → payload → final encoded value
- Safe publication practices with flags and direct-answer material intentionally redacted

## Investigation chain

```text
System artifacts
      │
      ▼
strings / targeted grep
      │
      ▼
Encoded PowerShell loader
      │
      ├── Base64 decode
      └── null-byte cleanup
      │
      ▼
WMI custom configuration reference
      │
      ▼
Base64 + Raw Inflate
      │
      ▼
Embedded Windows PE
      │
      ▼
ILSpy static analysis
      │
      ▼
Encoded value in .NET code
      │
      ▼
Base64 decode
      │
      ▼
FLAG [REDACTED]
```

## Repository layout

```text
After-Hours-TryHackMe-Walkthrough/
├── Documentation/
│   ├── Documentation.md
│   └── Documentation.docx
├── Resources/
│   └── notes.md
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
├── docs/
│   └── index.md
├── .github/workflows/pages.yml
└── README.md
```

## Public-writeup policy

This write-up intentionally does **not** publish:

- the room attachment password;
- full Base64 loader/payload strings;
- the final encoded flag value;
- the recovered flag itself.

The screenshots were also prepared for portfolio publication with answer-bearing values redacted.

## Documentation

The full investigation is available in:

**[Documentation/Documentation.md](Documentation/Documentation.md)**

A portfolio-rendered version is available through **GitHub Pages**:

**[Open the web documentation](https://anurag-rvnkr1.github.io/After-Hours-TryHackMe-Walkthrough/)**

## Ethics

This material documents a controlled TryHackMe lab. The techniques are presented for authorized security training, forensic analysis, reverse engineering practice, and portfolio documentation only.

---

Made for a cybersecurity learning portfolio.
