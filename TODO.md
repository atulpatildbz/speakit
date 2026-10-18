# Todo

## Kokoro in the browser: probed 2026-10-18

Tested `kokoro-js@1.2.1` + `onnx-community/Kokoro-82M-v1.0-ONNX` in Chrome on this Mac (probe pages were throwaway).

- Loads and speaks fully in the page, no server.
- Speed, warm: WebGPU fp32 about 0.24x real time (4x faster than playback; 325 MB model, ~25 s first load).
  WASM q8 (92 MB) about 1.5-1.7x real time, too slow to stream (test server had no COOP/COEP headers, so single-threaded; not retried).
- **No word timings:** the ONNX export outputs only `waveform`, not per-token durations. Other published exports have the same graph.
  Sentence-level timing is exact (waveform length); word-level has to come from one of:
  1. estimate within each sentence from phoneme counts plus pauses at punctuation (cheap, approximate),
  2. re-export the model to also output the duration predictor's `pred_dur` (the PyTorch model computes it; exact, but a Python export job),
  3. forced-align each sentence with a small Whisper model in the page (extra model, slower).
- Not tested: Android phone (WebGPU support and speed), voice quality next to Zoe (Premium).

Other options considered: VoiceStudio (desktop app + Python server, phone is a companion to it, no word timings documented, AGPL): only as an optional remote backend.

Not an option: reusing the Speechify login. It's their private API; the supported route is the paid Speechify API with its own key.
