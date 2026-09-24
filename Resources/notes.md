# After Hours — Quick Analyst Notes

## Room
- Hacker Holidays 2026 — Day 12
- Challenge: After Hours
- Category: Forensics
- Difficulty: Medium

## Core workflow

```text
strings -a *
    ↓
grep -i powershell | sort -u
    ↓
Base64 decode / remove null bytes
    ↓
identify Win32_HardwareTelemetry
    ↓
search surrounding strings
    ↓
Base64 + Raw Inflate
    ↓
MZ / PE validation
    ↓
ILSpy
    ↓
locate embedded encoded value
    ↓
Base64 decode
```

## Useful commands

```bash
strings -a * > strings-encoded.txt
grep -i powershell strings-encoded.txt | sort -u
grep -C 3 'Win32_HardwareTelemetry' strings-encoded.txt | sort -u
file payload.exe
```

## Static-analysis observations

- Encoded PowerShell is used as the first-stage loader.
- The loader accesses a custom WMI provider/class.
- `ConfigData` contains another encoded/compressed stage.
- The recovered object is a Windows PE.
- The PE is a .NET assembly and can be inspected safely with ILSpy.
- The .NET entry point includes host-specific logic and a hidden command invocation.
- The final answer is stored as an encoded value and requires one final Base64 decode.

## Public portfolio redactions

Do not publish:
- room attachment password;
- complete Base64 loader;
- complete embedded payload;
- final encoded answer;
- recovered flag.

These notes are intentionally focused on methodology rather than answer disclosure.
