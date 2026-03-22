# Audio-Video Skill — Feature Overview

A complete reference of what this skill can do and why each capability matters.

---

## Core Features (Original)

| Feature | Description |
|---------|-------------|
| Format Conversion | Transcode between MP4, MKV, MOV, WebM, and more |
| Audio Processing | Extract, convert, normalize, filter audio tracks |
| Video Editing | Trim, concat, scale, rotate, crop, overlay, color grade |
| Subtitles | Burn-in or soft subtitles, SRT/ASS support |
| Thumbnails | Single frame, sprite sheets, auto-best-frame |
| HLS / DASH / RTMP | Adaptive bitrate streaming and live streaming |
| Screen & Webcam Capture | Record screen and webcam on macOS, Linux, Windows |
| GIF & WebP | High-quality animated GIF and WebP generation |
| Batch Processing | Convert entire directories, parallel encoding |
| Advanced Filtergraphs | Side-by-side, PiP, Ken Burns, blur, zoom/pan |
| Quality Analysis | PSNR, SSIM, VMAF perceptual quality metrics |
| Platform Presets | Ready commands for YouTube, TikTok, Instagram, Discord, etc. |

---

## New Features

### 1. Video Stabilization
**Skill section:** SECTION M | **Filter:** `vidstabdetect` + `vidstabtransform`

Footage from handheld cameras, drones, or action cameras is often shaky. Two-pass stabilization smooths this out in post without needing a gimbal. Pass 1 (`vidstabdetect`) analyzes the motion in the video and writes a transforms file. Pass 2 (`vidstabtransform`) applies the correction. An optional `unsharp` pass sharpens the result to counteract the softening caused by motion correction.

Common use cases: vloggers, event videographers, drone footage, sports and action cameras (GoPro, DJI).

**Supports:** macOS, Linux, Windows (requires ffmpeg built with `--enable-libvidstab`)

---

### 2. 360° / VR Video
**Skill section:** SECTION N | **Filter:** `v360`

360° cameras (GoPro Max, Insta360, Ricoh Theta) output footage in equirectangular or cubemap projections. Without proper projection conversion and spherical metadata injection, platforms like YouTube VR and Facebook 360 won't recognize the file as a 360° video — it will just play as a flat distorted video.

This feature covers:
- Equirectangular ↔ cubemap (3x2) conversion
- Extracting a flat "normal" view from a 360° file
- Injecting spherical metadata for platform compatibility

**Supports:** macOS, Linux, Windows (v360 filter is cross-platform)

---

### 3. HDR / Color Science
**Skill section:** SECTION O | **Filters:** `zscale`, `tonemap`, `colorspace`

Modern phones and cameras (iPhone, Android, Sony, Canon) shoot in HDR by default — 10-bit wide color gamut (bt2020, smpte2084). When that footage is shared on an SDR platform, edited in an SDR timeline, or played on an SDR display without proper conversion, colors look washed out, blown out, or oversaturated.

This feature covers:
- HDR10 → SDR tone mapping (hable, reinhard, mobius algorithms)
- Colorspace conversion (bt2020 → bt709)
- Encoding proper HDR10 output with display metadata
- Tagging files with correct color space metadata

**Supports:** macOS, Linux, Windows (requires ffmpeg with `--enable-libzimg`)

---

### 4. Advanced Streaming
**Skill section:** SECTION P | **Protocols/muxers:** `srt`, `tee`, `segment`

Three distinct streaming capabilities:

**SRT (Secure Reliable Transport):** SRT is replacing RTMP for live contribution because it actively compensates for packet loss and network jitter. Critical for streaming over unstable connections — cellular hotspots, satellite uplinks, long-distance contribution feeds.

**Tee muxer (multi-endpoint restream):** Stream to YouTube, Twitch, and Facebook simultaneously from a single ffmpeg process. No need to run multiple encoders or pay for a restreaming service.

**Rolling window DVR:** Continuous loop recording that keeps only the last N minutes of footage, automatically deleting older segments. Essential for security cameras and broadcast monitoring where storage is limited and you only need recent footage.

**Supports:** macOS, Linux, Windows (SRT requires ffmpeg with `--enable-libsrt`, included in full builds)

---

### 5. Repair & Recovery
**Skill section:** SECTION Q | **Flags:** `-err_detect`, `-fflags`, `-vsync`

Real-world files get corrupted — interrupted recordings, SD card failures, network drops, bad downloads. This feature covers recovering usable video from files that media players and editors outright reject.

Also covers VFR → CFR conversion: variable frame rate footage (the default from phones and screen recorders like OBS) causes audio sync drift in editing software like Premiere Pro, DaVinci Resolve, and Final Cut. Converting to constant frame rate fixes this at the source.

Capabilities:
- Ignore errors and discard corrupt packets to extract a playable file
- Increase probe/analyze buffers for files with damaged headers
- Force container format detection when the header is unreadable
- VFR → CFR at any target frame rate
- Fix audio/video sync offsets
- Rebuild broken MP4 index (moov atom)

**Supports:** macOS, Linux, Windows

---

### 6. Metadata Management
**Skill section:** SECTION R | **Flags:** `-metadata`, `-map_metadata`, `-map_chapters`

Media files carry more than just audio and video — they contain metadata that affects how files are displayed, organized, and distributed.

**Why it matters:**
- **Chapter markers** make long videos (podcasts, tutorials, lectures, YouTube) navigable. YouTube and most media players display a chapter timeline when chapters are embedded.
- **Cover art** is essential for podcast MP3s and audiobooks. Without it, every podcast app and media player shows a blank grey square.
- **Language tags** are required for international distribution — they tell players which audio/subtitle track to select by default.
- **Stripping metadata** is a privacy necessity. Photos and videos from phones often carry GPS coordinates, device serial numbers, and user account info that should be removed before publishing online.

**Supports:** macOS, Linux, Windows

---

### 7. Testing & Debugging
**Skill section:** SECTION S | **Sources/tools:** `smptebars`, `sine`, `ffprobe -show_packets`

**SMPTE color bars + tone:** The broadcast-standard way to verify your encoding pipeline end-to-end. Generate a known-good reference signal, encode it, and check the output. If the bars are correct and the 1kHz tone is at the right level, your pipeline is working. Used before live broadcasts and when delivering files to clients.

**Packet-level analysis:** When a specific file causes playback issues or crashes an editor, `ffprobe -show_packets` lets you inspect every individual packet — find exactly where corruption starts, where keyframes are missing, or where a PTS gap causes A/V desync.

**Encoding benchmark:** Before committing to a long batch encode, benchmark your hardware against different presets and codecs. Understand the tradeoff between encode speed, output file size, and quality on your specific machine.

**Bit stream inspection:** Dump raw codec headers and NAL units to diagnose encoder-level issues that don't surface at the container level.

**Supports:** macOS, Linux, Windows (Python 3 required for benchmark script)
