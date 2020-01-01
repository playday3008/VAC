# VAC 🛡️
This repository contains parts of source code of Valve Anti-Cheat recreated from machine code.

## Introduction
Valve Anti-Cheat (VAC) is user-mode noninvasive anti-cheat system developed by Valve. It is delivered in form of modules (dlls) streamed from remote server. `steamservice.dll` handles module loading.

## Modules
| ID | Purpose | .text section raw size | Source folder |
| --- | --- | --- | --- |
| 1 | Collect information about system configuration.<br>This module is loaded first and sometimes even before any VAC-secured game is launched. | 0x5C00 | Modules/SystemInfo

## Encryption / Hashing
VAC uses several encryption / hashing methods:
- MD5 - hashing data read from process memory
- ICE - decryption of imported functions names and encryption of scan results
- CRC32 - hashing table of WinAPI functions addresses
- Xor - encryption of function names on stack, e.g `NtQuerySystemInformation`. Strings are xor-ed with `^` or `>` or `&` char.

## Module Description

### #1 - SystemInfo
This module is loaded first and sometimes even before any VAC-secured game is launched.

It calls `NtQuerySystemInformation` API funcion with following `SystemInformationClass` values (in order they appear in code):
 - SystemTimeOfDayInformation
 - SystemCodeIntegrityInformation
 - SystemDeviceInformation
 - SystemKernelDebuggerInformation
 - SystemBootEnvironmentInformation
 - SystemRangeStartInformation

 For more information about `SYSTEM_INFORMATION_CLASS` enum see [Geoff Chappell's page](https://www.geoffchappell.com/studies/windows/km/ntoskrnl/api/ex/sysinfo/class.htm).
