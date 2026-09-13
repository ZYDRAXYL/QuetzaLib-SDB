# QuetzaLib-SDB

Schema repository for **QuetzaLib** — the published snapshot of the app's local
SQLite schema and of the backup-archive format, kept as a versioned reference
separate from the app that defines them.

The app itself lives in [QuetzaLib-APP](https://github.com/ZYDRAXYL/QuetzaLib-APP).

## What this repo is for

QuetzaLib stores everything on-device in SQLite. The schema is **authored in
Dart**, in `lib/services/database_service.dart` in the APP repo — the
`CREATE TABLE` statements, the `_dbVersion` counter, and the `onUpgrade`
migration ladder all live there. This repo publishes a readable, taggable
snapshot of that, so a schema version can be cited, diffed, and pointed at
without reading Dart.

That direction matters: **APP is upstream of SDB, not the other way round.**
Nothing here is the source of truth for a running app.

```
schema/         SQLite schema snapshot + migration history
backup-format/  the .zip backup archive layout that backup_service.dart writes
```

## Status

The chain wiring is in place; the schema artifacts are **not generated yet**.
`node tools/chain-propagate.mjs` in APP reports the `generated-schema` edge as
unimplemented and skips it, rather than writing an empty change. Populating it
means writing the extractor in APP first — not hand-writing files here, which
would immediately make them the kind of hand-edited "generated" file the chain
contract forbids.

## Releases

`sdb-vX.Y.Z`. Its own namespace, independent of APP's `v*` and EXE's `exe-v*` —
they share WEB's release mirror, so a bare "latest release" means nothing across
products.

## The chain

This repo is part of QuetzaLib's six-repository architecture. `chain/chain.json`
is the contract; `tools/` and `.claude/` are mirrored from APP and must not be
edited here.

```bash
node tools/chain-lib.mjs      # this repo's place in the chain
node tools/chain-survey.mjs   # what moved in the other repos
```

Full write-up: [`chain/README.md` in QuetzaLib-APP](https://github.com/ZYDRAXYL/QuetzaLib-APP/blob/main/chain/README.md).
