# Agent Skills

This repository contains skills you can install and use with coding agents like Claude Code, VS Code, OpenCode, Cursor etc.

## Important:

All skills in this repository are huge.
In your coding agent, ensure `context-mode` MCP is already installed before using these skills. `context-mode` has excellent performance in saving tokens. See: https://github.com/mksglu/context-mode

## Skills in this repository

### audio-video

Comprehensive audio/video processing skill powered by `ffmpeg` and `ffprobe`. Handles transcoding, compression, format conversion, audio extraction, trimming, filtering, and more.

**Prerequisites:** 

Install `ffmpeg` on your system before using this skill.

- On MacOS:

    ```bash
    brew install ffmpeg-full
    ```

    OR

    ```bash
    brew install ffmpeg
    ```

- Linux and Windows: Follow the [link](https://ffmpeg.org/download.html).

## Installing in Claude Code

Register this repository as a Claude Code Plugin marketplace:

```
/plugin marketplace add kunalshah/agent-skills
```

Then install the audio-video skill:

```
/plugin install audio-video@kunalshah-agent-skills
```

## Installing with skills CLI

with skill CLI, you can install the skill to several coding agents like Google Antigravity, OpenCode etc.

```bash
npx skills add https://github.com/kunalshah/agent-skills
```

OR

```bash
npx skills add https://github.com/kunalshah/agent-skills/tree/main/skills/audio-video
```

## Using the audio-video skill

After installing, you can use the skill by mentioning it. For example: `"Use the audio-video skill to compress this video for web."`
