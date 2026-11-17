English | [简体中文](./README.zh.md)

# Arietta Game Pack Index

The default browse & download index for [Arietta](https://github.com/PaperKito/Arietta) game packs, hosted on GitHub Pages:

```
https://arietta-gamepacks.github.io/index/index.json
```

- **For users**: the landing page at <https://arietta-gamepacks.github.io/index/> lists every published pack with a direct zip download. Inside Arietta (`Settings → Game packs`), the same index powers the pack list — install, update — automatically.
- **For the host**: Arietta reads `index.json` (the address is user-configurable, so self-hosted mirrors work). The index stores metadata only, never game assets; pack zips always live in each pack repo's GitHub Release (anonymous direct-link downloads).

## index.json fields

| Field | Meaning |
| --- | --- |
| `version` | Index format version, currently `1` |
| `packs[].id` | Pack id, lowercase ascii/digits/hyphens; equals the install folder name |
| `packs[].names` | UI language tag → title |
| `packs[].author` | Author name (must match `signature.json` inside the pack) |
| `packs[].publicKey` | Author public key (SPKI base64). The host verifies the zip's `signature.json` against it and refuses mismatches |
| `packs[].version` | Pack version |
| `packs[].api` | Provider protocol version |
| `packs[].capabilities` | Capability flags, identical to the pack's `game.json` |
| `packs[].repo` | Pack source repository |
| `packs[].download` | Zip direct link (points to a Release asset) |
| `packs[].sha256` | sha256 of the zip |

## Submitting your pack

1. Organize your pack folder per the *Game Pack Development Guide* and sign it with `tools/GamePackSign` (`signature.json` must be inside the zip);
2. Zip the pack folder (`<id>-<version>.zip`, files at zip root, keep empty-directory placeholders) and upload it to your own GitHub Release;
3. Open a PR editing `index.json` in this repo with the fields above, using your own `publicKey`.

## Updating a published pack

**No PR needed.** Push a new Release to your own repo (tag `v<version>`, zip asset) — the host resolves
`releases/latest` at check time and takes the zip asset with its GitHub-computed sha256 digest.
The index's own `version`/`download`/`sha256` are the fallback for API failures, rate limits and
non-GitHub hosts. A PR is only required to onboard a pack, or to change its author/public key.

Verification chain: the index pins the author's `publicKey` (identity) → zip is downloaded
(sha256 cross-checked against the GitHub digest when available) → `signature.json` verified against
that pinned key. Any mismatch and the host refuses to install.
