# Highlight — releases

A Chrome extension that highlights the important sentences on long pages and emails. Every sentence
is scored by an AI model and tinted green in proportion to how much it matters.

This repository only hosts the built extension. Download it from the
[Releases page](../../releases/latest).

## Install

1. Download `highlight-ext-<version>.zip` from the [latest release](../../releases/latest) and unzip it.
2. Open `chrome://extensions` and turn on **Developer mode** (top right).
3. Click **Load unpacked** and choose the unzipped folder.
4. Pin the extension, open a long article, click the icon, and press **Highlight this page**
   (or press **Alt+Shift+H**).

Chrome may occasionally remind you that a developer-mode extension is installed; that is expected for
extensions loaded this way. To update, download the new zip and click the reload icon on the
extension's card at `chrome://extensions`.

## Pick a model

You need one of these:

- **Local — Tev1 or Nimble through [Ollama](https://ollama.com).** Install Ollama, then run
  `ollama pull tev1` (faster) and/or `ollama pull nimble` (more accurate). Everything stays on your
  machine.
- **Jev — [TypeSafe](https://typesafe.ai) cloud.** Choose *Jev* in the popup and paste your own
  TypeSafe API key. The key is saved in this browser only, so you enter it once.

## Modes

- **Key points** — what matters most on the page.
- **Main idea per paragraph** — one winning sentence per paragraph.
- **Custom** — describe what to highlight, such as "sentences with statistics".

After a run, a small panel on the page has a slider to raise or lower the cut-off, a Stop button, and
a button to clear the highlights. If a run stops, hits its limit or fails, the panel offers
**Run again**.

**PDFs.** Chrome's built-in PDF viewer can't be scripted by any extension, so on a PDF the extension
opens it in its own viewer and highlights it there. Click **Highlight this page** (or press
**Alt+Shift+H**) on a PDF tab. The first time for a site, Chrome asks whether to let the extension
access that site, so it can download the PDF; click **Allow**. The viewer's **Open original** link takes
you back. Running headers, page numbers, footnotes, tables, author lists and references are left out,
and a sentence that crosses a page break is read as one. For PubMed Central and recent arXiv PDFs the
popup also offers the paper's web version.

**Pages that load as you scroll** (a PDF open in a web viewer, infinite feeds): a run keeps following
the page and scores new text as it appears. Text the page draws again is repainted from scores the
extension already has, with no new requests. With Jev, following only happens on sites where you chose
**Always on this site**.

## Privacy

- The extension has no server and collects no data. It has no analytics.
- It runs only on the page you click it on (it uses Chrome's `activeTab` permission). The one addition
  is PDFs: to show a PDF in its own viewer the extension must download it, so Chrome asks you to allow
  access to that one site (an optional permission, requested per site, only when you click Highlight on
  a PDF there). The PDF is fetched with your cookies from that site, kept briefly in this browser, and
  never uploaded anywhere except as sentences to the model you chose. With Jev, the "Always on this
  site" choice for a PDF applies to the site it came from, not to all PDFs.
- With a local model, page text goes only to Ollama on `localhost:11434`.
- With Jev, the page's text is sent to `api.typesafe.ai`, one request per sentence, and counts against
  your TypeSafe quota. The extension asks before the first cloud run on each site.
- Your API key is stored unencrypted in this browser profile's extension storage and is read only by
  the extension's background worker. Use a key you can revoke.
- Scores are cached for the browser session in memory-backed extension storage; page text is never
  stored.

## Limits

Up to 600 sentences per run, counting text that appears while the run follows the page; **Run again**
continues from what's on screen. A web PDF viewer only exposes the text of pages it has drawn, so that
is all that can be highlighted; the extension's own PDF viewer draws pages as you scroll and the run
follows you. That viewer has no zoom, search, print or download (use **Open original**), doesn't open
local `file://` PDFs, and can't read scanned (image-only) or password-protected PDFs. Paragraphs in a
PDF are inferred from its layout, so unusual layouts can come out wrong. Text inside iframes is not read.
