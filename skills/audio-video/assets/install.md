# Installation instructions

## macOS
```bash
# Option 1: Homebrew (recommended)
brew install ffmpeg

# Note: Homebrew removed --with-* option flags. The default formula includes
# the most common codecs. For a build with more codecs (libfdk-aac, etc.):
#   brew tap homebrew-ffmpeg/ffmpeg && brew install homebrew-ffmpeg/ffmpeg/ffmpeg
# See: https://formulae.brew.sh/formula/ffmpeg

# Verify
ffmpeg -version
```

## Windows
```powershell
# Option 1: winget
winget install Gyan.FFmpeg

# Option 2: Chocolatey
choco install ffmpeg

# Option 3: Scoop
scoop install ffmpeg

# Option 4: Manual — download from https://www.gyan.dev/ffmpeg/builds/
# Extract and add to PATH
```

## Linux (Ubuntu/Debian)
```bash
sudo apt update && sudo apt install -y ffmpeg

# Or for latest static build:
wget https://johnvansickle.com/ffmpeg/releases/ffmpeg-release-amd64-static.tar.xz
tar xf ffmpeg-release-amd64-static.tar.xz
FFDIR=$(find . -maxdepth 1 -type d -name 'ffmpeg-*-static' | head -n1)
sudo mv "$FFDIR/ffmpeg" "$FFDIR/ffprobe" /usr/local/bin/
```

## Linux (RHEL/Fedora/CentOS)
```bash
sudo dnf install -y ffmpeg ffmpeg-devel
# Or via RPM Fusion:
sudo dnf install -y https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install -y ffmpeg
```

## Docker (any platform)
```bash
# macOS / Linux
docker run --rm -v "$(pwd):/work" -w /work jrottenberg/ffmpeg:latest \
  -i input.mp4 output.mp4

# Windows PowerShell
docker run --rm -v "${PWD}:/work" -w /work jrottenberg/ffmpeg:latest `
  -i input.mp4 output.mp4
```
