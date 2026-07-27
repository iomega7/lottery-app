# Lottery OCR — Thai Lottery Ticket Recorder

A small static web app for recording Thai Government Lottery ticket numbers (by photo or by hand) and printing them onto A4 paper as ticket-sized cover sheets for checking against results later.

**Live app:** https://iomega7.github.io/lottery-app/ (password-protected — see [Security / password gate](#security--password-gate))

## What it does

- Reads a ticket's QR / Data Matrix code from a photo and auto-fills ชุดที่ (set number) and เลขรางวัล (6-digit number), or lets you type them in by hand.
- Groups entries into "ใบปะหน้า" (cover sheets) of up to 20 numbers each, up to 6 sheets total.
- Exports the selected sheets as a printable PDF, each sheet sized 120×60mm (12×6cm) to match a real ticket, 6 sheets per A4 landscape page.

## How to use

1. **Add entries** — either:
   - Tap "📷 ถ่ายรูป / เลือกรูปสลาก" to take a photo of a ticket or pick one from your photo library; the app decodes the QR/Data Matrix code automatically, or
   - Type ชุดที่ and เลขรางวัล directly and tap "+ เพิ่มเข้าใบปะหน้า".
2. Switch between sheets using the tabs in section 2 (max 6 sheets, 20 entries each).
3. In section 3, check the sheets you want to print (or tap "เลือกทั้งหมด" to select all), then tap "ดาวน์โหลดเป็นไฟล์ PDF สำหรับพิมพ์บน A4".
4. Print the PDF on A4 paper — each sheet comes out at real ticket size for easy checking/filing.

## Files

- `index.html` — the whole app: markup, styles, and script in one file. Edit it directly;
  GitHub Pages serves it at the repo root, so the name is fixed.

There used to be a second copy, `Lottery APP.html`, kept byte-identical by hand. Nothing
enforced that, so the served page could silently drift from the file being edited. It was
removed on 2026-07-27; `index.html` is now the only source.

## Tech notes

- Fully static and client-side only — no backend, no server, no API keys, no per-user quota to worry about.
- QR / Data Matrix decoding runs entirely in the browser using [jsQR](https://github.com/cozmo/jsQR), [zxing-wasm](https://github.com/Sec-ant/zxing-wasm), and the native `BarcodeDetector` API where available (all loaded from public CDNs).
- PDF export via [jsPDF](https://github.com/parallax/jsPDF).
- All saved entries live in the browser's `localStorage` only — nothing is uploaded anywhere.

## Security / password gate

- A password prompt blocks the app until the correct passphrase is entered. It's defined as the `APP_PASSWORD` constant near the top of the `<script>` block in `index.html`.
- **This is a casual deterrent, not real security.** Since the site is a static page with no server, anyone opening browser dev tools can read the passphrase straight from the page source or bypass the check entirely. Don't rely on it to protect anything sensitive, and don't reuse a real password here.
- Once unlocked, the state is remembered per browser/device via `localStorage`, so you won't be asked again on that same browser — until: you clear that site's browsing data, you switch browsers or devices, you use Private/Incognito mode, you use "Add to Home Screen" for the first time (separate storage context), or (rarely) Safari's tracking-prevention cleanup clears storage after ~7 days without a visit.

## Known limitations

- **No live camera scanner.** An earlier version had a live in-page camera scanner; it was removed because continuous low-resolution video frames couldn't reliably read the dense Data Matrix codes used on Thai lottery tickets, and it had no manual shutter control. The photo-picker flow (native camera or photo library → full-resolution image → decode with contrast-stretch/tiling fallback) replaced it and is more reliable.
- **iOS Safari:** the file input intentionally omits `capture="environment"` so tapping the button shows the full native picker (Photo Library / Take Photo / Browse) instead of forcing straight into a restricted camera capture view.
- Repo is public (required for free GitHub Pages) — no personal ticket data is stored in the repo itself; all saved entries stay in each user's own browser storage.

## Changelog

- **2026-07-27** — Removed the duplicate `Lottery APP.html`; `index.html` is now the single source file.
- **2026-07-17** — Added a password gate before the app loads (client-side only, see Security notes above).
- **2026-07-08** — Removed the unreliable live-camera QR scanner; simplified section 1 to a single photo-picker button with clearer instructions.
- **2026-07-08** — Added `index.html` and enabled GitHub Pages (repo made public); live app first published.
- **2026-07-08** — Resized the printed ticket to 120×60mm (12×6cm) landscape to match a real ticket; switched A4 export to landscape at 6 sheets/page (2×3 grid); capped total sheets at 6; added a select-all/deselect-all export button; enlarged and bolded the PDF header/footer captions; removed the "ลป." header caption; removed `capture="environment"` from the file input for correct iOS picker behavior.
- **2026-07-08** — Initial commit of the app.
