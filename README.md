# Agent Skills

This repository contains skills you can install and use with coding agents like Claude Code, VS Code, OpenCode, Cursor etc.

## Important:

All skills in this repository are huge.
In your coding agent, ensure `context-mode` MCP is already installed before using these skills. `context-mode` has excellent performance in saving tokens. See: https://github.com/mksglu/context-mode

These repository has one skill named `audio-video`.

## audio-video

Comprehensive audio/video processing skill powered by `ffmpeg` and `ffprobe`. Handles transcoding, compression, format conversion, audio extraction, trimming, filtering, and more.

### Prerequisites - `ffmpeg` and `jq`

#### MacOS

```bash
brew install ffmpeg-full jq
```

OR

```bash
brew install ffmpeg jq
```

#### Linux

- ffmpeg
  - Follow the [link](https://ffmpeg.org/download.html)

- jq
  - Follow the [link](https://jqlang.org/download/)

#### Windows

- ffmpeg
  - Follow the [link](https://ffmpeg.org/download.html)

- jq

  ```bash
  winget install jqlang.jq
  ```
  OR
  ```bash
  choco install jq
  ```
  OR
  ```bash
  scoop install jq
  ```

### Installing skill in Claude Code

Register this repository as a Claude Code Plugin marketplace:

```
/plugin marketplace add kunalshah/agent-skills
```

Then install the skill:

```
/plugin install audio-video@kunalshah-agent-skills
```

### Installing with skills CLI

with skill CLI, you can install the skill to several coding agents (e.g. Google Antigravity, OpenCode, Gemini CLI, Claude Code etc) at once.

```bash
npx skills add https://github.com/kunalshah/agent-skills --skill audio-video
```

### Using the audio-video skill

After installing, you can use the skill by mentioning it.

For example: `Use the audio-video skill to compress this video for web.`
