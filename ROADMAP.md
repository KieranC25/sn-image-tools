# Image Tools roadmap

## Open

### Publish generic Image Tools branding (2026-11-20)

[PR #6](https://github.com/KieranC25/sn-image-tools/pull/6) merged as `4c69507`. The production page still showed its previous title and a request without parameters to `/api/brandfetch` returned the expected HTTP 400 JSON. Approval review blocked the documented direct upload because it could not confirm that Wrangler would include the existing Brandfetch function.

**Owner.** Project owner to approve the explicit Cloudflare Pages bundle. Release maintainer to verify and publish from the current reviewed `main` commit once approved.

**Prepared.** A local Pages preview served the bundle containing `index.html`, `_headers`, the compiled `_worker.js`, and `_routes.json` routing `/api/brandfetch` to that worker. The local route returned the expected response for a missing API key. Chromium resized a 200 × 100 PNG via the thumbnail preset and downloaded a valid 150 × 75 PNG.

**Done when.** Production serves the `Image Tools` title, heading and share metadata, a browser completes the thumbnail resize and download, and `/api/brandfetch` returns the same HTTP 400 JSON for a request without search parameters.
