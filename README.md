
# 🚀 Hyprland + NVIDIA + OBS: Stable Configuration Guide

This repository documents a high-performance, reproducible configuration for running **Hyprland** on **NVIDIA GPUs** with a specific focus on **OBS Studio** stability. 

The goal of this configuration is to eliminate micro-stutter, ensure consistent frame pacing, and provide a reliable environment for both daily productivity and high-quality content creation.

## ⚙️ Hyprland Configuration

### Display Settings
To maintain perfect frame pacing on NVIDIA + Wayland, fractional scaling is intentionally avoided.

```hyprlang
# monitor = name, resolution@hz, pos, scale
monitor = eDP-1, 1920x1200@165, 0x0, 1
```

### Environment Variables
Add these to your `hyprland.conf` to optimize the NVIDIA driver's behavior under Wayland and ensure toolkits (GTK/QT) prioritize Wayland.

```bash
# Fix micro-stutter and ensure consistent frame timing
env = __GL_VRR_ALLOWED,0
env = __GL_GSYNC_ALLOWED,0
env = __GL_SYNC_TO_VBLANK,1

```

---

## 🎥 OBS Studio Configuration

### Optimized Launch Method
To ensure stable screen capture on NVIDIA-based Wayland sessions, it is recommended to force OBS into XWayland mode:

```bash
obs --disable-wayland
```

### 🔧 Output Settings (Advanced Mode)
These settings target high-fidelity recording with minimal performance impact.

| Setting | Value |
| :--- | :--- |
| **Format** | MKV (Prevents data loss on crash) |
| **Encoder** | NVIDIA NVENC (New) |
| **Rate Control** | CQP |
| **CQ Level** | 18 |
| **Preset** | P5 (Quality) |
| **Profile** | Main |
| **B-Frames** | 2 (Enabled as Reference) |

### Video & Rendering
| Setting | Value |
| :--- | :--- |
| **Base/Output Resolution** | 1920×1200 |
| **Common FPS** | 60 |
| **Color Format** | NV12 |
| **Color Space / Range** | 709 / Partial |

---

## 🎞️ Encoder Selection Guide

| Codec | Recommended Use |
| :--- | :--- |
| **AV1** | Best for high-efficiency archival and modern hardware. |
| **HEVC (H.265)** | The "Sweet Spot" for balance between file size and quality. |
| **H.264** | Use only for maximum compatibility with older software. |

---

## ⚠️ Best Practices & Notes

> [!IMPORTANT]
> **Avoid Fractional Scaling:** Even on modern NVIDIA drivers, fractional scaling (e.g., 1.25) can introduce sub-pixel blurring and frame-time variance.

- **Disable VRR:** Variable Refresh Rate on laptop panels can cause flickering in certain Wayland implementations.
- **Stay Standard:** Avoid injecting custom NVENC parameters unless specifically troubleshooting.
- **File Safety:** Always record to `.mkv`. You can use OBS's "Remux to MP4" feature after the recording is finished.
