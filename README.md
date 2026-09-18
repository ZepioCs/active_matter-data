# active_matter-data

Curated game data for the Active Matter Field Archive wiki. Single source of
truth for all record data — edit here, the website picks it up, no redeploy.

## Files (`data/`)

| File | Contents |
|------|----------|
| `catalog.json` | All records (weapons, mods, equipment, anomalies, locations). `schemaVersion: 1`. |
| `attachments.json` | Weapon mod slots + per-mod effects/compatibility for loadout math. |
| `ammo.json` | Calibers with game-files damage model (`hitPowerMult`, falloff). |
| `compare-config.json` | Compare page sections and target HP. |
| `damage.json` | Body-part HP and damage notes. |
| `image-credits.json` | Image provenance. |

Entry ids are stable and append-only: never rename an id, never reuse one.

## Images (`images/`)

Canonical image store, mirrored from the wiki's `public/images/`. Catalog
`image` fields stay site-relative (`/images/<file>`); the wiki serves its
bundled copy. New artwork (e.g. rendered mod thumbnails) lands here first,
then syncs to the wiki. Filenames are case-sensitive and must match the
catalog exactly.

## How the wiki consumes this

Production data URL (jsDelivr CDN, CORS-enabled):

```
https://cdn.jsdelivr.net/gh/ZepioCs/active_matter-data@main/data/catalog.json
```

The wiki defaults to this base and falls back to its bundled copy when the
CDN is unreachable. Override locally with `VITE_DATA_BASE_URL` (see the
wiki repo `.env.example`).

## Updating

1. Edit the JSON here (keep `schemaVersion`, keep ids stable).
2. Copy the changed files into the wiki repo's `public/` as the
   offline fallback:
   `Copy-Item data\*.json <wiki>\public\data\ -Force`
   `Copy-Item images\* <wiki>\public\images\ -Recurse -Force`
3. The wiki's `bun test` validates cross-references (attachment weapons,
   short codes, ammo calibers, image files) — run it before pushing.

Methodology notes live with the wiki repo (`docs/damage-calculation.md`,
`docs/mods.md`).
