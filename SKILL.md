# VietVoice — Vietnamese TTS / Voice Clone / Podcast (≤10 hosts)

Control VietVoice (fork of VieNeu-TTS) from OpenClaw or any app via CLI or REST API.
Use this skill when the user wants Vietnamese text-to-speech, voice cloning from a
sample, or a multi-host podcast/dialogue (up to 10 speakers).

## When to use
- "đọc đoạn này thành giọng nói", "tạo audio tiếng Việt", "TTS"
- "clone giọng này", "nhân bản giọng từ file mẫu"
- "làm podcast", "hội thoại nhiều người", "10 host"
- Any request to drive VietVoice programmatically.

## Two ways to call

### A) REST API (preferred when server is running — fast, GPU on the 3090)
Base URL default: `http://localhost:8000` (set `VIETVOICE_REMOTE` to override).

```bash
# health / discovery
curl -s $BASE/health
curl -s $BASE/v1/voices         # → {"voices":[{"id","label"}...]}
curl -s $BASE/v1/models

# single-voice TTS → wav
curl -s -X POST $BASE/v1/tts -H 'Content-Type: application/json' \
  -d '{"text":"Xin chào","voice":null,"temperature":0.8}' --output out.wav

# clone from a sample (multipart)
curl -s -X POST $BASE/v1/clone \
  -F "text=Câu cần đọc" -F "ref_audio=@sample.wav" --output clone.wav

# podcast (≤10 hosts)
curl -s -X POST $BASE/v1/podcast -H 'Content-Type: application/json' -d '{
  "hosts":[{"name":"Phương","voice":"VOICE_ID_1"},{"name":"Dũng","voice":"VOICE_ID_2"}],
  "script":"Phương: Chào.\nDũng: Yo.",
  "silence_between":0.35
}' --output podcast.wav
```

### B) CLI (works local or remote; good when no server is up)
```bash
vietvoice voices                                  # list voices
vietvoice tts --text "Xin chào" --out hi.wav
vietvoice clone --text "..." --ref sample.wav --out c.wav
vietvoice podcast --script script.txt \
  --hosts "Phương=VOICE_ID_1,Dũng=VOICE_ID_2" --out pod.wav
# Add --remote http://HOST:8000 to use a running server instead of local engine.
# Add --json for machine-readable output (use this from OpenClaw automation).
```

## Recommended flow (from OpenClaw)
1. Check server: `curl -s http://localhost:8000/health` → if ok, use REST; else use CLI local.
2. Always fetch real voice IDs first (`/v1/voices` or `vietvoice voices --json`); do not guess IDs.
3. For podcast, map each speaker name in the script to a real voice ID. Names are matched case-insensitively; unmapped speakers fall back to the first host's voice.
4. Output is a `.wav`. Deliver the file to the user (do not paste binary).

## Notes / limits
- Max 10 hosts per podcast (`VIETVOICE_MAX_HOSTS`, default 10).
- Voice cloning needs model v3+ (default `v3turbo`).
- GPU (cuda) is auto-used if available; CPU fallback is much slower.
- Repo: `repos/VietVoice`. Start server: `python -m apps.api_server` or `run_vietvoice.sh api`.

## Env vars
`VIETVOICE_REMOTE` (CLI default server), `VIETVOICE_MODE` (v3turbo|standard|fast|turbo_gpu),
`VIETVOICE_DEVICE` (cuda|cpu), `VIETVOICE_HOST`, `VIETVOICE_PORT`, `VIETVOICE_MAX_HOSTS`.
