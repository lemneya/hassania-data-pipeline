# EPIC Issue for hassania-qwen-finetune

**Create this issue at:** https://github.com/lemneya/hassania-qwen-finetune/issues/new

---

## Issue Title
```
EPIC: Consolidate Hassaniya datasets (Option B) — license + conversion + paths
```

## Issue Labels
```
epic, consolidation, blocking
```

## Issue Body

```markdown
# EPIC: Hassaniya Dataset Consolidation (Option B)

## Overview

Merge `hassania-data-pipeline` INTO this repo (`hassania-qwen-finetune`) as the Single Source of Truth (SSOT) for all Hassaniya dialect training data.

**Audit Reference:** lemneya/hassania-data-pipeline#PR (link to merged audit PR)

## Why This Matters

| Current State | Target State |
|---------------|--------------|
| 3 fragmented repos | 1 SSOT repo |
| 99.99% unique data across repos | Unified, deduplicated dataset |
| Unclear licensing | Rights Ledger with GREEN/YELLOW/RED status |
| ~724K total tokens scattered | ~724K tokens in one place |

## NO-GO Blockers (must resolve before merge)

### GATE-1: License Audit (Rights Ledger) — HIGH PRIORITY ❌

**Why:** "MIT repo" + "unclear source rights" = legal/commercial landmine

**Deliverable:** `/docs/RIGHTS_LEDGER.md`

**Acceptance Criteria:**
- [ ] Every source has a row in the ledger
- [ ] Each row includes: source name, URL, collection method, allowed use, proof, status
- [ ] Status is GREEN/YELLOW/RED
- [ ] Only GREEN content can go into `exports/`
- [ ] YELLOW/RED is quarantined or excluded

**Sources to audit:**
| Source | Repo | Priority |
|--------|------|----------|
| Peace Corps Mauritania | data-pipeline | HIGH |
| Voursa Marketplace | data-pipeline | HIGH |
| YouTube Language Beat | data-pipeline | MEDIUM |
| Facebook Marketplace | data-pipeline | HIGH (ToS risk) |
| mo3jam Dictionary | data-pipeline | MEDIUM |
| Omniglot | data-pipeline | LOW |
| Legacy import (train_phase2_quality) | qwen-finetune | CRITICAL |

---

### GATE-2: Schema Conversion — MEDIUM PRIORITY ❌

**Why:** data-pipeline uses flat schema, qwen-finetune uses HDRP v1.0 Episode schema

**Deliverable:** Converter script + import bundle

**Acceptance Criteria:**
- [ ] Script: `scripts/convert_flat_to_hdrp.py`
- [ ] Input: flat JSONL (6 fields)
- [ ] Output: HDRP Episode JSONL (nested segments)
- [ ] 100% deterministic conversion
- [ ] `input_records = output_episodes + dropped_records`
- [ ] Dropped records have explicit reasons in manifest
- [ ] Output bundle: `imports/data-pipeline_YYYYMMDD/`
  - `episodes.jsonl`
  - `manifest.json` (source tags + rights status)
  - `conversion_report.md`

---

### GATE-3: Hardcoded Paths — LOW PRIORITY ❌

**Why:** Both repos have `/home/ubuntu/`, `/tmp/`, or Mac-specific paths

**Deliverable:** Portable path handling

**Acceptance Criteria:**
- [ ] Zero hardcoded absolute paths (grep scan = 0)
- [ ] ENV overrides: `DATA_DIR`, `RAW_DIR`, `OUT_DIR`
- [ ] `scripts/run_pipeline.sh` works on fresh clone
- [ ] Tested on Mac and Linux
- [ ] "fresh clone → one command → success"

---

## External Datasets to Evaluate

| Dataset | Link | Size | Format | License |
|---------|------|------|--------|---------|
| Mendeley HASSANIYA | https://data.mendeley.com/datasets/m2swkr2bhx/1 | 2,000 records | CSV | CC BY 4.0? |
| ADI-20 | https://arxiv.org/abs/2511.10070 | 3,556 hrs audio | Audio | Research |
| ADI-17 | https://github.com/swshon/arabic-dialect-identification | ~3,000 hrs | YouTube IDs | Research |

---

## Target Architecture (post-merge)

```
hassania-qwen-finetune/          # SSOT
├── hdrp/
│   ├── data/
│   │   ├── raw/                 # Metadata + manifests only
│   │   │   ├── peace_corps/
│   │   │   ├── voursa/
│   │   │   ├── youtube/
│   │   │   └── ...
│   │   └── processed/
│   │       ├── episodes/
│   │       ├── dqs/
│   │       └── exports/         # Only GREEN-licensed data
│   ├── pipeline/
│   │   ├── collector/
│   │   ├── refinery/
│   │   └── finetune/
│   └── specs/
├── docs/
│   └── RIGHTS_LEDGER.md         # NEW
├── imports/                      # NEW
│   └── data-pipeline_YYYYMMDD/
└── reports/
```

**Key principle:** Raw scraped files stay in private storage (bucket or local), NOT in this repo. Only metadata, manifests, and hashes live here.

---

## Definition of Done

- [ ] GATE-1 complete: Rights Ledger with all sources classified
- [ ] GATE-2 complete: Schema converter tested and validated
- [ ] GATE-3 complete: Portable paths, works on fresh clone
- [ ] data-pipeline content imported via `imports/` bundle
- [ ] Refinery pipeline re-run with new data
- [ ] Leakage check passes (0 train/eval overlap)
- [ ] `hassania-data-pipeline` archived (read-only)
- [ ] README updated with consolidation notes

---

## Related Links

- [ ] Audit PR: lemneya/hassania-data-pipeline#XX
- [ ] Rights Ledger: /docs/RIGHTS_LEDGER.md
- [ ] Schema spec: /hdrp/docs/README.md
```
