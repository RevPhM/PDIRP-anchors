# PDIRP daily anchors — how to verify

This repository publishes one JSON file per day:

anchors/YYYY-MM-DD.json

Each file contains:
- anchor_date_utc
- anchor_hash_sha256
- records_count
- range_start_utc / range_end_utc
- created_at_utc

## What this proves

It proves that the global state of the registry for that UTC day was committed publicly.
It does NOT publish individual record hashes.

## Verification logic (independent)

For a given UTC date D:

1) Download anchors/D.json from this repository.
2) Obtain the full list of records created in the registry during:
   [range_start_utc, range_end_utc)
3) Recompute the anchor hash locally from that list, using the public protocol rules.
4) Compare your result to anchor_hash_sha256 in anchors/D.json.

If the hashes match, the day-state is verified.

## Notes

- This repository is a publication channel. The registry operator cannot change past commits without leaving traces.
- Multiple publication channels can be used for redundancy.
