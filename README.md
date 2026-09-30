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
- **Real-time export** — the export plays the clip through once while capturing frames. On Chrome and Edge, frames are encoded via the WebCodecs API with exact source timestamps, so the output preserves the source's frame timing. Firefox and Safari fall back to MediaRecorder to keep the audio track.
- **Output format depends on your browser** — usually MP4 (H.264) in Chrome and Safari, WebM (VP9) in Firefox.

## Browser support

| Browser | Status |
| :--- | :--- |
| Chrome / Edge | Full support |
| Firefox | Full support |
| Safari 16+ | Full support |
| Mobile browsers | Works, but performance depends on device |

## Getting started locally

Clone the repository:

    git clone https://github.com/OogaBoogaPooga/Unwatermark.git

Open the file in your browser:

    cd Unwatermark
    open index.html

Or just double-click `index.html`. That's it. No build step, no dependencies, no package manager. It's a single HTML file.

## Deployment

The site is a static HTML file, so it deploys anywhere.

**GitHub Pages**

1. Push your `index.html` to the `main` branch.
2. Go to your repo Settings, then Pages.
3. Set Source to "Deploy from a branch" and pick `main` with the `/ (root)` folder.
4. Save. Your site goes live at `https://<yourgithubname>.github.io/Unwatermark/` within a minute or two.

**Netlify**

1. Go to app.netlify.com/drop and drag your `index.html` onto the page.
2. You'll get a temporary URL instantly.
3. Create a free Netlify account to keep the site permanently.
4. Under Domain management, add your custom domain. Netlify gives you the exact DNS records to paste into Cloudflare.

**Cloudflare Pages**

1. Push your code to GitHub.
2. In the Cloudflare dashboard, go to Workers & Pages.
3. Create an application, choose Pages, then import an existing Git repository.
4. Select the repo. Leave the framework preset as "None" and the build command empty.
5. Save and deploy. Cloudflare gives you a free `.pages.dev` URL.

## Custom domain

This project uses a free `eu.org` domain pointed through Cloudflare:

1. Register at nic.eu.org and request your subdomain.
2. Add the domain to Cloudflare and copy the two nameservers they give you.
3. Paste those nameservers into the EU.org request form and submit.
4. Once approved, point your DNS records at your host (GitHub Pages, Netlify, or Cloudflare Pages).
5. Turn the orange cloud on in Cloudflare for free HTTPS and DDoS protection.

## Built with

- **HTML/CSS** — no framework, no build step
- **Vanilla JavaScript** — the inpainting engine, UI logic, and encoding pipeline
- **Canvas API** — frame processing and preview
- **MediaRecorder API** — local video encoding
- **Web Audio API** — audio routing for the exported file

No npm packages. No bundler. No dependencies.

## License

MIT — see [LICENSE](LICENSE) for details.

## Credits

Made by [shifyee](https://www.youtube.com/@shifyee).

If this tool saved you time, consider subscribing or starring the repo.

---

**Disclaimer:** Stripping a watermark from content you don't own or have a licence for is copyright infringement in most jurisdictions. Common legitimate uses include cleaning up footage you shot yourself, removing a stock preview mark after licensing, or de-cluttering archive material you hold rights to. It's on you to get that right.
