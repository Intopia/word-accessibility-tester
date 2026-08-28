# Handoff: Word document accessibility tester

Companion tool to the HTML email accessibility tester. Same visual identity, same layout, same interaction patterns, same accessibility-of-the-tool-itself standards. Completely different test logic underneath, since `.docx` is a fundamentally different file format from HTML.

This doc is written so a fresh conversation (or a fresh developer) can pick it up without re-deriving any of the design decisions already made and tested on the email tool.

Core flow: upload a Word file → click "Run accessibility tests" → results screen with a summary list plus detailed test sections → "Start over" resets everything.

---

## 1. What to reuse exactly (design tokens, verbatim)

Pull these straight from the email tool's `index.html`, don't redesign them.

```css
:root {
  --ink: #16233d;
  --ink-soft: #4a5875;
  --paper: #efefef;
  --panel: #ffffff;
  --line: #d9d2be;
  --border-strong: #dddddd;
  --red: #a5322a;
  --red-bg: #fbeae7;
  --green: #29614a;
  --green-bg: #e9f2ec;
  --amber: #93641a;
  --amber-bg: #faf1dc;
  --neutral: #5b6472;
  --neutral-bg: #f4f3ef;
  --info: #29506b;
  --info-bg: #e6eef3;
  --focus: #1b4fbf;
  --brand: #c03c0c;
  --font-brand: 'Source Sans Pro', 'Helvetica Neue', Helvetica, Arial, Frutiger, 'Frutiger Linotype', Univers, Calibri, 'Gill Sans', 'Gill Sans MT', 'Myriad Pro', Myriad, 'DejaVu Sans Condensed', 'Liberation Sans', 'Nimbus Sans L', Tahoma, Geneva, sans-serif;
  --font-display: 'Space Grotesk', sans-serif;
  --font-body: 'IBM Plex Sans', sans-serif;
  --font-mono: 'IBM Plex Mono', monospace;
  --radius: 4px;
}
```

Font import (Google Fonts, loaded via `@import` in the stylesheet):
```css
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap');
```

Logo (180px wide, sits above the h1, not beside it):
```html
<img src="https://intopia.digital/wp-content/themes/intopia-refresh/assets/images/intopia.png" alt="Intopia logo" class="intopia-logo">
```

Favicon reference (needs its own `favicon.ico` file alongside the new tool):
```html
<link rel="shortcut icon" href="favicon.ico">
```

h1: brand colour (`var(--brand)`), `var(--font-brand)`, `2.4rem`, weight `600`, no letter-spacing. h2/h3 elsewhere: black (`#000000`), not brand colour, brand colour on every heading reads as an error state once red fail badges are on the same screen, this was a real bug we fixed, don't reintroduce it.

## 2. What to reuse exactly (component patterns)

**Badges** — plain colour-coded text pills, never icons. Four states: Pass (green), Fail (red), Needs review (amber), Not tested (grey/neutral). No glyphs, no checkmarks, text label is the only signal. This was a deliberate accessibility decision on the email tool (icon-only status indicators aren't reliably accessible), carry it forward unchanged.

**Summary list** — a single-column `<ul>`, not a grid. A grid caused real overflow/wrapping bugs on the email tool. Each row: badge, then test name, as a horizontal flex row, full width, linking down to that test's detail section.

**Test cards** — one `<section class="test-card">` per test, white background, `2px solid #ddd` border. Header row: `<h3>Test N: Name</h3>` on the left, badge on the right (`justify-content: space-between`). A `.test-summary` paragraph underneath stating what the test checks in one sentence.

**The manual-review note pattern** — see section 4 below, this is important enough to get its own section.

**Result lists** — `<ul class="result-list">`, each `<li class="result-item">` a flex row that wraps, red-flagged items get `.result-item--flag` (red left border, red background tint) plus one or more `.flag-badge` red pill labels naming the specific problem. Neutral/informational badges use `.badge-neutral`, same shape, grey instead of red.

**One button, two ways in** — "Option 1: Paste your [content] markup" (textarea) and "Option 2: Upload a [file type]" (native `<input type="file">`), no divider between them, the labels alone carry the distinction. File selection reads the file and drops its content into the same textarea Option 1 uses, so there's exactly one path through the logic afterward, not two. A single "Run accessibility tests" button underneath both. Do not add a third "load a sample" button, we built one, it caused real confusion (people read "load a sample" as "load your own file"), and removed it.

**Focus management** — after running, focus moves to a real, visible `<h2 id="results-heading" tabindex="-1">Results</h2>`, not a hidden element. It gets a blue focus ring that fades to transparent over 2 seconds (respecting `prefers-reduced-motion`, no animation, ring just stays solid, for anyone with that preference set). Heading hierarchy: H1 (page title) → H2 "Results" → H3 "Summary" and H3 per test. Don't let per-test headings be H2, we shipped that bug once already.

**No unnecessary ARIA.** No `role="alert"` on error messages if focus management already handles it (moving focus to an invalid field is enough, the field's own `aria-describedby` does the announcing). No hidden `aria-live` status region if a real, focused heading is already doing that job. We built and then deliberately removed both of these from the email tool, they were redundant once the underlying focus/semantics were correct. Don't reintroduce either "for accessibility", check whether something already covers the need first.

## 3. What's completely different, needs fresh design

**Parsing.** A `.docx` file is a zip archive of XML, not plain markup. `DOMParser` (which the email tool relies on entirely) does nothing useful here. You'll need:
- **JSZip** (or equivalent) to unzip the file client-side, still 100% in-browser, still nothing uploaded anywhere, just a real added dependency where the email tool had zero.
- Parse `word/document.xml` as XML. Tags are namespaced (`w:p` for paragraph, `w:r` for run, `w:t` for text, `w:pStyle` for the paragraph's style reference).
- Named styles live in `word/styles.xml`, separate from the document body. This matters a lot: HTML email leans on inline styles almost universally (which is *why* the colour contrast test on the email tool works reliably), Word does the opposite, formatting is usually resolved through a named style, not sitting directly on the run. Any Word contrast check would need to resolve style references across two files, and should be scoped conservatively, or possibly not attempted at all in v1, rather than quietly wrong.
- Document properties (title, language) live in `docProps/core.xml` and `word/settings.xml` respectively, not in anything resembling `<title>` or `lang=`.

**This breaks the "zero dependencies" claim the email tool's README makes.** Worth deciding deliberately, and saying so plainly in this tool's own README, rather than letting it be an accidental inconsistency between the two.

## 4. The manual-review caveat system, carry the principle forward

Every test on the email tool falls into one of two categories, and this is the design principle to apply here too, not just the specific email tests:

> Can this test's own Pass be technically correct while still missing the point of the rule it exists to enforce?

If no, the test states a single verifiable fact (a language attribute is present, two ids collide, a document title exists) and needs no caveat. If yes, and it's yes for most content-quality tests, the results include a plain-language `.test-note` callout stating exactly what the test can't see, with the words **manual review** bolded inline. No icons, no apology, just a clear statement of scope. Example, verbatim from the email tool, as a template for tone:

> This test confirms alt text is present. It can't judge whether that text is meaningful or accurate, that needs **manual review**.

Do not build a test that tries to auto-judge subjective quality (we built and then removed exactly that, an alt-text "quality" heuristic checking for generic/filename-like text). If a test can't be reduced to a deterministic yes/no, it either needs a manual-review caveat on a real structural test, or it shouldn't be a test at all.

## 5. Suggested starting test set for Word

Not exhaustive, and not all of these need to ship at once, the email tool didn't either, it started with 7 tests and grew to 10 over many iterations, each one verified against real files before being trusted. Recommend the same approach here: 3–4 tight tests first, prove the parsing pipeline is solid, then layer in more.

**Likely unambiguous (no manual-review note needed):**
- Document title present (`docProps/core.xml`)
- Document language set (`word/settings.xml`)
- Duplicate bookmark names or similar structural collisions, if relevant to Word's model

**Likely needs a manual-review note (confirms a necessary condition, not the goal):**
- Headings use real Word heading styles (`Heading 1`–`Heading 6`), present, and not skipping levels, same shape as the email tool's heading test
- Images have alt text set (Word stores this differently depending on version, check `wp:docPr` / `descr` attribute)
- Table header rows marked as such (Word's "repeat header row" / header row property)

**Word's own signature issue, no equivalent in the email tool:**
- **Fake lists.** Someone typing "1. Item" and pressing enter, instead of using Word's real numbered-list formatting, is one of the single most common real-world Word accessibility failures. A screen reader reads manually-typed numbers as plain text, not as a list with item count, "1. Item, 2. Item" instead of "list, 2 items, item 1." This is clean and binary: does this paragraph start with a plausible list marker as literal text, while not being a real Word list-formatted paragraph? If yes, flag it. This is worth building early, it's distinctive to this tool and catches a very real, very common problem.

## 6. Working method, carry this forward without exception

Nothing on the email tool shipped on "the logic looks right." The standard that actually caught real bugs: pull the check logic out, run it against real files (not just synthetic test cases) using a headless DOM environment, print the actual result, verify by hand, before trusting any change. This caught a false-positive bug, a status-logic bug, and an accidental deletion mid-edit, none of which would have been caught by reading the code back. Build the same verification habit into this project from test 1, not after problems appear.

Also build the same small isolated test-file sets we built for the email tool, one `.docx` file per test/outcome, so every test can be demonstrated and regression-checked on its own.
