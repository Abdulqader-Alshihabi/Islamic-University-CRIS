<div align="center">

# Islamic University CRIS
### Comprehensive Research Information System & Institutional Intelligence Platform

[![Live Deployment](https://img.shields.io/badge/Live%20Platform-iu--cris--system.vercel.app-006C35?style=for-the-badge&logo=vercel&logoColor=white)](https://iu-cris-system.vercel.app/)
[![Capstone Project](https://img.shields.io/badge/B.Sc.%20Capstone-Computer%20Science-1E3A8A?style=for-the-badge)](https://iu-cris-system.vercel.app/)
[![Verified Publications](https://img.shields.io/badge/Verified%20Corpus-7%2C359%20Papers-047857?style=for-the-badge)](#-live-platform-showcase)
[![Reconciled Citations](https://img.shields.io/badge/Citations%20Tracked-71%2C567-D97706?style=for-the-badge)](#-the-core-problem--why-naive-systems-fail)
[![Global Journals](https://img.shields.io/badge/Indexed%20Journals-49%2C654-0284C7?style=for-the-badge)](#-global-journal-discovery--arabicsharia-intelligence)
[![Forecast R2](https://img.shields.io/badge/Forecast%20Accuracy-R%C2%B2%20%3D%200.98-7C3AED?style=for-the-badge)](#-hierarchical-predictive-forecasting-engine)

[**Explore Live System**](https://iu-cris-system.vercel.app/) · [**The Problem**](#-the-core-problem--why-naive-systems-fail) · [**Architecture**](#-system-architecture) · [**ETL & Forensics**](#-multi-source-etl--identity-resolution) · [**Scalability**](#-engineered-for-seamless-scalability) · [**Showcase**](#-live-platform-showcase)

</div>

> [!IMPORTANT]
> **Repository Scope & Confidentiality**  
> This repository hosts the complete architectural documentation, engineering blueprints, and system showcase for our Computer Science graduation capstone (**CRIS**) built for the **Islamic University of Madinah**. In adherence to institutional data governance and server credential policies, proprietary source files and database seeds remain private, while the live deployment is accessible at **[https://iu-cris-system.vercel.app/](https://iu-cris-system.vercel.app/)**.

---

## Where Does a University's Research Actually Live?

At the Islamic University of Madinah, research lived in several places at once: international bibliographic databases and disconnected internal platforms, each holding its own records, often reconciled by hand. The same paper could appear twice or disappear completely, co-authorship across faculties was ambiguous, post-publication incentive claims had to be re-verified manually over and over, and executive leadership had no unified picture of university performance.

**CRIS (Current Research Information System)** turns that fragmentation into a single, verifiable, database-enforced source of truth spanning **7,359 publications**, **71,567 citations**, **1,375 researchers**, **49,654 global journals**, **graduate theses**, **startup ventures**, and **multi-stage financial rewards**—delivered through a bilingual (**Arabic RTL / English LTR**) SSR platform with **8 database-enforced security roles**.

---

## 1. The Core Problem — Why Naive Systems Fail

If you simply pull an external API into a React table, your numbers will be **wrong**—sometimes catastrophically wrong. When unifying real bibliometric data across **Elsevier Scopus**, **Clarivate Web of Science (WoS)**, **OpenAlex**, and Arabic **ARCIF** catalogs, we encountered **six hidden real-world failure modes** and engineered deterministic database-level solutions for each:

```mermaid
flowchart TD
    Root["Fragmented University Research Ecosystem"] --> F1["1. Institution Collision\n(OpenAlex bare Arabic name mixed IU Gaza papers into IU Madinah)"]
    Root --> F2["2. Mirror Inflation\n(Preprints on arXiv/SSRN counted alongside published journal DOIs)"]
    Root --> F3["3. Source Disagreement\n(Only 21 of top 150 shared papers match citation counts)"]
    Root --> F4["4. Live-Sync Audit Paradox\n(2026 rank changes corrupting 2023 reward eligibility)"]
    Root --> F5["5. Identity & Script Drift\n(Arshad Ali x12 ghost profiles + Arabic/Sharia STEM blind spots)"]
    Root --> F6["6. Small-College Blindness\n(Cutoffs dropping 9/13 colleges & naive seasonality broken by Jan 1 spikes)"]

    F1 --> S1["Multi-Hop SQL Forensics\n(Non-destructive rejection + affiliate reset)"]
    F2 --> S2["Title + Namespace Deduplication\n(Keep published DOI, audit-link preprints)"]
    F3 --> S3["Non-Destructive Provenance\n(sources array + Discrepancy Explorer)"]
    F4 --> S4["Temporal Metric Separation\n(Frozen publish quartile vs live current quartile)"]
    F5 --> S5["Transliteration & Arabic Engines\n(Scored duplicates UI + normalize_arabic_text)"]
    F6 --> S6["Empirical Bayes Hierarchy\n(5-yr empirical pacing + damped trend R2 0.98)"]
```

### Deep Dive into Every Problem We Solved

<details>
<summary><b>Problem 1 &amp; 2: Phantom Institutions &amp; Preprint Double-Counting (Open to inspect exact logic)</b></summary>

1. **The "Islamic University" Gaza Misattribution Trap:**  
   OpenAlex indexed the bare Arabic string `"الجامعة الإسلامية"` as an alias of Islamic University of Madinah (`I4210151290`). As a result, papers published by authors from the **Islamic University of Gaza (`I181086730`, DOI prefix `10.33976/iugj...`)** who omitted their city name were automatically credited to Madinah—creating phantom papers and ghost faculty accounts.  
   **Our Fix:** A multi-hop forensic quarantine inside PostgreSQL combining DOI prefix matching (`10.33976/`), co-occurring translated records without DOIs, and lateral JSON affiliation scanning that verifies zero Madinah/Saudi tokens exist before non-destructively setting `verification_status = 'rejected'`, resetting external author rows (`is_iu = false`), and keeping an explicit rollback trail.

2. **Preprints Masquerading as Separate Publications:**  
   Researchers upload preprints (`arXiv`, `SSRN`, `TechRxiv`, `OSF`, `Zenodo`, `ResearchSquare`) months before publishing the final peer-reviewed journal article under a completely different DOI.  
   **Our Fix:** Our deduplication pass normalizes titles (`> 25` alphanumeric characters) within a 2-year window across preprint DOI regex namespaces: whenever a non-preprint journal version exists, preprint mirrors are non-destructively excluded with provenance links (`kept_id`, `kept_doi`) rather than inflated in institutional counts.

</details>

<details>
<summary><b>Problem 3 &amp; 4: Why Sources Disagree &amp; Why Historical Quartiles Must Be Immutable</b></summary>

1. **Citation Counts Never Agree Across Databases:**  
   In an empirical audit of the **150 highest-cited papers indexed in both Scopus and OpenAlex**, only **21 papers (14.0%)** had identical citation counts (**104** counted higher in Scopus; **25** counted higher in OpenAlex due to preprints and cross-publisher links). Overwriting one source with another destroys audit truth.  
   **Our Fix:** Every publication preserves a multi-source provenance array (`sources metric_source[]`) alongside per-source rows in `publication_metrics`, allowing side-by-side trend comparison and a live Discrepancy Explorer.

2. **Protecting Past Faculty Rewards During Live API Syncs:**  
   A journal ranked **Q1 in 2023** may drop to **Q2 in 2026**. If a live Scopus sync updates `journals.citescore` globally and overwrites paper standings, researchers lose legal eligibility for historical awards—or fail compliance audits.  
   **Our Fix:** CRIS enforces strict **Temporal Metric Governance**: reward engines evaluate the journal's frozen historical metric year (`metric_year = publication_year`), while live API syncs refresh separate monitoring fields (`current_citescore_quartile`, `current_sjr_quartile`, `last_metrics_sync_at`).

</details>

<details>
<summary><b>Problem 5 &amp; 6: Identity Splitting ("Arshad Ali x12") &amp; Arabic/Sharia Neglect</b></summary>

1. **Fragmented Faculty Profiles Without False Auto-Merges:**  
   A single scholar often publishes under inconsistent Latin spellings (`Imran Shakir` vs. `Imran Shakyr`, `Ali Al-Zahrani` vs. `Aly Alzahrany`), or common names collide (`"Arshad Ali"` appeared across **12 separate OpenAlex author IDs**—and two distinct researchers named *Arshad Ali* co-authored the same paper!). Blind auto-merging corrupts authorship permanently.  
   **Our Fix:** A reversible, explainable admin review engine (`researcher_duplicate_candidates`) powered by PostgreSQL `_researcher_name_norm()` (folding Arabic `أإآ -> ا`, `ة -> ه`, `ى -> ي`, unifying `y -> i` transliteration, and splitting attached `al-/el-` prefixes) plus an auditable rollback snapshot table (`researcher_merge_history`).

2. **Serving Arabic, Sharia, Hadith &amp; Quran Colleges Fairly:**  
   Conventional bibliometric tools treat English STEM journals as the default and fail Arabic humanities colleges. We built `normalize_arabic_text()` and `classify_journal_major_fields()` natively in SQL—stripping Arabic diacritics (*Tashkeel*/*Tatweel*) and definite articles, isolating STEM keywords from *Fiqh/Sharia/Usul/Tafsir/Linguistics* terms across **49,654 journals**, and integrating **ARCIF** coverage.

</details>

---

## 2. System Architecture

CRIS uses a **Zero-Trust Database-Governed SSR Architecture**. Critical invariants—role scopes, salary-linked reward calculations, deduplication transactions, Arabic journal classification, and statistical forecasting—execute inside hardened **PostgreSQL `SECURITY DEFINER` RPCs and Row-Level Security (RLS) policies**, while the **React 19 + TanStack Start + Nitro** server/client layer orchestrates streaming SSR, optimistic queries, and bilingual RTL/LTR rendering.

```mermaid
flowchart TB
    subgraph Client ["Bilingual SSR & Interactive UI (Vercel + Nitro Edge)"]
        App["React 19.2 + TanStack Start\n(Arabic RTL / English LTR)"]
        Nav["Data-Driven Registry & RoleGuard\n(navConfig.ts + File-Based Route Tree)"]
        Cache["TanStack React Query v5 + Recharts\n(Client Telemetry & Normalized SSR Errors)"]
    end

    subgraph Security ["PostgreSQL Security & Governance Perimeter"]
        RBAC["8-Role RBAC Gate & RLS Engine\n(has_role + Scoped Visibility + audit_log)"]
    end

    subgraph Core ["Database Business Logic & Analytics Core (118+ Versioned Migrations)"]
        ETL["Staging & Provenance Engine\n(scopus_works_staging, openalex_staging)"]
        Identity["Identity & Deduplication\n(Weighted Scoring + Merge Snapshot Undo)"]
        Analytics["Analytics Explorer & KPIs\n(Materialized CTEs + Temporal Quartile Split)"]
        Forecast["Empirical Bayes Forecaster\n(train_forecast_models → forecast_cache)"]
        Journals["Journal Smart Matcher\n(49,654 Catalog + pg_trgm + Arabic Classifier)"]
        Rewards["Zero-Trust Incentive Engine\n(incentive_quote + Advisory Xact Locks)"]
    end

    subgraph External ["External Bibliographic & Storage Ecosystem"]
        APIs["Scopus APIs · OpenAlex · Clarivate WoS · ARCIF"]
        Storage["Supabase Storage\n(Verified Evidence PDF Bucket)"]
    end

    App --> Nav --> Cache --> RBAC
    RBAC --> Analytics
    RBAC --> Forecast
    RBAC --> Journals
    RBAC --> Rewards
    RBAC --> Identity
    APIs --> ETL --> Identity
    Identity --> Analytics
    Identity --> Forecast
    Rewards --> Storage
```

### 8-Role Database Security Topology

Rather than relying on hide/show UI flags, every page and RPC enforces hierarchical and domain isolation directly inside PostgreSQL:

```mermaid
graph TD
    DB["PostgreSQL Security Core\n(SECURITY DEFINER + RLS)"]
    DB --> Global["University-Wide Governance\nAdmin · DeanOfScientificResearch · Auditor"]
    DB --> ScopeCollege["College-Scoped Leadership\nCollegeDean (Own College Only + Anti-Tamper Audit)"]
    DB --> ScopeDept["Department-Scoped Leadership\nDepartmentHead (Own Dept KPIs, Reports & Review Stage 1)"]
    DB --> Venture["Ecosystem Operations\nEntrepreneurship (Innovation Projects + Startups Registry)"]
    DB --> Personal["Self-Service Academic Users\nFaculty (Publications, Journals, Rewards) · Student (Graduate Theses)"]
```

- Cross-scope tampering attempts (e.g., a `CollegeDean` querying a department outside their own college) are **blocked at the RPC boundary** and recorded automatically in `public.audit_log` as `security_violation` events.

---

## 3. Multi-Source ETL & Identity Resolution

Our ingestion pipeline treats external APIs as **untrusted, conflicting witnesses** rather than absolute truth:

```mermaid
flowchart LR
    subgraph Raw ["1. External Sync & Quota Telemetry"]
        S1["Scopus REST API\n(source_api_keys quota tracking)"]
        S2["OpenAlex API\n(Works + Author IDs)"]
        S3["WoS / ARCIF Feeds\n(Designations & Index Windows)"]
    end

    subgraph Stage ["2. Conflict-Proof Staging"]
        ST["Staging & Trigger Sanitization\n(Array coalescing & type guards)"]
        Gaza["Gaza / Non-IU Forensics\n(Prefix + JSON affiliation scan)"]
        Pre["Preprint Classifier\n(Title norm + DOI regex window)"]
    end

    subgraph Match ["3. Explainable Identity Resolution (0-100 Score)"]
        Score["Weighted Candidate Scoring\n+40 Same ORCID | +30 Exact Name\n+20 Same Dept | +24 Shared Coauthors\n-35 Diff ORCID | -100 Same Paper Block"]
        Undo["Human Admin Review Queue\n(merge_researchers + Undo Snapshot)"]
    end

    subgraph LiveDB ["4. Reconciled System of Record"]
        Pubs["publications\n(sources array + Frozen Pub-Year Rank)"]
        Metrics["publication_metrics\n(Per-source rows + Live Refresh)"]
    end

    S1 --> ST
    S2 --> ST
    S3 --> ST
    ST --> Gaza
    ST --> Pre
    Gaza --> Match
    Pre --> Match
    Score --> Undo --> LiveDB
    LiveDB --> Pubs
    LiveDB --> Metrics
```

---

## 4. Hierarchical Predictive Forecasting Engine

To eliminate impossible spikes and cover **all 13 colleges and 29 academic departments** without arbitrary `< 30` exclusion cutoffs, `train_forecast_models()` executes a 4-stage statistical pipeline directly in PostgreSQL:

```mermaid
flowchart TD
    Hist["10-Year Verified History\n(tmp_univ_hist)"] --> Pacing["Step 1: Empirical 5-Yr Seasonal Pacing\nExcludes Jan 1 batch-date artifacts;\ncaptures Nov-Dec publishing surge"]
    Hist --> Slope["Step 2: Damped Log-Linear Trend\nregr_slope(ln(cnt)) with damping phi = 0.85\n(University Model Fit: R2 = 0.98)"]
    Pacing --> Univ["University Forecast Target\n2,314 YTD → 2,642 (2026) → 3,546 (2027)"]
    Slope --> Univ
    Univ --> Colleges["Step 3: Empirical Bayes College Share\nLaplace Smoothing (alpha = 1.0)\nAcross ALL 13 Colleges"]
    Colleges --> Depts["Step 4: Coherent Dept Reconciliation\nLaplace Smoothing (alpha = 0.5)\nAcross ALL 29 Departments"]
    Depts --> Cache["Materialized in forecast_cache\n(Instant Dashboard Retrieval)"]
```

- **Why not divide YTD output linearly by `day_of_year / 365`?** Institutional papers cluster heavily toward November–December, and missing publication months default to `January 1` in external feeds. Filtering `Jan 1` artifacts and measuring true 5-year historical Day-of-Year progression prevents early-year under-prediction and late-year distortion.

---

## 5. Global Journal Discovery & Arabic/Sharia Intelligence

How does a university catalog **49,654 global journals** so both an AI researcher seeking IEEE Transactions and an Islamic Jurisprudence scholar seeking an Arabic **ARCIF** venue get instant, accurate matches?

```mermaid
flowchart LR
    Input["Free-Text Manuscript Input\n(Arabic or English Title / Abstract)"] --> Norm["PostgreSQL Normalization\n(normalize_arabic_text:\nStrip Tashkeel, Unify Alef/Yaa/Taa, Strip Prefixes)"]
    Norm --> Guard{"Cross-Lingual Domain Guard"}
    Guard -->|"Fiqh / Hadith / Sharia Tokens\n(Exclude pure STEM triggers)"| Sharia["Sharia, Islamic Studies & Humanities\n(IU Published + ARCIF Support)"]
    Guard -->|"STEM & CS Tokens / ASJC Categories"| Stem["Engineering / CS / Science\n(CiteScore + SJR + JIF Ranking)"]
    Sharia --> Index["Trigram & GIN Indexing\n(pg_trgm over 49,654 global_journals)"]
    Stem --> Index
    Index --> Out["Ranked Venue Cards & Table\n(Quartile Q1-Q4 · Open Access · History)"]
```

---

## 6. Zero-Trust Incentive Engine & Multi-Stage Review

Reward claims involve real financial disbursement. `incentive_quote()` and `incentive_submit()` enforce **100% server-authoritative computation**: client inputs for salary, nationality, quartile, author count, or payout amount are rejected by design.

```mermaid
flowchart TD
    Req["Faculty Selects Verified Publication"] --> Lock["Acquire pg_advisory_xact_lock\n+ Verify Live Co-Authorship (is_iu = true)"]
    Lock --> Gate["Eligibility & Quota Gates\n- Role & Verified Nationality Check\n- Author Count Completeness Audit\n- Annual Cap Check (max 3/year) & Duplicate Lock"]
    Gate --> Policy{"Select Active Versioned Policy\n(incentive_policies.config)"}
    Policy -->|"Excellence Reward"| Salary["Verified HR Salary x Quartile Rate\n(Q1: 30% | Q2: 20% | Q3: 10% | Univ: 30%)\ndivided by total verified authors"]
    Policy -->|"Post-Publication Reward"| Tariff["Category Match Cascade:\n1. Exact → 2. Related Edge → 3. Same Major → 4. Ceiling\nApplied to Internal / External / Nature-Science SAR Tariffs"]
    Salary --> Bind["Bind Exact Expected Quote Hash\n+ Validate Uploaded PDF in Storage Bucket"]
    Tariff --> Bind
    Bind --> Stages["4-Stage Audited Approval Chain:\nDepartment → College → Research Deanship → Approved"]
```

---

## 7. Engineered for Seamless Scalability

CRIS was architected from day one so new faculties, bibliographic vendors, regulatory policies, and institutional centers plug in cleanly without rewriting core schemas or breaking historical records:

```mermaid
flowchart LR
    subgraph Plug ["Zero-Friction Extension Points"]
        P1["1. New UI Module or Center\n(Add entry to navConfig.ts\n+ TanStack file route)"]
        P2["2. New Bibliographic Feed\n(Register in source_api_keys\n+ Staging worker → sources[])"]
        P3["3. New Institutional Role\n(Extend app_role enum\n+ Reuse _cris_v_* SQL validators)"]
        P4["4. Revised Reward Bylaws\n(Insert new incentive_policies\nJSON version + year bounds)"]
    end

    P1 --> Core1["Auto-updated Sidebar, Tabs,\nTitle, RoleGuard & Tests"]
    P2 --> Core2["Instant Dashboard Series,\nProvenance & Quartile Toggles"]
    P3 --> Core3["Isolated Theses / Startups Modules\nWith Zero Impact on Legacy Tables"]
    P4 --> Core4["Live Quotes Use vN While\nLegacy Claims Stay Pinned to v1"]
```

1. **Single-Source Navigation & Code Splitting (`src/layouts/navConfig.ts`):** Module groups, tabs, permissions, and document titles derive from one typed configuration synced with file-based route splitting via TanStack Router (`src/routeTree.gen.ts`).
2. **Non-Disruptive Domain Expansion:** When the **Innovation & Entrepreneurship Center**, **Quarterly Startup Tracking**, and **Graduate Theses** domains were introduced late in development (`migrations 0061–0063`), adding the 8th role (`Entrepreneurship`) reused shared SQL validation primitives (`_cris_fail`, `_cris_v_*`) without touching core publication or reward code.
3. **Pluggable Multi-Source & Quartile Architecture:** Bibliographic keys and sync health are decoupled behind `source_api_keys` and isolated staging tables, while KPI queries parameterize the metric framework (`SCImago SJR`, `Scopus CiteScore`, `WoS JCR`)—allowing seamless activation of Clarivate Web of Science and ARCIF streams.
4. **Immutable Versioned JSON Policies (`incentive_policies`):** Reward formulas live as versioned `jsonb` policies linked by `policy_id` on every claim alongside `[from_year, to_year]` journal designation bounds—zero code redeploys when university bylaws evolve.
5. **Corpus-Scale Database Performance:** `pg_trgm` and GIN array indexes handle 49,654 journals interactively, `MATERIALIZED` CTE analytics isolate heavy multi-chart aggregations, and `forecast_cache` eliminates runtime regression overhead.

---

## 8. Live Platform Showcase

### 1) Executive Overview — 7,359 Publications & 71,567 Citations
Unified KPIs, historical growth curves across `All Series` / `University Total` / `OpenAlex` / `Scopus`, historical quartile rings, and bilingual toggle (`Arabic` / English).

![CRIS Executive Dashboard Overview](assets/screenshots/2-dashboard.png)

---

### 2) Citation Discrepancy Audit — Scopus vs. OpenAlex (150 Top Works)
Revealing why multi-source provenance beats overwriting records: only **21 papers match equally** across both indexes (**104** higher in Scopus; **25** higher in OpenAlex).

![Scopus vs OpenAlex Citation Discrepancy Analysis](assets/screenshots/1-citation-discrepancy.png)

---

### 3) Analytics Explorer & Faculty Breakdown
Scoped institutional velocity (`1,743` papers, `9,668` citations, `59.6%` Q1+Q2 share, `93.2%` external collaboration, `71.3%` dual-indexed rate in the 2025 benchmark) and multi-college stacked comparisons with `Chart | Table | Both`, `Count | Share %`, and CSV export.

![Analytics Explorer KPIs and Quartile Distribution](assets/screenshots/3-analytics-kpis.png)

![Colleges Breakdown Stacked Chart and Summary Table](assets/screenshots/4-colleges-breakdown.png)

---

### 4) Empirical Bayes Predictive Model (`R² = 0.98`)
10-year damped log-linear forecast projecting **2,642** publications for end-of-2026 (from **2,314** YTD) and **3,546** in 2027 at **41% annual growth**, hierarchically reconciled across all colleges and departments.

![Predictive Forecast Chart for University Output](assets/screenshots/5-predictive-model.png)

---

### 5) Scientific Journal Catalog & Manuscript Smart Matcher (49,654 Journals)
Paste any Arabic or English working abstract or title to discover optimal venues filtered by `CS`, `SJR`, `JIF`, quartile (`Q1–Q4`), Open Access, prior IU publishing track record, or `Arabic & Theoretical Colleges`.

![Scientific Journals Catalog and Manuscript Smart Matcher](assets/screenshots/6-journal-finder.png)

---

### 6) Zero-Trust Incentive & Reward Submission Wizard
3-step guided application computing salary-percentage **Excellence** or fixed-SAR **Post-Publication** quotes on the server from verified HR facts, ASJC subject alignment, and compliance evidence.

![Research Rewards Eligibility and Submission Wizard](assets/screenshots/7-rewards-workflow.png)

---

## Technology Stack

| Layer | Technologies & Engineering Role |
| :--- | :--- |
| **SSR & Routing Framework** | React 19.2, TanStack Start, TanStack Router, Nitro, Vite 7, TypeScript 5.8 |
| **State & Validation** | TanStack React Query v5, React Hook Form, Zod, Shared SQL `<area>:<field>:<rule>` Error Contracts |
| **UI & Visualization** | Tailwind CSS v4, Radix UI Primitives, Recharts (Area, Trend, Scatter, Stacked Quartiles), AR RTL / EN LTR |
| **Database & Business Core** | Supabase / PostgreSQL (118+ SQL Migrations), `SECURITY DEFINER` RPCs, 8-Role RLS, `pg_trgm` & GIN Indexes |
| **Scholarly Integrations** | Elsevier Scopus APIs, OpenAlex API, Clarivate Web of Science (WoS), SCImago SJR, ARCIF |
| **Testing & Automation** | Unit test suites (`navConfig`, journal relevance, duplicate matching, validation), SQL test harnesses (`test_incentive_engine.sql`), GitHub Actions keep-alive pipeline |

---

## Acknowledgments & Credits

Whatever merit this work has, after God, belongs to our supervisor, **Prof. Dr. Ali bin Mousa Gharmallah Alzahrani**, Vice President for Graduate Studies and Scientific Research, and to our academic and technical advisor, **Dr. Oussama Benrhouma** — our sincere thanks to both for their mentorship, trust, and guidance throughout this project.

Deep appreciation also goes to every companion and teammate on our Computer Science graduation capstone team at the Islamic University of Madinah for sharing this engineering journey from first API experiments to production deployment.

**Author & Engineering Contact:**  
**Abdulqader Alshihabi** — Software & AI Engineer · Computer Science (Class of 2027)  
- Live Platform: [https://iu-cris-system.vercel.app/](https://iu-cris-system.vercel.app/)  
- Portfolio: [https://abdulqader-alshihabi.github.io/](https://abdulqader-alshihabi.github.io/)  
- GitHub: [@Abdulqader-Alshihabi](https://github.com/Abdulqader-Alshihabi)  
- Email: [abdulqader.moh.2003@gmail.com](mailto:abdulqader.moh.2003@gmail.com)
