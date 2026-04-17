# transcode-scripts - OpenCode Guide

## Purpose
Unified FFmpeg transcoding script using AMD Advanced Media Framework (AMF) hardware acceleration.

## Usage
```bash
./transcode -i <input_file> [-codec av1|h265] <bitrate>
```

**AV1 options:** `1M`, `5M`, `20M`, `40M`, `100M`
**H265 options:** `1M`, `5M`, `20M`, `40M`, `100M`

### Examples
```bash
./transcode -i video.mkv -codec av1 40M    # AV1, 40 Mbit
./transcode -i video.mkv -codec h265 20M   # H265, 20 Mbit
```

### Output
Transcoded files are saved to the script's directory (current working directory).

## Critical Path
1. AMD GPU required with AMF drivers loaded
2. Output files use `{base}_av1_<bitrate>.mp4` format with auto-incrementing suffix for collisions

## AMF Quirks
- **Resolution must be even** or AMF returns `AMF_INVALID_RESOLUTION`
- **AV1:** applies `scale='trunc(iw/2)*2':'trunc(ih/2)*2',format=nv12`
- **H265:** applies `format=nv12` (assumes input already scaled)

## FFmpeg Syntax
- **Bitrate:** `-b <option>` (e.g., `-b 40M` for 40 Mbit)
- Uses system `ffmpeg` command from `$PATH`
