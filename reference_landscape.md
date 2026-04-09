# Reference Landscape: Synthetic Data Generation for Decisioning Systems

> Researched: April 2026  
> Purpose: Landscape survey of commercial vendors and open-source libraries relevant to the Generic Synthetic Data Generation for Decisioning Strategies codeathon challenge.

---

## Table of Contents

1. [Commercial / SaaS Vendors](#1-commercial--saas-vendors)
   - 1.1 Tonic.ai
   - 1.2 MOSTLY AI
   - 1.3 Gretel.ai
2. [Open-Source Libraries — Statistical / ML Synthetic Data](#2-open-source-libraries--statistical--ml-synthetic-data)
   - 2.1 SDV (Synthetic Data Vault)
   - 2.2 MOSTLY AI SDK (Open Source)
   - 2.3 Gretel Synthetics (Open Source)
   - 2.4 YData Synthetic
   - 2.5 DataSynthesizer
   - 2.6 SynthCity
3. [Open-Source Libraries — Rule-Based / Fake Data Generators](#3-open-source-libraries--rule-based--fake-data-generators)
   - 3.1 Faker
   - 3.2 Mimesis
   - 3.3 factory_boy
4. [Evaluation & Profiling Tools](#4-evaluation--profiling-tools)
   - 4.1 ydata-profiling (formerly pandas-profiling)
5. [Gap Analysis: What Does NOT Exist Yet](#5-gap-analysis-what-does-not-exist-yet)
6. [Quick Comparison Matrix](#6-quick-comparison-matrix)

---

## 1. Commercial / SaaS Vendors

---

### 1.1 Tonic.ai

| Attribute | Detail |
|---|---|
| **Website** | https://tonic.ai |
| **Documentation** | https://docs.tonic.ai |
| **Type** | Commercial SaaS + SDK |
| **License** | Proprietary |
| **GitHub** | https://github.com/TonicAI |
| **Primary Audience** | Engineering / QA / Data teams in enterprise |

#### Product Suite

Tonic.ai is split into **three distinct products**:

**A. Tonic Structural** (structured & semi-structured data)
- Clone, de-identify and subset production relational databases to create safe, high-fidelity replicas for test/dev/staging environments.
- Maintains **referential integrity** across complex foreign key relationships.
- Supports PII detection, masking, de-identification, and subsetting.
- Preserves statistical patterns and distributions of original production data.
- Output formats: relational DB replicas, CSV/Parquet exports.

**B. Tonic Fabricate** (data from scratch)
- Generates fully synthetic relational datasets **from scratch** — no production data required.
- Includes an AI-powered **"Data Agent"**: a conversational chat interface where you describe schema requirements, volumes, and distributions.
- Can generate complex multi-table relational databases with referential integrity.
- Can produce **mock APIs** that return synthetic data for development/testing workflows.
- Can generate unstructured datasets and free-text fields.

**C. Tonic Textual** (unstructured data)
- Synthesizes or redacts sensitive information within unstructured data: free-text fields, logs, documents, files.
- Uses proprietary Named Entity Recognition (NER) models to identify and replace sensitive entities.
- Prepares unstructured data for safe use in AI workflows (RAG systems, LLM training).

#### Key Relevance to the Challenge
- **Fabricate** is the closest product to this challenge: generating synthetic relational data from scratch using schema + constraint definitions.
- Supports rule-based, transformative de-identification, and model-based/agentic AI approaches.
- Financial services use case is explicitly covered (PCI-compliant data, credit/account schemas).
- CI/CD integration supported through SDKs and developer tools.

#### What It Does NOT Do (for this challenge)
- Does not expose rule coverage reports, scenario manifests, or boundary-condition detection out of the box.
- Not specifically designed for testing decisioning policy logic — it is general-purpose test data.
- Tonic's rule system is about data quality and referential integrity, not credit policy rule validation.

---

### 1.2 MOSTLY AI

| Attribute | Detail |
|---|---|
| **Website** | https://mostly.ai |
| **Documentation** | https://mostly.ai/docs |
| **SDK Docs** | https://mostly-ai.github.io/mostlyai/ |
| **Type** | Commercial Platform + Open-Source SDK |
| **License** | Platform: Proprietary; SDK: Apache v2 |
| **GitHub (SDK)** | https://github.com/mostly-ai/mostlyai |
| **Stars** | ~762 (as of April 2026) |
| **Primary Audience** | Data science / ML / privacy engineering teams |

#### Platform Capabilities

MOSTLY AI distinguishes between three data types on its platform:

**A. Synthetic Data**
- AI-generated synthetic data that statistically mirrors real data but contains no real PII.
- Supports single-table, multi-table, and time-series data structures.
- Differentiates itself from "simple rule-based mock data" — uses AI training rather than templates.

**B. Mock Data**
- Realistic, production-like test data for reliable development and QA.
- More like template/schema-driven generation.

**C. Simulated Data**
- Data-driven simulations that predict outcomes (e.g., what-if scenarios).

#### Open-Source SDK Key Features (github.com/mostly-ai/mostlyai)
- **Broad Data Support**: Mixed-type data (categorical, numerical, geospatial, text). Single-table, multi-table, time-series.
- **Model Types**: State-of-the-art TabularARGN; DNN matchmaking for graph relations; Fine-tune Hugging Face LLMs; LSTM for text.
- **Advanced Training**: GPU/CPU support, Differential Privacy, progress monitoring.
- **Automated Quality Assurance**: Quality metrics for fidelity and privacy; in-depth HTML reports for visual analysis.
- **Flexible Sampling**:
  - Up-sample to any data volume.
  - **Conditional simulations** based on any column values (e.g., generate 10k records of 24-year-old males).
  - Re-balance underrepresented segments.
  - Context-aware data imputation.
  - Statistical fairness controls.
  - **Rule-adherence via temperature**.
- **Seamless Integration**: Connect to external databases (Databricks, BigQuery, Snowflake, Postgres, etc.) and cloud storages.
- LOCAL mode (fully on-premise) and CLIENT mode (cloud platform).

#### SDK Quick Install
```bash
pip install mostlyai           # CLIENT mode only
pip install mostlyai[local]    # CLIENT + LOCAL mode (includes PyTorch)
```

#### What It Does NOT Do (for this challenge)
- Does not natively accept credit policy rules or scorecards as input constraints.
- The "rule-adherence via temperature" is a soft probabilistic control, not hard deterministic rule enforcement.
- No explicit rule coverage report or scenario manifest output.
- Primarily a statistical learner — requires existing data to train on; doesn't synthesize from pure business rules alone.

---

### 1.3 Gretel.ai

| Attribute | Detail |
|---|---|
| **Website** | https://gretel.ai |
| **Documentation** | https://docs.gretel.ai |
| **Type** | Commercial Platform + Open-Source Core |
| **License** | Platform: Proprietary; Synthetics lib: Apache v2 |
| **GitHub (Core)** | https://github.com/gretelai/gretel-synthetics |
| **Stars** | ~676 (as of April 2026) |
| **PyPI** | `pip install gretel-synthetics` |
| **Primary Audience** | Data science / AI/ML / privacy engineering |

#### Platform Capabilities

**A. Gretel Synthetics (Tabular / Structured)**
- **ACTGAN**: Extension of CTGAN (Conditional Tabular GAN) — optimized for tabular/conditional data generation. Provides autodetection and transformation of columns, and improved memory usage.
- **LSTM**: Supports tabular, text, and time-series data.
- **Timeseries DGAN**: PyTorch implementation of DoppelGANger optimized for time-series data.

**B. Privacy & Safety Features**
- **Differential Privacy**: Optional mathematical guarantee that synthetic data does not memorize individual records.
- **Privacy Filters**: Protect against adversarial attacks (membership inference, re-identification).
- **PII Redaction (Gretel Transform)**: Redact, replace or mask PII before synthesis.
- **Overfitting Prevention**: Regularization and batch-based optimization during model training.

**C. Quality & Privacy Reports**
- Detailed reports evaluating statistical accuracy (fidelity) and privacy of the synthetic dataset vs. original.
- Gretel Blueprints/Examples: A GitHub repository with code examples for financial data and other sensitive domains.

**D. Gretel Safe Synthetics SDK**
- Builder pattern to construct pipelines: transformation → synthesis → privacy-preserving steps.
- `pip install gretel-client` for Python SDK access.

#### Gretel Synthetics (Open Source) — GitHub Details
- Tags: `privacy`, `differential-privacy`, `synthetic-data`, `tensorflow`, `artificial-intelligence`
- 676 stars, 98 forks, 360 commits.
- Supports structured/unstructured text and tabular data with differentially private learning.
- Models included: ACTGAN, Timeseries DGAN, and older TensorFlow-based text approaches.

#### What It Does NOT Do (for this challenge)
- Does not accept credit rule definitions or policy scorecards as structured inputs.
- No native rule coverage or boundary condition reporting.
- Privacy filters are about statistical re-identification, not credit policy validation.
- Primarily learns statistical distributions — not rule-driven deterministic generation.

---

## 2. Open-Source Libraries — Statistical / ML Synthetic Data

---

### 2.1 SDV (Synthetic Data Vault)

| Attribute | Detail |
|---|---|
| **Website** | https://sdv.dev |
| **Documentation** | https://docs.sdv.dev/sdv |
| **GitHub** | https://github.com/sdv-dev/SDV |
| **Stars** | ~3,500 (as of April 2026) |
| **Forks** | ~413 |
| **Commits** | ~2,200 |
| **License** | Business Source License (BSL) — free for non-commercial; enterprise licensing for production |
| **PyPI** | `pip install sdv` |
| **Primary Audience** | Data scientists, ML engineers, analytics teams |
| **Backing** | DataCebo (spun out of MIT Data to AI Lab in 2020) |

#### What It Does
Originally developed at MIT in 2016, SDV is described as "the largest ecosystem for synthetic data generation & evaluation." It is the most popular open-source library in this space.

**Key Capabilities**:

- **Single-table synthesis**: Multiple model options — GaussianCopula (statistical), CTGAN (deep learning GAN).
- **Multi-table synthesis**: Models relationships via primary/foreign key constraints. Synthesizers: HMA, HSA, Independent. Preserves referential integrity.
- **Sequential / time-series data**: Separate synthesizers for sequential tables.
- **Constraint support**: Define business rules as logical constraints (e.g., `value_A > value_B`, `column_C in [...]`). Ensures generated values adhere to business logic (like credit limits, account balances).
- **Anonymization**: Different anonymization types per column.
- **Preprocessing**: Data preprocessing control to improve synthesis quality.
- **Evaluation**: Built-in `evaluate_quality()` function generates a quality report with column-shape scores and column-pair trend scores. Visual comparison of synthetic vs. real data.
- **Conditional Sampling**: Fix specific column values and generate records around them.

**Multi-Table Conditional Sampling** (Enterprise):
- Targeted Sampling feature in SDV Enterprise Bundle allows fixing values across multiple joined tables while maintaining referential integrity.

**Install**:
```bash
pip install sdv
# or
conda install -c pytorch -c conda-forge sdv
```

**Relevant Gap for this Challenge**:
- Constraints must be manually defined per schema — no automatic rule-from-policy-doc extraction.
- No rule coverage report or scenario manifest output.
- Conditional sampling becomes computationally expensive for rare/extreme cases (reject sampling).
- BSL license may constrain commercial use.

---

### 2.2 MOSTLY AI SDK (Open Source)

*(See Section 1.2 above — the open-source SDK is Apache-licensed and fully local.)*

- GitHub: https://github.com/mostly-ai/mostlyai
- Stars: ~762, Forks: ~65
- License: Apache v2

---

### 2.3 Gretel Synthetics (Open Source)

*(See Section 1.3 above — the open-source core is Apache-licensed.)*

- GitHub: https://github.com/gretelai/gretel-synthetics
- Stars: ~676, Forks: ~98
- License: Apache v2

---

### 2.4 YData Synthetic

| Attribute | Detail |
|---|---|
| **Website** | https://ydata.ai |
| **GitHub** | https://github.com/ydataai/ydata-synthetic |
| **License** | MIT |
| **PyPI** | `pip install ydata-synthetic` |
| **Primary Audience** | Data scientists / ML engineers |
| **Status** | Primarily educational; creators recommend migrating to `ydata-sdk` for production |

#### What It Does
- Open-source package for generating synthetic tabular and time-series data using generative models.
- Implements a variety of GAN architectures: Vanilla GAN, CGAN, WGAN, WGAN-GP, DRAGAN, Cramer GAN, CWGAN-GP, **CTGAN**, TimeGAN.
- Includes a **Streamlit-based UI** for a guided "low-code" experience: train synthesizers, generate data, profile results.
- Evaluation tools: compare real vs. synthetic data with profile comparisons and fidelity metrics.
- Supports tabular (no temporal dependence) and time-series data.

#### Relevance to Challenge
- CTGAN and conditional generation could be adapted for credit attribute generation.
- Streamlit UI could serve as a quick prototype scaffold.
- No native rule/policy integration.

---

### 2.5 DataSynthesizer

| Attribute | Detail |
|---|---|
| **Origin** | University of Washington |
| **GitHub** | https://github.com/DataResponsibly/DataSynthesizer |
| **License** | MIT |
| **PyPI** | `pip install DataSynthesizer` |
| **Primary Audience** | Researchers, privacy practitioners |

#### What It Does
- Generates synthetic data that mimics the statistical properties of a given input dataset.
- Designed to facilitate safe data sharing and collaboration.

**Architecture — Three Modules**:
1. **DataDescriber**: Computes statistics and builds a data model (metadata).
2. **DataGenerator**: Uses the generated model to synthesize new data samples.
3. **ModelInspector**: Compares original and synthetic datasets using visualizations (histograms, correlation matrices).

**Key Features**:
- **Differential Privacy**: Injects noise into statistical computations (epsilon parameter controls noise level) using Bayesian networks.
- **Bayesian Networks**: Captures relationships and correlations between features.
- **Generation Modes**: Random, independent, and correlated attribute generation.

#### Relevance to Challenge
- Differential privacy with Bayesian network makes it suitable for privacy-safe credit attribute modeling.
- Small, auditable codebase — good for understanding internals.
- No native policy/rule constraint support.

---

### 2.6 SynthCity

| Attribute | Detail |
|---|---|
| **GitHub** | https://github.com/vanderschaarlab/synthcity |
| **License** | Apache v2 |
| **PyPI** | `pip install synthcity` |
| **Primary Audience** | ML researchers, privacy ML practitioners |

#### What It Does
- A comprehensive benchmark and library of synthetic data models.
- Includes implementations of: **PrivBayes**, **DP-GAN**, CTGAN, TVAE, AdsGAN, DECAF, and many more.
- Unified API for training/sampling across all models.
- Built-in benchmarking: fidelity, privacy, and utility metrics.
- Particularly strong for **privacy-preserving** models (PrivBayes is Bayesian-network-based with differential privacy).

#### Relevance to Challenge
- PrivBayes is highly relevant for credit data where inter-attribute correlations (income ↔ credit score ↔ bureau) matter.
- Unified benchmarking API could be used to evaluate multiple generation strategies.
- No rule/policy layer.

---

## 3. Open-Source Libraries — Rule-Based / Fake Data Generators

---

### 3.1 Faker

| Attribute | Detail |
|---|---|
| **GitHub** | https://github.com/joke2k/faker |
| **Stars** | ~18,000+ |
| **License** | MIT |
| **PyPI** | `pip install Faker` |
| **Primary Audience** | Developers, QA engineers |

#### What It Does
- The industry-standard library for generating plausible-looking fake data: names, addresses, emails, phone numbers, dates, credit card numbers (fake), UUIDs, etc.
- Supports 70+ locales.
- Simple, intuitive API.
- Does NOT model statistical correlations — each field is independently random.
- Slower than Mimesis for very large volumes.

#### Relevance to Challenge
- Useful for generating customer identity fields (name, address, SSN) within a larger rule-driven synthetic pipeline.
- Not suitable as a primary generator — no correlations, no rule enforcement.

---

### 3.2 Mimesis

| Attribute | Detail |
|---|---|
| **GitHub** | https://github.com/lk-geimfari/mimesis |
| **Stars** | ~4,000+ |
| **License** | MIT |
| **PyPI** | `pip install mimesis` |
| **Primary Audience** | Developers needing high-volume fake data |

#### What It Does
- High-performance fake data generator — typically 10–15× faster than Faker.
- Highly structured, type-hinted API (great for IDE autocompletion).
- Supports multiple locales.
- Like Faker: no statistical correlations, no rule enforcement.
- Best for populating large test databases where referential accuracy is not critical.

#### Relevance to Challenge
- Could be used for high-volume identity/demographic field generation as a sub-component.

---

### 3.3 factory_boy

| Attribute | Detail |
|---|---|
| **GitHub** | https://github.com/FactoryBoy/factory_boy |
| **Stars** | ~3,500+ |
| **License** | MIT |
| **PyPI** | `pip install factory_boy` |
| **Primary Audience** | Python developers using ORMs (Django, SQLAlchemy) |

#### What It Does
- Fixtures replacement library for Python.
- Generates model instances with defined attribute factories.
- Supports **Sequences**, **LazyAttributes**, **SubFactories** (for related models), and **Traits** (predefined profile sets).
- Works with ORMs and plain Python objects.
- Often used alongside Faker for value generation.

#### Relevance to Challenge
- factory_boy's **Traits** pattern is conceptually similar to "scenario manifests" — defining named profiles (e.g., `approved_borderline_customer`, `fraud_risk_case`).
- Could be the backbone of a scenario-based test data framework when combined with a rule engine.
- Closest existing open-source tool to what the challenge needs conceptually.

---

## 4. Evaluation & Profiling Tools

---

### 4.1 ydata-profiling (formerly pandas-profiling)

| Attribute | Detail |
|---|---|
| **GitHub** | https://github.com/ydataai/ydata-profiling |
| **License** | MIT |
| **PyPI** | `pip install ydata-profiling` |
| **Primary Audience** | Data scientists / analysts |

#### What It Does
- Automated Exploratory Data Analysis (EDA) — generates interactive HTML or JSON reports with a single function call.
- Provides: summary statistics, distribution plots, missing value analysis, correlation matrices, data quality alerts.
- Supports tabular, time-series, text, and image data.
- Useful for automated data quality checks in data pipelines.
- `minimal=True` mode available for large datasets.

#### Relevance to Challenge
- Can be used to generate the **Data Quality Scorecard** output artifact required by the challenge.
- Comparing real vs. synthetic data profiles is a standard use case.

---

## 5. Gap Analysis: What Does NOT Exist Yet

This is the core opportunity space the challenge is targeting. No existing open-source tool fully addresses the following combination:

| Required Capability | Tonic.ai | MOSTLY AI | Gretel | SDV | factory_boy | Faker |
|---|---|---|---|---|---|---|
| Accept credit policy rules / scorecards as input | ❌ | ❌ | ❌ | Partial (constraints) | Partial (traits) | ❌ |
| Deterministic, rule-driven scenario generation | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Rule coverage report output | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Scenario manifest (why each record exists) | ❌ | ❌ | ❌ | ❌ | Partial | ❌ |
| Boundary-condition dataset generation | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Invalid/conflicting rule combination detection | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Explainable linkage: record ↔ rule being tested | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Privacy-safe (no PII) output | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| CI/CD pipeline integration | Partial | Partial | Partial | Partial | ✅ | ✅ |

**Conclusion**: The gap is the **policy/rule input layer** combined with **coverage-driven, explainable, deterministic scenario generation**. All commercial and open-source tools focus on statistical distribution replication from existing data. None are designed specifically for:
- Ingesting a decision rule set / scorecard as a first-class input.
- Systematically generating boundary-probing scenarios from those rules.
- Producing a coverage report mapping test records to the rules they exercise.

---

## 6. Quick Comparison Matrix

| Tool | Type | License | Primary Purpose | Conditional Gen | Rule Constraints | Coverage Report | Privacy | Multi-Table |
|---|---|---|---|---|---|---|---|---|
| **Tonic Fabricate** | Commercial SaaS | Proprietary | Test data from scratch | ✅ (AI agent) | Basic | ❌ | ✅ | ✅ |
| **Tonic Structural** | Commercial SaaS | Proprietary | De-identify prod data | ❌ | Referential integrity | ❌ | ✅ | ✅ |
| **MOSTLY AI Platform** | Commercial SaaS | Proprietary | Statistical synthetic data | ✅ | Soft (temperature) | ❌ | ✅ | ✅ |
| **MOSTLY AI SDK** | Open Source | Apache v2 | Statistical synthetic data | ✅ | Soft (temperature) | HTML quality report | ✅ | ✅ |
| **Gretel.ai Platform** | Commercial SaaS | Proprietary | Statistical synthetic data | ✅ | Privacy only | Quality + privacy | ✅ | Partial |
| **Gretel Synthetics** | Open Source | Apache v2 | Statistical synthetic data | ✅ (ACTGAN) | ❌ | ❌ | ✅ (DP) | ❌ |
| **SDV** | Open Source | BSL | Tabular synthetic data | ✅ | Business constraints | Quality report | ✅ (anonymization) | ✅ |
| **YData Synthetic** | Open Source | MIT | GAN-based synthetic data | ✅ (CTGAN) | ❌ | Profile comparison | ❌ | ❌ |
| **DataSynthesizer** | Open Source | MIT | Privacy-safe data sharing | ❌ | ❌ | Visual comparison | ✅ (DP) | ❌ |
| **SynthCity** | Open Source | Apache v2 | ML model benchmarking | ✅ | ❌ | Benchmark suite | ✅ (PrivBayes) | ❌ |
| **factory_boy** | Open Source | MIT | ORM test fixtures | Partial (traits) | Partial (lazy attrs) | ❌ | ❌ | ✅ (subfactories) |
| **Faker** | Open Source | MIT | Fake data generation | ❌ | ❌ | ❌ | ❌ | ❌ |
| **Mimesis** | Open Source | MIT | High-volume fake data | ❌ | ❌ | ❌ | ❌ | ❌ |
| **ydata-profiling** | Open Source | MIT | Data quality reporting | N/A | N/A | ✅ (EDA report) | N/A | N/A |

---

## Reference Links

### Commercial Vendors
| Name | URL | Purpose |
|---|---|---|
| Tonic.ai — Main Site | https://tonic.ai | Commercial platform overview |
| Tonic.ai — Documentation | https://docs.tonic.ai | Structural, Textual, Fabricate docs |
| Tonic Fabricate Guide | https://docs.tonic.ai/fabricate | Synthesize relational data from scratch |
| Tonic Structural Guide | https://docs.tonic.ai/app | De-identify production data |
| MOSTLY AI — Main Site | https://mostly.ai | Synthetic data platform |
| MOSTLY AI — Synthetic Data SDK | https://mostly.ai/synthetic-data-sdk | SDK overview page |
| MOSTLY AI — SDK Docs | https://mostly-ai.github.io/mostlyai/ | Full SDK documentation |
| MOSTLY AI — Testing & QA use case | https://mostly.ai/use-case/synthetic-test-data-for-software-testing | Testing use case |
| MOSTLY AI — Simulated Data | https://mostly.ai/simulated-data | Simulation use case |
| Gretel.ai — Main Site | https://gretel.ai | Synthetic data platform |
| Gretel.ai — Documentation | https://docs.gretel.ai | Full docs (synthetics, privacy, SDK) |
| Gretel — Blueprints (GitHub) | https://github.com/gretelai/gretel-blueprints | Code examples including financial data |
| Gretel — Synthetics Platform | https://gretel.ai/platform/synthetics | Synthetics product page |

### Open-Source GitHub Repositories
| Name | URL | Stars | License |
|---|---|---|---|
| SDV (Synthetic Data Vault) | https://github.com/sdv-dev/SDV | ~3,500 | BSL |
| MOSTLY AI SDK | https://github.com/mostly-ai/mostlyai | ~762 | Apache v2 |
| Gretel Synthetics | https://github.com/gretelai/gretel-synthetics | ~676 | Apache v2 |
| YData Synthetic | https://github.com/ydataai/ydata-synthetic | ~1,300 | MIT |
| DataSynthesizer | https://github.com/DataResponsibly/DataSynthesizer | ~500 | MIT |
| SynthCity | https://github.com/vanderschaarlab/synthcity | ~1,200 | Apache v2 |
| factory_boy | https://github.com/FactoryBoy/factory_boy | ~3,500 | MIT |
| Faker | https://github.com/joke2k/faker | ~18,000 | MIT |
| Mimesis | https://github.com/lk-geimfari/mimesis | ~4,000 | MIT |
| ydata-profiling | https://github.com/ydataai/ydata-profiling | ~12,000 | MIT |

### Academic / Technical Papers
| Title | URL |
|---|---|
| The Synthetic Data Vault (MIT, 2016 — IEEE DSAA) | https://dai.lids.mit.edu/wp-content/uploads/2018/03/SDV.pdf |
| MOSTLY AI Technical White Paper | https://arxiv.org/abs/2508.00718 |
| TabularARGN architecture paper | https://arxiv.org/abs/2501.12012 |

### SDV Ecosystem Documentation
| Name | URL |
|---|---|
| SDV Docs | https://docs.sdv.dev/sdv |
| SDV Single Table Synthesizers | https://docs.sdv.dev/sdv/single-table-data |
| SDV Multi Table Synthesizers | https://docs.sdv.dev/sdv/multi-table-data |
| SDV Constraints / Business Rules | https://docs.sdv.dev/sdv/concepts/constraints |
| SDV Evaluation | https://docs.sdv.dev/sdv/evaluation |
| DataCebo Blog | https://datacebo.com/blog |
| DataCebo Forum | https://forum.datacebo.com |
