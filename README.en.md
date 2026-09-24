# MAAD: Cross-chain Message Acceptance Event Evidence Dataset

[中文](README.md) · v0.2.0 · Search cutoff: 2026-09-22 · MIT

MAAD curates public reports about cross-chain requests accepted with incorrect source semantics, compromised signing trust, invalid or misbound proofs, replayed authorization, or excessive destination execution authority. Valid signatures and real source transactions do not necessarily imply valid business authorization.

**This release is an event evidence catalog, not a message-level ground-truth benchmark.**

<!-- MAAD:STATS:BEGIN -->
Core acceptance events: **31** (malicious: 31; whitehat: 0).

Catalog: 206 records; 41 report-supported records; 33 unresolved groups.

Evidence: 44 registered sources; 84 claims; 15 unique report transaction links; 46 logged searches.

Access status: {"partial_text_read": 42, "search_snippet_only": 1, "unavailable": 1}.

Preserved v0.1.0 layer: 61 observed transactions, 24 message candidates, 22 acceptance observations, 0 semantically verified messages, 0 strict benchmark records.
<!-- MAAD:STATS:END -->

Start with [events.csv](event-evidence/data/events.csv), [sources.csv](event-evidence/data/sources.csv), [claims.csv](event-evidence/data/claims.csv) and [transaction_links.csv](event-evidence/data/transaction_links.csv). JSONL counterparts retain nulls and array types. [Statistics](event-evidence/statistics.json), [data card](DATA_CARD.md), [dictionary](event-evidence/DATA_DICTIONARY.md) and [quality issues](event-evidence/quality_issues.json) describe limitations.

Core inclusion requires a resolved event, in-scope acceptance, report-supported review, malicious/whitehat event nature, no blocking conflict, and a supporting mechanism claim from a directly read report. Use `core_event_ids` from statistics. Scope alone is not sufficient. Ronin 2024 has a supported mechanism but unknown actor nature in this release; returned funds alone are not used to infer whitehat intent.

The catalog covers observation, attestation, verification, acceptance and execution. It includes application-level integration failures, not just bridge cryptography. Disclosure-only vulnerabilities, failed attempts, operational disruptions and local approval drains are separated. Unknown candidates are not negative examples. Many inherited names/dates/chains remain unverified; inspect `review_status`, `date_basis`, claims and limitations.

## Reproduce

Python 3.10+, standard library only:

```sh
python -B event-evidence/build.py
python -B -m unittest discover -s event-evidence -p 'test_*.py'
python -B event-evidence/validate.py
python -B -m unittest discover -s message-acceptance -p 'test_*.py'
python -B message-acceptance/validate.py
python -B -m unittest discover -s release -p 'test_*.py'
python -B release/package.py
```

Edit only `event-evidence/curation/*.jsonl` for evidence annotations. Exports and statistics are deterministic. The original message layer remains at v0.1.0 and its manifest must not be interpreted as v0.2.0 event counts. Frozen input hashes protect inherited observations and labels.

Before training a detector, reconstruct source/destination receipts, logs, traces, identifiers and historical verification configurations. Split by incident/campaign; never randomly split transactions from the same attack. Reports are not substitutes for message truth. There has been no dual-annotator fact-labeling exercise or complete on-chain replay. Most events have fewer than two independently established evidence roots. Search coverage is purposive and non-exhaustive; annual counts are not industry incidence rates.

Upstream: [Justin Zhou's Cross-chain-anomaly-event-dataset](https://github.com/justinzjj/Cross-chain-anomaly-event-dataset), revision `bdb0893c51cca7ce2dee52f8228b90bab4d0f290`. Original attribution is retained. MIT applies to the distributed dataset annotations/code under the retained notices; external reports retain their own copyright. See [NOTICE](NOTICE.md) and [CITATION.cff](CITATION.cff).
