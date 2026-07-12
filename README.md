# Kai Reads — Game Content

This repo holds the live game content for the **Kai Reads** iPhone & iPad app.
The app fetches `kai-content.json` from this repo's raw URL on launch, caches
it, and falls back to its bundled copy when offline.

**To add or change content: open a pull request editing `kai-content.json`
and bump `contentVersion`.** Direct pushes to `main` are blocked by a repo
ruleset — GitHub's web editor will offer to create a branch + PR for you when
you edit the file, which works fine for a single-file change like this. The
app only applies a pack whose `contentVersion` is higher than what it already
has.

## Daily word sets

The app doesn't show the whole list every session — each of the three word
games (phonics, builder, sight) picks **up to 5 words per day** from whatever
is in this file, via a deterministic day-seeded shuffle on the app side (same
set all day, new set tomorrow). So adding more words here increases variety
across days rather than making any single session longer. See
`DailyWords.swift` in the app repo if you want the exact mechanics.

## Current content

The word lists cover every word on the **Dolch pre-primer list** (the
standard 40-word sight-word set for pre-K/K readers), split across the three
games by how decodable each word is: simple CVC-style words (`cat`, `big`,
`up`) go to phonics, words with blends/silent-e/vowel teams (`play`, `make`,
`three`) go to word-builder, and irregular high-frequency words (`said`,
`one`, `two`) go to sight words.

## Editing rules

- `schemaVersion` stays `1` — don't change it (the app rejects unknown schemas).
- `contentVersion` must **increase** with every edit, or the app ignores the update.
- **phonicsWords** — short, decodable words for sounding out; `emoji` is the picture on the asteroid.
- **builderWords** — `tiles` must contain **exactly the letters of `word`**, in any
  (scrambled) order, one letter per tile. `["s","g","o","h","t"]` for `"ghost"`. The
  app rejects the whole file if tiles don't spell the word. Keep words to 5
  letters or fewer — that's the longest the tile row is laid out for.
- **sightRounds** — `options` must include `target` (plus 2 decoys, ideally visually similar
  to the target, e.g. `here`/`hear`, `go`/`got`).
- **storyPages** — each page is one sentence plus a big emoji.
- **letterSounds** *(optional)* — per-letter pronunciation overrides for the
  phonics letter taps, e.g. `"letterSounds": { "e": "ebb" }`. Keys must be a
  single **lowercase** letter; values are what the speech synthesizer speaks.
  Use this to fix a mispronounced letter without waiting for an app update —
  overrides win over the app's built-in sounds. Omit the section entirely to
  use the defaults.

If the file has a mistake (bad JSON, tiles that don't spell the word, a missing
target), the app silently keeps its previous content — nothing breaks, the edit
just doesn't take effect. Validate your JSON before committing if in doubt.
