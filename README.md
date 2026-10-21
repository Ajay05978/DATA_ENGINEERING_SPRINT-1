# Data Sprint 1

**India Company Risk and Verification Data Platform · 15 weeks · Snowflake, dbt, Python, Airflow, Great Expectations, Metabase**

Welcome. This is the first project of the data engineering program. Over 15 weeks the cohort builds one real data platform together.

---

## Read these in this order

| # | File | When |
|---|---|---|
| 1 | `docs/00-START-HERE.md` | right now, before anything else |
| 2 | `docs/01-project-brief.md` | before week 1 begins |
| 3 | `docs/02-tools-setup.md` | before week 1 begins, with a terminal open |
| 4 | `docs/03-student-guide.md` | before your first commit, then whenever you forget a command |
| 5 | `docs/14-vscode-snowflake-setup-guide.md` | week 1, with tasks G0.1 to G0.4: Git, VS Code, Snowflake, your first commit |
| 6 | `docs/15-team-project-repo-and-delivery.md` | week 1, then whenever you work on the team project or the `delivery/` folder |
| 7 | `students/DEx/week1/problem_statement.md` | when week 1 opens, and every week after |

Reference files, open them when something points you at them:

| File | Open it when |
|---|---|
| `docs/04-week-map.md` | you want to see the whole 15 weeks at once |
| `docs/05-task-list.md` | you want the full task list |
| `docs/06-team-roles.md` | you want to know who reviews what and how rotations work |
| `docs/07-platform-and-cicd-guide.md` | you are on the platform rotation this week |
| `docs/08-glossary.md` | a document uses a word you do not know |
| `docs/09-resources.md` | you need the reading or video for this week's topic |
| `docs/10-troubleshooting.md` | Git, Snowflake, dbt, or Python breaks on you |
| `docs/11-tools-and-technology.md` | you want to know what a tool is for and why we chose it |
| `docs/platform-rotation-log.md` | you need to record your platform rotation turn |
| `CONTRIBUTING.md` | before your first pull request |

---

## What we are building, in four sentences

The client is a fintech due diligence company. Their analysts need to check any registered company in India: is it real and active, has it ever been in insolvency proceedings, and how does it compare to other companies in its state.

We bring four Indian public data sources into one platform to answer that: the MCA company registry, IBBI insolvency records, the MCA CDM statistics portal, and RBI policy rates. The platform refreshes on a schedule, keeps status history, and ends in a dashboard a non-technical analyst can use.

Full story, sources, and the reasoning behind every design choice: `docs/01-project-brief.md`.

---

## Two design words you will hear every week

* **Medallion pipeline** means data moves through three layers. **Bronze** holds it exactly as it arrived. **Silver** holds the cleaned version. **Gold** holds the final tables the dashboard uses.
* **Kimball star schema** is how we design those gold tables: fact tables for events, dimension tables for the things those events are about.

Both are explained in plain language in `docs/01-project-brief.md`, section 4, and defined in `docs/08-glossary.md`. You are not expected to know them yet.

---

## The 15 weeks at a glance

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

Week by week detail: `docs/04-week-map.md`.

---

## What is in each folder

| Folder | What it holds | Who writes in it |
|---|---|---|
| `students/DEx/weekY/` | One practice folder per cohort member, one subfolder per week | You, in your own folder only |
| `platform/` | The one real shared pipeline: raw, staging, marts, quality, orchestration, dbt | Only that week's platform rotation |
| `delivery/` | Shared outputs: discovery, design, dashboard, presentation | Only that week's platform rotation |
| `docs/` | Everything in the reading order above | Core admins, plus students when a task says so |
| `admin/` | A pointer only. No answer keys live in this repository. | Core admins |

---

## Your first hour

1. Read `docs/00-START-HERE.md` end to end. It is short.
2. Follow `docs/02-tools-setup.md` and get Git, Python, and a Snowflake login working.
3. Fork and clone this repository using `docs/03-student-guide.md`, section 1.
4. Find your own folder, for example `students/DE12/`, and open `week1/problem_statement.md`.
5. Post in the cohort Discord channel that you are set up, and say which DE number you are.

---

## Where to get help

Ask in the cohort Discord channel. Say what you were trying to do, what you tried, what happened, and paste the error. Ask for a hint, not for the finished answer.

Getting stuck is part of the work. Telling the group where you are stuck is the fastest way through it. If the problem looks like a tooling failure rather than a thinking failure, check `docs/10-troubleshooting.md` first.
