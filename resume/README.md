# cv.html

**A resume you can read as a person, read as a machine, and edit right on the page.**

One HTML file. No build step, no framework, no backend. Open it in a browser and you have a CV that:

- shows the same content in **four views**: human, machine, brutal, and medium
- lets you **edit every line in place** and saves to your browser
- exports to **JSON, DOCX, plain text, and markdown**
- ships an **llms.txt** summary so language models get a clean, structured version without scraping

Live example: [zack.cx/cv](https://zack.cx/cv)

---

## Why

Most resumes are written for one reader at one moment. This one assumes two kinds of readers: a human skimming, and an AI summarizing on someone's behalf. Both get the same source lines, rendered differently, so neither gets a lossy copy of the other.

The page is built from **statements** (single lines of writing) rather than a rigid template. The statement is the unit; the views are different ways of laying statements out.

## The four views

| View | What it is |
| --- | --- |
| **human** | The lines as written. Default. The line you're writing in is sharp; the rest soften. |
| **machine** | The same lines as a file tree. Also works as a writing surface. |
| **brutal** | A section as one continuous string. Dense on purpose. |
| **medium** | A reader-first surface: serif column, expanded media, no focus blur. |

Switch with the view HUD (movable, top centre), or set the starting view with a URL hash:

```
index.html#human
index.html#machine
index.html#brutal
index.html#medium
```

Sections can also be toggled individually; the toggle appears on hover at the section divider.

## Quick start

```bash
git clone https://github.com/<you>/<repo>.git
cd <repo>
open cv.html          # or just double-click it
```

To serve it locally instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/cv.html
```

That's the whole setup.

## Make it yours

### 1. Replace the content

Your content is the `SEED` array near the top of the main script. Each entry is `[role, text]`:

```js
var SEED = [
  ["kicker", "by [Your Name](https://example.com)"],
  ["title",  "Your Name"],
  ["dek",    "One line that holds the rest"],
  ["p",      "YOUR NAME _/_ ==ROLE== _/_ https://example.com _/_ you@example.com"],
  ["sec",    "WORK EXPERIENCE_city"],
  ["p",      "**Company** _/_ ==Title== _/_ 2022 – Present _/_ ''What you did''."],
  ["sig",    "best — [example.com](https://example.com)"],
];
```

**Roles:** `kicker`, `title`, `dek`, `p` (body), `sec` (section divider), `sig`.

**Inline markup:**

| Write | Get |
| --- | --- |
| `**text**` | bold |
| `==text==` | highlight |
| `++text++` | underline |
| `_text_` | italic |
| `''text''` | styled emphasis for key phrases |
| `[text](url)` | link |
| `_/_` | field separator within a line |

**Important:** edits made in the browser are saved to `localStorage` and take priority over `SEED`. After changing `SEED`, bump `SEEDV` so returning browsers pick up your new content, or use **Reset demo** in the tools panel.

### 2. Replace the identity bits

Search the file for these and swap in your own:

- `<title>`, `description`, `og:*`, and `twitter:*` meta tags, plus `<link rel="canonical">`
- The favicon (inline SVG data URI in `<head>`)
- The resume `.docx` URL, which appears twice: the download link in the header and the preview fetch in the last small script
- The `<script type="text/plain" id="llmsTxt">` block (your llms.txt content)
- The `ifmf-` / `zcx-` localStorage key prefixes, if you want your own namespace

### 3. Check what you're publishing

The demo content includes real names, employers, and quotes from named co-workers. **Remove or replace all of it** before publishing your own version, and don't republish other people's words without permission.

## Editing on the page

Click any line to edit it.

- `Enter` starts a new line; `Shift+Enter` breaks a line within a statement
- `Cmd/Ctrl+Z` undo, `Cmd/Ctrl+Shift+Z` redo
- A format bar appears while a line is open (bold, highlight, link, image/video/iframe by URL)
- A glyph widget offers copy-paste characters such as — – … and arrows, dots, and stars
- Each statement can be resized independently; the global text-size gauge scales them all proportionally
- `L` toggles the light effect (when you're not typing)

## Export and import

From the tools panel:

| Action | Output |
| --- | --- |
| Export JSON / Copy JSON | Full state: text, per-statement sizes, settings |
| Import JSON | Restores an export |
| Export DOCX | A `.docx` built by hand (store-only zip, no libraries) that keeps bold, underline, highlight, and links |
| Copy TXT | Plain text, the safest format for keeping structure in transit |
| Copy | Markdown |
| Clear / Reset demo | Empty the page or restore the seed content |

The **llms.txt** button in the header opens a popover where the plain-text summary can be copied or downloaded.

## What gets stored locally

Everything stays in your browser. Nothing is sent anywhere.

| Key | Holds |
| --- | --- |
| `zcx-bny-v1` | content and settings |
| `zcx-cv-view2`, `zcx-cv-sv` | view choice and per-section views |
| `zcx-vhud` | view HUD position |
| `ifmf-theme` | light/dark theme |
| `leaf-light` | light-effect settings |

## Accessibility and embedding

- Respects `prefers-reduced-motion`
- Light and dark themes (set before first paint, so no flash)
- When framed in an iframe, the page adds an `is-embed` class that stands down heavy effects and shared storage

## Dependencies

The core page has none. Loaded from the network:

- **Satoshi** font via [Fontshare](https://www.fontshare.com) (falls back to system sans)
- **jszip** and **docx-preview**, used only for the hover preview of a hosted `.docx` resume. If you don't host one, you can delete the two `<script>` tags and the small script after them.

## Known rough edges

Contributions welcome on any of these:

- The jszip and docx-preview scripts load eagerly but are only needed on hover; they should load on demand
- The resume preview fetch adds a cache-busting query string and reads from `raw.githubusercontent.com`; hosting the file alongside the page would cache better
- The Fontshare stylesheet blocks rendering; add `preconnect` hints or self-host the font
- CSS and JS are inline in one file, which is simple to ship but can't be cached separately
- Heavy use of `backdrop-filter` and `filter` may be costly on low-end phones; worth profiling

## Contributing

Issues and pull requests are welcome. Keep it a single file with no build step unless there's a strong reason. If you adapt it for your own CV, I'd love to see it: tag me on LinkedIn.

## License

Code: MIT (see `LICENSE`).
The demo content (text, names, and testimonials) is © Zack Gort and isn't covered by the code license. Replace it with your own.

## Author

**Zack Gort**, ux / creative technologist / designer
[zack.cx/cv](https://zack.cx/cv) · [LinkedIn](https://linkedin.com/in/zackgort) · [@ZCKGRT](https://twitter.com/ZCKGRT) · zckgrt@gmail.com
