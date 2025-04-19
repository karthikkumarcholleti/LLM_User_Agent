# LLM USER AGENT for Mining EHR & PGHD Data.
Developing a LLM User Agent using Llama3.1 to mine EHR and PGhD data of  the patients.

YOUTUBE LINK: https://youtu.be/wQ5l5yrvWdM


# LLM‑UA: AI‑Powered Clinical Assistant for EHR & PGHD

## Table of Contents
1. [Overview](#overview)  
2. [Motivation](#motivation)  
3. [Research Objectives](#research-objectives)  
4. [System Architecture](#system-architecture)  
5. [Data Sources & Preprocessing](#data-sources--preprocessing)  
6. [Baseline Testing & Evaluation](#baseline-testing--evaluation)  
7. [Fine‑Tuning & Model Improvement](#fine‑tuning--model-improvement)  
8. [Visualization & Chat Interface](#visualization--chat-interface)  
9. [Deployment & Integration](#deployment--integration)  
10. [Real‑World Impact](#real‑world-impact)  
11. [Future Work & Extensions](#future-work--extensions)  
12. [Getting Started](#getting-started)  
13. [Contributing](#contributing)  
14. [License](#license)  

---

## Overview
This project develops a **Large Language Model (LLM) User Agent** to mine and interpret both structured Electronic Health Records (EHR) and unstructured Patient‑Generated Health Data (PGHD). By combining FHIR‑standardized data ingestion, LLM‑powered question answering, and rich time‑series visualizations, the agent assists clinicians in tracking vital trends, summarizing patient trajectories, and supporting treatment decisions.

A proof‑of‑concept large language model user‑agent that:
- Ingests and cleans CCDA/FHIR EHR data  
- Generates interactive time‑series visualizations of patient vitals  
- Produces concise clinical summaries via LLaMA 3.1  
- Supports a stateful, follow‑up chat session  
- Includes a presentation‑ready notebook and slides

## Motivation  
- **Data Overload**: Clinicians struggle to synthesize millions of records and free‑text notes.  
- **Interoperability**: Diverse hospital systems use different formats—FHIR standardizes them.  
- **Actionable Insights**: LLMs can surface early warning signs (e.g., rising blood pressure) and suggest next steps, reducing diagnostic delays.

## Research Objectives  
1. **Automated Extraction**  
   - Convert ORU, ADT, CCDA messages to FHIR JSON.  
   - Mask PHI and enforce differential‑privacy safeguards.  
2. **LLM‑Driven Querying**  
   - Baseline evaluation of Llama 3.1 on unstructured vs. structured data.  
   - Fine‑tune for clinical Q&A, endpoints, and prompt optimization.  
3. **Visualization & Chat**  
   - Time‑series plots of vitals (heart rate, BP, glucose…) with normal/warning zones.  
   - Interactive chatbot for follow‑ups and multi‑patient comparisons.  


## Key Features
- **Multi‑format ingestion**: Converts hospital ORU/ADT/CCDA feeds into FHIR JSON; ingests unstructured clinical narratives from MIMIC‑IV.
- **Privacy & normalization**: Masks PHI; normalizes units via a `LOINC_UNIT_MAP` with sensible fallbacks.
- **Label enrichment**: Resolves missing LOINC display names from a curated CSV lookup to ensure human‑readable observations.
- **Baseline evaluation**: Tests LLaMA 3.1’s out‑of‑the‑box performance on structured vs. unstructured data.
- **Fine‑tuning pipeline**: Iteratively refines the model on 200 training patients, reserves 30 for blinded testing.
- **Visualization suite**: Interactive Plotly graphs with color‑coded normal/warning/abnormal zones, annotations, and multi‑patient comparison.
- **Chat session engine**: A stateful interface enabling follow‑up queries, real‑time plotting, and summary generation.

---

## Data Sources & Preprocessing

### Structured Data (FHIR)
- **Sources**: ORU, ADT, CCDA feeds from UPHCS.
- **Conversion**: Interface engine → HAPI FHIR server → JSON bundles.
- **Preprocessing**:
  - **PHI Masking** & **Differential Privacy** filters.
  - **Unit normalization** via `LOINC_UNIT_MAP` for missing units.
  - **Display name mapping**: Loaded from `loinc_code_display_mapping.csv` to replace “Unknown” labels.

### Unstructured Data (Clinical Notes)
- **Sources**: MIMIC‑IV Note & MIMIC‑IV‑Ext‑BHC collections.
- **Processing**:
  - Tokenization and cleaning of free‑text narratives.
  - Prompt‑based extraction of chief complaints, diagnoses, timelines.

---

## Real‑World Impact
- Accelerates clinical workflows by surfacing trends and anomalies.
- Enhances longitudinal care management for chronic diseases (e.g., hypertension, diabetes).
- Facilitates interoperable data sharing across health systems.
- Lays the groundwork for advanced AI‑driven care coordination tools.

---

## License & Acknowledgments
This work was conducted under CITI certification and institutional approvals. Code and preprints will be shared under the MIT License. Special thanks to the UPHCS data team and the MIMIC consortium for data access.



## System Architecture  
```text
              +---------------+      
              |  Source EHR   |  ──┐
              +---------------+    │   ┌───────────────┐      ┌────────────┐
              +---------------+    └─> │ Interface      │      │ HAPI FHIR  │
              | PGHD / Notes  |        │ Engine (HL7→FHIR)────>│ Server     │
              +---------------+        └───────────────┘      └────────────┘
                                                          │
                                                          ▼
      ┌───────────────────────────────────────────────────┐
      │        Data Preprocessing & PHI Masking          │
      └───────────────────────────────────────────────────┘
                                                          │
                                                          ▼
      ┌───────────────────────────┐     ┌─────────────────┐
      │  LLM User Agent (Llama)   │<───►│ Visualization & │
      │  • Prompt classification  │     │ Chat Interface │
      │  • Summary generation     │     └─────────────────┘
      └───────────────────────────┘
