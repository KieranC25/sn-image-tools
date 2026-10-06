# ServiceNow Image Tools: agent guidance

This repository is a single-page image preparation tool for ServiceNow Knowledge Base articles. `index.html` contains the interface and image processing; `functions/api/brandfetch.js` proxies Brandfetch requests. Keep the vanilla HTML/CSS/JavaScript structure and the no-build workflow. The README describes the features and KB0696767 presets; the project wiki article records decisions and lessons.

## Development and deployment

- Open `index.html` for local image resizing. Run `npx wrangler pages dev .` when testing the Pages Function and brand search.
- Cloudflare Pages uses a manual direct upload to project `servicenow-image-resizer`; pushing Git does not deploy it. Follow prompt 133 for the current release procedure and verify the deployed commit and changed behaviour.
- Never deploy the repository root. Stage only `index.html` and `_headers` as the Pages output; Wrangler picks up `functions/` from the working directory. This keeps repo instructions and Git metadata out of the public site.

## Behaviour to preserve

- Treat presets as maximum-width/maximum-height bounding boxes. Preserve aspect ratio; only pad to exact dimensions when the user selects that option. Keep the same preset behaviour in both columns.
- Keep KB inputs to JPG, PNG and GIF, sanitise output filenames to letters, numbers, hyphens and underscores, and warn before upscaling. The global paste handler must ignore text inputs and textareas.
- Use Pica for final resampling. Canvas is used to prepare and export images, not as the final resize algorithm. A GIF input is exported as PNG because Canvas cannot export GIF.
- Read current dimensions when a debounced resize executes, and rerun if inputs change during an active resize. Test wide and tall images and rapid dimension changes after image-processing edits.
- Keep the server-side `BRANDFETCH_API_KEY` in the Pages Function. A user-provided key may be sent through `X-User-Api-Key`; do not embed the server key in browser code.
- Validate proxied image URLs with `new URL()` and an exact or subdomain hostname match against the Brandfetch allowlist. Never use a substring match or allow arbitrary URLs.
- Put untrusted text into the DOM with `textContent`, not `innerHTML`. Keep SRI on the Pica CDN script; regenerate its hash when changing the script URL or version.
- Keep CORS headers on success and error responses, handle OPTIONS preflight, and preserve upstream error status and message. When changing the proxy, check both the allowed and rejected URL paths.
- Keep `_headers` compatible with Pica's WASM loading. Verify the CSP in a browser after changing it.

## Documentation

Update this file when repo-specific commands or lasting rules change. Record project decisions and incident lessons in `wiki:projects/sn-image-tools` under **Decisions and lessons**. The existing `FORKIERAN.md` is a read-only archive.
