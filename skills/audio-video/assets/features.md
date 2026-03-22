# Audio-Video Skill — Feature Overview

A complete reference of every capability in this skill, organized by section.

---

## SECTION A — Format Conversion & Transcoding

Convert and transcode between any video format or codec. Handles the full range from modern codecs (AV1, HEVC) to universal compatibility (H.264), with hardware acceleration on all major platforms.

**Capabilities:**
- Transcode to H.264, H.265/HEVC, AV1, VP9/WebM
- Hardware-accelerated encoding: VideoToolbox (macOS), NVENC (NVIDIA), QSV (Intel)
- Container remux (MP4 ↔ MKV ↔ MOV) without re-encoding via stream copy
- Convert PNG/JPEG image sequences into video
- Extract video frames as image sequences (all frames, at specific fps, keyframes only)

**Key tools:** `libx264`, `libx265`, `libaom-av1`, `libvpx-vp9`, `h264_videotoolbox`, `h264_nvenc`, `-c copy`, `-crf`, `-preset`, `-movflags +faststart`

---

## SECTION B — Audio Processing

Extract, convert, filter, and analyze audio from any media file. Covers everything from simple format conversion to EBU R128 broadcast-standard loudness normalization.

**Capabilities:**
- Extract audio tracks as AAC, MP3, FLAC, WAV, or Opus
- Convert between audio formats; downmix multi-channel to stereo; change sample rate
- EBU R128 loudness normalization (two-pass, broadcast standard)
- Audio filters: volume adjustment, high-pass, low-pass, noise reduction, dynamic range compression, fade in/out, stereo↔mono
- Advanced: remove silence, change speed with pitch preservation (0.5–2.0×), pitch shift without tempo change, generate waveform stats, generate spectrogram PNG

**Key tools:** `libmp3lame`, `loudnorm`, `highpass`, `lowpass`, `anlmdn`, `acompressor`, `afade`, `silenceremove`, `atempo`, `showspectrumpic`

---

## SECTION C — Video Editing

The full editing toolkit: cut, join, resize, rotate, crop, overlay, and color grade — all without leaving FFmpeg.

**Capabilities:**
- **Trim & cut:** Fast keyframe-accurate trim (stream copy) or frame-accurate trim (re-encode); remove specific time ranges
- **Concatenation:** Join same-codec files without re-encoding; join different codecs/resolutions with re-encode
- **Scaling:** Resize to exact dimensions or platform targets (4K, 1080p, 720p, 480p); preserve aspect ratio with padding
- **Frame rate:** Change fps; smooth slow-motion via frame interpolation (`minterpolate`)
- **Rotation & flipping:** 90°/180° rotation, horizontal/vertical flip, auto-rotate from phone metadata
- **Cropping:** Crop to coordinates, square center crop, auto-detect and remove black bars (`cropdetect`)
- **Overlays & watermarks:** Image watermark, text watermark with custom font/color/alpha, timed animated overlays, picture-in-picture (PiP)
- **Color grading:** Brightness, contrast, saturation, gamma; LUT (`.cube`) color grading; curves (S-curve); hue/saturation shift

**Key tools:** `fps`, `minterpolate`, `transpose`, `vflip`, `hflip`, `crop`, `cropdetect`, `overlay`, `drawtext`, `eq`, `lut3d`, `curves`, `hue`, `scale`, `filter_complex`

---

## SECTION D — Subtitles & Captions

Add, remove, convert, and embed subtitle tracks in any format.

**Capabilities:**
- Burn subtitles permanently into video (hard subtitles, not removable)
- Add soft subtitle tracks that viewers can toggle on/off
- Extract subtitle tracks from MKV, MP4, and other containers
- Convert between SRT and ASS/SSA formats
- Tag subtitle tracks with language metadata

**Key tools:** `subtitles` filter, `mov_text`, `-c:s`, `-metadata:s:s:0 language=`

---

## SECTION E — Thumbnails & Screenshots

Extract frames and generate thumbnails for any use — preview images, video players, web galleries.

**Capabilities:**
- Extract a single frame at any timestamp
- Generate high-quality PNG thumbnails
- Auto-select the "best" frame (highest scene complexity)
- Sprite sheets / contact sheets: multiple thumbnails tiled into a grid image
- Extract one thumbnail every N seconds (for timeline preview strips)

**Key tools:** `-ss`, `-vframes 1`, `thumbnail` filter, `scale`, `tile`, `fps`

---

## SECTION F — Streaming & Adaptive Bitrate

Produce industry-standard streaming outputs — HLS, DASH, and live RTMP — from any source.

**Capabilities:**
- **HLS:** Generate segments + `.m3u8` playlist; multi-bitrate ABR ladder (1080p/720p/480p) with master playlist
- **DASH:** Generate `.mpd` manifest with segmented output for MPEG-DASH delivery
- **RTMP live streaming:** Stream to Twitch, YouTube, or any RTMP endpoint; screen capture to RTMP

**Key tools:** `-hls_time`, `-hls_playlist_type`, `-hls_segment_filename`, `-var_stream_map`, `-master_pl_name`, `-f dash`, `-seg_duration`, `-f flv`, `-re`, `-g`

---

## SECTION G — Screen & Webcam Capture

Record your screen and webcam natively on every platform — no third-party tools needed.

**Capabilities:**
- **Screen recording:** macOS (AVFoundation), Linux (x11grab / PipeWire), Windows (gdigrab) — with audio capture
- **Webcam capture:** macOS (AVFoundation), Linux (v4l2), Windows (dshow) — custom resolution and framerate
- List available input devices on each platform

**Key tools:** `-f avfoundation`, `-f x11grab`, `-f pipewire`, `-f gdigrab`, `-f v4l2`, `-f dshow`

---

## SECTION H — GIF & Animated Images

Convert video to high-quality animated GIFs and WebP. FFmpeg's two-pass palette approach produces GIFs that are far smaller and sharper than naive conversion.

**Capabilities:**
- Video → GIF with two-pass palette generation (palettegen → paletteuse) for optimal color accuracy and small file size
- GIF → video conversion (MP4/WebM)
- WebP animation with custom fps and resolution

**Key tools:** `palettegen`, `paletteuse`, `dither` modes, `-loop 0`, `-fps_mode passthrough`

---

## SECTION I — Batch Processing & Scripting

Process entire directories of files automatically. Supports both shell scripting (macOS/Linux) and PowerShell (Windows), with parallel processing and progress reporting.

**Capabilities:**
- Batch convert all files in a directory (bash/zsh loop, PowerShell loop)
- Parallel batch processing with GNU parallel (4+ jobs simultaneously)
- Machine-readable progress output with duration-aware percentage display
- Two-pass encoding for precise target bitrate control

**Key tools:** `-progress pipe:1`, `-nostats`, ffprobe duration extraction, `-pass 1`/`-pass 2`, GNU `parallel`

---

## SECTION J — Advanced Filtergraphs

Complex multi-input filter chains that go beyond single-stream processing.

**Capabilities:**
- Side-by-side video comparison (`hstack`)
- Vertical stacking (`vstack`)
- 2×2 grid layout from 4 inputs
- Ken Burns zoom/pan effect (`zoompan`)
- Vignette effect
- Full-frame and selective region blur (`boxblur`, `overlay`)
- Audio/video sync repair via stream offset (`-itsoffset`)

**Key tools:** `hstack`, `vstack`, `zoompan`, `vignette`, `boxblur`, `overlay`, `-filter_complex`, `-itsoffset`

---

## SECTION K — Quality Analysis & Verification

Measure perceptual and mathematical video quality, and validate that output files are correct before delivery.

**Capabilities:**
- **PSNR** (Peak Signal-to-Noise Ratio) — mathematical fidelity metric
- **SSIM** (Structural Similarity Index) — perceptual similarity metric
- **VMAF** (Netflix perceptual quality model) — industry standard for encode quality comparison
- Output validation: verify duration, size, bitrate, stream count, codec, frame rate
- Detect corrupt packets in any file

**Key tools:** `psnr`, `ssim`, `libvmaf`, `-f null`, ffprobe stream inspection

---

## SECTION L — Platform-Specific Presets

Ready-to-use ffmpeg commands tuned to the exact specs each platform requires. No guessing at bitrates or codec settings.

**Platforms covered (20+):**

| Category | Platforms |
|----------|-----------|
| Video platforms | YouTube 1080p, YouTube Shorts, Vimeo, TikTok, Instagram Reels, Instagram Feed (1:1 / 4:5), Twitter/X, Facebook |
| Live streaming | Twitch (RTMP 6000kbps), OBS Virtual Camera |
| Mobile | iOS (H.264 max compat), Android (H.264 / VP9) |
| Post-production | Apple ProRes 422 HQ, Apple ProRes 4444 (alpha), Avid DNxHD 185, Lossless Archival (FFV1+FLAC/MKV) |
| Web/HTML5 | Web MP4 (H.264 faststart), Web WebM (VP9), Optimized GIF |
| Messaging | Discord (≤8MB), WhatsApp (≤16MB / ≤3min) |
| Audio | Podcast MP3 (stereo 192kbps), Audiobook (mono, loudness-normalized) |

---

## SECTION M — Video Stabilization

Stabilize shaky footage from handheld cameras, drones, or action cameras. Uses a two-pass approach: pass 1 analyzes motion, pass 2 applies correction. Common use cases: vloggers, event videographers, drone footage, sports and action cameras (GoPro, DJI).

**Capabilities:**
- Two-pass stabilization: `vidstabdetect` (motion analysis) → `vidstabtransform` (correction)
- Tunable shakiness level (1–10) and smoothing radius
- Optional zoom to hide black border artifacts from stabilization
- `unsharp` pass to restore sharpness lost during warp correction

**Key tools:** `vidstabdetect`, `vidstabtransform`, `unsharp`

**Supports:** macOS, Linux, Windows (requires ffmpeg built with `--enable-libvidstab`)

---

## SECTION N — 360° / VR Video

Handle 360° footage from cameras like GoPro Max, Insta360, and Ricoh Theta. Without proper projection conversion and spherical metadata injection, platforms like YouTube VR and Facebook 360 won't recognize the file as 360° — it plays as a flat, distorted video.

**Capabilities:**
- Equirectangular → cubemap (3×2) conversion
- Cubemap → equirectangular conversion
- Extract a flat "normal" view from a 360° source (reframe with custom yaw/pitch/FOV)
- Inject spherical metadata for YouTube VR / Facebook 360 compatibility

**Key tools:** `v360` filter (`equirect`, `c3x2`, `flat`)

**Supports:** macOS, Linux, Windows (v360 is cross-platform)

---

## SECTION O — HDR / Color Science

Modern phones and cameras shoot HDR by default (10-bit, bt2020, smpte2084). Without proper tone mapping, HDR footage looks washed out or blown out on SDR displays and platforms.

**Capabilities:**
- HDR10 → SDR tone mapping using `zscale` + `tonemap` (hable, reinhard, mobius, clip algorithms)
- Colorspace conversion for bt2020-tagged files that aren't true HDR (`colorspace` filter)
- Encode proper HDR10 output with display metadata (master display, MaxCLL, MaxFALL) using libx265
- Tag existing files with correct color space metadata without re-encoding

**Key tools:** `zscale`, `tonemap`, `colorspace`, `format=gbrpf32le`, libx265 `-x265-params`

**Supports:** macOS, Linux, Windows (requires ffmpeg with `--enable-libzimg`)

---

## SECTION P — Advanced Streaming

Three distinct streaming capabilities beyond basic RTMP: low-latency SRT, multi-destination restreaming, and rolling DVR recording.

**Capabilities:**
- **SRT streaming:** Low-latency, packet-loss-resilient streaming over unstable connections (cellular, satellite). Supports caller and listener modes, configurable latency.
- **Multi-endpoint restreaming (tee muxer):** Stream to YouTube, Twitch, and Facebook simultaneously from one ffmpeg process — no external restreaming service needed.
- **Rolling window DVR:** Continuous loop recording in segments, keeping only the last N minutes. Ideal for security cameras and broadcast monitoring where storage is limited.

**Key tools:** `srt://` protocol, `-f tee`, `-f segment`, `-segment_time`, `-segment_wrap`, `-reset_timestamps`

**Supports:** macOS, Linux, Windows (SRT requires ffmpeg with `--enable-libsrt`, included in full builds)

---

## SECTION Q — Repair & Recovery

Real-world files get corrupted — interrupted recordings, SD card failures, bad downloads. This section covers extracting usable video from files that media players and editors reject, and fixing the most common structural problems.

**Capabilities:**
- Recover from corrupt/truncated files by ignoring errors and discarding bad packets
- Force container format detection when the file header is unreadable
- Increase probe/analyze buffer sizes for files with damaged or missing moov atoms
- **VFR → CFR conversion:** Fix audio sync drift caused by variable frame rate footage (common from phones and screen recorders like OBS)
- Fix audio/video sync offset (advance or delay audio by milliseconds)
- Rebuild broken MP4 index (moov atom repositioning)

**Key tools:** `-err_detect ignore_err`, `-fflags +discardcorrupt`, `-analyzeduration 100M`, `-vsync cfr`, `-itsoffset`, `-movflags +faststart`

**Supports:** macOS, Linux, Windows

---

## SECTION R — Metadata Management

Media files carry metadata that affects how they are displayed, organized, and distributed. This section covers reading, writing, and stripping metadata at the file, stream, and chapter level.

**Capabilities:**
- Embed key/value tags (title, artist, year, comment, album, track number)
- Strip all metadata for privacy (removes GPS coordinates, device info, user data)
- Add chapter markers with timestamps and titles (navigable in YouTube, VLC, media players)
- Embed cover art into MP3, M4A, and AAC audio files
- Tag audio/subtitle streams with language codes for international distribution
- Remove specific streams (e.g. unwanted audio tracks)
- Read and display all metadata from any file

**Key tools:** `-metadata`, `-map_metadata -1`, `-map_chapters`, `-disposition:v:0 attached_pic`, `-metadata:s:a:0 language=`, ffprobe JSON parsing

**Supports:** macOS, Linux, Windows

---

## SECTION S — Testing & Debugging

Verify encoding pipelines, diagnose file-level problems, and benchmark your hardware before committing to long encodes.

**Capabilities:**
- **SMPTE color bars + tone:** Generate the broadcast-standard calibration signal (smptebars / smptehdbars) with a 1kHz sine tone to verify the full encoding pipeline end-to-end
- **Packet-level analysis:** Inspect every packet in a file — total count, keyframe positions, PTS/DTS gaps, and corruption detection — using ffprobe + Python
- **Encoding benchmark:** Compare H.264 presets (ultrafast → slow) on your hardware to understand the speed/size/quality tradeoff before a batch encode
- **Bit stream inspection:** Dump raw codec headers and H.264 NAL units with `trace_headers` to diagnose encoder-level issues
- **Streamability check:** Verify moov atom position (faststart) and file structure with ffprobe

**Key tools:** `smptebars`, `smptehdbars`, `sine` lavfi sources, `ffprobe -show_packets`, `-bsf:v trace_headers`, `-benchmark`, Python 3 benchmark script

**Supports:** macOS, Linux, Windows (Python 3 required for benchmark script)
