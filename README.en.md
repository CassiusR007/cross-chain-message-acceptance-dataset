# MAAD: Cross-chain Message Acceptance Anomaly Evidence Dataset

[中文](README.md) · v0.2.0 · Search cutoff: 2026-09-22 · MIT

**[Download the complete v0.2.0 dataset](MAAD-v0.2.0.zip?raw=true)**

This release is distributed as a complete ZIP archive. Extract it to access all tables, provenance, preserved evidence, scripts and documentation. Relative links in the archived README resolve within the extracted directory.

The dataset organizes public security reports by cross-chain message lifecycle and verification semantics. It contains 31 core report-supported malicious acceptance events, 206 catalog records, 44 registered sources, 84 claims, 15 report-linked transaction references and 46 search records. Catalog entries and unresolved groups are not independent attack counts.

This is an event evidence collection, not a completed message-level machine-learning benchmark. The preserved message layer has 61 on-chain transaction observations, 24 message candidates, 22 acceptance observations, zero semantically verified messages and zero strict benchmark samples. Unresolved and out-of-scope entries must not be used as normal negative examples. Report-linked transactions have not all been independently verified on-chain. Source independence remains unknown unless established.

Use `event-evidence/data/` for CSV and JSONL tables. `event-evidence/statistics.json` lists `core_event_ids`; the dictionary, quality issues and archived README explain scope, fields and limitations. Split future training/test data by incident or campaign, not randomly across transactions from the same attack.

After extracting the archive:

```console
python event-evidence/validate.py
python message-acceptance/validate.py
python -m unittest discover -s event-evidence -p "test_*.py"
```

The archive contains 324 files: 323 manifest-listed content files and the manifest itself. Anonymous public download was verified against the local release byte-for-byte. See [verification.json](verification.json), [DATA_CARD.md](DATA_CARD.md), [NOTICE.md](NOTICE.md), [CHANGELOG.md](CHANGELOG.md) and [CITATION.cff](CITATION.cff). Original third-party report copyrights remain with their authors; inherited MIT attribution is preserved.
