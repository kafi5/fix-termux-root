# Termux Sudo/Root Fix (Tsu Patcher)

![Views](https://komarev.com/ghpvc/?username=msrofficial-fix-termux-root&label=Repository%20Views&color=0e75b6&style=flat)
![Termux](https://img.shields.io/badge/Termux-Android-green?logo=android)
![Root](https://img.shields.io/badge/Root-Required-red)
![Sudo](https://img.shields.io/badge/Fix-Sudo%2FTsu-blue)

**[📖 বাংলায় পড়তে এখানে ক্লিক করুন (Read in Bangla)](README-bn.md)**

**A complete and automated solution to fix "No superuser binary detected" or "Root permission denied" errors in Termux for Android.**

**Keywords:** `termux root fix`, `termux sudo error`, `tsu patcher`, `kernelsu termux fix`, `magisk termux root`, `termux su binary not found`, `android terminal root`, `msrofficial`

---

## The Problem
Sometimes, even if your Android device is rooted (especially with modern solutions like KernelSU or the latest Magisk), Termux's default `tsu` package fails to locate the root binary (`su`). This typically occurs because the default script does not search inside `/debug_ramdisk/su` or other custom mounting paths used by systemless root methods.

This repository provides an automated script to scan all possible root matrices, purge conflicting binaries, and inject a working `sudo` environment.

---

## Method 1: The Automated Script (Recommended)
This is the fastest and most reliable method. It downloads the `fix.sh` script, executes a root matrix scan, installs necessary dependencies, and drops you into a root shell.

Run the following command in your Termux terminal:

```bash
curl -sO https://raw.githubusercontent.com/kafi5/fix-termux-root/main/fix.sh && chmod +x fix.sh && ./fix.sh
```

---

## Method 2: The One-Line Patch (Fallback)
If the automated script fails or you prefer to strictly patch the existing `tsu` package without removing it, run this single command to backup and patch the file instantly:

```bash
pkg update && pkg install tsu -y
cp $PREFIX/bin/tsu $PREFIX/bin/tsu.bak && sed -i 's|^SU_BINARY_SEARCH=.*|SU_BINARY_SEARCH=("/system/xbin/su" "/system/bin/su" "/debug_ramdisk/su" "/sbin/su")|' $PREFIX/bin/tsu && echo "tsu patched successfully"
```

**Verification:** Type `tsu` or `sudo su`. If the `#` prompt appears, you are successfully rooted.

---

## Method 3: The Manual Edit (Fallback)
If you want full control over the process, you can manually edit the configuration file using a text editor.

### 1. Install necessary tools and repositories:
```bash
pkg install tsu nano file root-repo sudo -y
```

### 2. Open the tsu file in the nano editor:
```bash
nano $PREFIX/bin/tsu
```

### 3. Apply the fix:
Scroll down until you find the line starting with `SU_BINARY_SEARCH=` and add `"/debug_ramdisk/su"` inside the brackets. The modified line should look exactly like this:

```text
SU_BINARY_SEARCH=("/system/xbin/su" "/system/bin/su" "/debug_ramdisk/su")
```

### 4. Save and Exit:
* Press `CTRL + X`
* Press `Y` (for Yes)
* Press `Enter`

---

## Author & Connect
Developed and maintained by **RH KAFI**. Feel free to connect for updates or support:

* **GitHub:** [msrofficial](https://github.com/kafi5)
* **Telegram:** [@rhktech1m](https://t.me/rhktech1m)
* **Telegram Channel:** [@duomodhub](https://t.me/duomodhub)
