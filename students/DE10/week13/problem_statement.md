# Week 13 - Gold layer and the monthly refresh

**Data Sprint 1 · Week 13 of 15 · Theme: build the tables the client uses, then run the monthly refresh that keeps them correct**

## 1. What We Are Doing

This week you build the gold layer: the tables the client uses. You also run the full monthly refresh and record the status of every step. Read this whole file before you start. Then work through the task groups in order.

The task groups are:
- Group 1 - Layer Contracts: Task 1 (`D6.1`), Task 2 (`D6.2`)
- Group 2 - Gold Layer with dbt: Task 3 (`D7.1`), Task 4 (`D7.2`), Task 5 (`D7.3`)
- Group 3 - The Monthly Refresh **[MILESTONE]**: Task 6 (`P12.1`), Task 7 (`P12.2`)

## 2. Why We Are Doing It

The client is a fintech due-diligence company. Its analysts check Indian companies: is the company real and active, was it ever in insolvency, how does it compare with its state, and what was the interest-rate backdrop. This week you build the gold tables those analysts use. A wrong number must be traceable to the layer that broke a rule. The monthly refresh is the client's promise, so you must show the whole chain works.

**The milestone this week:** **P12 The Monthly Refresh, end to end.** Not the query you wrote in week 11, but the whole monthly refresh: new files land, silver runs, the merge applies, quality checks pass. One run, every step's status recorded.

## 3. What to Learn First (Prerequisites)

**Before this week: what you must already have**

| Week | Required item | How to check it exists |
|---|---|---|
| **Week 3** | your practice tables `RAW_MCA_<ROC>` (three of them) and `RAW_IBBI_SAMPLE`, in your own schema | in a Snowsight Workspace, run `select count(*) from YOUR_SCHEMA.RAW_MCA_<ROC>;` for each table |
| **Week 6** | a stage, a file format, and a `COPY INTO` script with a row-count check | open your week 6 folder and find the `.sql` file that contains `COPY INTO` |
| **Week 7** | one grain statement per table (one sentence saying what one row means) | open your week 7 notes; each table has one sentence |
| **Week 9** | your IBBI PDF extractor that turns the PDF into CIRP rows | open your week 9 folder and find the Python file that made the CIRP rows |
| **Week 11** | a Python script that connects to Snowflake using values from `.env`, and the "simulated monthly change" drill you did that week | open your week 11 folder and look at the script and the drill notes |
| **Week 12** | your dbt Core project with staging models, a marts folder, and at least one test | open `students/DEx/week12/dbt` and check that the folders exist |

If something is missing: open that week's `problem_statement.md` and redo only the task you need. Then ask your pair. Then ask the program lead in the class channel. Do not start week 13 without a stage and a working dbt project, because Tasks 3 to 7 build on them.

**By the end of this week you can**
- State, in writing, what each layer is allowed to do and what it is forbidden to do.
- Trace every source column to its final home, or say why it was dropped.
- Build dim_date, an SCD2 dim_company, and a fact table with relationship tests.
- Run a monthly refresh end to end and show the status of every step.

**Core concepts to review first**
- What each layer may and may not contain, and how each layer is stored (view or table).
- Your week 7 grain statements: one sentence for what one row means.
- dbt basics from week 12: models, materializations, tests and snapshots.
- Python pieces you reuse: argparse, the retry decorator, and the Snowflake connector with `.env`.

## 4. Where to Find Resources

**Where you work**
- VS Code is your home base. You write, save and commit every file there.
- Snowflake (Snowsight in the browser) is where SQL runs: Projects > Workspaces. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database, your own schema.
- Snowflake now asks for MFA (a second sign-in step) when you sign in with a password. Set it up at your first login.
- dbt Core runs in the VS Code terminal, with `.venv` activated. Its connection file `~/.dbt/profiles.yml` stays on your laptop and is never committed.
- The VS Code terminal also runs Python and Git. Run commands from the repo root with `.venv` activated.
- `DEx` means your own student number, for example DE12. So `students/DEx/week13/...` becomes `students/DE12/week13/...`.
- Git is explained once, in `docs/03-student-guide.md`. We do not repeat it every week.

**Docs you may need**

| I need... | Go here |
|---|---|
| platform and CI rules (read before you touch `platform/` or `quality/`) | `docs/07-platform-and-cicd-guide.md`, `docs/06-team-roles.md` |
| Git commands | `docs/03-student-guide.md` |
| the meaning of a word | `docs/08-glossary.md` |
| help with a broken tool | `docs/10-troubleshooting.md` |
| what a tool is for | `docs/11-tools-and-technology.md` |

**Your week in order**

| Step | LEARN | DO |
|---|---|---|
| **1** | kimballgroup.com, "Dimensional Modeling Techniques", read the conformed dimension pages (45 min). Skip the slowly changing dimension pages for now. Then re-read your own week 7 grain statements (15 min). | Tasks 1-2 - layer contract, column mapping |
| **2** | dbt docs, search "Snapshots" and read the whole page (40 min). Then search "Materializations" and read only the view vs table part (15 min). | Tasks 3-4 - dim_date, SCD2 dim_company |
| **3** | dbt docs, search "Add tests to your DAG" (30 min). Focus on generic tests and the relationships test. | Task 5 - the fact table and dim_state |
| **4** | Nothing new. You reuse P6 (argparse, week 8), P4 (retry decorator, week 7) and P11 (Snowflake connector, week 11). Re-read your own scripts (20 min). | Tasks 6-7 - snapshot prep, full refresh |
| **5** | - | Cohort review: SCD2 proof, refresh run log |

**Search keywords if you get stuck**
- dbt snapshots
- dbt relationships test
- dbt materializations
- Snowflake GENERATOR
- SCD2
- monthly refresh

> **How your script logs in.** From week 11 on, Python scripts and dbt talk to Snowflake with key-pair authentication or a Programmatic Access Token (PAT). The program lead tells you which one the cohort uses. Never paste a password into code or into a file in the repo. If a script login fails with an MFA message, ask the program lead. More detail: `docs/14`, section 5.8.

> **Team project repo.** Everyone keeps project work in the team repo on the laptop, in VS Code. Individual practice stays in `students/DEx/week13/`. Only the team leader copies the reviewed files from the team repo into the class repo `delivery/` or `platform/` folder, and the leader opens the pull request (see `docs/15-team-project-repo-and-delivery.md`).

## 5. Step-by-Step Guide

Work on one task at a time. One task, one file, one commit. The full Git steps are in `docs/03-student-guide.md`. Do not commit data files or secrets. This is new for most people. Go slowly.

**How your tasks are numbered.** B = business, S = SQL, P = Python, D = data platform, A = architecture. This week you have D and P tasks. The ID goes in your commit message and in your pull request title, for example `D7.1 build dim_date`.

### Group 1 - Layer Contracts

#### Task 1 - Write the layer contract (ID `D6.1`)

##### 1. What We Are Doing
You write a short contract for the bronze, silver and gold layers. The contract says what each layer may contain and what it must never contain. You work in VS Code.

##### 2. Why We Are Doing It
The client's analysts must trust a number in the dashboard. The contract says what each layer may contain, so a wrong number can be traced to the layer that broke the rule.

##### 3. What to Learn First
- `docs/11-tools-and-technology.md` for the bronze, silver and gold idea (15 min).
- dbt docs, search "Materializations" and read only the view vs table part (15 min), because a contract mentions how each layer is stored.
- Skip the incremental model pages.

##### 4. Where to Find Resources
- `docs/11-tools-and-technology.md` (15 min).
- dbt docs, search "Materializations" and read only the view vs table part (15 min). Skip the incremental model pages.
- Search keywords: "bronze silver gold", "dbt materializations", "view vs table".

##### 5. Step-by-Step Guide
1. In VS Code, right-click `students/DEx/week13` > New Folder, name it `design`. If it already exists, skip this.
2. Right-click `design` > New File, name it `layer_contract.md`.
3. Paste the skeleton below.
4. For each layer, write at least two "Allowed" lines and two "Forbidden" lines. One sentence per line.
5. Fill the "Stored as" line with view or table, as you learned in the dbt docs.

```
# Layer contract

## Bronze
Stored as:
Allowed:
Forbidden:

## Silver
Stored as:
Allowed:
Forbidden:

## Gold
Stored as:
Allowed:
Forbidden:
```

If you get stuck:
- Read your draft aloud. Does each line say one clear rule?
- Search "dbt materializations".
- Ask in Discord with your draft lines.

How to think about it: a forbidden line is a guard. Write the rule that stops a wrong number.

##### 6. What to Commit
- [ ] `students/DEx/week13/design/layer_contract.md`

**Done when:**
- [ ] The file has three sections: bronze, silver and gold.
- [ ] Each section has at least two allowed lines and two forbidden lines.
- [ ] Each section states view or table.

**Commit message:** `D6.1 write layer contract`

#### Task 2 - Map every source column (ID `D6.2`)

##### 1. What We Are Doing
You make one map from each source column to its final gold home, or to a clear reason for dropping it. You work in VS Code.

##### 2. Why We Are Doing It
When a client asks why a column is missing from the dashboard, this file is the answer. It also proves to your lead that nothing was lost by accident.

##### 3. What to Learn First
Nothing new. Use the MCA data dictionary you read in week 3 and your week 7 grain statements (20 min). Open the header row of one MCA CSV so you see the real column names.

##### 4. Where to Find Resources
- Your week 3 data dictionary notes.
- Your week 7 grain statements (20 min).
- Open the header row of one MCA CSV.
- Search keywords: "MCA CSV columns", "column mapping", "gold column".

##### 5. Step-by-Step Guide
1. Right-click `students/DEx/week13/design` > New File, name it `column_mapping.md`.
2. Paste the skeleton table below.
3. Add one row per source column. Cover all four sources: every column of one MCA CSV; the five fields you typed into `RAW_IBBI_SAMPLE` (CIN, company name, bench, start date, admitted claims); the CDM statistics columns you use for `dim_state`; and the RBI date and rate fields.
4. In "Decision" write `kept` or `dropped`. A `dropped` row must have a reason in plain words.
5. A `kept` row must name a gold table and a gold column. If the gold table does not exist yet, write the name you plan to use.

```
| Source file | Source column | Silver table.column | Gold table | Gold column | Decision | Reason |
|---|---|---|---|---|---|---|
| mca_roc_....csv | CIN |  |  |  | kept |  |
| mca_roc_....csv |  |  |  |  | dropped |  |
| mca_cdm_.... |  |  |  |  | kept |  |
| rbi_.... |  |  |  |  | kept |  |
```

If you get stuck:
- Open the MCA CSV header again.
- Search "column mapping".
- Ask in Discord with the column name and your reason.

How to think about it: a reason for a dropped column is the useful part. Write it for a reader who was not here.

##### 6. What to Commit
- [ ] `students/DEx/week13/design/column_mapping.md`

**Done when:**
- [ ] Every MCA CSV column, every IBBI field, and the CDM and RBI inputs appear once.
- [ ] Every `dropped` row has a reason.
- [ ] Every `kept` row names a gold table and gold column.

**Commit message:** `D6.2 map source columns to gold`

> **On Task 2:** "dropped" needs a reason, and the reason is the useful part. Six months from now, when a client asks why a column is missing from the dashboard, this file is the answer.

### Group 2 - Gold Layer with dbt

**How you run dbt in this cohort: dbt Core.** dbt Core is installed with pip and runs in the VS Code terminal.
- With `.venv` activated, install it once: `pip install dbt-snowflake`.
- The connection lives in `~/.dbt/profiles.yml`. The profile name there must match the `profile:` name in `dbt_project.yml`. Never commit this file.
- Run every dbt command from the repo root and add `--project-dir students/DEx/week13/dbt`. `dbt_project.yml` lives in that folder, not at the repo root.
- Run `dbt debug --project-dir students/DEx/week13/dbt` first. It checks the connection. Then use `dbt run`, `dbt test` and `dbt snapshot`, always with the same `--project-dir`.
- Optional extra: dbt Projects on Snowflake (in a Snowsight Workspace) can run the same project files. We do not use it as the default. See https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake.

#### Task 3 - Build dim_date (ID `D7.1`)

##### 1. What We Are Doing
You build a date dimension model called dim_date. It gives one row per date, with year, quarter, month and week. You write the file in VS Code, then run it with dbt Core in the VS Code terminal, against Snowflake.

##### 2. Why We Are Doing It
The client slices insolvency numbers by month and quarter. A date dimension is what makes those slices possible and correct.

##### 3. What to Learn First
- dbt docs, search "Materializations" and read the table part only (15 min).
- Open one of your week 12 staging models (10 min) to copy the file layout.

##### 4. Where to Find Resources
- dbt docs, search "Materializations" and read the table part only (15 min).
- One of your week 12 staging models (10 min).
- Search keywords: "dbt materializations", "date dimension", "row count check".

##### 5. Step-by-Step Guide
1. Copy your week 12 dbt project into the week 13 folder: copy `students/DEx/week12/dbt` to `students/DEx/week13/dbt`. Do not copy credentials, generated output (`target/`, `logs/`, `dbt_packages/`) or local environment files (`.venv`, `.env`).
2. In VS Code open `students/DEx/week13/dbt`. Create `models/marts` if it is missing (right-click > New Folder).
3. Right-click `models/marts` > New File, name it `dim_date.sql`.
4. Paste the skeleton below and fill the TODOs.
5. Find the first date across every source, not just one table: the MCA registration dates from all three RoC tables (the raw dates may be text, so use `TRY_TO_DATE`), the CIRP start dates, and the RBI date field. Take the smallest safely parsed date, and use it as the start of your date list. End at today plus 5 years.
6. To make the list of dates, search the Snowflake docs for "GENERATOR". Select your dates first in a Snowsight Workspace before you put them in the model.
7. Run it in the VS Code terminal, with `.venv` activated: `dbt run --project-dir students/DEx/week13/dbt --select dim_date`.
8. Check the row count is the number of days from your first date to your last date, plus one.

```
with date_spine as (
    -- TODO: one row per date from the earliest source date
    -- to today plus 5 years. Hint: search the Snowflake docs for "GENERATOR".
    select
        date_day
    from
)

select
    date_day as date_key
    -- TODO: year, quarter, month, week columns
from date_spine
```

If you get stuck:
- Read the Snowflake error message.
- Search "Snowflake GENERATOR".
- Check the row count by hand.

##### 6. What to Commit
- [ ] `students/DEx/week13/dbt/models/marts/dim_date.sql`
- [ ] `students/DEx/week13/dbt/run_results.md`

**Done when:**
- [ ] `dbt run --project-dir students/DEx/week13/dbt --select dim_date` finishes without error.
- [ ] It has columns for year, quarter, month and week.
- [ ] The row count matches a manual day count.
- [ ] There is at least one future date row.

**Commit message:** `D7.1 build dim_date`

> **On Task 3:** dim_date must cover the full range of every source, including future dates the fact table can reach. A fact row with a date that has no dimension row is a broken join, discovered late.

#### Task 4 - Build SCD2 dim_company (ID `D7.2`)

##### 1. What We Are Doing
You build an SCD2 company dimension. SCD2 (slowly changing dimension type 2) keeps every version of a row with start and end dates. You write the files in VS Code, run dbt Core in the VS Code terminal, and check the result in Snowflake.

##### 2. Why We Are Doing It
The client wants to see how a company changed over time, not only its current status.

##### 3. What to Learn First
- dbt docs, search "Snapshots" and read the whole page (40 min).
- Plain words: a snapshot file lives in the `snapshots/` folder; dbt writes a new row each time a watched column changes, with a valid-from and a valid-to date.
- The `check` strategy watches the columns you list. Here you watch `status`, authorised capital, paid-up capital and `address`, using the exact column names from your silver company model.

##### 4. Where to Find Resources
- dbt docs, search "Snapshots" and read the whole page (40 min).
- Search keywords: "dbt snapshots", "check strategy", "SCD2".

##### 5. Step-by-Step Guide
1. In VS Code open `students/DEx/week13/dbt`. Create the folder `snapshots` if it is missing.
2. New file `snapshots/dim_company_snapshot.sql` and paste the first skeleton. Fill the TODOs. In `check_cols`, use the exact silver column names for status, authorised capital, paid-up capital and address.
3. New file `models/marts/dim_company.sql` with the second skeleton, so gold reads the versioned rows.
4. Run the snapshot in the VS Code terminal: `dbt snapshot --project-dir students/DEx/week13/dbt`. Then change one company on purpose: update its status or address in your test schema. You used this drill in week 11.
5. Run the snapshot a second time. In a Snowsight Workspace, query the snapshot table for that CIN and confirm it now has two rows.
6. Paste the query and its result rows into `students/DEx/week13/dbt/scd2_proof.md`.
7. Now build the gold model from the snapshot: `dbt run --project-dir students/DEx/week13/dbt --select dim_company`.

```
{% snapshot dim_company_snapshot %}

{{ config(
    target_schema='<your test schema>',
    unique_key='cin',
    strategy='check',
    check_cols=['status', '<authorised capital column>', '<paid-up capital column>', 'address']
) }}

select
    cin,
    -- TODO: company name, status, authorised capital, paid-up capital, address
    -- use the exact column names from your silver company model
from {{ ref('<your silver company model>') }}

{% endsnapshot %}
```

```
-- students/DEx/week13/dbt/models/marts/dim_company.sql
-- Gold reads the versioned rows from the snapshot.
select * from {{ ref('dim_company_snapshot') }}
```

If you get stuck:
- Read the dbt error message.
- Search "dbt snapshots".
- Check the snapshot table with a simple select.

##### 6. What to Commit
- [ ] `students/DEx/week13/dbt/snapshots/dim_company_snapshot.sql`
- [ ] `students/DEx/week13/dbt/models/marts/dim_company.sql`
- [ ] `students/DEx/week13/dbt/scd2_proof.md`
- [ ] `students/DEx/week13/dbt/run_results.md`

**Done when:**
- [ ] The snapshot uses the check strategy on status, authorised capital, paid-up capital and address, with the exact silver column names.
- [ ] After two runs your changed company has two rows with different valid-from dates.
- [ ] `dim_company` is built from the snapshot with `dbt run --project-dir students/DEx/week13/dbt --select dim_company`.
- [ ] The proof file shows the query and the real result.

**Commit message:** `D7.2 build SCD2 dim_company with proof`

#### Task 5 - Build the fact table and dim_state (ID `D7.3`)

##### 1. What We Are Doing
You build the fact table for CIRP events and a dim_state table. CIRP rows are insolvency cases. You write the models and the test file in VS Code, then run `dbt test` in the VS Code terminal.

##### 2. Why We Are Doing It
The fact table holds the event the client counts, a company entering insolvency. dim_state gives the state comparison the client asked for.

##### 3. What to Learn First
- dbt docs, search "Add tests to your DAG" (30 min). Focus on generic tests and the relationships test.
- A DAG is the graph of your models and their dependencies.
- kimballgroup.com, search "Dimensional Modeling Techniques" and read the fact table page (15 min).

##### 4. Where to Find Resources
- dbt docs, search "Add tests to your DAG" (30 min). Focus on generic tests and the relationships test.
- kimballgroup.com, search "Dimensional Modeling Techniques" and read the fact table page (15 min).
- Search keywords: "dbt relationships test", "fact table", "dim_state".

##### 5. Step-by-Step Guide
1. New file `students/DEx/week13/dbt/models/marts/fct_cirp_event.sql`. Use a LEFT JOIN from the CIRP rows to `dim_company`, so no CIRP row is dropped. Join on `cin` and also on the event date inside the version range (`event_date >= valid_from` and `event_date < valid_to`, with the last row open-ended). Keep the matched version key from `dim_company`, plus the event date, bench and admitted claims.
2. New file `students/DEx/week13/dbt/models/marts/dim_state.sql`, built from your CDM state data.
3. New file `students/DEx/week13/dbt/models/marts/schema.yml` and paste the test skeleton below. Fill the TODOs.
4. Run `dbt test --project-dir students/DEx/week13/dbt` in the VS Code terminal. Read the failures, do not delete them. Add a second relationships test on the matched version key column, so each fact row points to exactly one dimension version.
5. Count the CIRP rows with no matching company. Write down that number and your handling rule, and put both in the pull request description.

```
version: 2

models:
  - name: fct_cirp_event
    columns:
      - name: cin
        tests:
          # TODO: add the dbt test that checks every fact CIN exists in the company dimension
          # hint: the test name is "relationships", with keys to and field
          - relationships:
              to: ref('dim_company')
              field: TODO
```

If you get stuck:
- Read the dbt test output.
- Search "dbt relationships test".
- Ask in Discord with the test output.

How to think about it: a failing relationship test is a finding. Write the rule you choose and the reason.

##### 6. What to Commit
- [ ] `students/DEx/week13/dbt/models/marts/fct_cirp_event.sql`
- [ ] `students/DEx/week13/dbt/models/marts/dim_state.sql`
- [ ] `students/DEx/week13/dbt/models/marts/schema.yml`
- [ ] `students/DEx/week13/dbt/run_results.md`

**Done when:**
- [ ] The fact table left-joins to the company dimension on CIN and on the event date inside the version range.
- [ ] The fact table keeps the matched version key.
- [ ] dim_state exists.
- [ ] The relationships test runs and you can show its output.
- [ ] The actual number of unmatched CIRP rows and the handling rule are written in the PR.

**Commit message:** `D7.3 build fct_cirp_event and dim_state with relationship tests`

> **On Task 5:** week 3 already told you that some insolvency CINs are not in the registry, so the relationships test may show unmatched rows. That is a real finding, not a bug in your model. Record the actual unmatched count and write down what the gold layer does with those rows.

### Group 3 - The Monthly Refresh **[MILESTONE]**

#### Task 6 - Write the snapshot preparation script (ID `P12.1`)

##### 1. What We Are Doing
You write a script that prepares the monthly snapshot. It downloads new RoC files, checks row counts, and lands the files to the stage. You write it in VS Code and run it in the VS Code terminal.

##### 2. Why We Are Doing It
New RoC files arrive every month. The client needs the same steps, in the same order, with a log that shows what landed and what did not.

##### 3. What to Learn First
Nothing new. You reuse P6 (argparse, week 8), P4 (retry decorator, week 7) and P11 (Snowflake connector, week 11). Re-read your own scripts (20 min). If argparse is fuzzy, read https://docs.python.org/3/library/argparse.html (20 min).

##### 4. Where to Find Resources
- Re-read your own scripts (20 min).
- If argparse is fuzzy, read https://docs.python.org/3/library/argparse.html (20 min).
- Corey video #52: Logging Basics - Logging to Files, Setting Levels, and Formatting, if the `logging` lines feel unclear.
- Search keywords: "argparse", "retry decorator", "Snowflake connector".

##### 5. Step-by-Step Guide
1. In VS Code right-click `students/DEx/week13` > New Folder, name it `python`. Then create `python/p12` the same way.
2. Right-click `python/p12` > New File, name it `snapshot_prep.py`.
3. Paste the skeleton below and fill the TODOs. Keep the three steps in three functions.
4. Downloaded files go to `data/raw/mca/` at the repo root. `.gitignore` already excludes this folder.
5. Read the Snowflake values from `.env`, never typed in the file. Run the row-count check before you land the file.
6. Run it from the repo root, with `.venv` activated: `python students/DEx/week13/python/p12/snapshot_prep.py --month 2026-10` (use your own month).

```
"""Download new RoC files, check row counts, land them in the stage."""
import argparse
import logging

def download(month: str) -> list[str]:
    # TODO: download the new RoC ZIP files into data/raw/mca/
    # TODO: reuse the retry decorator you wrote in week 7
    return []

def check_row_counts(files: list[str]) -> None:
    # TODO: count rows per file and compare with the catalog page on data.gov.in
    pass

def land_to_stage(files: list[str]) -> None:
    # TODO: PUT the files, then COPY INTO your RAW_MCA_<ROC> table
    pass

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--month", help="pull month, for example 2026-10")
    args = parser.parse_args()
    logging.basicConfig(level=logging.INFO)
    # TODO: call the three steps, log one summary line per file
```

If you get stuck:
- Read the Python error message.
- Search "argparse" or "retry decorator".
- Run `git status` to check no data file is staged.

##### 6. What to Commit
- [ ] `students/DEx/week13/python/p12/snapshot_prep.py`

**Done when:**
- [ ] The script runs from the repo root.
- [ ] It logs one line per file with file name and row count.
- [ ] No password is in the file.
- [ ] `git status` shows no data file.

**Commit message:** `P12.1 write monthly snapshot prep script`

#### Task 7 - Run the full refresh (ID `P12.2`)

##### 1. What We Are Doing
You run the full monthly refresh end to end. You run the dbt steps in the VS Code terminal and check row counts and query results in Snowflake. This is the milestone.

##### 2. Why We Are Doing It
This is the milestone. The client's promise is a monthly refresh, so you must show the whole chain works, not just one query.

##### 3. What to Learn First
Nothing new. Re-read your week 11 refresh script and your week 6 `COPY INTO` script (20 min).

##### 4. Where to Find Resources
- Your week 11 refresh script.
- Your week 6 `COPY INTO` script (20 min).
- Search keywords: "COPY INTO", "dbt snapshot", "refresh log".

##### 5. Step-by-Step Guide
1. Confirm you are on test schemas, not the shared ones. If you are not sure of the exact test schema name, ask the program lead.
2. Check whether the real MCA monthly file has changed since your last download. It often has not. If the file has not changed, run the load first, then apply the week 11 drill to the test source used by silver, before running silver and the snapshot. That is: change status and capital on a few rows, delete a few rows, and write "drill, not a real month" in the log.
3. Run the six steps in the table below, in order. After each step, save the real output line. Do not load the files twice: step 1 only downloads and validates, step 2 is the script's single upload and load.
4. Create the log file and put the table in it. Replace "ok" or "failed" with what really happened.
5. Add a short "Failed steps" section with the reason. A failed step is a fine result when the log says which step failed and why.
6. Save every dbt command you ran and its real output in `students/DEx/week13/dbt/run_results.md`.

| # | Step | How | What "ok" looks like |
|---|---|---|---|
| 1 | New files ready and checked | run `snapshot_prep.py` (download and row-count check only) | one log line per file, row count matches the catalog page |
| 2 | Land to stage | the same script's single upload and load: `PUT` then `COPY INTO`, test schema | `COPY INTO` reports loaded rows equal to rows in the file |
| 3 | Silver models | `dbt run --project-dir students/DEx/week13/dbt --select <silver models>` | every model passes, row counts above zero |
| 4 | SCD2 merge | `dbt snapshot --project-dir students/DEx/week13/dbt` | snapshot reports rows inserted or updated; the changed company has two versions |
| 5 | Gold models | `dbt run --project-dir students/DEx/week13/dbt --select dim_date dim_company fct_cirp_event dim_state` | every gold model builds, row counts above zero |
| 6 | Quality checks | `dbt test --project-dir students/DEx/week13/dbt` | all pass, or each failure listed with a reason |

If you get stuck:
- Read the log line and the error text.
- Search "COPY INTO" or "dbt snapshot".
- Ask in Discord with the error text.

##### 6. What to Commit
- [ ] `students/DEx/week13/python/p12/refresh_run_log.md`
- [ ] `students/DEx/week13/dbt/run_results.md`

**Done when:**
- [ ] The log lists all six steps.
- [ ] Each step shows ok or failed plus the real output line.
- [ ] The gold models were rebuilt after the snapshot.
- [ ] The SCD2 two-row evidence is included.
- [ ] The run used test schemas only.

**Commit message:** `P12.2 run full monthly refresh on test schemas`

> **On Task 7:** test schemas, not the shared ones. A failed step is a fine result as long as the log says which step failed and why. A run log that only says "success" tells the reader nothing.

## 6. What to Commit

- [ ] Tasks 1-2 - layer contract with forbidden lists, and a complete column map with reasons for drops, covering all four sources.
- [ ] Tasks 3-5 - dim_date, dim_company with two-version proof, fact plus dim_state with relationship tests.
- [ ] Tasks 6-7 - snapshot prep script and a full refresh run log with per-step status.
- [ ] Every dbt run has its results saved in `students/DEx/week13/dbt/run_results.md`, including failures.
- [ ] If you were on the platform rotation, your entry in `docs/platform-rotation-log.md` is updated.
- [ ] Everyone keeps project work in the team repo. Individual practice stays in `students/DEx/week13/`. If you are the team leader, you are the only person who copies the reviewed files from the team repo into the class repo `delivery/` or `platform/` folder (see `docs/15-team-project-repo-and-delivery.md`).

**If you are short on time, cut in this order:** Task 2 (D6.2), then Task 7 (P12.2) polish. Never cut Task 4 (D7.2). The SCD2 dimension is what the client asked for.

When your work is ready, open a pull request to the class repo and follow `docs/03-student-guide.md`. Do not squash-merge.

Next: `week14/problem_statement.md`.
