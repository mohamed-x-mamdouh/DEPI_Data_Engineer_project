# Requirements Gathering

## E-Commerce Data Integration & Analytics Pipeline

> **Status:** Draft requirements for review  
> **Project dates:** 16 October 2026 – 4 December 2026  
> **Team size:** 5 members

---

### Contents

1. [Purpose and Scope](#1-purpose-and-scope)
2. [Stakeholder Analysis](#2-stakeholder-analysis)
3. [User Stories](#3-user-stories)
4. [Use Cases](#4-use-cases)
5. [Functional Requirements](#5-functional-requirements)
6. [Non-functional Requirements](#6-non-functional-requirements)
7. [Data Rules Requiring Agreement](#7-data-rules-requiring-agreement)
8. [Requirements Traceability](#8-requirements-traceability)
9. [Assumptions and Open Decisions](#9-assumptions-and-open-decisions)
10. [Approval Checklist](#10-approval-checklist)

---

## 1. Purpose and Scope

This document translates the project proposal into:

- **Stakeholders and project responsibilities**
- **User stories and use cases**
- **Functional requirements**
- **Non-functional requirements**

> **Review note:** Requirements marked **Proposed** must be reviewed by the team and lecturer before being treated as final commitments.

The system will:

- Ingest **product, customer, order, and transaction data** from multiple sources.
- **Validate and transform** incoming data.
- Load processed data into a **relational analytical model**.
- Provide **SQL views** for reporting.
- Use **Airflow** as the proposed orchestration tool.

---

## 2. Stakeholder Analysis

| Stakeholder                   | Need / interest                                        | How the project addresses it                                        |
| ----------------------------- | ------------------------------------------------------ | ------------------------------------------------------------------- |
| Data Engineer / Operator      | Reliable, repeatable pipeline and visible run failures | Modular ETL, Airflow DAG, logs, and retries                         |
| Data Analyst                  | Consistent, queryable sales data                       | Central database, star schema, and SQL views                        |
| Project team                  | Clear responsibilities and reproducible setup          | GitHub repository, README, modular code, and documented workflow    |
| Lecturer / Evaluator          | Evidence that requirements are implemented and tested  | Requirements traceability, test results, diagrams, and presentation |
| External data-source provider | Respectful and permitted access to data                | Use documented APIs or scraping only where permitted                |

---

---

## 3. User Stories

| ID        | User story                                                                                                                                | Priority                              |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| **US-01** | As a Data Engineer, I want to ingest product data from an agreed REST API so that product information is available to the pipeline.       | **Must-have** (proposal)              |
| **US-02** | As a Data Engineer, I want to ingest customer and order data from CSV/JSON files so that data from different sources can be combined.     | **Must-have** (proposal)              |
| **US-03** | As a Data Engineer, I want to validate incoming records so that invalid data is detected before loading.                                  | **Must-have** (proposal)              |
| **US-04** | As a Data Engineer, I want to standardize data types and formats so that records can be joined consistently.                              | **Must-have** (proposal)              |
| **US-05** | As a Data Engineer, I want duplicate and missing-value rules to be applied consistently so that data-quality issues are controlled.       | **Must-have** (proposal)              |
| **US-06** | As a Data Analyst, I want sales data organized by customer, product, and date so that I can analyze sales using SQL.                      | **Must-have** (proposal)              |
| **US-07** | As a Data Engineer, I want the pipeline to be orchestrated as an Airflow DAG so that tasks run in the correct order and can be monitored. | **Proposed; confirm mandatory scope** |
| **US-08** | As a Data Engineer, I want safe repeated or incremental loads so that rerunning the pipeline does not duplicate sales.                    | **Must-have** (proposal)              |
| **US-09** | As a Data Analyst, I want reusable SQL views for common sales questions so that I do not need to rewrite the same joins each time.        | **Must-have** (proposal)              |
| **US-10** | As a Project Evaluator, I want setup instructions and test evidence so that I can reproduce and assess the project.                       | **Must-have** (proposal)              |
| **US-11** | As a Data Engineer, I want automated tests in a CI workflow so that changes are checked before merging.                                   | **Optional extension**                |

> **Note:** Priorities describe the proposed project scope, not an official DEPI grading classification. Confirm them with the lecturer.

---

---

## 4. Use Cases

### UC-01: Run the Data Pipeline

- **Primary actor:** Data Engineer / Operator
- **Goal:** Process available source data into the analytical

  database.

- **Preconditions:** Source configuration is available; database

  credentials are configured securely; schema exists.

- **Main flow:**

  1.  The operator triggers the Airflow DAG or a documented local run.

  2.  The system extracts data from the configured API and input

      files.

  3.  The system validates required fields, formats, and key values.

  4.  The system transforms and joins accepted records.

  5.  The system loads data using the selected duplicate-safe

      strategy.

  6.  The system runs post-load quality checks.

  7.  The system logs task status and processing counts.

- **Alternative / failure flows:**

  - If a source is temporarily unavailable, the configured retry

    policy is applied.

  - If a record fails validation, it is rejected or quarantined

    according to documented rules.

  - If a load or quality check fails, the run is marked failed and

    the error is logged.

- **Postconditions:** The run has a recorded status; accepted data is

  loaded only according to the defined loading rules.

### UC-02: Query Sales Analytics

- **Primary actor:** Data Analyst
- **Goal:** Analyze sales by product, customer, or date.
- **Preconditions:** The pipeline has loaded valid data and the

  analytical views exist.

- **Main flow:**

  1.  The analyst queries a documented SQL view.

  2.  The view joins the relevant fact and dimension tables.

  3.  The query returns results at the documented grain.

- **Postconditions:** Results are available for analysis, with metric

  definitions documented.

### UC-03: Verify Data Quality

- **Primary actor:** Data Engineer / Evaluator
- **Goal:** Confirm that loaded data satisfies defined quality rules.
- **Main flow:**

  1.  Execute quality checks.

  2.  Compare input, accepted, rejected, and loaded counts.

  3.  Check key uniqueness and referential integrity.

  4.  Verify that repeating the same load does not inflate sales.

  5.  Record pass/fail results and investigate failures.

---

## 5. Functional Requirements

| ID        | Requirement                                                                                                                           | Acceptance criterion                                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **FR-01** | The system shall extract product data from one agreed REST API or another permitted source.                                           | A repeatable test produces a saved or processed response; errors are reported.                                                   |
| **FR-02** | The system shall ingest customer/order data from CSV and/or JSON files.                                                               | Valid sample files are read and their records are counted. Confirm whether both formats are mandatory.                           |
| **FR-03** | The system shall validate required fields, key values, and supported data types.                                                      | Test records with valid and invalid values are classified according to documented rules.                                         |
| **FR-04** | The system shall detect duplicate records using defined business keys.                                                                | A test duplicate is identified and does not inflate loaded business totals.                                                      |
| **FR-05** | The system shall handle missing values according to field-specific rules.                                                             | Each required field has a documented reject, default, derive, or allow-null rule.                                                |
| **FR-06** | The system shall standardize supported data types and formats.                                                                        | Dates, identifiers, and numeric fields follow the documented target schema.                                                      |
| **FR-07** | The system shall perform required joins and transformations using Python/Pandas or an approved equivalent.                            | A known sample dataset produces expected transformed values and join results.                                                    |
| **FR-08** | The system shall store processed data in a relational database.                                                                       | Schema scripts create the required tables and sample records can be queried.                                                     |
| **FR-09** | The system shall implement Fact Sales, Dim Customer, Dim Product, and Dim Date.                                                       | Tables and key relationships match the approved star-schema design.                                                              |
| **FR-10** | The system shall support the agreed loading strategy: incremental or append-based.                                                    | The loading behavior is documented and a repeated-run test verifies duplicate safety.                                            |
| **FR-11** | The system shall provide SQL views for common analytical queries.                                                                     | At least the agreed views return correct results for a controlled test dataset.                                                  |
| **FR-12** | The system shall add indexes to selected frequently queried fields where justified.                                                   | The chosen indexes and rationale are documented; query behavior is checked where practical.                                      |
| **FR-13** | The system shall orchestrate Extract → Validate → Transform → Load → Quality Check using an Airflow DAG.                              | The DAG displays the defined dependencies and a test run reaches the expected final state. Confirm whether Airflow is mandatory. |
| **FR-14** | The system shall log relevant processing information and handle configured retries/errors.                                            | An induced failure is visible in logs and follows the documented retry/failure behavior.                                         |
| **FR-15** | The repository shall include source modules, SQL scripts, sample data, data-quality checks, architecture documentation, and a README. | A reviewer can locate the deliverables and follow the setup/execution instructions.                                              |
| **FR-16** | The project shall provide evidence of successful pipeline execution.                                                                  | Screenshots or equivalent evidence show a successful run and its results.                                                        |
| **FR-17** | The project may use Docker and GitHub Actions as optional extensions.                                                                 | If implemented, the team documents the setup and shows a successful container run or CI check.                                   |

---

---

## 6. Non-functional Requirements

The following are proposed and should be confirmed against the available environment. Numeric targets should be finalized after a baseline test.

| ID         | Quality attribute         | Proposed measurable criterion                                                                                                          |
| ---------- | ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **NFR-01** | Reliability               | At least 95% of planned test runs succeed, excluding tests intentionally designed to fail. Record the total number of runs.            |
| **NFR-02** | Data integrity            | No duplicate business sales are introduced by re-running the same input; key and relationship checks pass.                             |
| **NFR-03** | Data-quality transparency | Each input record is accounted for as accepted, rejected, or quarantined; counts reconcile to the input total.                         |
| **NFR-04** | Performance               | Record processing time for a documented sample dataset and machine configuration; agree on a numeric threshold after the baseline run. |
| **NFR-05** | Maintainability           | Extraction, validation, transformation, loading, and quality checks are separated into understandable modules.                         |
| **NFR-06** | Security                  | Credentials are provided through environment configuration or another secure mechanism; real secrets are not committed to GitHub.      |
| **NFR-07** | Reproducibility           | A team member who did not implement a task can set up and run the sample pipeline using the README.                                    |
| **NFR-08** | Observability             | Each pipeline run provides task status and useful logs for failures and data-quality results.                                          |
| **NFR-09** | Compatibility             | Supported software versions and required dependencies are documented and tested on the agreed environment.                             |
| **NFR-10** | Documentation             | Requirements, schema, run instructions, known limitations, and test evidence are maintained in GitHub.                                 |

---

---

## 7. Data Rules Requiring Agreement

> **Decision required:** These rules must be decided before final implementation.

1. **Duplicate definition:** Identify business keys for products, customers, orders, and order lines. Identical rows and repeated legitimate purchases must not be confused.
2. **Missing values:** Define field-specific behavior; do not replace all missing values with one arbitrary default.
3. **Invalid records:** Choose whether each rule causes rejection, quarantine, or correction. Record the reason.
4. **Fact Sales grain:** The proposed grain is one product line within one order. Confirm that the source data supports this.
5. **Sales calculation:** Define whether sales amount is quantity × unit price and how discounts, refunds, tax, and shipping are handled if present.
6. **Incremental logic:** Define how new and changed records are identified. Do not assume a source update timestamp exists until verified.
7. **Unknown dimension keys:** Decide how facts with unmatched product or customer references are handled.
8. **Data retention and privacy:** Use permitted sample data and avoid unnecessary personal or sensitive customer details.

---

## 8. Requirements Traceability

Use this mapping to keep design, implementation, and testing aligned.

| Requirement group                           | Design evidence                             | Implementation evidence                   | Test evidence                                             |
| ------------------------------------------- | ------------------------------------------- | ----------------------------------------- | --------------------------------------------------------- |
| **FR-01–FR-02: Ingestion**                  | Source/interface design and DFD             | Extract modules and sample inputs         | Valid source/file tests and source-error test             |
| **FR-03–FR-07: Quality and transformation** | Validation rules and transformation flow    | Python/Pandas modules                     | Valid/invalid, duplicate, missing-value, and join tests   |
| **FR-08–FR-12: Database and analytics**     | ERD, schema, star schema, SQL design        | DDL, load scripts, views, indexes         | Key integrity, expected query results, repeated-load test |
| **FR-13–FR-14: Orchestration**              | Architecture and sequence/activity diagrams | Airflow DAG, logging, retry configuration | Successful DAG run and induced-failure test               |
| **FR-15–FR-17: Delivery and extensions**    | Repository/deployment plan                  | README, sample data, optional Docker/CI   | Reproducibility check and execution evidence              |

---

---

## 9. Assumptions and Open Decisions

The following assumptions and decisions remain open:

- The team has five members, but names and responsibilities are not

  assigned yet.

- Python, SQL, Pandas, and Git/GitHub have been studied.
- The database engine and Airflow execution environment are not

  selected.

- The data-source selection is still open; CSV/JSON files are

  expected, and an API source remains to be confirmed.

- No lecturer feedback has been received yet.
- The proposed scope is based on the project description. The team

  must confirm whether all features are mandatory.

- Docker and GitHub Actions are treated as optional because the

  project proposal labels them optional.

- Performance thresholds and data volume are not yet specified and

  should be agreed after a baseline test.

---

## 10. Approval Checklist

Before the requirements are considered final, confirm each item below:

- [ ] Lecturer/team lead has clarified mandatory versus optional features.
- [ ] Database engine and Airflow execution method are selected.
- [ ] API and sample data formats are selected.
- [ ] Team members and responsibilities are assigned.
- [ ] Data-quality and duplicate-handling rules are agreed.
- [ ] Fact Sales grain and sales calculation rules are agreed.
- [ ] Measurable acceptance targets are approved.
