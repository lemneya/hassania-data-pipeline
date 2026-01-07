# HASSANIYA CONSOLIDATION AUDIT

**Generated:** 2026-01-07
**Auditor:** Claude (Opus 4.5)
**Scope:** lemneya Hassaniya repositories + external datasets

---

## 1) Executive Decision Summary

### Should We Merge? **PARTIAL YES**

**Recommendation:** Merge `hassania-data-pipeline` INTO `hassania-qwen-finetune`, but preserve separation of raw collection sources.

### Why

| Factor | Finding | Impact |
|--------|---------|--------|
| **Overlap** | 0.01% (3 of 24,904 hashes) | Datasets are **99.99% different** — no redundancy |
| **Maturity** | qwen-finetune is production-ready (HDRP v1.0 compliant) | Clear SSOT candidate |
| **Size** | qwen-finetune: ~716K tokens vs data-pipeline: ~333 records | 200x difference |
| **Schema** | Incompatible (flat vs nested segments) | Requires conversion |
| **Unique Value** | data-pipeline has 6 curated raw sources not in qwen-finetune | Worth preserving |

### Top 5 Risks + Mitigations

| # | Risk | Severity | Mitigation |
|---|------|----------|------------|
| 1 | **License Ambiguity** | HIGH | qwen-finetune claims MIT but sources (Peace Corps, Facebook) have unclear redistribution rights. Must audit each source. |
| 2 | **Schema Incompatibility** | MEDIUM | data-pipeline's flat schema needs conversion to HDRP Episode format before merge. |
| 3 | **PII in Legacy Import** | LOW | 52 "phone numbers" detected are false positives (URL article IDs). Verified safe. |
| 4 | **Hardcoded Paths** | LOW | Both repos have `/home/ubuntu/` or `/tmp/` paths. Fix before merge. |
| 5 | **Eval Contamination** | LOW | qwen-finetune has hash-based leakage checks (0 overlap verified in manifest). |

---

## 2) Repository/Data Inventory

### 2.1 GitHub Repositories (lemneya)

| Attribute | hassania-data-pipeline | hassania-qwen-finetune | Jais |
|-----------|------------------------|------------------------|------|
| **Link** | github.com/lemneya/hassania-data-pipeline | github.com/lemneya/hassania-qwen-finetune | github.com/lemneya/Jais |
| **Visibility** | Public | Public | Public |
| **Primary Purpose** | Raw data collection | HDRP refinery + train-ready exports | Fine-tuning script only |
| **Size on Disk** | 1.4 MB | 110 MB | 123 KB |
| **Key Folders** | `data/raw/`, `data/processed/`, `scripts/` | `hdrp/data/processed/`, `hdrp/pipeline/` | Single `.py` file |
| **Formats** | JSON, JSONL, CSV, PDF | JSONL (episodes, exports) | N/A |
| **Records** | 333 episodes | 28,612 episodes / 57,224 segments | N/A |
| **Estimated Tokens** | ~8,000 | ~716,000 | N/A |
| **Source Types** | Peace Corps, YouTube, Voursa, Facebook Marketplace, mo3jam, Omniglot | Legacy import (origin unclear) | N/A |
| **Schema Version** | Flat (6 fields) | HDRP v1.0 (nested segments) | N/A |
| **License Status** | UNCLEAR (claims "publicly available") | MIT (but source licenses unclear) | Implied MIT |
| **HEAD Commit** | `cddb4dd4` | `ab3b8988` | N/A |

### 2.2 Schema Comparison

**hassania-data-pipeline (Flat):**
```json
{
  "english": "string",
  "hassaniya_ar": "string",
  "hassaniya_en": "string",
  "bucket": "everyday_chat|marketplace_qa|public_comments",
  "source": "string",
  "context": "string"
}
```

**hassania-qwen-finetune (HDRP Episode):**
```json
{
  "episode_id": "EP-YYYYMMDD-XXXXXX",
  "source_type": "website|whatsapp|facebook|...",
  "source_uri": "string",
  "bucket": "everyday_chat|marketplace_qa|public_comments|tv_discussion|culture_story_poetry|parliament_politics",
  "interaction_mode": "dialogue|qa|narrative|monologue",
  "topic": "social_family|religion|trade_econ|...",
  "heat": 0,
  "segments": [{
    "segment_id": "SEG-NNN",
    "speaker": "spk1|spk2",
    "raw_text": "string",
    "text_variants": {"raw": "", "norm": "", "diac": ""},
    "lang_probs": {"hassaniya": 0.0, "msa": 0.0, "french": 0.0},
    "dqs": 0,
    "decision": "ACCEPT|REVIEW|REJECT|PENDING"
  }]
}
```

### 2.3 External Datasets

| Dataset | Link | Size | Format | License | Hassaniya Coverage |
|---------|------|------|--------|---------|-------------------|
| **Mendeley HASSANIYA** | data.mendeley.com/datasets/m2swkr2bhx/1 | 2,000 records | CSV/JSON | CC BY 4.0 (assumed) | 100% (sentiment labels) |
| **ADI-20** | arxiv.org/abs/2511.10070 | 3,556 hours audio | Audio + transcripts | Research license | Includes Mauritania |
| **ADI-17** | github.com/swshon/arabic-dialect-identification | ~3,000 hours | YouTube IDs | Research | Includes Mauritania |
| **DAH (HuggingFace)** | huggingface.co/datasets/hassan-IA/dah | Unknown | JSONL | Unknown | Referenced in README but inaccessible |

---

## 3) Uniqueness & Overlap Analysis

### 3.1 Methodology

```
1. Extract all text fields from both repositories
2. Normalize: lowercase, strip Arabic diacritics (tashkeel), collapse whitespace
3. Hash: SHA256 (first 16 chars) of normalized text
4. Filter: Exclude texts < 5 characters
5. Compute: Set intersection, Jaccard similarity
```

### 3.2 Overlap Matrix

| Metric | Value |
|--------|-------|
| **data-pipeline unique hashes** | 610 |
| **qwen-finetune unique hashes** | 24,297 |
| **Exact duplicates (intersection)** | 3 |
| **Union (total unique)** | 24,904 |
| **Jaccard Similarity** | 0.0001 (0.01%) |
| **% data-pipeline in qwen-finetune** | 0.5% |
| **% qwen-finetune in data-pipeline** | 0.0% |

### 3.3 Source Type Breakdown

**hassania-data-pipeline sources:**
| Source | Records | % |
|--------|---------|---|
| peace_corps_mauritania | 138 | 41.4% |
| voursa_marketplace | 56 | 16.8% |
| youtube_language_beat | 51 | 15.3% |
| facebook_marketplace_nouakchott | 44 | 13.2% |
| mo3jam_dictionary | 26 | 7.8% |
| omniglot | 18 | 5.4% |

**hassania-qwen-finetune sources:**
| Source Type | Episodes | % |
|-------------|----------|---|
| website (legacy import) | 28,612 | 100% |

### 3.4 Bucket Comparison

| Bucket | data-pipeline | qwen-finetune (episodes) |
|--------|---------------|--------------------------|
| everyday_chat | 229 (68.8%) | 25,268 (88.3%) |
| marketplace_qa | 100 (30.0%) | 1,120 (3.9%) |
| public_comments | 4 (1.2%) | 213 (0.7%) |
| tv_discussion | 0 | 32 (0.1%) |
| culture_story_poetry | 0 | 1,964 (6.9%) |
| parliament_politics | 0 | 15 (0.1%) |

### 3.5 Unique Sources Analysis

**Unique to data-pipeline (not in qwen-finetune):**
- Peace Corps Mauritania structured lessons (138 items)
- Voursa real estate marketplace (56 items)
- YouTube Language Beat transcriptions (51 items)
- Facebook Marketplace Nouakchott (44 items)
- mo3jam dictionary (26 items)
- Omniglot reference (18 items)

**Unique to qwen-finetune:**
- 28,612 episodes from "legacy import" (origin: `train_phase2_quality.jsonl`)
- Git history shows: Peace Corps/DLIFLC materials, Hassania books/literature, enriched datasets

### 3.6 Verdict on "New Data"

**Is the newly found Hassaniya data materially different from our current collection?**

**YES — 99.99% different.** The overlap is effectively zero (3 matching hashes out of 24,904).

| What's New | Quantity | Source |
|------------|----------|--------|
| qwen-finetune legacy data | 24,294 unique text hashes | Unclear origin (legacy import) |
| data-pipeline curated sources | 607 unique text hashes | 6 documented sources |
| External: Mendeley HASSANIYA | 2,000 sentiment-labeled records | Facebook scrape |
| External: ADI-20 audio | ~180+ hours Mauritanian | YouTube |

---

## 4) Data Quality & Risk Red-Team

### 4.1 PII Risk Assessment

| Check | Result | Evidence |
|-------|--------|----------|
| **Phone Numbers** | ⚠️ 52 pattern matches | FALSE POSITIVE: All are URL article IDs (e.g., `world-africa-12345678`) or vocabulary numbering |
| **Email Addresses** | ✓ 0 found | Clean |
| **WhatsApp Timestamps** | ✓ 0 found | No private chat format detected |
| **Name Prefixes** | ✓ 0 found | No "FirstName LastName:" patterns |

**Verdict:** LOW PII RISK — No actual personal data detected.

### 4.2 Data Leakage Risk

| Check | qwen-finetune | data-pipeline |
|-------|---------------|---------------|
| Train/Eval split by episode | ✓ Yes | N/A (no eval set) |
| Hash-based verification | ✓ 0 overlapping hashes | N/A |
| Manifest documentation | ✓ `export_manifest_v2` | None |

**Verdict:** LOW LEAKAGE RISK — qwen-finetune has proper safeguards.

### 4.3 Licensing Risk

| Source | License Status | Risk Level |
|--------|---------------|------------|
| Peace Corps materials | US Government (public domain) | LOW |
| YouTube transcripts | ToS unclear for training | MEDIUM |
| Facebook Marketplace | ToS prohibits scraping | HIGH |
| Voursa listings | Commercial site, unclear | MEDIUM |
| mo3jam dictionary | User-contributed, unclear | MEDIUM |
| Omniglot | Educational use, unclear | LOW |
| qwen-finetune "legacy" | UNKNOWN ORIGIN | HIGH |

**Verdict:** HIGH LICENSE RISK — Multiple sources have unclear redistribution rights.

### 4.4 Bias Gaps

| Topic | data-pipeline | qwen-finetune |
|-------|---------------|---------------|
| everyday_chat | 68.8% | 88.3% |
| marketplace | 30.0% | 3.9% |
| religion | 0% | ~5% (estimated) |
| politics | 0% | 0.1% |
| culture/poetry | 0% | 6.9% |

**Gaps Identified:**
- data-pipeline lacks culture/poetry and politics content
- qwen-finetune is heavily skewed toward everyday_chat (SFT = 100%)
- Neither has substantial formal/professional Arabic

### 4.5 SFT Formatting Issues

| Issue | Status |
|-------|--------|
| System prompts present | ✓ All 10,668 SFT records have system role |
| Role tags consistent | ✓ system/user/assistant format |
| Multi-turn support | ✓ Structure supports it |
| Empty responses | Not checked (requires deeper audit) |

---

## 5) Merge Options

### Option A: Keep Multi-Repo + SSOT Index

```
lemneya/
├── hassaniya-index/          # NEW: Metadata-only SSOT
│   ├── catalog.json          # Links to all repos
│   └── unified_schema.md
├── hassania-data-pipeline/   # Raw collection (unchanged)
├── hassania-qwen-finetune/   # Production training (unchanged)
└── Jais/                     # Training scripts (unchanged)
```

| Pros | Cons |
|------|------|
| No migration risk | Fragmented discovery |
| Independent versioning | Schema drift |
| Clear ownership | Duplicate tooling |

**Risk:** LOW
**Effort:** LOW

### Option B: Merge into hassania-qwen-finetune (RECOMMENDED)

```
hassania-qwen-finetune/
├── hdrp/
│   ├── data/
│   │   ├── raw/
│   │   │   ├── peace_corps/        # FROM data-pipeline
│   │   │   ├── voursa/             # FROM data-pipeline
│   │   │   ├── youtube/            # FROM data-pipeline
│   │   │   ├── facebook/           # FROM data-pipeline
│   │   │   ├── mo3jam/             # FROM data-pipeline
│   │   │   └── omniglot/           # FROM data-pipeline
│   │   └── processed/              # Existing exports
│   ├── pipeline/
│   │   ├── collector/
│   │   ├── refinery/
│   │   └── finetune/               # Include Jais scripts
│   └── specs/
├── reports/                        # FROM data-pipeline
└── README.md
```

| Pros | Cons |
|------|------|
| Single SSOT | Migration effort |
| Unified pipeline | History complexity |
| All training scripts together | Need to resolve path issues |

**Risk:** MEDIUM
**Effort:** MEDIUM

**Recommended Merge Steps:**
1. Convert data-pipeline records to HDRP Episode format
2. Add to `hdrp/data/raw/` by source
3. Re-run refinery pipeline
4. Merge Jais scripts into `pipeline/finetune/`
5. Archive data-pipeline repo (read-only)

### Option C: New Monorepo hassaniya-unified

```
hassaniya-unified/
├── data/
│   ├── raw/
│   │   ├── curated/            # FROM data-pipeline
│   │   └── legacy/             # FROM qwen-finetune
│   ├── hdrp/                   # HDRP processed
│   └── external/               # Mendeley, ADI subsets
├── pipeline/
│   ├── collector/
│   ├── refinery/
│   └── training/
│       ├── qwen/
│       ├── jais/
│       └── openai/
├── reports/
├── specs/
└── docs/
```

| Pros | Cons |
|------|------|
| Clean slate | Highest effort |
| Proper separation | Lose git history |
| Extensible | Requires full re-validation |

**Risk:** HIGH
**Effort:** HIGH

### Recommendation: **Option B**

`hassania-qwen-finetune` is already HDRP-compliant with a mature pipeline. Adding the curated raw sources from `data-pipeline` increases coverage while maintaining the existing infrastructure.

---

## 6) GO/NO-GO Gate Checklist

### Gate: Hassaniya Data Consolidation

| # | Deliverable | Acceptance Criteria | Evidence Required | Status |
|---|-------------|---------------------|-------------------|--------|
| G1 | License Audit | All sources have documented license status | License file or ToS link per source | NO-GO |
| G2 | PII Scrub | Zero actual PII in merged dataset | PII scan report with 0 findings | GO |
| G3 | Schema Conversion | data-pipeline records in HDRP format | Successful conversion log | NO-GO |
| G4 | Deduplication | Post-merge dedup < 1% | Hash overlap report | GO |
| G5 | Leakage Check | Train/eval hash overlap = 0 | Manifest with leakage_free=true | GO |
| G6 | Path Fixes | No hardcoded absolute paths | grep scan = 0 matches | NO-GO |
| G7 | CI/CD | Pipeline runs without error | Successful CI log | NO-GO |

### GO/NO-GO Rule

**MERGE IS NO-GO** until:
1. G1 (License Audit) is resolved — document each source's redistribution rights
2. G3 (Schema Conversion) is completed — write converter script
3. G6 (Path Fixes) is resolved — use relative paths or env vars

### Current Status: **NO-GO (3 blockers)**

---

## 7) Final Answer

### Is the newly found Hassaniya data materially different from our current collection?

**YES.**

| Dataset | Unique Hashes | Truly New | Evidence |
|---------|---------------|-----------|----------|
| hassania-data-pipeline | 610 | 607 (99.5%) | Jaccard = 0.0001 |
| hassania-qwen-finetune | 24,297 | 24,294 (99.99%) | Only 3 hash matches |
| Mendeley HASSANIYA | 2,000 | Unknown (not downloaded) | External |
| ADI-20 Mauritanian | ~180 hours | Unknown (audio) | External |

### What is new and how much?

1. **qwen-finetune legacy data:** 24,294 unique text segments (~716K tokens) — ORIGIN UNCLEAR
2. **data-pipeline curated sources:** 607 unique texts from 6 documented public sources — CLEAR PROVENANCE
3. **Mendeley HASSANIYA:** 2,000 sentiment-labeled Facebook comments — EXTERNAL, DOWNLOADABLE
4. **ADI-20:** ~180+ hours Mauritanian Arabic audio — EXTERNAL, RESEARCH LICENSE

### Consolidation Path

1. **Immediate:** Fix G6 (hardcoded paths) in both repos
2. **Short-term:** Complete G1 (license audit) and G3 (schema conversion)
3. **Medium-term:** Execute Option B merge into `hassania-qwen-finetune`
4. **Optional:** Evaluate Mendeley and ADI-20 for supplementary data

---

*Audit completed. No files modified. No PRs opened.*
