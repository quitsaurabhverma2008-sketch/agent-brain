---
name: bytecode-viewer
description: "GUI tool for viewing and decompiling Java bytecode, APK, DEX, JAR, and class files. Displays multiple decompilers side by side (CFR, Procyon, Fernflower, JD-GUI, Krakatau, JADX) with bytecode disassembly, search, and plugin support. Use when analyzing APK/JAR internals, comparing decompiler output, or inspecting smali/bytecode."
license: GPL-3.0 (Konloch/bytecode-viewer)
---

# Bytecode Viewer (Konloch)

Installed at: `/data/data/com.termux/files/home/tools/bytecode-viewer/Bytecode-Viewer-2.13.2.jar`
Launcher: `bytecode-viewer` (in Termux PATH)

## Usage
```bash
bytecode-viewer                            # open GUI
bytecode-viewer /path/to/app.apk           # open APK directly
bytecode-viewer /path/to/file.jar          # open JAR directly
```

## Requirements
- Java 8+ (OpenJDK 21 installed ✅)
- A display: GUI needs **X11** — on Termux use **Termux:X11** app or VNC server.

## When to use
- Compare decompiler outputs (CFR vs Procyon vs Fernflower vs JADX)
- Inspect APK/DEX/JAR structure visually
- Search strings/classes across the whole app
- Bytecode-level analysis alongside jadx CLI

## Command-line alternatives (headless)
- `jadx -d out app.apk` (decompile)
- `apktool d app.apk` (resources + smali)
- `unzip -l app.apk` (list contents)
- `dexdump`/`baksmali` for dex-level work
