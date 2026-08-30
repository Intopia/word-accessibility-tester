# All test results

---

## 01a-document-title-missing.docx
Badge: Fail
Message: No dc:title element found in docProps/core.xml.

## 01b-document-title-present.docx
Badge: Pass
Message: A document title is set.
- **Title** Document title present

---

## 02a-document-language-missing.docx
Badge: Fail
Message: No language is set anywhere in this document.

## 02b-document-language-present.docx
Badge: Pass
Message: A default language is set for the document.
- **Default language** en-AU — from word/styles.xml (document defaults)

---

## 03a-headings-none-present.docx
Badge: Needs review
Message: No Word heading styles were found in this document.

## 03b-headings-correct-levels.docx
Badge: Pass
Message: 5 headings found, in order, none empty.

## 03c-headings-incorrect-levels.docx
Badge: Fail
Message: 4 headings found, with 2 skipped levels.
- **Heading 1** Grants and funding
- **Heading 4** [Level skipped, 1 to 4] Eligibility
- **Heading 2** Reporting
- **Heading 5** [Level skipped, 2 to 5] Late reports

## 03d-headings-content-that-should-be-a-heading.docx
Badge: Pass
Message: 1 heading found, in order, none empty.
- **Heading 1** Grants and funding

## 03e-headings-no-heading-1.docx
Badge: Needs review
Message: 3 headings found, in order, none empty.
- **Headings** [No heading 1 in this document. The first heading is level 2.]
- **Heading 2** Grants and funding
- **Heading 3** Eligibility
- **Heading 3** Reporting

## 03f-headings-multiple-heading-1s.docx
Badge: Needs review
Message: 4 headings found, in order, none empty.
- **Headings** [2 heading 1s in this document.]
- **Heading 1** Grants and funding
- **Heading 2** Eligibility
- **Heading 1** Travel bursaries
- **Heading 2** Eligible costs

## 03g-headings-empty-heading.docx
Badge: Fail
Message: 3 headings found, with one empty heading.
- **Heading 1** Grants and funding
- **Heading 2** [No text]
- **Heading 2** Reporting

## 03i-headings-custom-style-with-outline-level.docx
Badge: Needs review
Message: 3 headings found, in order, none empty.
- **Custom style** ["Section title" is treated as heading 2, from its own outline level, used 2 times]
- **Heading 1** Grants and funding
- **Heading 2** Eligibility
- **Heading 2** Reporting


----

## 04a-lists-valid-unordered-list.docx
Badge: Pass
Message: 1 real list with 3 items.
- **Real list** Bulleted list, 3 items: Airfares within Australia

## 04b-lists-valid-ordered-list.docx
Badge: Pass
Message: 1 real list with 3 items.
- **Real list** Numbered list, 3 items: Create an account on the grants portal

## 04c-lists-fake-unordered-list.docx
Badge: Needs review
Message: 1 group of typed list markers.
- **Typed as text** [Typed bullet] ● Airfares within Australia
- **Typed as text** [Typed bullet] ● Accommodation up to seven nights
- **Typed as text** [Typed bullet] ● Registration fees

## 04d-lists-fake-ordered-list.docx
Badge: Needs review
Message: 1 group of typed list markers.
- **Typed as text** [Typed number] 1. Create an account on the grants portal
- **Typed as text** [Typed number] 2. Complete the application form
- **Typed as text** [Typed number] 3. Upload your supporting documents

## 04e-lists-nested-list.docx
Badge: Pass
Message: 1 real list with 4 items.
- **Real list** Bulleted list, 4 items, 3 levels: Travel

## 04f-lists-split-list.docx
Badge: Needs review
Message: 3 real lists with 6 items.
- **Lists** [What looks like one list of 6 items is 3 separate lists with nothing between them.]
- **Real list** Bulleted list, 2 items: Airfares within Australia
- **Real list** Bulleted list, 2 items: Accommodation up to seven nights
- **Real list** Bulleted list, 2 items: Registration fees

## 04g-lists-numbering-in-a-style.docx
Badge: Pass
Message: 1 real list with 3 items.
- **Real list** Bulleted list, 3 items, numbering comes from its paragraph style: Airfares within Australia

## 04h-lists-one-item-split-by-line-breaks.docx
Badge: Needs review
Message: 1 real list with 1 item.
- **List item** [Contains 2 line breaks] Airfares within Australia / ● Accommodation up to seven nights / ● Re…
- **Real list** Bulleted list, 1 item: Airfares within Australia / ● Accommoda…

## 04i-lists-single-item-list.docx
Badge: Needs review
Message: 1 real list with 1 item.
- **Lists** [A list with a single item]
- **Real list** Bulleted list, 1 item: Airfares within Australia

---

## 05a-image-alt-present.docx
Badge: Pass
Message: 1 image found, all with alt text or marked as decorative.
- **Image** "Picture 1": Bar chart of grants awarded by year. 42 in 2022, 58 in 2023, 51 in 2024 and 73 in 2025.

## 05b-image-alt-absent.docx
Badge: Fail
Message: 1 image found, 1 without usable alt text.
- **Image** [No alt text] "Picture 1"

## 05c-image-alt-empty-string.docx
Badge: Fail
Message: 1 image found, 1 without usable alt text.
- **Image** [Alt text is empty] "Picture 1"

## 05d-image-alt-whitespace-only.docx
Badge: Fail
Message: 1 image found, 1 without usable alt text.
- **Image** [Alt text is only spaces] "Picture 1"

## 05e-image-marked-as-decorative.docx
Badge: Needs review
Message: 1 image found, with something to look at.
- **Image** [Marked as decorative, so no alt text is expected] "Divider 1"

## 05f-image-alt-in-title-attribute-only.docx
Badge: Fail
Message: 1 image found, 1 without usable alt text.
- **Image** [No alt text] [A title is set, alt text is not] "Picture 1"

## 05h-image-alt-is-filename.docx
Badge: Pass
Message: 1 image found, all with alt text or marked as decorative.
- **Image** "Picture 1": chart001.png

## 05i-image-alt-auto-generated.docx
Badge: Needs review
Message: 1 image found, all with alt text, 1 of them written by Word.
- **Image** [Alt text was written by Word] "Picture 1": Chart, bar chart Description automatically generated

## 05k-image-floating.docx
Badge: Pass
Message: 1 image found, all with alt text or marked as decorative.
- **Image** "Picture 1" (floating): Bar chart of grants awarded by year. 42 in 2022, 58 in 2023, 51 in 2024 and 73 in 2025.

## 05l-image-in-header.docx
Badge: Fail
Message: 1 image found, 1 without usable alt text.
- **Image** [No alt text] "Picture 1" (in a page header)

## 05n-image-in-table-cell.docx
Badge: Fail
Message: 1 image found, 1 without usable alt text.
- **Image** [No alt text] "Picture 1" (in a table cell)

---

## 06a-tables-header-row-marked.docx
Badge: Pass
Message: 1 table found, each with a marked header row.
- **Table 1** 5 rows by 3 columns, 1 header row, starts "Year"

## 06b-tables-no-header-row.docx
Badge: Fail
Message: 1 table found, 1 without a usable header row.
- **Table 1** [No header row marked] 5 rows by 3 columns, starts "Year"

## 06c-tables-header-row-formatting-only.docx
Badge: Fail
Message: 1 table found, 1 without a usable header row.
- **Table 1** [No header row marked] 5 rows by 3 columns, starts "Year"

## 06e-tables-header-row-on-wrong-row.docx
Badge: Fail
Message: 1 table found, 1 without a usable header row.
- **Table 1** [Header row marked on row 2, not the first row] 5 rows by 3 columns, starts "Year"

## 06f-tables-alt-text-present.docx
Badge: Pass
Message: 1 table found, each with a marked header row.
- **Table 1** 5 rows by 3 columns, 1 header row, starts "Year"
- **Table 1** Alt text set: title "Grant applications and awards", description "Three column table listing the year, the number of applicat…"

## 06g-tables-alt-text-absent.docx
Badge: Pass
Message: 1 table found, each with a marked header row.
- **Table 1** 5 rows by 3 columns, 1 header row, starts "Year"

## 06i-tables-visible-caption-paragraph.docx
Badge: Needs review
Message: 1 table found, all with a header row, 1 needing a look.
- **Table 1** 5 rows by 3 columns, 1 header row, starts "Year"
- **Table 1** [A caption paragraph sits above this table, but nothing in the file links the two] Table 1: Grant applications and awards,…

## 06k-tables-horizontally-merged-cells.docx
Badge: Needs review
Message: 1 table found, all with a header row, 1 needing a look.
- **Table 1** [A cell spans columns] 6 rows by 3 columns, 1 header row, starts "Year"

## 06l-tables-vertically-merged-cells.docx
Badge: Needs review
Message: 1 table found, all with a header row, 1 needing a look.
- **Table 1** [Cells merged down rows] 5 rows by 3 columns, 1 header row, starts "Round"

## 06m-tables-nested-table.docx
Badge: Needs review
Message: 2 tables found, all with a header row, 1 needing a look.
- **Table 1** 2 rows by 2 columns, 1 header row, starts "Round"
- **Table 2** [Nested inside another table] 3 rows by 2 columns, 1 header row, starts "Year"

## 06p-tables-layout-table.docx
Badge: Needs review
Message: 1 table found, all with a header row, 1 needing a look.
- **Table 1** [Single row table] 1 row by 2 columns

## 06q-tables-first-column-looks-like-headers.docx
Badge: Needs review
Message: 1 table found, all with a header row, 1 needing a look.
- **Table 1** 5 rows by 3 columns, 1 header row, starts "Year"
- **Table 1** [The first column is styled as headers. Word has no way to mark row headers.]
Should this be at the top of the list?

## 06r-tables-table-in-page-header.docx
Badge: Fail
Message: 1 table found, 1 without a usable header row.
- **Table 1** [No header row marked] in a page header, 2 rows by 2 columns, starts "Document"

## 06s-tables-empty-header-cell.docx
Badge: Fail
Message: 1 table found, 1 without a usable header row.
- **Table 1** [Header cell 1 is empty] 5 rows by 3 columns, 1 header row

## 06t-tables-two-tables-one-passing-one-failing.docx
Badge: Fail
Message: 2 tables found, 1 without a usable header row.
- **Table 1** 5 rows by 3 columns, 1 header row, starts "Year"
- **Table 2** [No header row marked] 4 rows by 2 columns, starts "Category"

---

## 07a-links-descriptive-text.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "grants program guidelines" → https://www.intopia.digital/

## 07b-links-non-descriptive-text.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "click here" → https://www.intopia.digital/

## 07c-links-bare-url-as-text.docx
Badge: Needs review
Message: 1 link found, all usable, 1 needing a look.
- **Link 1** [Link text is the URL] "https://www.w3.org/WAI/WCAG22/quickref/" → https://www.w3.org/WAI/WCAG22/quickref/

## 07d-links-empty.docx
Badge: Fail
Message: 1 link found, 1 that a screen reader cannot use.
- **Link 1** [No link text] no text → https://www.intopia.digital/

## 07e-links-whitespace-only-text.docx
Badge: Fail
Message: 1 link found, 1 that a screen reader cannot use.
- **Link 1** [Link text is only spaces] no text → https://www.intopia.digital/

## 07f-links-repeated-text-different-destinations.docx
Badge: Needs review
Message: 3 links found, all usable, 3 needing a look.
- **Link 1** [Same text as another link with a different target] "Read more" → https://www.intopia.digital/
- **Link 2** [Same text as another link with a different target] "Read more" → https://www.w3.org/TR/WCAG22/
- **Link 3** Same text as another link with a different target] "Read more" → https://maxdesign.com.au/

## 07g-links-repeated-text-same-destination.docx
Badge: Pass
Message: 3 links found, each with text to announce and a target that resolves.
- **Link 1** "Read more" → https://www.intopia.digital/
- **Link 2** "Read more" → https://www.intopia.digital/
- **Link 3** "Read more" → https://www.intopia.digital/

## 07h-links-text-split-across-runs.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "grants program guidelines" → https://www.intopia.digital/

## 07i-links-non-descriptive-text-with-tooltip.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** [Tooltip: Grants program guidelines, Intopia] "Read more" → https://www.intopia.digital/

## 07j-links-image-link-with-alt.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** [Name comes from the image alt text] "Bar chart of grants awarded by year. 42 in 2022, 58 in 2023…" → https://www.intopia.digital/

## 07k-links-image-link-without-alt.docx
Badge: Fail
Message: 1 link found, 1 that a screen reader cannot use.
- **Link 1** [An image with no alt text is the only content, so nothing is announced] no text → https://www.intopia.digital

## 07l-links-image-link-via-hlinkclick.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** [Name comes from the image alt text] "Bar chart of grants awarded by year. 42 in 2022, 58 in 2023…" → https://www.intopia.digital/

## 07m-links-hyperlink-field-code.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** [Stored as a field code] "grants program guidelines" → https://www.intopia.digital/

## 07n-links-plain-text-url.docx
Badge: Needs review
Message: No links found, but 1 URL written as plain text.
- Not a link [For review] https://www.w3.org/WAI/WCAG22/quickref/ is written as text and is not clickable

## 07o-links-link-in-page-header.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "click here" in a page header → https://www.intopia.digital/

## 07p-links-link-in-footnote.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "grants program guidelines" in the footnotes → https://www.intopia.digital/

## 07q-links-link-in-table-cell.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "click here" → https://www.intopia.digital/

## 07r-links-internal-link-to-existing-bookmark.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** "reporting requirements" → this document, bookmark "reporting"

## 07s-links-internal-link-to-missing-bookmark.docx
Badge: Fail
Message: 1 link found, 1 that a screen reader cannot use.
- **Link 1** [Points at a bookmark named "reporting" that does not exist] "reporting requirements" → this document, bookmark "reporting"

## 07t-links-mailto-link.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** [Email address] "grants@example.gov.au" → mailto:grants@example.gov.au

## 07u-links-local-file-path-link.docx
Badge: Pass
Message: 1 link found, each with text to announce and a target that resolves.
- **Link 1** [Local file path, will not resolve for other people] "grants program guidelines" → file:///C:/Users/Documents/grants-guidelines.docx

## 07v-links-multiple.docx
Badge: Needs review
Message: 6 links found, all usable, 2 needing a look.
- **Link 1** "grants program guidelines" → https://www.intopia.digital/
- **Link 2** "click here" → https://www.w3.org/TR/WCAG22/
- **Link 3** "https://www.w3.org/WAI/WCAG22/quickref/" → https://www.w3.org/WAI/WCAG22/quickref/
- **Link 4** [Same text as another link with a different target] "Read more" → https://maxdesign.com.au/
- **Link 5** [Same text as another link with a different target] "Read more" → https://www.w3.org/TR/WCAG22/
- **Link 6** "Intopia contact page" → https://www.intopia.digital/

## 07w-links-displayed-url-does-not-match-target.docx
Badge: Needs review
Message: 1 link found, all usable, 1 needing a look.
- **Link 1** [Link text is a URL, but not the one this link points at] "https://www.w3.org/WAI/WCAG22/quickref/" → https://maxdesign.com.au/

---


