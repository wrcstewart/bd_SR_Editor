# bd_SR_Editor — Speech-Recognition Editor Prototype

**Live:** [wrcstewart.github.io/bd_SR_Editor/](https://wrcstewart.github.io/bd_SR_Editor/)

A single-page prototype for **read-and-weave dictation** — a mode where the user speaks a mixture of their own words and passages read aloud from a corpus, and the editor identifies the read portions, corrects Whisper's near-misses against the source, and marks tentative substitutions with `{?…}` so the user can review before accepting.

## Status

Prototype. Under active development in the parent [ButterflyDreaming graph viewer](https://github.com/wrcstewart/butterflydreaming-graphviewer1) repo where `sr_editor.html` sits alongside the rest of BD. This standalone copy is served via GitHub Pages so it can be tested on any device with an HTTPS-capable browser — the `getUserMedia` and `AudioWorklet` APIs both require a secure context.

Two-copy convention: this file mirrors [`sr_editor.html`](https://github.com/wrcstewart/butterflydreaming-graphviewer1/blob/main/sr_editor.html) in the BD repo. Changes made in one should be manually mirrored to the other.

## How it works

Full behavioural rules are in the parent repo:
- [Design intent](https://github.com/wrcstewart/butterflydreaming-graphviewer1/blob/main/BD_SR_Editor_Design_Notes_v0.1.md) — read-and-weave concept, D1–D7 constraints
- [Implementation rules](https://github.com/wrcstewart/butterflydreaming-graphviewer1/blob/main/SR_Editor_Rules_v0.1.md) — signal chain, bias prompt, alignment, substitution rules, em-dash boundaries, boot flow

Short summary:

1. Whisper base.en (~74 MB, one-time browser cache) via [transformers.js](https://huggingface.co/docs/transformers.js)
2. Direct-PCM capture via `AudioWorkletNode` (bypasses Safari's `MediaRecorder` truncation)
3. Optional in-graph dynamics compressor for hot-mic taming
4. Source pane text tokenised and fed as Whisper `prompt_ids` for corpus vocabulary biasing (tricky-word ×N repetition)
5. Local Smith-Waterman alignment with phonetic keys finds the read passage
6. Substitution mode 4 wraps near-misses / missing words / interjections in `{?…}` markers; em-dashes inserted at commentary/quote boundaries
7. "Accept {?…}" button strips markers when the user is happy

## Client-side only

Audio never leaves your device. All processing (recording, Whisper inference, alignment) runs in the browser. No server calls after the model download.

## Licence

[AGPL-3.0](LICENSE) — consistent with the parent ButterflyDreaming project.
