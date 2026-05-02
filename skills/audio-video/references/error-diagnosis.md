# Error Diagnosis

| Error | Likely Cause | Fix |
|-------|-------------|-----|
| `Encoder not found` | Codec not compiled into ffmpeg | Install full ffmpeg build |
| `Invalid option -crf` | Using `-crf` with copy or incompatible codec | Remove `-crf`, use `-b:v` |
| `moov atom not found` | Corrupt/truncated input | Use `-analyzeduration 100M -probesize 100M` |
| `No audio/video stream` | Wrong `-map` or stream missing | Check streams with `ffprobe`, fix `-map` |
| `Output file is empty` | Missing output path or filter error | Check filtergraph syntax, verify paths |
| `Trailing option(s) found` | Flag order wrong | Move input flags before `-i` |
| `Unable to find a suitable output format` | Missing output extension | Add correct extension to output file |
| `Conversion failed!` | See preceding error lines | Run with `-loglevel debug` for full trace |
