# Unicode text inspector

A standalone local tool for finding a hidden character or an equal-looking text difference, locating its original UTF-16 range, and exporting a precise review handoff. Open `index.html` in a modern browser. No build, account, installation or server is required.

## Try the review journey

1. Paste text, or choose **Load mixed example**. The synthetic example includes composed/decomposed accents, a no-break space, a tab, a supplementary emoji, an emoji joiner and a directional mark.
2. Choose **Inspect text**. The summary separates UTF-16 units, Unicode scalar values, unpaired surrogate units and browser grapheme segmentation.
3. Read the exact comparison and expand the escaped NFC/NFD previews. Equal-looking glyphs may have different UTF-16 sequences. The first different offset is explicit; this is not a complete alignment/diff algorithm.
4. Find a diagnostic row and choose **Select** to highlight that exact range in its source editor. Neither selection nor normalization changes the source.
5. Download the CSV. It includes every accepted original and enabled comparison item, not just the currently displayed page.

The second text is optional; its checkbox must be enabled to include it. An enabled empty comparison is a real empty string. Reset clears both texts and results. Editing either text or toggling comparison invalidates old results and disables their export.

## Interpret the positions accurately

- All indexes and offsets are **zero-based**. A UTF-16 range `[start, end)` includes its start and excludes its end.
- A supplementary code point occupies two UTF-16 units. A diagnostic item is one Unicode scalar value or one unpaired surrogate unit. Item indexes are scan positions, not grapheme indexes.
- A Unicode scalar value is a code point outside the surrogate range. A lone high or low surrogate is an invalid UTF-16 sequence element: the row retains its numeric unit and escaped representation, without inventing a scalar or character name.
- `Intl.Segmenter` with `granularity: "grapheme"` supplies the optional grapheme count. Its resolved locale is shown. A grapheme segment is not a guaranteed glyph, visible letter, display width or measure of comprehension. Missing/error states are explicit; no grapheme count is claimed when lone surrogates are present.
- Browser textareas normalize CRLF and CR to LF. The tool preserves the editor value as received by the browser, **not original file bytes or pre-paste line endings**. Source offsets refer to that value.

## Names, formatting and normalization

Names come from a small explicit built-in list covering selected controls, spaces, combining characters and demo emoji. A miss says **Name not supplied by this tool**. It does not mean unassigned, invalid, unsupported or nonexistent. No script/language or intent is inferred from appearance.

Directional controls, join controls, variation selectors, whitespace, combining marks and other format characters receive neutral context notes. They can be legitimate and necessary. Selected ranges are checked explicitly; additional mark/format/whitespace checks use browser Unicode properties. An ordinary category result is not a security, validity or “clean text” certificate. Unicode data can differ between browser versions.

NFC and NFD use `String.prototype.normalize`. They are **separate escaped previews**, never repairs. Counts and offsets of a normalized form belong to that form, not the original. The exact comparison uses UTF-16 equality and a first-difference offset. When both NFC results are available, their equality is also shown without claiming equal meaning. Normalization is not evaluated for lone-surrogate input; absent APIs, exceptions and preview limits are separate states.

Diagnostics use ASCII escapes for all input-derived non-ASCII characters and controls. Bidirectional input therefore cannot reorder the tool's own diagnostic labels. Markup is added as DOM text, never executed. The editors themselves show the entered text with ordinary browser bidirectional behavior.

## Bounds and CSV

Each editor is limited to **4,096 UTF-16 code units**, for at most 8,192 input units when comparison is enabled. Over-limit inspection is rejected, not truncated. Oversized clipboard pastes are stopped while retaining existing text. A browser input method or programmatic assignment can still place oversized text in a box; the inspection check rejects it.

The table adds up to **100 rows per display step**. CSV contains all accepted rows. Normalized previews are bounded separately at 16,384 UTF-16 units per form; an excess is reported, not silently shortened. Normalized forms are not included as original-range rows in the CSV.

The CSV has ten columns: Source, Item index (0-based), UTF-16 start, UTF-16 end exclusive, Unicode scalar value, UTF-16 hex units, Escaped ASCII text, Known name (limited list), Category and Review note. All values are quoted; quotes are doubled; rows use CRLF and the file has a UTF-8 BOM. Formula-like cells starting with `=`, `+`, `-` or `@` after whitespace/control characters receive a protective apostrophe. Keep that prefix when opening untrusted values in a spreadsheet.

There is deliberately **no raw-text CSV column**: UTF-8 encoding could replace an unpaired surrogate and raw direction controls could obscure the evidence. Concatenate each source's `UTF-16 hex units` in item order to reconstruct its exact browser string, including lone surrogates. Each hex token is a 16-bit unit; use `String.fromCharCode`, not `fromCodePoint`, when reconstructing those units. Escaped text uses `\n`, `\r`, `\t`, escaped backslashes/quotes and `\uXXXX` units. Numeric units remain authoritative if a spreadsheet alters display formatting.

## Privacy and human review

Processing stays in page memory. There are no external scripts, fonts, analytics, storage calls or automatic network requests. A restrictive meta content security policy blocks connections and external assets. Refreshing loses input. Optional reference links leave the tool only when followed.

[PewTranslate's guide to reviewing transcript and translation errors](https://pewtranslate.com/en/guides/check-sermon-transcript-translation-errors) gives context for recording uncertain wording and involving a competent reviewer. A code-unit difference does not resolve meaning, translation accuracy or intent.

AI-assisted original code by [PewTranslate](https://pewtranslate.com/). Live-translation accounts are closed. This standalone inspector does not translate or establish production availability, supported languages or professional certification.

Primary references: [Unicode normalization](https://www.unicode.org/reports/tr15/), [segmentation](https://www.unicode.org/reports/tr29/), [bidirectional algorithm](https://www.unicode.org/reports/tr9/), [JavaScript code points](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt), [normalization API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize), [Intl.Segmenter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Segmenter), and [HTML textarea behavior](https://html.spec.whatwg.org/multipage/form-elements.html#the-textarea-element).

## Publication and license

Public files: `index.html`, `README.md`, `LICENSE`. Exclude `.verification/`, which contains local test scripts, downloads, screenshots and manifests. Original code and documentation are MIT licensed, copyright 2026 szj2ys. This focused utility does not implement full Unicode conformance testing, confusable detection, encoding detection or automatic correction.
