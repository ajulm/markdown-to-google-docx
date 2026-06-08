# Project: Markdown → Google Docs

Single-file browser tool. No build step, no dependencies to install — all
libs load from CDN. The whole app is `markdown-to-gdocs.html`.

## Constraints that drive the design

Google Docs paste importer:
- **Strips** CSS classes and `<style>` rules.
- **Respects** inline `style="…"`, `<table border>`, inline `font-family`.
- Paints a paragraph `background` only behind text runs (character highlight),
  NOT full block width — so styled `<pre>` lands as broken gray bands. Use a
  **table cell** for any full-width background (e.g. code blocks).
- Infers heading level from font-size contrast — set explicit point sizes.

Any change to copy output must be tested by an actual paste into Google Docs;
the preview pane is not a faithful proxy for what Docs renders.

## Key functions

- `buildDocsHtml()` — clones the preview and inlines all Docs-safe styles.
- `serializeCode()` — code subtree → Docs-safe HTML (inline hljs colors,
  spaces → nbsp, `\n` → `<br>`).
- `copyAsRichText()` — writes `text/html` + `text/plain` to the clipboard.
