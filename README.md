# vietvoice — Vietnamese TTS, voice cloning, and multi-host podcast

> High-quality Vietnamese text-to-speech, voice cloning from audio samples, and multi-host dialogue for up to 10 speakers. GPU-accelerated REST API or CLI.

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blueviolet)](https://github.com/NachaFromMars)

## Overview
vietvoice (fork of VieNeu-TTS) provides Vietnamese TTS, voice cloning from reference audio, and multi-host podcast generation for up to 10 speakers. Available as REST API (default localhost:8000) or CLI. NVIDIA 3090 GPU-accelerated.

## Features
- TTS: `POST /v1/tts {text, voice, temperature}` → wav
- Voice clone: `POST /v1/clone` (text + ref_audio multipart) → wav
- List voices: `GET /v1/voices`
- Multi-host: up to 10 speakers
- Override URL: `VIETVOICE_REMOTE` env

## Usage / Quick Start
```bash
curl -X POST localhost:8000/v1/tts   -H 'Content-Type: application/json'   -d '{"text":"Xin chào","voice":null,"temperature":0.8}' --output out.wav
```

## Trigger Keywords (OpenClaw)
Vietnamese TTS, đọc tiếng Việt, tạo audio tiếng Việt, clone giọng, voice cloning Vietnamese, làm podcast, multi-host audio

---
Part of the [NachaFromMars](https://github.com/NachaFromMars) OpenClaw skill ecosystem.
