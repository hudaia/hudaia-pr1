# Hudaia Talks Catalog

The single source of truth for the public Talks page is [data/talks.json](../data/talks.json). The page [talks/index.html](../talks/index.html) renders the catalog; do not hard-code talk rows in HTML.

## Data contract (version 3)

Each talk has:
- `id`: immutable unique ID (e.g. `talk-bEjL_QizhDA`), used for updates, deletions and page anchors.
- `youtube_id`, `source_url`: original video identity and link.
- `language`: spoken language of the video, `ar` (Arabic), `en` (English), or `undetermined` until verified. This is independent of bilingual UI translations.
- `title`, `title_ar`, `description`, `description_ar`: bilingual display content.
- `topic_id`: stable category identifier; `topic` and `topic_ar` retained for backward compatibility and searching.
- `date_added`: calendar date added to the Hudaia library (YYYY-MM-DD).
- `created_at`, `updated_at`: ISO 8601 UTC timestamps for catalog metadata creation and last edit. The existing talk's `created_at` was recorded during the metadata migration on October 17, 2026, not at the time it was originally added.

Each topic has an immutable `id`, bilingual names, and an `order` field. Topic names can change without breaking IDs.

## Editing workflow

1. Find the talk by its `id` in `data/talks.json`.
2. **Add**: append a unique ID and all required fields, use the requested topic ID and spoken `language`, set `date_added`, `created_at`, and `updated_at`.
3. **Modify**: change the fields, keep `id`, `date_added` and `created_at` unchanged, set `updated_at` to the edit time.
4. **Delete**: remove the matching object from `talks`; preserve the Git commit history for audit and recovery.
5. Commit and push. GitHub Pages redeploys automatically.

Permanent talk URL format: `https://hudaia.github.io/hudaia-pr1/talks/#<talk-id>`.

The catalog is a static JSON file, not a database. No login or web-based admin editing is available yet. Editing/deleting requires updating the repository. Git history records changes, but a deleted talk's old deep link no longer resolves to that entry.

The public library divides each topic into Arabic and English subsections and offers a language filter. Unknown-language videos appear in a separate unclassified subsection until reviewed.
