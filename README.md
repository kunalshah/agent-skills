# Agent Skills

This repository contains skills you can install and use with coding agents like Claude Code, VS Code, OpenCode, Cursor etc.

This repository has one skill: `audio-video`.

## Important:

`audio-video` skill in this repository is quite comprehensive with many features. Thus, the size of the SKILL.md is huge.

In your coding agent, ensure `context-mode` MCP is already installed before using this skills. `context-mode` saves your tokens by a huge margin. See: https://github.com/mksglu/context-mode

## audio-video

Comprehensive audio/video processing skill powered by `ffmpeg` and `ffprobe`. Handles transcoding, compression, format conversion, audio extraction, trimming, filtering, and more.

### Prerequisites - `ffmpeg` and `jq`

#### MacOS

```bash
brew install ffmpeg-full jq
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

### audio-video skill features

See [here](./skills/audio-video/assets/features.md).
