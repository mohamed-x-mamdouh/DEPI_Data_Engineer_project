# Project Planning & Management

## E-Commerce Data Integration & Analytics Pipeline

> **Status:** Draft for team & instructor review  
> **Project dates:** 16 October 2026 -- 4 December 2026  
> **Team size:** 5 members  
> **Repository:** [GitHub Repo](https://github.com/mohamed-x-mamdouh/DEPI_Data_Engineer_project)

## 1. Project Overview

The project aims to build an automated end-to-end data pipeline for an
e-commerce analytics use case. Data will be collected from multiple
sources, validated, cleaned, transformed, and loaded into a relational
database organized for analytical queries. Apache Airflow will
orchestrate the pipeline, subject to confirmation of the execution
environment.

## 2. Problem Statement

E-commerce data may arrive through different sources and formats,
including REST APIs, CSV files, and JSON files. The data can contain
duplicates, missing values, inconsistent formats, and updates to
existing records. These issues make reliable sales reporting difficult.

The project will address this problem by creating a repeatable pipeline
that standardizes incoming data, applies data-quality checks, loads the
data into a centralized analytical model, and exposes common reporting
queries through SQL views.

## 3. Objectives

1.  Collect product, customer, order, and transaction data from agreed
    sources.
2.  Validate incoming records and document how invalid or incomplete
    data is handled.
3.  Clean and transform data using Python and Pandas.
4.  Design and implement a relational database and a sales-oriented star
    schema.
5.  Support safe repeated runs and incremental or append-based loading,
    with duplicate prevention appropriate to the selected strategy.
6.  Create SQL views for common analytical questions and indexes where
    justified.
7.  Orchestrate Extract → Validate → Transform → Load → Quality Check
    using an Airflow DAG.
8.  Provide logging, retry/error handling, tests, setup instructions,
    and evidence of successful and failed-run handling.

## 4. Scope

### 4.1 Proposed in-scope features

- Product data ingestion from a REST API or permitted scraping source.
- Customer/order ingestion from CSV/JSON files.
- Data validation, duplicate handling, missing-value handling, and
  format/type standardization.
- Pandas-based joins and transformations.
- Relational database design and implementation.
- Star schema with Fact Sales, Dim Customer, Dim Product, and Dim
  Date.
- Incremental or append-based loading, with a documented
  duplicate-handling strategy.
- SQL analytical views and justified indexes.
- Airflow DAG for pipeline orchestration.
- Logging, retries, and error handling.
- Python modules, SQL scripts, sample datasets, data-quality checks,
  documentation, architecture diagram, and screenshots/evidence of
  Airflow runs.

### 4.2 Optional extensions

- Docker containerization.
- GitHub Actions CI/CD workflow.

### 4.3 Scope decisions requiring confirmation

The proposal lists a broad set of features, but the team has not yet
confirmed whether every feature is mandatory for the DEPI assessment.
Ask the lecturer/team lead to confirm the required minimum scope. Until
then, treat the features in Section 4.1 as the **proposed target
scope**, not as a confirmed grading requirement.

The following decisions are also open:

- **Database engine:** PostgreSQL, MySQL, SQL Server, or another approved option.
- **Airflow execution environment:** Docker or local installation.
- **API/data source:** Exact source and access terms.
- **Data volume:** Expected volume and measurable performance targets.
- **Team members:** Names and individual responsibilities.

## 5. Project Timeline and Milestones

---

| Milestone                                    | Target date | Expected deliverable                                                                  |
| -------------------------------------------- | ----------: | ------------------------------------------------------------------------------------- |
| **Project Planning & Management**            | 16 Oct 2026 | Proposal, timeline, task allocation, risk register, and KPIs                          |
| **Literature Review**                        | 16 Oct 2026 | Source-backed review, technology rationale, and feedback/improvement log              |
| **Requirements Gathering**                   | 16 Oct 2026 | Stakeholders, user stories/use cases, and functional and non-functional requirements  |
| **System Analysis & Design**                 |  6 Nov 2026 | Architecture, diagrams, database design, data-flow plan, and testing/deployment plans |
| **Implementation (Source Code & Execution)** | 30 Nov 2026 | Working pipeline, database scripts, Airflow DAG, README, and execution evidence       |
| **Final Presentation, Testing & Reports**    |  4 Dec 2026 | Test results, bug reports, technical/user documentation, presentation, and demo       |

These dates come from the project guideline. The team should add
intermediate tasks and owners in a Gantt chart after confirming
individual availability.

## 6. Team Roles and Task Allocation

Team size is five. Names and skills have not yet been provided, so roles
below are **proposed role areas**, not assigned people. One person may
cover more than one area, and responsibilities should be balanced after
a team discussion.

---

| Role area                                | Main responsibilities                                                                           | Assigned member |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------- | --------------- |
| **Project coordination & documentation** | Maintain the timeline, meeting notes, requirements, and README                                  | To be assigned  |
| **Data ingestion**                       | Select sources and implement API/CSV/JSON extraction                                            | To be assigned  |
| **Data cleaning & transformation**       | Define validation rules, implement Pandas transformations and joins, and perform quality checks | To be assigned  |
| **Database & analytics**                 | Design the schema and star schema; create SQL scripts, views, and indexes                       | To be assigned  |
| **Orchestration & testing**              | Develop the Airflow DAG; manage logging/retries, integration tests, and execution evidence      | To be assigned  |

All members should review the architecture, understand the end-to-end
pipeline, use Git/GitHub, and prepare to explain their contributions.

## 7. Risk Assessment and Mitigation

---

| Risk                                                    | Likelihood / impact | Mitigation                                                                                             |
| ------------------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------ |
| **API is unavailable, rate-limited, or changes format** | Medium / High       | Choose a documented API, handle timeouts/retries, and save sample responses for repeatable tests.      |
| **Source data is incomplete or inconsistent**           | High / High         | Define validation rules, quarantine or report invalid rows, and track rejected-record counts.          |
| **Duplicate records inflate sales**                     | Medium / High       | Define stable keys, use a safe loading strategy, and test repeated runs.                               |
| **Database/Airflow setup is difficult on team laptops** | Medium / High       | Decide the environment early, test it on one machine, and document dependencies.                       |
| **Scope is too large for the deadline**                 | Medium / High       | Confirm mandatory features with the lecturer; prioritize the core pipeline before optional extensions. |
| **Team responsibilities are unclear**                   | Medium / Medium     | Assign owners and backups; review progress at regular check-ins.                                       |
| **Secrets are accidentally committed to GitHub**        | Low / High          | Use environment variables and an example environment file; never commit real credentials.              |
| **Documentation differs from implementation**           | Medium / Medium     | Update docs when requirements, schema, or execution steps change.                                      |

Likelihood/impact ratings are initial estimates and should be reviewed
by the team.

## 8. Proposed KPIs and Acceptance Targets

The targets below are **proposals to validate**, not official DEPI
grading criteria. Final targets should be agreed before testing and
should match the available test environment.

---

| KPI                       | Proposed measurement / target                                                                               |
| ------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Pipeline completion**   | At least 95% of planned test runs complete successfully, excluding intentionally induced failures.          |
| **Duplicate safety**      | Re-running the same input does not increase the number or sales value of already loaded business records.   |
| **Data validation**       | Every input record is classified as accepted or rejected/quarantined; counts reconcile to the input total.  |
| **Referential integrity** | 100% of loaded fact rows resolve to valid dimension keys or an explicitly documented unknown-member policy. |
| **Reproducibility**       | A new team member can run the project using the README and sample data.                                     |
| **Performance**           | Measure runtime on a documented sample dataset and set a numeric target after a baseline run.               |
| **Error visibility**      | Failed tasks produce a useful log/error message and are visible in Airflow.                                 |

## 9. Project Management Approach

- Track tasks and blockers in a shared board or GitHub Issues.
- Use feature branches and meaningful commit messages.
- Review scope and risks at each milestone.
- Store project documentation in the GitHub repository.
- Record decisions that affect tools, schema, data sources, and
  acceptance criteria.

## 10. Immediate Actions

1.  Confirm with the lecturer whether all proposed features are required
    or whether a minimum viable scope is acceptable.
2.  Assign names to the five role areas.
3.  Select the database engine and Airflow execution environment.
4.  Choose a suitable API and define sample CSV/JSON structures.
5.  Agree on measurable KPIs and maintain this document as decisions are
    finalized.
