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

## Two verification modes

### Mode A — Full recomputation (requires full record list)

For a given UTC date D:

1) Download anchors/D.json from this repository.
2) Obtain the full list of records created in the registry during:
   [range_start_utc, range_end_utc)
3) Recompute the anchor hash locally from that list, using the public protocol rules.
4) Compare your result to anchor_hash_sha256 in anchors/D.json.

If the hashes match, the day-state is verified.

### Mode B — Inclusion proof (does NOT require the full record list)

This mode allows a verifier to recover a verifiable date bound for a declaration
without access to the registry database or other records.

The holder of a declaration keeps a minimal client-side bundle:

- fact_hash
- created_at_utc
- leaf_hash
- leaf_index
- anchor_date_utc
- anchor_hash
- (optional) Merkle siblings when the bucket contains multiple declarations

#### Canonical leaf hash

The protocol defines the canonical leaf hash as:

leaf_hash = SHA256( fact_hash || "|" || created_at_utc )

#### Verification (offline)

1) Recompute leaf_hash from fact_hash and created_at_utc.
2) Recompute the bucket root using leaf_index and siblings (if any).
3) Verify equality with anchor_hash (from anchors/YYYY-MM-DD.json).
4) The verifiable date of the declaration is the publication date of anchor_date_utc.

This proves antériorité (existence before a date). It does not prove authorship,
ownership, accuracy, or truth.

## Notes

- This repository is a publication channel. The registry operator cannot change past commits without leaving traces.
- Multiple publication channels can be used for redundancy.
