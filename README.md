# Word document accessibility tester

A single HTML page that checks a Word `.docx` file for a small set of accessibility problems. Upload a file, run the tests, read the results. Companion tool to the HTML email accessibility tester, sharing its visual identity, layout and interaction patterns.

Everything runs in the browser. The file you choose is never uploaded anywhere.

## Running it

Open `index.html` in a browser. Nothing to install, no build step, no server.

Two files sit alongside it:

- `index.html`, the whole tool
- `favicon.ico`, referenced by the page

## Dependencies

There are none, and that was a deliberate decision rather than an accident.

A `.docx` is a zip archive of XML. The obvious approach is JSZip, which would have made this the first of the two tools to carry a dependency. Instead the tool reads the zip itself: about 60 lines that parse the central directory, plus the browser's built in `DecompressionStream('deflate-raw')` to inflate the three or four parts the tests actually need.

The trade-off is a browser floor:

| Browser | Minimum version |
| --- | --- |
| Chrome, Edge | 80 |
| Firefox | 113 |
| Safari | 16.4 |

If `DecompressionStream` is missing the tool says so plainly rather than failing silently. Both zip compression methods that Word produces are handled, stored and deflated.

This means the "zero dependencies" claim the email tool's README makes still holds here. There is one network request on load, for the Google Fonts stylesheet, which the email tool also makes.

## What a status is allowed to mean

This is the position the tool takes, and it applies to the HTML email tester too.

A **Pass** or a **Fail** is only issued where the answer is fully determined by the file's structure, with no human judgement involved. Everything else is either **Needs review**, when the tool can state a fact that a person has to resolve, or reported with no badge at all, when the tool can only show what is there.

The tool never assesses writing quality. Alt text that reads `chart001.png`, link text that reads "click here", a heading that does not describe its section: all of these are printed in the results so a reviewer sees them immediately, and none of them is flagged. This was decided deliberately rather than overlooked. Judging whether such text is adequate needs the surrounding context, which the tool cannot read, and there is no principled place to stop once it starts guessing.

The line that makes this workable: a fact about the file can be flagged, an opinion about the writing cannot. "The link text is identical to its target URL" is a string comparison, so it is flagged. "The link text is unhelpful" is a judgement, so it is not.

Anything raised for a person to resolve keeps a test off a **Pass**, even where nothing has failed. A document with no heading 1, an image the author marked decorative, a caption paragraph that is not linked to its table, a link to a local file path: none of these is a failure, and none of them should let someone walk away believing the document passed. In the code these rows carry a `raise` marker, so a new test has to decide deliberately whether a badge raises something or merely describes it.

Badges that describe rather than raise leave a Pass intact: a second language in use, a tooltip, a link whose name comes from image alt text, a link stored as a field code, an email address. If every descriptive badge forced a review, one mailto link would stop a document ever passing and the status would carry no information.

Messages state what is in the file and stop there. They do not explain why something matters, what a screen reader will do, or how to fix it. That is training material, not tool output, and the tool was doing it inconsistently and sometimes more confidently than the behaviour warrants.

## Result rows

Every row has the same shape:

    label            the subject: "Heading 2", "Table 1", "Link 3", "Image"
    flag-badge       a problem, red
    badge-neutral    a finding worth a look, or a plain description, grey
    value            what is actually in the file

The value only ever holds file content: heading text, a link's text and target, a table's dimensions, alt text. Anything the tool has to say about that content goes in a badge. A row with nothing from the file in it is badge-only with no value.

Findings that describe the whole document sort above the inventory of what was found. Findings about one specific item stay next to that item.

## What it tests

Eight tests. Each reports one of four statuses: **Pass**, **Fail**, **Needs review**, or **Not tested** when the document has nothing for that test to look at.

### 1. Document title

Reads `dc:title` from `docProps/core.xml`. Fails when it is missing, empty, or whitespace. The title is printed in the results so a meaningless one is visible at a glance.

### 2. Document language

Reads the default language from `w:docDefaults/w:rPrDefault/w:rPr/w:lang` in `word/styles.xml`, falling back to `w:themeFontLang` in `word/settings.xml`. Note that `settings.xml` alone is not the right source: `themeFontLang` is about font selection, not the language assistive technology announces.

Fails when no language is set anywhere. Needs review when content carries a language but no document default is set. Other languages used in the body are listed as information.

### 3. Headings

Detects headings three ways, in order: a style named `heading 1` through `heading 9`, a direct `w:outlineLvl` on the paragraph, then an outline level resolved up the style's `w:basedOn` chain. That last one is why a custom style such as "Section title" based on Heading 2 is correctly treated as a heading.

Fails on a skipped level or a heading with no text, naming each. Reports no heading 1, or several heading 1s, as information rather than failure. Prints the full heading outline, capped at 30 rows.

### 4. Lists

Real lists are paragraphs carrying `w:numPr`, either directly or through their paragraph style. `numId` 0 means numbering has been removed and is not a list.

Typed list markers are flagged when two or more neighbouring paragraphs open with the same kind of marker. A single line starting "1990. That was the year" is not enough, which is deliberate: a lone typed marker is never flagged, in exchange for far fewer false positives.

Also reported: list items containing line breaks, single item lists, and two shapes of split list. Blocks with nothing between them are the strong case, several lists that look like one on the page. Blocks separated by other content are only raised when they share the same abstract numbering.

### 5. Image alt text

Reads `@descr` on `wp:docPr` for every `wp:inline` and `wp:anchor` drawing, in the document body and in headers, footers, footnotes and endnotes. Header part names are discovered from the zip rather than assumed.

Fails on absent, empty or whitespace-only alt text, and on an image that has only a `@title`, since a title is not announced as alt text. An image marked decorative with `adec:decorative` correctly has no alt text, and is raised for review rather than passed, because the tool has no way to confirm the image really is decorative.

Alt text carrying Word's own boilerplate, such as "Description automatically generated", is flagged as needing review. That match only works on English-language Word.

Alt text that is just a filename is reported in full but is not flagged. Judging whether alt text is meaningful is left to manual review.

### 6. Tables

Reads rows and cells as direct children, never descendants, so a nested table is reported separately rather than corrupting its parent's row and cell counts.

The header row is `w:tblHeader` on the first row, read with its `w:val` honoured: absent means true, and `false`, `0` or `off` mean the row is not a header even though the element is present. Word writes `w:val="false"` when the Header Row checkbox is used without Repeat Header Rows, which looks correct on screen and is not.

Three distinct failures: no header row marked, header row marked on a later row so Word ignores it, and an empty header cell. Merged cells, nested tables and single row tables are reported as needing review. A single row table is never failed for lacking a header, since that is the shape most layout tables take.

Word's table alt text (`w:tblCaption`, `w:tblDescription`) is reported either way, as a per-table row when it is set and as a single collapsed row naming how many tables lack it when it is not. A Caption-styled paragraph sitting above a table, and a first column styled to look like headers, are both raised for review: neither is a failure, but both are things an author is likely to believe they have already handled.

### 7. Links

Finds links three ways: `w:hyperlink` with an `r:id`, `w:hyperlink` with a `w:anchor` naming a bookmark, and a `HYPERLINK` field code holding its target as literal text in `w:instr` with no relationship at all. An image can also carry its own link through `a:hlinkClick` with no `w:hyperlink` around it.

Relationships are resolved through each part's own `.rels` file, so a link in `header1.xml` resolves through `word/_rels/header1.xml.rels`. Note that the `Relationship` elements in a `.rels` part use the package relationships namespace, while the `r:id` attribute pointing at them uses the officeDocument one. They are not the same namespace.

Fails only where a screen reader cannot use the link: no text, whitespace-only text, an image with no alt text as the sole content, a target missing from the file, or an anchor pointing at a bookmark that does not exist.

Needs review for facts that need a human: the same visible text pointing at different targets, link text identical to its target URL, link text that is a URL but not the one the link points at, and a URL written as plain text that is not clickable.

Everything else is listed with its text and target and no verdict, including generic text, tooltips, mailto targets and local `file:` paths.

### 8. Text boxes

A modern text box is a `wps` shape holding `w:txbxContent`, written inside `mc:Choice` with a VML copy of the same content in `mc:Fallback`. A legacy text box is a `v:textbox` inside `w:pict` with no DrawingML copy. A shape with no `w:txbxContent` is not a text box and is not reported here.

Reports each box with what it holds and where it sits. Raised for review: a box that floats, since its place in the reading order depends on its anchor; a box with no alt text on the shape; a box containing nothing at all; and two boxes anchored to the same paragraph that sit on the page in the opposite order to the file. An inline box in the text flow is none of those things and passes.

The floating note is one row per test rather than one per box, because it says the same thing about every one of them.

## Content inside a text box

Content in a text box is reported twice, by different tests saying different things. The links test says there is a link and it sits in a text box; the text box test says there is a box and it holds a link. Someone auditing links wants the first, someone auditing structure wants the second, and hiding a broken link from the link test because of where it sits would be the wrong outcome.

One exception. A heading inside a text box is listed and raised, because Word's navigation pane and outline do not include it, but it is left out of the skipped-level sequence. It cannot skip a level in an outline it is not part of. Reporting and sequencing are separate decisions.

## Three rules the text box work established

These were all bugs in tests that had already been signed off, and none of them was visible until a file contained a text box.

**Ignore `mc:Fallback` everywhere.** The shared element lookup filters it out, so a modern text box is read once rather than twice. Without this every test double counts everything inside a box, and the list test invents a split list out of the duplicate.

**A paragraph's properties are direct children, never descendants.** A paragraph containing a text box has no `pPr` of its own, so a descendant search falls through into the box and reads its style instead. This produced a phantom empty heading on the file with a heading inside a box.

**An image belongs to its own nearest drawing.** Find each `pic:pic` and walk up to the closest `wp:inline` or `wp:anchor`, rather than asking whether a drawing contains a picture anywhere below it. Otherwise a text box with a picture in it is reported as an image itself.

**A paragraph's text stops at a drawing.** Text inside a box belongs to the box, not to the paragraph the shape is anchored to.

## The manual review principle

Every test either states a single verifiable fact, or carries a `.test-note` callout saying exactly what it cannot see, with **manual review** in bold.

The rule: if a test's own Pass can be technically correct while missing the point of the rule it exists to enforce, it needs the caveat. No test tries to auto-judge subjective quality. If a check cannot be reduced to a deterministic yes or no, it either becomes a caveat on a real structural test or it is not a test at all.

The gaps worth knowing: text made big and bold instead of styled as a heading, a layout table used to arrange content, whether alt text is meaningful, and which column should hold row headers. Word records nothing that would let any of those be detected.

## Working method

Nothing here shipped on the logic looking right.

Every test was pulled out and run against real Word files in a headless environment, printing actual results for inspection, before being trusted. Three tests are only correct because real files disagreed with what seemed reasonable:

- A split list keeps no shared numbering. Word mints a fresh `w:num` and a fresh `w:abstractNum` on paste, so adjacency is the only signal.
- The decorative flag's `val` attribute is unprefixed, so a namespaced read returns nothing and turns the one correct Pass into a false Fail.
- `w:tblHeader` can be present and false.

Two rules that came out of getting this wrong:

**Verify the built file, not the working copy.** The tests are assembled into `index.html` from a working core file. Checking the working copy while shipping the build is how a broken build once passed verification.

**Keep one test file per outcome.** The set covers each test's pass, each distinct failure, and the false positives each test should not raise. Files that look alike but differ in one XML detail have caught more bugs than any amount of reading the code back.

## Adding a test

1. Write the check as a function taking the parsed parts and returning `{ id, name, summary, status, detail, items, note }`. Rows are `{ label, value, flags, badges, raise }`.
2. Add any new part it needs to `WANTED_PARTS`, or to the pattern that discovers headers and footers.
3. Register it in `TESTS`.
4. Build real `.docx` fixtures first, one per outcome, including at least one file the test must not flag.
5. Run it against them and read the actual output before trusting it.

6. Decide for each badge whether it raises something for a person to resolve, and set `raise` if so. A test with a raised row cannot report a Pass.
