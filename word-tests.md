All four tests pass using real word files:

01a-document-title-missing.docx
01b-document-title-present.docx
02a-document-language-missing.docx
02b-document-language-present.docx

What shall we move onto next? accessible headings, image alts, lists, tables?

---

Next: testing all heading level issues

## 03a-headings-none-present.docx
No Word heading styles were found in this document.

## 03b-headings-correct-levels.docx
5 headings found, in order, none empty.

## 03c-headings-incorrect-levels.docx
4 headings found, with 2 skipped levels.

## 03d-headings-content-that-should-be-a-heading.docx
1 heading found, in order, none empty.
This is expected, and why manual review is till vital

## 03e-headings-no-heading-1.docx
3 headings found, in order, none empty.
For review: No heading 1 in this document. The first heading is level 2.

## 03f-headings-multiple-heading-1s.docx
4 headings found, in order, none empty.
2 heading 1s in this document. That can be correct, it depends on the document.

## 03g-headings-empty-heading.docx
3 headings found, with one empty heading.
Heading 2 No text (No text)

## 03i-headings-custom-style-with-outline-level.docx
3 headings found, in order, none empty.
Custom style: "Section title" is treated as heading 2, from its outline level
Custom style: "Section title" is treated as heading 2, from its outline level

----

## 04a-lists-valid-unordered-list.docx
1 real list with 3 items.
Bulleted list, 3 items: Airfares within Australia

## 04b-lists-valid-ordered-list.docx
1 real list with 3 items.
Numbered list, 3 items: Create an account on the grants portal

## 04c-lists-fake-unordered-list.docx
1 group of typed list markers.
Typed as text Typed bullet ● Airfares within Australia
Typed as text Typed bullet ● Accommodation up to seven nights
Typed as text Typed bullet ● Registration fees

## 04d-lists-fake-ordered-list.docx
1 group of typed list markers.
Typed as text Typed number 1. Create an account on the grants portal
Typed as text Typed number 2. Complete the application form
Typed as text Typed number 3. Upload your supporting documents

## 04e-lists-nested-list.docx
1 real list with 4 items.
Bulleted list, 4 items, 3 levels: Travel

## 04f-lists-split-list.docx
3 real lists with 6 items.
Real list Bulleted list, 2 items: Airfares within Australia
Real list Bulleted list, 2 items: Accommodation up to seven nights
Real list Bulleted list, 2 items: Registration fees

## 04g-lists-numbering-in-a-style.docx
1 real list with 3 items.
Real list Bulleted list, 3 items, numbering comes from its paragraph style: Airfares within Australia

## 04h-lists-one-item-split-by-line-breaks.docx
1 real list with 1 item.
Real list Bulleted list, 1 item: Airfares within Australia / ● Accommoda…
List item Contains 2 line breaks Airfares within Australia / ● Accommodation up to seven nights / ● Re…

## 04i-lists-single-item-list.docx
1 real list with 1 item.
Real list Bulleted list, 1 item: Airfares within Australia
For review A list with a single item: Airfares within Australia. That is sometimes deliberate.
