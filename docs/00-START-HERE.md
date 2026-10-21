# Start Here

**Read this before anything else. It takes about ten minutes.**

If you have never used Git, never touched a cloud database, and never written a data pipeline, you are in the right place. Nothing in this sprint assumes you did it before. It assumes you follow the order.

**About paths in this file:** `DEx` means your own student number. If you are student 12, then `students/DEx/week1/` means `students/DE12/week1/`. Always replace `DEx` with your number.

## 1. What We Are Doing

This guide gives you the full picture of the 15 week sprint before you start. It tells you what we are building and why, the order to read the other files, which tool you use for what, how task groups and milestones work, and the rules that never bend.

By the end of this guide you will know exactly what to open first, where you do your work, and what to finish before week 1 opens. This guide does not ask you to write code. It asks you to understand the shape of the work.

Short list of what is inside:

- How this cohort works
- What you will learn, week by week (the 15 week plan)
- The order to read the files
- Which tool for what
- The four tracks (B, S, P, D)
- Task groups and the five milestones
- The two zones in the repository, and the team project repo
- Your week, every week
- The five rules that never bend
- How writing tasks are graded
- Your checklist before week 1

## 2. Why We Are Doing It

The client is a fintech due diligence company. Due diligence means checking a company before a lender lends to it or a buyer buys it. Their analysts check Indian companies by hand, one company at a time, across two government websites and a spreadsheet.

The analysts answer four questions:

- Is the company real and active? (MCA, the company registry)
- Has it ever been in insolvency? (IBBI)
- How does it compare with other companies in its state? (MCA CDM statistics)
- What was the interest rate at the time? (RBI policy rate)

We build them one platform for all four.

Fifteen weeks. One real data platform, built by the whole cohort together, for a client who exists on paper but whose data is completely real.

By week 15 there is a pipeline that refreshes on a schedule, a set of tested tables, and a dashboard a non-technical person can use. Every part of it is in this repository, reviewed, and traceable to whoever wrote it.

**About the work:** this is new for most people. Go slowly. One task at a time.

## 3. What to Learn First (Prerequisites)

**Before week 1 opens: what you must already have**

| # | You must have | Notes |
|---|---|---|
| 1 | `docs/01-project-brief.md` read end to end | Done before week 1 opens |
| 2 | `docs/02-tools-setup.md` completed | Git configured, Python 3.11 or newer, VS Code, Snowflake login working |
| 3 | Repository forked and cloned | `upstream` remote added (`docs/03-student-guide.md`, section 1) |
| 4 | Your DE number known | You have found `students/DEx/week1/problem_statement.md` |
| 5 | `data.gov.in` opened once | You have found the Company Master Data catalog page |
| 6 | Discord joined | You have said hello with your DE number |

**By the end of the 15 weeks you can:**

- Fork, clone, commit and open a pull request on a real repository
- Write and run SQL in Snowflake, and save the working version in VS Code
- Write Python scripts that read files, validate data, retry failed calls, and log what they do
- Load raw files into Snowflake with a repeatable load, and prove row counts match
- Design a star schema and explain your choices
- Build dbt models, dbt tests, and read a DAG
- Build history tables with start and end dates (SCD2)
- Orchestrate a pipeline in Airflow, find a failure, and write a postmortem
- Build a dashboard in Metabase and hand the project over with a runbook

**Core concepts to review first:**

- **Task ID.** A short code like `S1.2` that names one task. The letter is the track, the number is the week and the task. It goes in your commit message and pull request title.
- **Task group.** A small set of related tasks with one simple name, for example `Databases and Tables`. Tasks inside a group are numbered 1 to N in working order.
- **Milestone.** A checkpoint the whole cohort must reach before anyone moves far past it.
- **Practice zone.** Your own folder, `students/DEx/`, where mistakes cost nothing.
- **Shared zone.** `platform/` and `delivery/`, where the one real pipeline lives.
- **Team project repo.** Your team's own repo on a laptop, where the real project files live. One person, the team leader, copies the reviewed files into the class repo.

## 4. Where to Find Resources

> **Where you work**
>
> - **VS Code** is your home base. You write and save every file here: `.md` notes, `.sql` files, `.py` scripts.
> - **VS Code terminal** (Terminal > New Terminal) is where Git commands and Python run.
> - **Snowflake (Snowsight)** is a browser tab. You run your SQL there. You also create tables and load files with the web wizard there.
> - **The web** is where you download data files by hand.
>
> `DEx` means your number. Replace it everywhere, for example DE12.

**Read in this order**

| # | File | When | Why |
|---|---|---|---|
| 1 | `docs/00-START-HERE.md` | now | The overview. This file. |
| 2 | `docs/01-project-brief.md` | before week 1 opens | What we are building and why. The client story, the four data sources, the design decisions. |
| 3 | `docs/02-tools-setup.md` | before week 1 opens | Git, Python, Snowflake, VS Code, and the accounts you need. Do it with a terminal open. |
| 4 | `docs/03-student-guide.md` | before your first commit | Fork, clone, commit, pull request. The loop you repeat all sprint. |
| 5 | `docs/14-vscode-snowflake-setup-guide.md` | in week 1, with tasks G0.1 to G0.4 | Connect VS Code to Snowflake and run a `.sql` file from VS Code. |
| 6 | `docs/15-team-project-repo-and-delivery.md` | in week 1, before your team starts the team project repo | The team project repo, and the `delivery/` folder. Only the team leader updates `delivery/` and `platform/` in the class repo. |
| 7 | `students/DEx/week1/problem_statement.md` | when week 1 opens | Your actual tasks for week 1. For example `students/DE12/week1/problem_statement.md`. |

After that, every week you open exactly one new file: `students/DEx/weekN/problem_statement.md` for that week.

Week 1 has no Python tasks and no SQL tasks, on purpose. Week 1 is for your tools, your first commits, and the client ask. Python starts in week 2, from zero, with short scripts and the Corey Schafer videos your week file names. SQL starts in week 2 as well, with your first tables.

**Reference files. Open them when a task points you at them:**

| File | Open it when |
|---|---|
| `docs/04-week-map.md` | you want to see all 15 weeks and what each one gives you |
| `docs/05-task-list.md` | you want the complete task list |
| `docs/06-team-roles.md` | you want to know who reviews your work and how the rotations work |
| `docs/07-platform-and-cicd-guide.md` | you have been picked for the platform rotation |
| `docs/08-glossary.md` | a word appears that you do not know |
| `docs/09-resources.md` | you need this week's reading or video |
| `docs/10-troubleshooting.md` | a tool breaks and the error means nothing to you |
| `docs/11-tools-and-technology.md` | you want to know what a tool is for and why we chose it |
| `CONTRIBUTING.md` | before your first pull request |

**Which tool for what**

This table answers the most common first-week question: "where do I do this?"

| Tool | Where it is | What you do there |
|---|---|---|
| VS Code | an app on your laptop | Your home base. You write and save every file here: `.md` notes, `.sql` files, `.py` scripts. To add a file, right-click the folder in VS Code and choose New File. |
| VS Code terminal | inside VS Code (Terminal > New Terminal) | Git commands (`git add`, `git commit`, `git push`). Python with your `.venv` activated. Later, dbt commands, because this cohort uses dbt Core. |
| Snowflake (Snowsight) | a browser | You run your SQL here. You also create tables and load a file with the web wizard. SQL is written in VS Code and run in Snowflake. |
| Data websites | a browser | `data.gov.in` for the MCA Company Master Data catalog, `ibbi.gov.in` for insolvency PDFs, `mcacdm.nic.in` for state statistics tables, `data.rbi.org.in` and `www.rbi.org.in` for the policy rate. You download the files by hand into `data/raw/`. |
| GitHub | a browser | Your fork, your pull requests, and the reviews you give and receive. |

Two habits to fix now:

1. **Write in VS Code, run in Snowflake.** When your SQL works in Snowflake, make sure the saved `.sql` file in VS Code holds the final working version. Then commit it.
2. **Download once, never commit.** Data files go into `data/raw/` at the repo root, which `.gitignore` already excludes. Data and secrets never go into a commit.

To activate your Python environment in the VS Code terminal:

- macOS or Linux: `source .venv/bin/activate`
- Windows: `.venv\Scripts\activate`

**Search keywords if you get stuck:**

- `git fork clone upstream remote`
- `git commit message task id`
- `python venv activate windows`
- `Snowflake load data using the web interface`
- `pull request from fork to upstream`
- `gitignore data files`

## 5. Step-by-Step Guide

Read this guide in order. Then read `docs/01-project-brief.md`. Then do `docs/02-tools-setup.md` with a terminal open. Do not skip ahead to week 1 tasks before the setup is finished.

### 5.1 How this cohort works

There are about 30 students, DE1 to DE30. You all work in one class repository, and each student has their own space inside it.

- **Your folder.** `students/DEx/` is yours. Inside it there are 15 week folders, `week1` to `week15`. Every file you make for a week goes inside that week's folder, for example `students/DE12/week1/sql/s1/01_create_database.sql`.
- **Your week file.** Each week folder has a `problem_statement.md`. That is the only file you must open each week. It holds your tasks for that week.
- **One task, one file, one commit.** Do a task, save the file, commit it with the task ID in the message, then move to the next task. Never put a whole week into one giant file.
- **Pull request.** Push your commits to your fork and open a pull request to the class repo, one per task or one per task group. The task ID goes in the pull request title.
- **Review.** You review at least one teammate's pull request each week. Someone reviews yours. Reviews are part of the work, not extra work.
- **Never squash merge.** Squashing turns everyone's commits into one and erases who wrote what. The commit history is part of your record here.

### 5.2 What you will learn, in order

Each week builds on the one before it. Week 1 is setup and the client ask, with no Python and no SQL tasks, on purpose. Weeks 2 and 3 teach Python from zero, with short scripts and the Corey Schafer videos named in your week file. Weeks 4 to 6 give you joins, cleaner SQL and the first proper load into Snowflake. Weeks 7 to 9 are design, cleaner Python and the extractors. Weeks 10 to 15 are tests, history, the shared platform, orchestration, the dashboard and the handover.

| Week | You will learn | You will make |
|---|---|---|
| 1 | Set up tools: Git, Snowflake, VS Code | Your own folder, your first commits and pull request, and the discovery brief |
| 2 | First Snowflake tables; first Python (variables, lists, loops, functions) | Your first tables in Snowflake, and your first small Python scripts |
| 3 | Python reads files (CSV); check data quality with SQL | Scripts that read the RoC CSV files, and SQL checks on the data |
| 4 | SQL queries and joins; check data with Python (type hints, validation) | Queries with joins, and a Python check of the data |
| 5 | Missing values, text and date functions | Queries that handle missing values, text and dates |
| 6 | First proper load into Snowflake; design the star schema and pipeline diagrams | A repeatable load with a row count check, the star schema, and the pipeline diagrams |
| 7 | Views, CTEs, final star schema design; retry logic | Views and CTE queries, the agreed star schema, and code that retries |
| 8 | Normalization, big files and generators, logging, command line tools | A command line tool that logs what it does |
| 9 | Extractors for PDF and web pages with classes; design review | An extractor for a PDF and an extractor for a web page, and your design review notes |
| 10 | Window functions and tests with pytest | Window function queries and tests for your own Python code |
| 11 | History (SCD2), parallel downloads, Python talks to Snowflake, technical brief | The history tables, a parallel download script, and the technical brief |
| 12 | Shared platform: stages, bronze, dbt Core staging | A Snowflake stage, the bronze load, and your first dbt staging models |
| 13 | Layer contracts, gold layer, monthly refresh | The gold layer with layer contracts, and the monthly refresh |
| 14 | Quality checks, Airflow, peer review, break and fix | Quality checks, an Airflow DAG, and a postmortem of the injected failure |
| 15 | Dashboard, presentation, handover | The Metabase dashboard, the presentation, the runbook and the handover |

The exact tasks and task IDs for a week are always in that week's `problem_statement.md`. This table shows the shape of the work, not every task.

> **If you are new to Python or SQL**
>
> You are still in the right place. You do not need to study a whole course before week 1.
>
> - Week 1 has no Python and no SQL tasks. That is on purpose: first the tools and the client ask, then the code.
> - Python starts in week 2, from zero. It comes from Corey Schafer's **Python Tutorials** playlist, but only the videos listed in `docs/09-resources.md` and named in your week file. Do not work through all 158 videos.
> - Every task in every week file has a **Learn first** line. It names the exact videos or pages for that task, and what you can skip. Watch or read only that. Then do the task.
> - SQL comes from Kudvenkat's **SQL Server tutorial for beginners** playlist. The parts you need are listed per week. He teaches SQL Server and we use Snowflake, so watch for the idea, type the Snowflake version, and run it in Snowflake.
> - The rule is simple: one task at a time, and the Learn first line before the Steps line.

### 5.3 The four tracks

Every task in this sprint belongs to one of four tracks. The track letter is the first letter of the task ID, so `S3.2` is a SQL task and `P7.1` is a Python task.

| Letter | Track | What it teaches |
|---|---|---|
| **B** | Business and Delivery | Turning a vague client ask into a written brief, reviewing other people's work, presenting to a non-technical stakeholder, handing a project over |
| **S** | SQL and Data Modeling | Databases, tables, constraints, joins, NULL handling, functions, views, CTEs, window functions, MERGE, star schema design |
| **P** | Python | Files and functions, validation, retries, generators, logging, command line tools, classes, tests, concurrency, the Snowflake connector |
| **D** | Data Platform | Snowflake stages, bronze loads, dbt staging and marts, Great Expectations, Airflow, the clean run and the handover |

The tracks run in parallel, not one after another. In a normal week you touch two or three of them. That is on purpose: real data work is never one skill at a time.

### 5.4 Task groups and milestones

A **task group** is a small set of related tasks with one simple name, for example `Databases and Tables`. The week files number the tasks in each group 1 to N in working order. Every task keeps a stable ID like `S1.2`. That ID goes in your commit message and your pull request title, so anyone can find the task behind any commit.

A **milestone task group** is a checkpoint the whole cohort must reach before anyone moves far past it, because the next groups depend on a shared decision. There are five:

| Milestone | Around | Why it gates everyone |
|---|---|---|
| Discovery Brief | week 1 | Building the wrong thing perfectly is the most expensive mistake in data work |
| First Snowflake Load | weeks 6 and 12 | Everyone must be able to move a file into the warehouse and prove the row counts match |
| Star Schema Design | weeks 7 and 9 | Table and column names must be agreed before anyone builds the gold layer |
| SCD2 Build | weeks 11 and 13 | The history logic is the client's core requirement and the hardest code in the project |
| Project Handover | week 15 | The pipeline must run from a clean checkout using only the runbook |

When the cohort stops at a milestone, new task group work pauses until the group catches up. That is not a punishment. It is how a team avoids building two halves of one platform that do not fit.

### 5.5 Two zones in this repository

This is the most important thing to understand about how the repo is laid out.

```
Data-Sprint-1/
├── students/DE1 ... DE30/       PRACTICE ZONE. Your own folder. Mistakes cost nothing.
│   └── week1 ... week15/
│       └── problem_statement.md
│
├── platform/                    SHARED ZONE. The one real pipeline. Stricter rules.
│   └── raw/  staging/  marts/  quality/  orchestration/  dbt/
│
├── delivery/                    SHARED ZONE. discovery/ design/ dashboard/ presentation/
│
└── docs/                        Everything you read.
```

**Practice zone.** You work in `students/DEx/weekY/`. You try every skill yourself. If you break something, you broke your own copy, and that is exactly what this zone is for.

**Shared zone.** `platform/` and `delivery/` hold the one real version. A small rotating group builds there each week. A mistake there blocks the whole cohort, so that code gets automated checks and a review from a core admin.

**The team project repo.** Each team also keeps its own project repo on a laptop, in VS Code, where the real project files live. Everyone on the team can have a local copy. The team leader is the only person who updates the class repository's `delivery/` and `platform/` folders, by copying the reviewed files and opening a pull request. The details are in `docs/15-team-project-repo-and-delivery.md`.

Everyone gets a turn in the shared zone. See `docs/06-team-roles.md`.

### 5.6 Your week, every week

- **Week opens.** The goal for the week is posted in Discord. The roles for the week are named.
- **During the week.** You work through your own `problem_statement.md`, one task at a time. One commit per task. One pull request per task or per task group. You review at least one teammate's pull request.
- **Week closes.** Cohort review: architecture walkthrough, terminology check, one failure story, a preview of next week.

The expectation is about output, not hours. By the time the week closes, the tasks in your week file are done, committed, and in pull requests.

Each week file ends with an end of week checklist and an "if you are short on time, cut in this order" line. Use them honestly.

### 5.7 The five rules that never bend

1. **One task, one file, one commit.** Never build one giant file across a week. The task tells you the exact path.
2. **Name the task ID.** In the commit message and the pull request title. `S1.2 create raw MCA table`, not `update`.
3. **Stay in your own folder** unless you are on the platform rotation this week.
4. **Never squash merge.** Squashing collapses everyone's commits into one and erases individual authorship, which is the record you are here to build.
5. **No data files, no secrets.** No downloaded CSV, ZIP, PDF, or Excel. No passwords, no `profiles.yml`. Check `.gitignore` first, and ask in Discord if you are unsure.

The full rule list is in `CONTRIBUTING.md`. The commands are in `docs/03-student-guide.md`.

### 5.8 How writing tasks are graded

Some tasks ask for a short explanation in a notes file instead of code. Those are graded exactly like code.

A good answer names the tradeoff. "We use VARCHAR in bronze" is a fact. "We use VARCHAR in bronze because a typed column would reject the whole file when one row has a bad date, and losing the file is worse than storing a bad value we can find later" is an answer.

You will be asked to say these out loud at the cohort review, from memory. Write them in your own words for that reason, not because copying is against the rules.

**How to think about it:** when you write an answer, ask yourself "what is the other option, and why did we not pick it?" If you can name the other option, your answer is strong.

### 5.9 What will be hard, and it is meant to be

**The client ask is vague on purpose.** "We want to check companies faster" is not a specification. Week 1 is about turning it into something answerable. If you start writing SQL on day one, you will build the wrong thing.

**The data is genuinely messy.** State names written several ways. Dates in several formats. Capital amounts with commas and currency symbols. One source only exists inside a PDF. Another only exists as an HTML table. This is not a simplified teaching version of real data. It is real data.

**Something will break in week 14 on purpose.** A failure is injected into the data or the pipeline. You will find it using logs, tests, and row counts, and write a postmortem about it.

**You will be asked "why" more than "what".** Every design choice in this project has a reason and an alternative that was rejected. Knowing the reason is the difference between a data engineer and someone who copied a tutorial.

## 6. What to Commit

This guide does not ask you to commit a file of your own. It asks you to be ready. When the checklist below is true, you are ready for week 1.

- [ ] `docs/01-project-brief.md` read end to end
- [ ] `docs/02-tools-setup.md` completed: Git configured, Python 3.11 or newer, VS Code, Snowflake login working
- [ ] Repository forked and cloned, `upstream` remote added (`docs/03-student-guide.md`, section 1)
- [ ] You know your DE number and have found `students/DEx/week1/problem_statement.md`
- [ ] You have opened `data.gov.in` once and found the Company Master Data catalog page
- [ ] You have joined the cohort Discord channel and said hello with your DE number
- [ ] Python environment activates without an error:
  - macOS or Linux: `source .venv/bin/activate`
  - Windows: `.venv\Scripts\activate`
- [ ] You can say the five rules from memory: one task one file one commit, name the task ID, stay in your own folder, never squash merge, no data files and no secrets
- [ ] Your notes answers are in your own words, and each one names the tradeoff

**In week 1 you also finish these, before the week closes:**

- [ ] `docs/14-vscode-snowflake-setup-guide.md` read and done, with tasks G0.1 to G0.4
- [ ] `docs/15-team-project-repo-and-delivery.md` read, and your team knows who the team leader is
- [ ] Your first commit and pull request are open (see `docs/03-student-guide.md`)

**Done when:** all boxes above that apply to you are checked. Then open `students/DEx/week1/problem_statement.md` and start Task 1.

When you make your first commit, use the task ID in the message and open a pull request (see `docs/03-student-guide.md`). Do not squash merge.

Next: `docs/01-project-brief.md`.
