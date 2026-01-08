# Hassaniya Data Rights Ledger

**Version:** 1.0
**Last Updated:** 2026-01-08
**Maintainer:** [Your Name]

---

## Purpose

This ledger documents the licensing and usage rights for all data sources in the Hassaniya dataset collection. Only **GREEN** status sources may be included in training exports.

## Status Definitions

| Status | Meaning | Action |
|--------|---------|--------|
| 🟢 GREEN | Clear rights for training + redistribution | Include in exports |
| 🟡 YELLOW | Unclear rights, needs verification | Quarantine until resolved |
| 🔴 RED | Prohibited or high-risk | Exclude permanently |

---

## Rights Ledger

### hassania-data-pipeline Sources

| # | Source Name | Origin URL | Collection Method | Records | Allowed Use | Proof | Status | Action |
|---|-------------|------------|-------------------|---------|-------------|-------|--------|--------|
| 1 | **Peace Corps Mauritania** | https://files.peacecorps.gov/ | Manual download (PDF) | 138 | US Government work - Public Domain | [Link to source](https://files.peacecorps.gov/multimedia/audio/languagelessons/mauritania/MR_Hassaniya_Language_Lessons.pdf) | 🟢 GREEN | Include |
| 2 | **Voursa Marketplace** | https://voursa.mr/ | Web scrape | 56 | Commercial site - ToS unclear | None | 🟡 YELLOW | Quarantine |
| 3 | **YouTube Language Beat** | https://youtube.com/@languagebeat | Manual transcription | 51 | YouTube ToS - training unclear | None | 🟡 YELLOW | Quarantine |
| 4 | **Facebook Marketplace Nouakchott** | facebook.com/marketplace | Web scrape | 44 | ToS prohibits scraping | None | 🔴 RED | Exclude |
| 5 | **mo3jam Dictionary** | https://ar.mo3jam.com/dialect/Hassaniya | Web scrape | 26 | User-contributed - CC unclear | None | 🟡 YELLOW | Quarantine |
| 6 | **Omniglot** | https://www.omniglot.com/writing/arabic_hassaniya.htm | Manual copy | 18 | Educational - fair use? | None | 🟡 YELLOW | Quarantine |

### hassania-qwen-finetune Sources

| # | Source Name | Origin URL | Collection Method | Records | Allowed Use | Proof | Status | Action |
|---|-------------|------------|-------------------|---------|-------------|-------|--------|--------|
| 7 | **Legacy Import (train_phase2_quality)** | UNKNOWN | Unknown | 28,612 episodes | **UNKNOWN ORIGIN** | None | 🔴 RED | Quarantine until provenance established |
| 8 | **Peace Corps/DLIFLC Materials** | Various | Unknown | Unknown | US Government - likely public domain | Needs verification | 🟡 YELLOW | Verify source |
| 9 | **Hassania Books/Literature** | Unknown | Unknown | Unknown | Copyright status unknown | None | 🔴 RED | Quarantine |

### External Datasets (Not Yet Imported)

| # | Source Name | Origin URL | Collection Method | Records | Allowed Use | Proof | Status | Action |
|---|-------------|------------|-------------------|---------|-------------|-------|--------|--------|
| 10 | **Mendeley HASSANIYA** | https://data.mendeley.com/datasets/m2swkr2bhx/1 | Published dataset | 2,000 | CC BY 4.0 (assumed) | Check Mendeley page | 🟡 YELLOW | Verify license |
| 11 | **ADI-20 (Mauritanian subset)** | https://arxiv.org/abs/2511.10070 | Research dataset | ~180 hrs | Research license | Paper citation | 🟡 YELLOW | Contact authors |
| 12 | **ADI-17 (Mauritanian subset)** | https://github.com/swshon/arabic-dialect-identification | YouTube IDs | Unknown | Research only | GitHub repo | 🟡 YELLOW | Research use only |

---

## Summary Statistics

| Status | Count | % of Sources |
|--------|-------|--------------|
| 🟢 GREEN | 1 | 8% |
| 🟡 YELLOW | 8 | 67% |
| 🔴 RED | 3 | 25% |

**Current exportable data:** Only Peace Corps Mauritania (138 records) is confirmed GREEN.

---

## Verification Process

### To move YELLOW → GREEN:

1. **Commercial websites (Voursa, mo3jam):**
   - Email site owner requesting permission for AI training use
   - Document response with screenshot/email
   - If no response after 30 days, mark RED

2. **YouTube content:**
   - Check if creator has explicit license (CC-BY, etc.)
   - Contact creator for permission
   - Consider fair use analysis for educational content

3. **User-contributed content:**
   - Check platform ToS for user content licensing
   - Verify if users granted redistribution rights

4. **Government materials:**
   - Verify publication is official US Government work
   - Check for any copyright notices
   - US Gov works are public domain

5. **Research datasets:**
   - Check paper/repo for explicit license
   - Contact authors if unclear
   - Note any restrictions (research-only, non-commercial)

### To move RED → Excluded:

- Remove from all training exports
- Keep metadata only for audit trail
- Document reason for exclusion

---

## Action Items

- [ ] **PRIORITY 1:** Trace origin of legacy import (28,612 episodes) — this is 97% of qwen-finetune data
- [ ] **PRIORITY 2:** Email Voursa.mr for scraping permission
- [ ] **PRIORITY 3:** Check Mendeley HASSANIYA license page
- [ ] **PRIORITY 4:** Contact ADI-20 authors for Mauritanian subset access
- [ ] **PRIORITY 5:** Review mo3jam ToS for user content licensing

---

## Changelog

| Date | Change | By |
|------|--------|----|
| 2026-01-08 | Initial ledger created with 12 sources | Claude |

---

## Notes

### On "Legacy Import" Risk

The `train_phase2_quality.jsonl` file in qwen-finetune contains 28,612 episodes but has `source_uri: "legacy://..."` with no clear provenance. This is the **highest risk item** because:

1. It's 97% of the qwen-finetune data
2. Origin is undocumented
3. May contain copyrighted/scraped content
4. Cannot be exported until provenance is established

**Recommendation:** Trace git history to find original source, or quarantine entire legacy dataset.

### On Facebook Data

Facebook's ToS explicitly prohibits scraping. The 44 Facebook Marketplace records should be:
1. Removed from any public exports
2. Replaced with legitimately-obtained alternatives
3. Or: obtain explicit written permission from Facebook (unlikely)

---

*This ledger is the authoritative source for data rights decisions. Update before any export or training run.*
