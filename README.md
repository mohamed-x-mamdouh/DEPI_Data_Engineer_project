# E-Commerce Data Integration & Analytics Pipeline

> **Project status:** Planning / requirements draft\
> **Project period:** 16 October 2026 -- 4 December 2026\
> **Team size:** 5 members

## Overview

This proposed end-to-end data engineering project integrates e-commerce
product, customer, order, and transaction data from multiple sources. It
validates, cleans, and transforms the data, loads it into a relational
analytical database, and supports sales analysis through SQL queries and
views.

Apache Airflow is proposed to orchestrate the pipeline. The database
engine, exact data sources, and Airflow execution environment are still
open decisions and must be confirmed with the team and lecturer.

## Problem Statement

E-commerce data can arrive in different formats and contain missing
values, duplicates, inconsistent data types, and updates to existing
records. These issues make reliable reporting difficult. The project
aims to create a repeatable pipeline that standardizes data, checks
quality, stores it centrally, and supports consistent analysis.

## Objectives

-   Ingest product data from an agreed REST API or other permitted
    source, plus customer/order CSV or JSON files.
-   Validate incoming records and document how invalid or incomplete
    data is handled.
-   Clean and transform data using Python and Pandas.
-   Design a relational analytical database and proposed star schema.
-   Use a documented loading and duplicate-handling strategy to support
    safe repeated runs.
-   Create reusable SQL queries/views and justified indexes.
-   Orchestrate tasks with an Apache Airflow DAG, subject to scope
    approval.
-   Include logging, error handling, data-quality checks, tests, and
    reproducible documentation.

## Proposed Architecture

``` text
Product REST API   Customer CSV   Orders JSON
        \               |             /
         +--------------+------------+
                        |
                     Extract
                        |
                Validate & Profile
                        |
               Clean & Transform
                  (Python/Pandas)
                        |
                       Load
                        |
            Relational Analytical Database
                        |
           Star Schema + SQL Views/Queries
                        |
                Sales Analysis/Reporting

Apache Airflow coordinates task order and execution.
```

This is the intended design, not a claim that every component has been
implemented.

## Proposed Data Model

The initial star schema is expected to contain:

-   **Fact Sales** --- sales measures at an agreed grain, provisionally
    one row per order line.
-   **Dim Customer** --- descriptive customer attributes.
-   **Dim Product** --- descriptive product attributes.
-   **Dim Date** --- date attributes for time-based analysis.

Columns, keys, sales calculations, and the treatment of discounts,
returns, tax, and shipping will be finalized during system analysis and
design.

## Proposed Technology Stack

  -----------------------------------------------------------------------
  Technology              Intended use            Status
  ----------------------- ----------------------- -----------------------
  Python                  ETL and validation      Planned
                          logic                   

  Pandas                  Data cleaning and       Planned
                          transformation          

  SQL                     Database operations and Planned
                          analytics               

  Relational database     Analytical storage      Engine not yet
                                                  approved; PostgreSQL is
                                                  a candidate

  Apache Airflow          Pipeline orchestration  Proposed; environment
                                                  undecided

  Git/GitHub              Version control and     Planned
                          collaboration           

  Docker                  Containerization        Optional; deferred
                                                  until core pipeline
                                                  works

  GitHub Actions          Automated checks/CI     Optional; deferred
                                                  until core pipeline
                                                  works
  -----------------------------------------------------------------------

## Proposed Repository Structure

The final layout may differ; this is a starting proposal.

``` text
ecommerce-data-pipeline/
├── dags/                    # Airflow DAG definitions
├── src/
│   ├── extract/
│   ├── validate/
│   ├── transform/
│   └── load/
├── sql/
│   ├── schema.sql
│   └── views.sql
├── data/
│   └── sample/
├── tests/
├── docs/
│   ├── 01_project_planning_management.md
│   ├── 02_literature_review.md
│   └── 03_requirements_gathering.md
├── .env.example
├── .gitignore
└── README.md
```

## Requirements and Setup

The exact installation commands will be added after the team confirms
the database engine, dependency versions, and Airflow environment.
Expected prerequisites include:

-   A compatible Python version and Git.
-   The selected relational database.
-   Apache Airflow in the approved execution environment, if it remains
    in scope.
-   Access to the agreed API, if an API source is selected.

Intended setup workflow:

1.  Clone the repository.
2.  Create and activate a Python virtual environment.
3.  Install dependencies from the finalized dependency file.
4.  Configure database connection details and any API settings through
    environment variables.
5.  Initialize the database using the finalized SQL schema.
6.  Place approved sample CSV/JSON files in the configured input
    location.
7.  Run the ETL pipeline or trigger the Airflow DAG using the final
    environment-specific instructions.

### Environment Variables

The implementation may need variables such as these; names are
placeholders until finalized:

``` dotenv
DATABASE_HOST=
DATABASE_PORT=
DATABASE_NAME=
DATABASE_USER=
DATABASE_PASSWORD=
PRODUCT_API_BASE_URL=
PRODUCT_API_KEY=
```

Only retain variables required by the selected source and database.
Never commit real credentials or API keys. Keep local `.env` files out
of version control and provide a sanitized `.env.example`.

## Intended Pipeline Execution

1.  Extract source data.
2.  Validate incoming records.
3.  Clean and transform the data.
4.  Load records into the relational database.
5.  Run data-quality checks.
6.  Review logs and SQL outputs.

Airflow, when configured, coordinates task dependencies. Python/Pandas
modules perform the data processing itself.

## Data Quality and Reliability

Planned checks include:

-   Required-field and data-type validation.
-   Consistent date, numeric, and text formats.
-   Field-specific handling of missing values.
-   Duplicate detection using agreed business keys.
-   Referential-integrity checks between fact and dimension tables.
-   Checks that repeated runs do not unintentionally duplicate sales.
-   Logging of execution results and clear failure handling.

Invalid-record handling rules will be documented before implementation.
Matching customer or product values alone do not necessarily mean a
record is a duplicate.

## Planned Analytics

The database should support questions such as:

-   What are total sales over a selected period?
-   Which products generate the most sales?
-   How do sales change over time?
-   Which customers contribute most to sales?

Exact metrics and views depend on the agreed sales calculation and
available source fields.

## Testing and Evidence

Planned deliverables include transformation/validation tests,
data-quality checks, SQL verification queries, evidence of successful
pipeline runs and failure handling, an architecture diagram, database
design/ERD, and reproducible setup instructions. Numeric performance
targets will be defined after baseline testing and agreement on data
volume.

## Project Documentation

-   [`01_project_planning_management.md`](docs/01_project_planning_management.md)
    --- objectives, scope, milestones, risks, and proposed team role
    areas.
-   [`02_literature_review.md`](docs/02_literature_review.md) ---
    relevant concepts and technology/design considerations.
-   [`03_requirements_gathering.md`](docs/03_requirements_gathering.md)
    --- stakeholders, user stories, functional/non-functional
    requirements, and open decisions.

Update these links if the team stores the documents elsewhere.

## Timeline

  Milestone                                  Target date
  ------------------------------------------ ------------------
  Project Planning & Management              16 October 2026
  Literature Review                          16 October 2026
  Requirements Gathering                     16 October 2026
  System Analysis & Design                   6 November 2026
  Implementation (Source Code & Execution)   30 November 2026
  Final Presentation, Testing & Reports      4 December 2026

## Open Decisions

-   Confirm minimum mandatory scope with the lecturer.
-   Approve the database engine; PostgreSQL is only a candidate.
-   Select the product API/data source and verify its access terms.
-   Confirm the Airflow execution environment.
-   Finalize repository layout and dependency versions.
-   Agree on fact-table grain, business keys, missing-value rules, and
    sales calculations.
-   Assign responsibilities across the five team members.
-   Define measurable performance targets after baseline testing.

## Team

**Team size:** 5 members\
**Names and responsibilities:** To be assigned.

## Project Status

This README documents the proposed design and planned deliverables. It
does not claim that the pipeline has already been implemented or tested.
Update it as decisions are approved and working functionality becomes
available.
