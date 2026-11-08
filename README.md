# XYTOS record

Every night, XYTOS publishes a fingerprint of each file its results come from. It publishes only the fingerprints, never the files. XYTOS is a paper-trading and investing system: it trades pretend money only.

A fingerprint is a SHA-256 hash. If even one character of a file changes, its fingerprint changes completely.

## What each night's file holds

`days/YYYY/MM/YYYYMMDD-HHMMSS.json` holds one entry per results file:
- `sha256` and `bytes`: the file's fingerprint and length, as it stood in that night's backup;
- `lines`: its number of rows.

The files covered are every strategy's closed trades (`trades_*.jsonl`), the funds' value and rebalance logs, the daily marks, the interest ledger, and the logs of the readiness test.

The night's file also carries `prev`: the SHA-256 of the previous night's file, byte for byte. Together they form a chain, so changing an old night breaks every night after it.

## How to check a later copy of the files

The results files are only ever appended to. So the first `bytes` bytes of a file must still give the published `sha256`, on any later day:

    head -c <bytes> trades_ch44.jsonl | shasum -a 256

To check the chain, compare the previous night's file with this night's `prev`:

    shasum -a 256 days/2026/11/<previous night>.json

## What it does not prove

- **That a row was right when it was written.** It proves only that the row has not changed since.
- **When a night was published.** The time comes from this repository's push history on GitHub, not from the clock of the machine that made it.

## History cannot be rewritten

Since 8 Nov 2026 GitHub blocks force pushes to and deletion of `main` in this repository, for its owner too. Nights can only be added.
