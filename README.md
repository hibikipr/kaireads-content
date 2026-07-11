# Kai Reads — Game Content

This repo holds the live game content for the **Kai Reads** iPad app. The app
fetches `kai-content.json` from this repo's raw URL on launch, caches it, and
falls back to its bundled copy when offline.

**To add or change content: edit `kai-content.json` right here on GitHub and
bump `contentVersion`.** The app only applies a pack whose `contentVersion` is
higher than what it already has.

## Editing rules

- `schemaVersion` stays `1` — don't change it (the app rejects unknown schemas).
- `contentVersion` must **increase** with every edit, or the app ignores the update.
- **phonicsWords** — short words for sounding out; `emoji` is the picture on the asteroid.
- **builderWords** — `tiles` must contain **exactly the letters of `word`**, in any
  (scrambled) order, one letter per tile. `["s","g","o","h","t"]` for `"ghost"`. The
  app rejects the whole file if tiles don't spell the word.
- **sightRounds** — `options` must include `target` (plus decoys).
- **storyPages** — each page is one sentence plus a big emoji.

If the file has a mistake (bad JSON, tiles that don't spell the word, a missing
target), the app silently keeps its previous content — nothing breaks, the edit
just doesn't take effect. Validate your JSON before committing if in doubt.
