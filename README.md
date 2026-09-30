# Unwatermark

**Remove static watermarks from video — entirely in your browser.**

Unwatermark is a free, open-source web tool that removes logos, timestamps, and text overlays from video. It runs 100% client-side: your files never leave your device.

[**Try it live →**](https://unwatermark.eu.org)

---

## Why this exists

Every other watermark remover either charges you, caps your clip at ten seconds, or uploads your file to a server you've never heard of. This one does none of that. Pick a video, draw a box, download the result. No accounts, no credits, no length limits.

## Features

- **100% offline** — no uploads, no servers, no telemetry. Turn off Wi-Fi after the page loads and it still works.
- **No artificial limits** — no ten-second caps, no resolution ceilings, no daily quotas, no watermarks added to your output.
- **Multiscale inpainting** — the repair engine solves Laplace's equation on an image pyramid, reconstructing the hole from its surrounding pixels. This is the same family of technique as Poisson inpainting in professional retouching tools.
- **Multiple regions** — mark as many watermarks as you need in a single clip.
- **Adjustable quality** — Fast, Balanced, and Best modes trade speed for fidelity.
- **Live preview** — see the repair before you commit to a full export.
- **Open source** — MIT licensed and about a thousand lines. Audit the inpainting code yourself, fork it, or self-host it.

## How it works

1. **You mark the watermark.** Drag a rectangle over the logo, timestamp, or text. Regions are stored as fractions of the frame, so they scale correctly at any resolution.
2. **We reconstruct the hole from its edges.** The marked pixels become a hole in the image. A multiscale diffusion solver fills it by propagating colour inward from the surrounding border, using a coarse-to-fine pyramid so low frequencies converge in a handful of passes.
3. **The patch is blended back.** The filled area is composited with a smooth falloff at the edges, so there's no visible seam.
4. **Frames are re-encoded locally.** Every processed frame is drawn to a canvas and captured by the browser's own encoder. Nothing is transmitted.

## Limitations (honest ones)

- **Best on smooth backgrounds** — sky, walls, gradients, shallow depth-of-field. On busy or high-detail backgrounds you'll see some softening or smearing, because the algorithm is reconstructing detail that isn't there.
- **Static watermarks only** — if the watermark drifts across the frame, you'd need keyframing, which isn't supported yet.
- **Real-time export** — browser encoding runs against the wall clock, so a two-minute video takes about two minutes to export. Faster-than-realtime export via the WebCodecs API is on the roadmap.
- **Output format depends on your browser** — usually MP4 (H.264) in Chrome and Safari, WebM (VP9) in Firefox.

## Browser support

| Browser | Status |
| :--- | :--- |
| Chrome / Edge | ✅ Full support |
| Firefox | ✅ Full support |
| Safari 16+ | ✅ Full support |
| Mobile browsers | ⚠️ Works, but performance depends on device |

## Getting started locally

```bash
# Clone the repository
git clone https://github.com/OogaBoogaPooga/Unwatermark.git

# Open the file in your browser
cd Unwatermark
open index.html   # or just double-click it
