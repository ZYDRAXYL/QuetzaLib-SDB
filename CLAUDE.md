# CLAUDE.md

Guidance for Claude Code working in **QuetzaLib-SDB**.

## What this repo is

The published snapshot of QuetzaLib's local SQLite schema (`schema/`) and of the
`.zip` backup-archive format (`backup-format/`). It holds **no application
code** and nothing here is loaded by a running app.

## The one rule that matters here

**The schema is authored in APP, not here.** `lib/services/database_service.dart`
in [QuetzaLib-APP](https://github.com/ZYDRAXYL/QuetzaLib-APP) holds the real
`CREATE TABLE` statements, the `_dbVersion` counter, and the `onUpgrade`
migration ladder. This repo is downstream of that.

So:

- A schema change starts in **APP**. Adding a column here changes nothing and
  will be overwritten the moment the generator exists.
- The `backup-format/` spec describes what `lib/services/backup_service.dart`
  actually writes. If the two disagree, APP is right and this is stale.
- Nothing in this repo may point back at APP as a dependency. APP is the root of
  the chain graph; an edge into it makes propagation loop.

## Status

The chain wiring is in place; the artifacts are **not generated yet**. The
`generated-schema` handler in `tools/chain-propagate.mjs` reports that and skips
rather than writing an empty change. Do not fill `schema/` in by hand to make it
look finished — write the extractor in APP first.

## Mirrored files — do not edit here

`chain/chain.json`, `tools/chain-*.mjs`, and everything under `.claude/` are
**generated output**, mirrored from APP by `tools/mirror-claude.mjs`. Each
carries a header saying so. A local edit is lost on the next mirror.

To change one: edit it in `QuetzaLib-APP/.claude/` (or `chain/`, or `tools/`),
then run `node tools/mirror-claude.mjs` from APP.

## Useful commands

```bash
node tools/chain-lib.mjs      # this repo's upstream/downstream edges
node tools/chain-survey.mjs   # what moved in the other repos since last look
```

## Releases

`sdb-vX.Y.Z`, tracked by `schema/version.json#schemaVersion`. Its own namespace,
independent of APP's `v*` and EXE's `exe-v*`.
