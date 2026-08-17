# Sustainability Reporting Evolution in UK Banking (2017–2024)

### An NLP-Based Analysis of Topic Modelling and Sentiment Dynamics Across Six Major UK Banks

**Module:** BUSI 1783 — Business Analytics Project
**Programme:** MSc Business Analytics, University of Greenwich
**Author:** Thulasi Thananjeyan (Student ID - 001487710)

**Supervisor:** Thamaraikani Chandrasooden
**Submission date:** 25 August 2026

\---

## Overview

This repository contains the complete code, data and documentation for an MSc Business
Analytics project that applies Natural Language Processing (NLP) to a corpus of 48
sustainability and ESG reports published by six major UK-listed banks such as **HSBC, Barclays,
Lloyds, NatWest, Standard Chartered and Santander UK** over the period **2017 to 2024**.

The project addresses a single research question:

> \\\*How have sustainability themes and their associated sentiment evolved in the ESG and
> sustainability reports of six major UK-listed banks between 2017 and 2024, and to what
> extent do these patterns differ across institutions?\\\*

The analysis combines **LDA topic modelling** (to identify sustainability themes and track
their prevalence over time) with **FinBERT sentiment analysis** (to quantify the tone of
disclosure at document and topic level), and validates the sentiment measures at the panel
level against **Refinitiv ESG pillar scores** using Spearman rank correlation.

\---

## Repository Structure

```
Sustainability\\\_Project/
├── README.md                     <- this file
├── UK\\\_Banking\\\_NLP\\\_Pipeline.ipynb <- main analysis notebook (run top to bottom)
├── corpus\\\_tracker.xlsx           <- inventory of all 48 source documents
│
├── data/
│   ├── raw\\\_pdfs/                 <- source PDF reports, organised by bank (NOT uploaded — copyright)
│   │   ├── HSBC/                 (HSBC\\\_2017.pdf … HSBC\\\_2024.pdf)
│   │   ├── Barclays/
│   │   ├── Lloyds/
│   │   ├── Natwest/
│   │   ├── StandardChartered/
│   │   └── Santander/
│   ├── processed\\\_text/           <- extracted \\\& cleaned text (.txt per document)
│   └── esg\\\_scores/
│       └── eikon\\\_esg\\\_scores.csv  <- Refinitiv ESG pillar scores (NOT uploaded — licensed)
│
└── outputs/
    ├── figures/                  <- generated charts (PNG) and pyLDAvis (HTML)
    └── tables/                   <- generated result tables (CSV) + RESULTS\\\_FOR\\\_CHAPTER4.xlsx
```

\---

## Data Sources

All primary data are **publicly available corporate disclosures** downloaded directly from
the official websites of the six banks. No proprietary or paid data access was required for
the text corpus.

|Bank|Source|
|-|-|
|HSBC|ESG reporting centre \| HSBC Holdings plc|
|Barclays|home.barclays — ESG Resource Hub - Annual Reports|
|Lloyds Banking Group|Annual report archive - Lloyds Banking Group plc<br />|
|NatWest Group|natwestgroup.com — Sustainability Downloads – Results centre|
|Standard Chartered|Sustainability library \| Standard Chartered|
|Santander UK|Publications \| Sustainability \| Santander UK|

The full document-by-document inventory (report title, type, year and source) is in
`corpus\\\_tracker.xlsx`.

**ESG validation data:** Environmental and Governance pillar scores were obtained from the
**Refinitiv / LSEG Workspace (EIKON)** platform via the University of Greenwich library.
Refinitiv coverage for these banks begins around 2020–2021, so the validation is based on
26 complete bank-year observations rather than the full 48.

> \\\*\\\*Note on redistribution:\\\*\\\* Refinitiv/LSEG data is licensed and must not be redistributed
> publicly. The bank PDFs are copyright of the respective banks. Neither the raw PDFs nor the
> ESG CSV are included in this repository for that reason.

\---

## Methodology Summary

1. **Text extraction** — `pdfplumber` extracts text from each PDF. For full annual reports,
only pages with a sufficient density of sustainability keywords are retained, isolating
the sustainability narrative from financial-statement content.
2. **Pre-processing** — tokenisation (NLTK), custom stop-word removal (including bank names
and financial boilerplate), lemmatisation (spaCy), and bigram detection (gensim `Phrases`).
3. **Topic modelling** — LDA (gensim); the number of topics *k* is selected by maximising the
`c\\\_v` coherence score over k = 5–12. The final model uses **k = 10** (coherence 0.385).
4. **Sentiment analysis** — FinBERT (`ProsusAI/finbert` via HuggingFace `transformers`),
scored per sentence and aggregated to document level and to topic level.
5. **Validation** — panel-level Spearman rank correlation between document net positivity and
Refinitiv Environmental and Governance pillar scores.

\---

## How to Reproduce

### Requirements

* Python 3.10+
* Packages listed in the installation command below

### Setup

```bash
# 1. Clone or download this repository
# 2. Place the 48 source PDFs in data/raw\\\_pdfs/<Bank>/ using the naming BankName\\\_Year.pdf
# 3. Install dependencies:
pip install pdfplumber nltk spacy gensim transformers torch pandas matplotlib seaborn scipy pyLDAvis openpyxl tqdm
python -m spacy download en\\\_core\\\_web\\\_sm
```

### Run

Open `UK\\\_Banking\\\_NLP\\\_Pipeline.ipynb` in Jupyter and run the cells in order. The notebook:

* reads the PDFs and `corpus\\\_tracker.xlsx`;
* extracts and pre-processes the text;
* trains the LDA model and exports topics and prevalence;
* runs FinBERT document-level and topic-level sentiment;
* runs the panel-level Spearman validation (requires `eikon\\\_esg\\\_scores.csv`);
* writes all figures to `outputs/figures/` and all tables to `outputs/tables/`;
* bundles the key results into `outputs/tables/RESULTS\\\_FOR\\\_CHAPTER4.xlsx`.

> The first run downloads the FinBERT model (\\\~440 MB) and the spaCy model; an internet
> connection is required for that initial run only. FinBERT runs on CPU (a GPU is optional
> and faster).

\---

## Key Findings

* The **climate-transition theme** rose more than eightfold in prevalence (0.020 → 0.167)
between 2017 and 2024, plateauing from 2020, consistent with the tightening regulatory
environment (TCFD 2017; NZBA 2021; mandatory TCFD 2022).
* A **just-transition / affordable-housing theme** emerged after 2021, while older
"corporate citizenship" language declined.
* **Topic-level sentiment** varies systematically: banks are most positive about social
themes and most measured about the climate transition.
* **Validation:** a negative but non-significant correlation between disclosure positivity
and the Refinitiv Environmental pillar score (ρ = −0.329, p = 0.100, n = 26).

\---

## Ethical Statement \& AI Use

All data are publicly available corporate disclosures; no personal data or human participants
are involved. AI tools were used to support research scoping, code development and formatting.
AI was **not** used to generate the assessed written content of the project. A Declaration of
AI Use accompanies the submitted dissertation.

\---

## Licence

This repository is submitted as academic coursework for the University of Greenwich.
Source reports remain the copyright of the respective banks. Refinitiv/LSEG data is licensed
and must not be redistributed.

