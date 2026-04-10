# Incident Report: VLC Multimedia Codec Issues on Fedora Workstation 42

**Date:** 2026-04-03
**Status:** RESOLVED
**System:** Fedora Workstation 42
**Category:** Multimedia / Codec

---

## Overview

VLC Media Player on a fresh Fedora Workstation 42 installation could not play .mkv video files. Error messages indicated missing codec support for `hevc` (H.265) video and `eac3` (E-AC3) audio formats.

---

## Root Cause

Fedora's official repositories adhere to a FOSS-only policy and do not distribute patent-encumbered multimedia codecs. The default VLC package is compiled without support for proprietary formats like HEVC/H.265 and E-AC3.

---

## Error Messages

```
Codec not supported: VLC could not decode the format "hevc" (MPEG-H Part2/HEVC (H.265))
Codec not supported: VLC could not decode the format "eac3" (A/52 B Audio (aka E-AC3))
```

---

## Solution

Enable the RPM Fusion third-party repository and install the required codec packages.

### Commands (Fedora 42 / dnf5)

```bash
# 1. Enable RPM Fusion free and nonfree repositories
sudo dnf5 install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

# 2. Swap Fedora's limited ffmpeg-free for full ffmpeg from RPM Fusion
sudo dnf5 swap ffmpeg-free ffmpeg --allowerasing

# 3. Install essential multimedia codec packages
sudo dnf5 install gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly lame-libs

# 4. Reinstall VLC to link against the new codec libraries
sudo dnf5 reinstall vlc

# 5. Reboot to apply changes system-wide
reboot
```

---

## Key Learnings

- **dnf5 is the new standard in Fedora 42** — legacy `dnf` commands like `groupupdate` are obsolete
- **Package group names are deprecated** — "Multimedia" and "Sound and Video" groups no longer exist. Direct package installation is the reliable method.
- **Swap with --allowerasing is necessary** — the ffmpeg-free → ffmpeg swap removes the old package, so `--allowerasing` is required

---

## Action Items

- [ ] Document this for future Fedora 42 installations
- [ ] Consider if this needs to be part of the standard Fedora setup process
