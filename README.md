# UC PACS — releases

Installers for **UC PACS**, plus the small `update.json` the application reads
to find out whether a newer version exists.

This repository is machinery, not a download page. UC PACS checks it by itself
and offers the update inside the program — there is nothing here that a clinic
needs to visit or download by hand.

## What is here

| File | Purpose |
|---|---|
| `update.json` | The current release, and the earlier ones still considered good. |
| Releases | Each version's `UC_PACS_<version>_Setup.exe`, attached to its tag. |

## `update.json`

```jsonc
{
  "version": "2026.8.17",            // the current release
  "download_url": "...",             // its installer
  "checksum": "sha256:...",          // verified after download; a mismatch is discarded
  "release_notes": "...",            // shown to doctors and staff — plain English
  "stable": true,                    // clear this to withdraw a bad release
  "schema": "1.0",                   // database structure marker; guards going backwards
  "history": []                      // earlier releases, newest first
}
```

Two of these do real work and are easy to get wrong:

- **`stable`** — set it to `false` to withdraw a release. Anyone who has not
  taken it stops being offered it, and it disappears from the "install a
  different version" list. The entry is **kept**, so a clinic already running
  it can still be told what it is. Withdrawing beats deleting.
- **`schema`** — bump this whenever a release changes the database structure in
  a way an older version could not read back. Going backwards across a change
  in this marker is **refused**, because installing an older build does not
  undo a database migration.

`release_notes` is read by doctors and front-desk staff, not developers. Write
what changed for them, not what changed in the code.

## Source

The application itself lives in a private repository. This one exists only so
UC PACS has somewhere public to fetch from, without shipping a credential.
