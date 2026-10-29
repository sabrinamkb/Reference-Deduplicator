# Reference Deduplicator

A single-file, browser-based tool for finding duplicate references during a systematic review. Drop in your exported search results, review the suspected duplicates, and download a cleaned list.

Everything runs locally in your browser. No installation, no server, no uploads, and no internet connection needed once you have the file.

## Quick start

1. Download `dedupe.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Drag your reference files onto the drop zone, or click it to choose files. You can add several files at once.
4. Review the duplicate groups and adjust the settings if needed.
5. Download the unique records and the removed-duplicates log.

## Supported formats

| Format | Extensions | Typical sources |
|---|---|---|
| RIS | `.ris`, `.txt` | PubMed, Embase, Scopus, Web of Science, EndNote, Zotero |
| BibTeX | `.bib` | Zotero, Google Scholar, Overleaf |
| CSV / TSV | `.csv`, `.tsv` | Rayyan, Covidence, spreadsheets |

CSV files need a header row with at least a `title` column. The tool also recognises `authors`, `year`, `doi`, `journal` and `abstract` columns, matched case-insensitively.

## How duplicates are detected

Every pair of records is compared in this order:

1. **Same DOI**: flagged as a duplicate.
2. **Identical title** after normalisation (lowercased, accents, punctuation and HTML tags removed).
3. **Similar title**: bigram (Dice) similarity at or above the chosen threshold (default 90%). By default this also requires the publication years to be within 1 of each other and the first authors to share a surname.

Safeguards:

- If both records have a DOI and the DOIs differ, they are never matched on title. This avoids merging a paper with its erratum or a similarly titled paper.
- Titles shorter than about 15 characters are only compared by DOI.

Matching records are linked into groups. In each group the most complete record (DOI and abstract preferred) is pre-selected to keep.

## Settings

- **Title similarity slider (70-100%)**: how close two titles must be to count as the same. Lower it to catch more duplicates (at the cost of more false matches); raise it for stricter matching. It only affects "Similar title" matches.
- **Years must be within 1**: requires publication years to be within one year of each other when both are present.
- **First authors must match**: requires the first authors to share a surname when both are present.

## Reviewing results

- Each group shows why it was flagged (Same DOI, Identical title, Similar title).
- Choose which record to keep with the radio buttons.
- Click **Not duplicates** to keep every record in a group.
- Live counts (records imported, duplicate groups, duplicates removed, unique records left) can be used for your PRISMA flow diagram.

## Exports

- **Unique records (CSV)**: your deduplicated list.
- **Unique records (RIS)**: for re-importing into Rayyan, Covidence, Zotero, etc.
- **Removed duplicates (CSV)**: an audit log of what was removed.

Exports reflect your manual choices in the review step.

## Limitations

- Always review the "Similar title" groups manually. Exact DOI and title matches are very reliable, but fuzzy matching can produce false positives and negatives.
- Records without DOIs and with very short or missing titles may be missed.
- Conference papers and their later journal versions, or preprints and published versions with different DOIs, are not matched on title.
- Only title, authors, year, journal, DOI and abstract are carried through. Other fields (volume, pages, keywords, notes) are not preserved in the exported files, so keep your original files.
- Performance is fine for thousands of records, but comparison time grows with the square of the number of records.
- Please validate the tool on a sample of your own data before relying on it, and describe your deduplication method in your review's methods section.

## Credits

Created with [Claude](https://claude.ai) (Claude Sonnet 5.5), an AI assistant made by [Anthropic](https://www.anthropic.com), in October 2026. The design, requirements and testing on real review data were directed by the repository author.


