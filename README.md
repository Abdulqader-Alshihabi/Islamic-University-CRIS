# Islamic University CRIS - Comprehensive Research Information System

[![Academic Project](https://img.shields.io/badge/Project-B.Sc.%20Graduation%20Capstone-blue?style=for-the-badge)](https://github.com/Abdulqader-Alshihabi/Islamic-University-CRIS)
[![Institution](https://img.shields.io/badge/Institution-Islamic%20University%20of%20Madinah-006C35?style=for-the-badge)](https://github.com/Abdulqader-Alshihabi/Islamic-University-CRIS)
[![Verified Papers](https://img.shields.io/badge/Indexed%20Corpus-7%2C352%20Papers-green?style=for-the-badge)](https://github.com/Abdulqader-Alshihabi/Islamic-University-CRIS)
[![Citations Tracked](https://img.shields.io/badge/Citations%20Tracked-71%2C567-orange?style=for-the-badge)](https://github.com/Abdulqader-Alshihabi/Islamic-University-CRIS)
[![Scholarly Integrations](https://img.shields.io/badge/Integrations-Elsevier%20Scopus%20%7C%20OpenAlex-red?style=for-the-badge)](https://github.com/Abdulqader-Alshihabi/Islamic-University-CRIS)

Institution: Islamic University of Madinah (Deanship of Scientific Research)  
Category: Computer Science B.Sc. Graduation Capstone & Enterprise Scholarly Intelligence Platform  
Role: Lead Full-Stack Architect, Database Engineer & ETL Pipeline Developer  

---

## Institutional and Proprietary Notice

> [!IMPORTANT]
> Institutional Project Notice:  
> This repository serves as an Architectural Case Study and Capstone Showcase for the Islamic University Comprehensive Research Information System (CRIS). In compliance with institutional intellectual property policies, academic data protection, and university server credentials, internal production keys and administrative database seeds are not publicly hosted.

---

## Project Overview

The Islamic University Comprehensive Research Information System (CRIS) is an enterprise academic intelligence and research analytics platform built to centralize, audit, and analyze the university's complete research corpus. 

Originally restricted to a single bibliographic source (OpenAlex), this capstone project engineered an automated multi-source ingestion pipeline integrating Elsevier Scopus data, reconciliation engines, and dean-level research analytics.

### Key Metrics and Scale
- **7,352 Verified Scientific Publications**: Total institution-wide corpus cataloged and deduplicated.
- **71,567 Academic Citations**: Reconciled citation metrics across Scopus and OpenAlex with discrepancy reporting.
- **5,223 Multi-Source Reconciled Works**: Dual-indexed research papers mapped and synchronized.
- **2,426 University Researchers**: Profiles enriched with Scopus Author IDs and ORCID mappings.
- **1,477 Academic Journals**: Integrated with CiteScore journal quartiles (Q1-Q4) and SCImago SJR metrics.
- **32,207 Author Affiliations**: Enriched historical institutional affiliation records.

---

## Architectural Highlights and Data Engineering

```
┌────────────────────────────────────────────────────────────────────────┐
│                     Scholarly Ingestion Pipeline                       │
│    ┌───────────────────────────┐      ┌───────────────────────────┐    │
│    │    Elsevier Scopus API    │      │       OpenAlex API        │    │
│    │    (CiteScore, Papers)    │      │    (Bibliographic Open)   │    │
│    └─────────────┬─────────────┘      └─────────────┬─────────────┘    │
└──────────────────┼──────────────────────────────────┼──────────────────┘
                   │                                  │
                   ▼                                  ▼
┌────────────────────────────────────────────────────────────────────────┐
│                  PostgreSQL Automated ETL & Staging                    │
│   - Staging Tables: scopus_works_staging, scopus_authors_staging       │
│   - Reconciliation Procedure: import_scopus_from_staging()            │
│   - Conflict-Proof Upserts with Dynamic Deduplication Array            │
│   - Automated CiteScore & SJR Quartile Classifier (Q1 - Q4)            │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│               Dean & Executive Research Analytics Layer                │
│   - Stored RPCs: publications_trend_by_source, citation_discrepancy   │
│   - Analytical Explorer: Dynamic Quartile System Switcher              │
│   - Faculty Identity Audit: v_faculty_scopus_audit                     │
│   - Frontend: React 18 • TypeScript • Tailwind CSS • Recharts          │
└────────────────────────────────────────────────────────────────────────┘
```

### 1. Robust PostgreSQL ETL Ingestion Pipeline
- Engineered the core `import_scopus_from_staging()` procedure resolving high-volume unique constraint conflicts and foreign-key dependencies.
- Built a multi-source schema tracking indexed sources via PostgreSQL arrays (`sources text[]`).
- Created staging tables for batched ingestion of 5,848 Scopus records, CiteScore quartile classifications, and journal metrics.

### 2. Multi-Source Publication Trend and Citation Reconciliation
- Replaced monolithic single-source trend queries with the enriched `publications_trend()` stored procedure returning verified university totals, OpenAlex volumes, and Scopus volumes by academic year.
- Developed `citation_discrepancy()` RPC to identify variances between Scopus citation metrics and open bibliographic sources.
- Designed `v_faculty_scopus_audit` database view auditing faculty Scopus identifier completeness across university colleges and departments.

### 3. Executive Dean-Level Analytics Cockpit
- High-contrast, clean KPI cards displaying institution-wide publication volume and citations.
- Interactive multi-variable comparative area charts with segmented toggle controls (All Series, University Total, OpenAlex, Scopus).
- Dynamic quartile system switcher enabling university executives to switch all calculations (KPIs, college charts, department rankings) between SCImago SJR and Scopus CiteScore in real time.

---

## Technology Stack

| Layer | Technologies and Tools |
| :--- | :--- |
| **Frontend Framework** | React 18, TypeScript, Vite |
| **Styling and UI Tokens** | Tailwind CSS, Radix UI, Shadcn UI, Lucide Icons |
| **Data Fetching and State** | TanStack React Query v5, Supabase JS Client |
| **Database and ETL Engine** | PostgreSQL, Stored Procedures (PL/pgSQL), Database Views, RPCs |
| **Scholarly Integrations** | Elsevier Scopus REST API, OpenAlex Bibliographic API |
| **Analytics and Charts** | Recharts (Responsive Area, Multi-Series Trends, Bar Metrics) |

---

## Core Modules Implemented

- Comprehensive Publications Catalog: Multi-source search with source origin badges (Scopus, OpenAlex, Dual).
- Researcher Profile Augmentation: Automatic CiteScore metrics, h-index calculation, and Scopus Author ID linking.
- Journals Directory with Dual Quartiles: Side-by-side CiteScore quartile and SCImago SJR classification.
- College and Department Drill-Downs: Hierarchical performance breakdown with publication velocity metrics.
- Source Growth Monitoring: Tracking indexing expansion and multi-source corpus health over time.

---

## Author and Engineering Credits

**Abdulqader Alshihabi**  
Computer Science Graduate | Software & AI Engineer  
Islamic University of Madinah  
- Portfolio: [https://abdulqader-alshihabi.github.io/](https://abdulqader-alshihabi.github.io/)  
- GitHub: [@Abdulqader-Alshihabi](https://github.com/Abdulqader-Alshihabi)  
- Email: [abdulqader.moh.2003@gmail.com](mailto:abdulqader.moh.2003@gmail.com)  
