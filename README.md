# ThinkPad X270 Hackintosh — Custom Sleep & USB-Optimized EFI
<img src="[https://github.com/fahmin002/Thinkpad-X270-Hackintosh-EFI/ScreenShot.png](https://github.com/fahmin002/Thinkpad-X270-Hackintosh-EFI/blob/main/ScreenShot.png)" alt="Screen Shot" />

This is my personal OpenCore Hackintosh EFI for the **Lenovo ThinkPad X270**, tuned for:

- **Stable sleep (lid close)** without instant wake  
- **Correct USB mapping** using USBToolBox + UTBMap  
- Minimal essential kexts & ACPI  
- No NVRAM clearing (safe for ThinkPad BIOS)  
- Lean and maintainable EFI suitable for Sonoma/Ventura

Inspired by  
[`BlackOtton/ThinkPad-X270-OpenCore-Hackintosh`](https://github.com/BlackOtton/ThinkPad-X270-OpenCore-Hackintosh) :contentReference[oaicite:0]{index=0}

---

## 🚀 Overview

| Feature | Status |
|---------|--------|
| Full USB stability | ✅ |
| Lid sleep / open wake | ✅ |
| No instant wake (XDCI) | ✅ |
| Vanilla ACPI (minimal) | ✅ |
| Safe pmset config | ✅ |
| Only essential kexts | ✅ |

---

## 📁 EFI FOLDER STRUCTURE

EFI/
├── BOOT/
├── OC/
| ├── ACPI/
| | ├── SSDT-EC.aml
| | ├── SSDT-PLUG.aml
| | └── (others only if needed)
| ├── Kexts/
| | ├── Lilu.kext
| | ├── VirtualSMC.kext
| | ├── SMCProcessor.kext
| | ├── SMCBatteryManager.kext
| | ├── USBToolBox.kext
| | ├── UTBMap.kext
| | └── (other drivers you actually need)
| ├── Drivers/
| | ├── OpenRuntime.efi
| | └── OpenCanopy.efi
| └── config.plist

## 🔌 USB MAPPING (USBToolBox + UTBMap)

USB is mapped manually with **UTBMap.kext**. The key port assignment is:

| Port | Connector |
|------|-----------|
| HS01–HS03 | `0` (USB 2.0) |
| HS04–HS06 | `255` (Internal) |
| SS01–SS03 | `USB 3.x` (default) |

⚠️ HS ports should *never be mapped as USB3/typeC* if they are physical USB2 on the laptop — incorrect mapping causes wake events (`XDCI`).  
This solved instant wake issues without NVRAM reset.

---

## ⚡ POWER MANAGEMENT (pmset)

These settings ensure lid sleep / idle sleep behave correctly:

```bash
sudo pmset -a lidwake 1
sudo pmset -a sleep 10
sudo pmset -a displaysleep 2
sudo pmset -a proximitywake 0
sudo pmset -a powernap 0
sudo pmset -a tcpkeepalive 0
