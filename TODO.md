# Todo

## Parked: neural voice in the browser (Kokoro via WASM/WebGPU)

Why: Siri (Voice 1) on macOS 26 is the downloaded voice "aaron"
(`com.apple.siri.tts.voice.en_US.aaron.natural.premium`), but apps can't reach it.
Checked: not in `say -v ?`, AVSpeechSynthesisVoice, NSSpeechSynthesizer or `speechSynthesis`;
`say -v Aaron` silently falls back to the default voice; guessed identifiers don't resolve.
Premium system voices (Zoe) work today.

Idea: add Kokoro (~80 MB, open source, runs locally, free) as a second voice option next to the system voices.

Open questions:
- Can it give word timings? Kokoro predicts per-token durations, but kokoro-js may not expose them. If not, needs forced alignment.
- WebGPU vs WASM speed on this Mac, and first-load model download.
- Chunking for long text so playback starts quickly.

Not an option: reusing the Speechify login. It's their private API; the supported route is the paid Speechify API with its own key.
