# Markdown → Google Docs

A single-file web tool that turns Markdown into rich text you can paste
straight into Google Docs — formatting intact.

Paste Markdown on the left, get a GitHub-style preview on the right, hit
**Copy for Docs**, and paste (`Cmd/Ctrl+V`) into a Google Doc.

## Why

Google Docs strips CSS classes and stylesheets on paste. It *does* respect
inline `style="…"`, the `border` attribute on tables, and inline font
families. This tool builds a Docs-safe HTML clone by walking the rendered
preview and inlining every style, so the paste survives.

## Features

- **GitHub-flavored Markdown** — headings, lists, tables, blockquotes, links,
  rules (via [marked](https://github.com/markedjs/marked)).
- **Syntax-highlighted code** — colors inlined as `<span style="color:…">` so
  they survive the paste (via [highlight.js](https://highlightjs.org/)).
- **Code blocks that don't break** — emitted as a 1×1 shaded table cell, the
  only paste construct Docs fills edge-to-edge. No broken per-line gray bands.
- **Real headings** — explicit point sizes give Docs' importer an unambiguous
  signal, so headings land in the outline and body text stays Normal.
- **Sanitized** — output runs through [DOMPurify](https://github.com/cure53/DOMPurify).
- **No build, no server** — one HTML file. Open it from `file://` or host it
  anywhere static.

## Usage

**Live:** https://ajulm.github.io/markdown-to-google-docx/

Or open `index.html` in a browser. That's it.

1. Type or paste Markdown in the left pane.
2. Check the live preview on the right.
3. Click **Copy for Docs** (or `Cmd/Ctrl+Enter`).
4. Paste into Google Docs.

Click **Example** to load a sample document.

## How it works

`buildDocsHtml()` clones the preview DOM and rewrites it for Docs:

- Tables get `border` attr + inline borders, header shading, zebra rows.
- Code blocks are serialized (hljs colors inlined, spaces → `&nbsp;`,
  newlines → `<br>`) into one shaded table cell.
- Inline code becomes a gray pill; blockquotes get a left rule; links get
  underline + color.
- Headings get explicit point sizes; body text is pinned to 11pt.

The result is written to the clipboard as `text/html` via a `copy`-event
handler, with an async Clipboard API fallback.

## License

MIT
