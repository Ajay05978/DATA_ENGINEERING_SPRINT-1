# Week Map

All 15 weeks at a glance. This file is built from the week files in `docs/_week-templates/`. If a week file and this map disagree, the week file wins.

Every week has the same six parts: What We Are Doing, Why We Are Doing It, What to Learn First, Where to Find Resources, Step-by-Step Guide, What to Commit. Open your own `students/DEx/weekN/problem_statement.md` for the real tasks.

| Week | Theme | Task groups | Task IDs |
|---|---|---|---|
| 1 | Set up your tools and understand the ask | Set up your tools; Discovery Brief **[MILESTONE]**; Source Summaries and the Client Ask | G0.1, G0.2, G0.3, G0.4, B1.1, B1.2, A1.1, B2.1, B2.2 |
| 2 | First tables and first Python | Databases and Tables; Python From Zero | S1.1, S1.2, S1.3, P0.1, P0.2, P0.3, P0.4 |
| 3 | Python reads files; trust the data | Python Basics: Files and Functions; Data Integrity: Constraints | P1.1, P1.2, P1.3, S2.1, S2.2, S2.3 |
| 4 | Queries, joins, and checking data with Python | Queries, Grouping and Joins; Type Hints and Data Validation | S3.1, S3.2, S3.3, S3.4, S3.5, P2.1, P2.2, P2.3 |
| 5 | Missing values, messy strings | Missing Values and Set Operations; SQL Functions: String, Date, Math | S4.1, S4.2, S4.3, S4.4, S5.1, S5.2, S5.3, S5.4, S5.5 |
| 6 | First load, and the design begins | First Snowflake Load [MILESTONE]; Design Record, ERD and Architecture; Pipeline Flow Diagram | P3.1, P3.2, P3.3, B3.1, B3.2, B3.3, A2.1 |
| 7 | Readable SQL and the agreed model | Views and CTEs; Star Schema Design [MILESTONE]; Retry Logic and Context Managers | S6.1, S6.2, S6.3, S7.1, S7.2, S7.3, P4.1, P4.2, P4.3 |
| 8 | Big files and cleaner tooling | Normalization; Generators and Large Files; Logging and Command Line Tools | S8.1, S8.2, P5.1, P5.2, P5.3, P6.1, P6.2 |
| 9 | Extractors and the design sign-off | Object Oriented Extractors; Design Review from the Python Side [MILESTONE]; Design Review Update | P7.1, P7.2, P7.3, P8.1, A3.1 |
| 10 | Window functions and tests | Window Functions; Testing with pytest | S9.1, S9.2, S9.3, S9.4, P9.1, P9.2, P9.3 |
| 11 | History that survives | Slowly Changing Dimensions (SCD2) [MILESTONE]; Parallel Downloads; Python and Snowflake Together; Technical Brief [MILESTONE] | S10.1, S10.2, S10.3, P10.1, P10.2, P11.1, P11.2, D1.1, D1.2 |
| 12 | The shared platform begins | Stages and File Formats; Bronze Loads; Teach-Back: the Load Path **[MILESTONE]**; dbt Staging Models | D2.1, D2.2, D2.3, D3.1, D3.2, D3.3, D4.1, D5.1, D5.2, D5.3 |
| 13 | Gold layer and the monthly refresh | Layer Contracts; Gold Layer with dbt; The Monthly Refresh **[MILESTONE]** | D6.1, D6.2, D7.1, D7.2, D7.3, P12.1, P12.2 |
| 14 | Orchestrate, break, fix | Quality Gates with Great Expectations; Airflow Orchestration; Peer Review; Break, Fix and Postmortem | D8.1, D8.2, D8.3, D9.1, D9.2, B4.1, B4.2, B5.1, B5.2 |
| 15 | Present and hand over | Dashboard; Stakeholder Delivery; Final Architecture for Handover; Project Handover [MILESTONE] | B6.1, B6.2, B7.1, B7.2, A4.1, B8.1, B8.2, D10.1, D10.2 |

## Week by week

### Week 1 - Set up your tools and understand the ask

Theme: set up your tools, understand the client, and make your first page and diagram

- **Set up your tools**: G0.1, G0.2, G0.3, G0.4
- **Discovery Brief **[MILESTONE]****: B1.1, B1.2, A1.1
- **Source Summaries and the Client Ask**: B2.1, B2.2

Full instructions: `students/DEx/week1/problem_statement.md` (source: `docs/_week-templates/week1.md`).

### Week 2 - First tables and first Python

Theme: make your first Snowflake database and table, then write your first tiny Python scripts

- **Databases and Tables**: S1.1, S1.2, S1.3
- **Python From Zero**: P0.1, P0.2, P0.3, P0.4

Full instructions: `students/DEx/week2/problem_statement.md` (source: `docs/_week-templates/week2.md`).

### Week 3 - Python reads files; trust the data

Theme: read real files in Python, then check the data before you trust it

- **Python Basics: Files and Functions**: P1.1, P1.2, P1.3
- **Data Integrity: Constraints**: S2.1, S2.2, S2.3

Full instructions: `students/DEx/week3/problem_statement.md` (source: `docs/_week-templates/week3.md`).

### Week 4 - Queries, joins, and checking data with Python

Data Sprint 1 | Week 4 of 15 | Theme: answer questions with SQL joins, then check records with Python

- **Queries, Grouping and Joins**: S3.1, S3.2, S3.3, S3.4, S3.5
- **Type Hints and Data Validation**: P2.1, P2.2, P2.3

Full instructions: `students/DEx/week4/problem_statement.md` (source: `docs/_week-templates/week4.md`).

### Week 5 - Missing values, messy strings

Theme: handle what is missing and clean what is messy, so the data says what it means

- **Missing Values and Set Operations**: S4.1, S4.2, S4.3, S4.4
- **SQL Functions: String, Date, Math**: S5.1, S5.2, S5.3, S5.4, S5.5

Full instructions: `students/DEx/week5/problem_statement.md` (source: `docs/_week-templates/week5.md`).

### Week 6 - First load, and the design begins

Data Sprint 1 - Week 6 of 15 - Theme: get every file into Snowflake with counts that match, then start designing the gold layer

- **First Snowflake Load [MILESTONE]**: P3.1, P3.2, P3.3
- **Design Record, ERD and Architecture**: B3.1, B3.2, B3.3
- **Pipeline Flow Diagram**: A2.1

Full instructions: `students/DEx/week6/problem_statement.md` (source: `docs/_week-templates/week6.md`).

### Week 7 - Readable SQL and the agreed model

Theme: write SQL someone else can review, then agree the shape of the gold layer

- **Views and CTEs**: S6.1, S6.2, S6.3
- **Star Schema Design [MILESTONE]**: S7.1, S7.2, S7.3
- **Retry Logic and Context Managers**: P4.1, P4.2, P4.3

Full instructions: `students/DEx/week7/problem_statement.md` (source: `docs/_week-templates/week7.md`).

### Week 8 - Big files and cleaner tooling

Theme: read a large CSV one row at a time without loading the whole file, then make code a teammate can debug

- **Normalization**: S8.1, S8.2
- **Generators and Large Files**: P5.1, P5.2, P5.3
- **Logging and Command Line Tools**: P6.1, P6.2

Full instructions: `students/DEx/week8/problem_statement.md` (source: `docs/_week-templates/week8.md`).

### Week 9 - Extractors and the design sign-off

Theme: get real data out of a PDF and a web page, then put what you learned back into the design

- **Object Oriented Extractors**: P7.1, P7.2, P7.3
- **Design Review from the Python Side [MILESTONE]**: P8.1
- **Design Review Update**: A3.1

Full instructions: `students/DEx/week9/problem_statement.md` (source: `docs/_week-templates/week9.md`).

### Week 10 - Window functions and tests

Theme: Compare rows with window functions and check Python behaviour with automated tests

- **Window Functions**: S9.1, S9.2, S9.3, S9.4
- **Testing with pytest**: P9.1, P9.2, P9.3

Full instructions: `students/DEx/week10/problem_statement.md` (source: `docs/_week-templates/week10.md`).

### Week 11 - History that survives

Data Sprint 1 - Week 11 of 15. Theme: apply a month of changes without destroying last month's truth, then write the technical brief

- **Slowly Changing Dimensions (SCD2) [MILESTONE]**: S10.1, S10.2, S10.3
- **Parallel Downloads**: P10.1, P10.2
- **Python and Snowflake Together**: P11.1, P11.2
- **Technical Brief [MILESTONE]**: D1.1, D1.2

Full instructions: `students/DEx/week11/problem_statement.md` (source: `docs/_week-templates/week11.md`).

### Week 12 - The shared platform begins

Data Sprint 1 - Week 12 of 15 - Theme: stop practising separately and build the one real pipeline

- **Stages and File Formats**: D2.1, D2.2, D2.3
- **Bronze Loads**: D3.1, D3.2, D3.3
- **Teach-Back: the Load Path **[MILESTONE]****: D4.1
- **dbt Staging Models**: D5.1, D5.2, D5.3

Full instructions: `students/DEx/week12/problem_statement.md` (source: `docs/_week-templates/week12.md`).

### Week 13 - Gold layer and the monthly refresh

Theme: build the tables the client uses, then run the monthly refresh that keeps them correct

- **Layer Contracts**: D6.1, D6.2
- **Gold Layer with dbt**: D7.1, D7.2, D7.3
- **The Monthly Refresh **[MILESTONE]****: P12.1, P12.2

Full instructions: `students/DEx/week13/problem_statement.md` (source: `docs/_week-templates/week13.md`).

### Week 14 - Orchestrate, break, fix

Theme: schedule the pipeline, then break it on purpose and prove you can find it

- **Quality Gates with Great Expectations**: D8.1, D8.2, D8.3
- **Airflow Orchestration**: D9.1, D9.2
- **Peer Review**: B4.1, B4.2
- **Break, Fix and Postmortem**: B5.1, B5.2

Full instructions: `students/DEx/week14/problem_statement.md` (source: `docs/_week-templates/week14.md`).

### Week 15 - Present and hand over

Theme: make the work visible to the client, then hand it to a stranger

- **Dashboard**: B6.1, B6.2
- **Stakeholder Delivery**: B7.1, B7.2
- **Final Architecture for Handover**: A4.1
- **Project Handover [MILESTONE]**: B8.1, B8.2, D10.1, D10.2

Full instructions: `students/DEx/week15/problem_statement.md` (source: `docs/_week-templates/week15.md`).

