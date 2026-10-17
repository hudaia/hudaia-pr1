# Hudaia Talks Catalog

The single source of truth for the public Talks page is [data/talks.json](../data/talks.json). The page [talks/index.html](../talks/index.html) renders the catalog; do not hard-code talk rows in HTML.

## Data contract (version 4)

Each talk has:
- `id`: immutable unique 8-digit numeric ID in `YYWWDNNN` format (e.g. `26425002`), used for updates, deletions and page anchors.
- `id_date`: date used to generate the ID (normally the video event date, e.g. Friday khutba date), separate from the date actually added to the catalog.
- `legacy_ids`: old anchors retained so previously shared links keep working.
- `youtube_id`, `source_url`: original video identity and link.
- `language`: spoken language of the video, `ar` (Arabic), `en` (English), or `undetermined` until verified. This is independent of bilingual UI translations.
- `title`, `title_ar`, `description`, `description_ar`: bilingual display content.
- `topic_id`: stable category identifier; `topic` and `topic_ar` retained for backward compatibility and searching.
- `date_added`: calendar date added to the Hudaia library (YYYY-MM-DD).
- `created_at`, `updated_at`: ISO 8601 UTC timestamps for catalog metadata creation and last edit. The existing talk's `created_at` was recorded during the metadata migration on October 17, 2026, not at the time it was originally added.

Each topic has an immutable `id`, bilingual names, and an `order` field. Topic names can change without breaking IDs.

## ID allocation

Use Sunday-start weeks: week 01 begins on January 1 and ends on the first Saturday, week 02 starts the next Sunday, etc. Compute `WW = floor((day_of_year_zero_based + weekday_of_Jan_1)/7) + 1`. `D` is Sunday 0, Monday 1, … Friday 5, Saturday 6. For the chosen `id_date`, allocate the next unused `NNN` from 001 through 999. Check all existing IDs in `data/talks.json` before allocation, never reuse a deleted ID, and keep a deletion/tombstone record if deletions are implemented. Do not change an ID when editing a talk. The ID is an opaque string (do not parse it as a number). For October 16, 2026 (Friday), IDs start at `26425001`; the second video is `26425002`. This week convention is explicitly not ISO-8601 weeks.

## Editing workflow

1. Find the talk by its `id` in `data/talks.json`.
2. **Add**: append the next available ID for its `id_date` and all required fields, use the requested topic ID and spoken `language`, set `date_added`, `created_at`, and `updated_at`.
3. **Modify**: change the fields, keep `id`, `id_date`, `date_added` and `created_at` unchanged, set `updated_at` to the edit time.
4. **Delete**: remove the matching object from `talks`; preserve the Git commit history for audit and recovery.
5. Commit and push. GitHub Pages redeploys automatically.

Permanent talk URL format: `https://hudaia.github.io/hudaia-pr1/talks/#<talk-id>`.

The catalog is a static JSON file, not a database. No login or web-based admin editing is available yet. Editing/deleting requires updating the repository. Git history records changes, but a deleted talk's old deep link no longer resolves to that entry.

The public library divides each topic into Arabic and English subsections and offers a language filter. Unknown-language videos appear in a separate unclassified subsection until reviewed.
