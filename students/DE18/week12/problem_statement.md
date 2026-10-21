# Week 12 - The shared platform begins

**Data Sprint 1 - Week 12 of 15 - Theme: stop practising separately and build the one real pipeline**

## 1. What We Are Doing

This week you stop working only in your own folder. You build the one real pipeline that loads four sources into bronze, and you start the shared dbt project. This is new for most people. Go slowly.

Read this whole file before you start. Then work through the task groups in order.

The task groups this week:

- **Group 1 - Stages and File Formats:** Task 1 (`D2.1`), Task 2 (`D2.2`), Task 3 (`D2.3`)
- **Group 2 - Bronze Loads:** Task 4 (`D3.1`), Task 5 (`D3.2`), Task 6 (`D3.3`)
- **Group 3 - Teach-Back: the Load Path [MILESTONE]:** Task 7 (`D4.1`)
- **Group 4 - dbt Staging Models:** Task 8 (`D5.1`), Task 9 (`D5.2`), Task 10 (`D5.3`)

**How your tasks are numbered.** The letter shows the task family: B = Business and Delivery, S = SQL and Data Modeling, P = Python, D = Data Platform, A = Architecture diagrams. Week 12 has D tasks only. The ID goes in your commit message and in your pull request title, for example `D2.1 create stages for four sources`.

## 2. Why We Are Doing It

The client checks Indian companies for due diligence. The platform must answer: is this company real and active, was it ever in insolvency, how does it compare to its state, and what was the interest rate at the relevant date. This week you stop practising in separate folders and build the one real pipeline that answers those questions. The pipeline must work for the whole cohort, not just for you. Another teammate must be able to repeat the load.

Weeks 1 to 11 happened in your own folder, where a mistake cost you nothing. From this week, part of the work goes into `platform/`, the one real version the whole cohort depends on.

**Before you touch `platform/`, read `docs/15-team-project-repo-and-delivery.md`.** Only the team leader writes there. If you are not the team leader, you do the same tasks in your own week folder, and you review the team leader's pull requests.

> **Team project repo**
> Everyone keeps practice files in `students/DEx/week12/`. If you are the team leader, mirror this week's reviewed files into the class repo `delivery/` or `platform/` folder. See `docs/15-team-project-repo-and-delivery.md`.

**Who works where this week:**

- Every student practises in `students/DEx/week12/`.
- The team leader mirrors the reviewed files into the class repo `platform/` (or `delivery/`) and opens the pull request there.

**The milestone this week:** **D4 First Snowflake Load.** This is a teach-back. You are not done when it works. You are done when someone else can do it while you watch.

## 3. What to Learn First (Prerequisites)

**Before this week: what you must already have**

Check these before Task 1. If one is missing, you cannot finish the week.

| Have you got it? | What you must already have |
|---|---|
| - [ ] | **Week 2 and Week 3:** the three MCA RoC ZIP files, unzipped into `data/raw/mca/` (download in Week 2; used by task group P1.1-P1.3 in Week 3). Files must stay out of Git. |
| - [ ] | **Week 6:** the bronze tables you loaded properly with a stage and COPY INTO (task group P3.x). This week you repeat that load as the shared platform version, so keep your Week 6 tables as a size check. |
| - [ ] | **Week 7:** the RBI CSV you downloaded from DBIE by hand, saved in `data/raw/rbi/` with the pull date in the file name (task P4.2). The RBI data for this cohort ends in 2024. That is OK. |
| - [ ] | **Week 10:** your state-name frequency map (the list of how many spelling variants each state has). Task D5.2 uses it. |
| - [ ] | **Week 9:** the IBBI extractor output, an IBBI CSV from the PDF (task group P7.x). |
| - [ ] | **Your CDM CSV** (MCA state statistics): no earlier task creates it. Before Task 5, open `https://mcacdm.nic.in` in the browser, download or export the state statistics table to `data/raw/cdm/cdm_state_statistics.csv`, and record the export date. If you cannot find it, ask your program lead before Task 5. Do not type fake data. |
| - [ ] | **Your Snowflake values:** account URL, username, role (usually `STUDENT_ROLE`), warehouse (usually `STUDENT_WH`), your database and your schema. |

If something is missing: redo the earlier task when you can. If you cannot, say so in your notes and in the cohort channel. Never fill a gap with invented data.

**By the end of this week you can:**

- Set up stages and file formats for four sources that arrive in four different shapes
- Load every source into bronze with file name and load timestamp on every row
- Teach the load path to a teammate and watch them do it
- Write dbt staging models with tests that fail when the data is wrong

**Core concepts to review first:**

- Snowflake stages, file formats, `PUT`, `COPY INTO`, and `METADATA$FILENAME`.
- Bronze, silver and gold: bronze keeps raw text, silver types and cleans, gold makes business decisions.
- dbt Core models, sources, tests, `dbt run`, `dbt test`, and `dbt debug`.
- The RBI policy rate is a step series. Use carry-forward when no new row appears. The RBI file ends in 2024. Carry the last rate forward.

## 4. Where to Find Resources

> **Where you work (same rule every week)**
> 1. VS Code is your main work place. You write files here, save them here, and commit here.
> 2. Snowflake (Snowsight, in the browser) is where SQL runs. Open Snowsight > Projects > Workspaces. Paste your SQL, pick the role and warehouse, run it.
> 3. VS Code terminal is where you run Git and Python (with `.venv` active).
> 4. `DEx` in a path means your own number, for example `students/DE12/week12/...`.
> 5. SQL files are saved in VS Code so they can be committed. Snowflake only runs them.

> **How your script logs in**
> The program lead sets the login method for Python and dbt: key-pair or a Programmatic Access Token (PAT). You never paste a password in code. If a script login fails with an MFA message, ask the program lead. Search the Snowflake docs for the title "Planning for the deprecation of single-factor password sign-ins". Ask your program lead for the cohort authentication settings.

**Before you open this file**

| File | What it answers |
|---|---|
| `docs/15-team-project-repo-and-delivery.md` | What `platform/` and `delivery/` are, and who is allowed to write there |
| `docs/03-student-guide.md` | Git commands, branches, pull requests |
| `docs/08-glossary.md` | Any word you do not know |
| `docs/10-troubleshooting.md` | Broken tool, empty dbt model |
| `docs/11-tools-and-technology.md` | What a tool is used for |

**Your week in order:**

| Step | Learn first (exact place, time) | Then do |
|---|---|---|
| **1** | Snowflake docs search "CREATE STAGE", "PUT", "file format options" (about 55 min) | Tasks 1-3 - stages, file formats, the Tasks paragraph |
| **2** | Snowflake docs search "COPY INTO table", "METADATA$FILENAME" (about 45 min) | Tasks 4-5 - MCA loads, enrichment loads |
| **3** | Nothing new. You use Tasks 4-5 | Tasks 6-7 - reconciliation, teach-back |
| **4** | dbt Learn "dbt Fundamentals" (free, skip the dbt Cloud UI parts) plus dbt docs "About dbt projects" (about 2 hours) | Tasks 8-10 - dbt project, staging models, tests |
| **5** | dbt docs "Sources", "Add tests to your DAG" (about 30 min) | Cohort review: bronze reconciliation numbers, dbt test results |

**Search keywords if you get stuck:** "Snowflake CREATE STAGE", "Snowflake COPY INTO", "METADATA$FILENAME", "dbt sources", "dbt tests", "dbt debug".

## 5. Step-by-Step Guide

Work in one task, one file, one commit. Do not try to do many tasks at once. Git steps are not repeated here. See `docs/03-student-guide.md` for Git commands, branches and pull requests. Work through the task groups in order. Do not skip the milestone.

### Group 1 - Stages and File Formats

#### Task 1 - Create the stages (ID `D2.1`)

##### 1. What We Are Doing

You will create one Snowflake stage for each of the four sources. You write the SQL file in VS Code, then run it in Snowflake. In Snowsight, open Projects > Workspaces.

##### 2. Why We Are Doing It

Our four sources arrive in four different shapes. One stage per source keeps the files apart, so you can always say which file a wrong row came from. That trace is what a due-diligence reviewer asks for.

##### 3. What to Learn First

- Snowflake docs, https://docs.snowflake.com . Search "CREATE STAGE" and read the page.
- Search "PUT" and read only the examples.
- About 30 minutes.

##### 4. Where to Find Resources

- Snowflake docs, https://docs.snowflake.com , search "CREATE STAGE" and "PUT".
- Search keywords: "CREATE STAGE", "PUT", "stage prefix", "SHOW STAGES".

##### 5. Step-by-Step Guide

1. In VS Code, open `students/DEx/week12/`. Right-click the `sql` folder > New Folder > name it `d2`.
2. Right-click `students/DEx/week12/sql/d2` > New File > `01_stages.sql`.
3. Paste this starter. Fill the TODOs. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, and the database and schema your program lead gave you.

```sql
USE ROLE STUDENT_ROLE;
USE WAREHOUSE STUDENT_WH;
USE DATABASE <your_database>;
USE SCHEMA <your_schema>;

-- One internal stage per source, one clear folder prefix per source.
CREATE OR REPLACE STAGE stg_mca
  COMMENT = 'MCA RoC files';

-- TODO: stg_ibbi, stg_cdm, stg_rbi
-- TODO: decide the prefix you will PUT into for each stage
```

4. In Snowsight, open Projects > Workspaces. Paste and run the file. Then run `SHOW STAGES;` and copy the output into your notes.
5. Make the saved `.sql` file in VS Code match what actually worked.
6. Right-click `students/DEx/week12/sql/d2` > New File > `notes.md`. Add a table with one row per source: stage name, prefix, file type, the person who owns it.

**How to think about it:** one stage per source keeps files apart. The prefix is part of the trace from a row back to its file.

If you get stuck: read the full error text in Snowsight; run `SHOW STAGES;` to see what exists; search the Snowflake docs for "CREATE STAGE"; ask in Discord and paste the error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/sql/d2/01_stages.sql`
- [ ] `students/DEx/week12/sql/d2/notes.md`

**Done when**

- [ ] `SHOW STAGES` lists your four stages.
- [ ] The notes table has one row per source.
- [ ] The `.sql` file in VS Code matches what ran.

**Commit message:** `D2.1 create stages for four sources`

#### Task 2 - Define the file formats (ID `D2.2`)

##### 1. What We Are Doing

You will create named Snowflake file formats for the sources. You write the SQL in VS Code, then run it in Snowflake.

##### 2. Why We Are Doing It

The MCA files, the IBBI CSV, the CDM CSV and the RBI file are not identical. A named file format makes those differences visible and repeatable instead of retyped in every load.

##### 3. What to Learn First

- Snowflake docs search "CREATE FILE FORMAT" and read the list of options.
- About 25 minutes.

##### 4. Where to Find Resources

- Snowflake docs, https://docs.snowflake.com , search "CREATE FILE FORMAT".
- Search keywords: "CREATE FILE FORMAT", "SKIP_HEADER", "FIELD_OPTIONALLY_ENCLOSED_BY", "encoding".

##### 5. Step-by-Step Guide

1. Create `students/DEx/week12/sql/d2/02_file_formats.sql`.
2. Start from this skeleton. Add the second format only if one source needs it.

```sql
USE ROLE STUDENT_ROLE;
USE WAREHOUSE STUDENT_WH;
USE DATABASE <your_database>;
USE SCHEMA <your_schema>;

CREATE OR REPLACE FILE FORMAT ff_csv_header
  TYPE = 'CSV'
  FIELD_DELIMITER = ','
  SKIP_HEADER = 1
  FIELD_OPTIONALLY_ENCLOSED_BY = '"'
  -- TODO: set the remaining options you need, for example ENCODING,
  -- NULL_IF, EMPTY_FIELD_AS_NULL, TRIM_SPACE
  ;
```

3. Run it in Snowsight.
4. In `students/DEx/week12/sql/d2/notes.md`, write one line per option: what you set and why. One line each, no essays.
5. Record the encoding each source needed.

**On Task 2:** record the encoding each source needed too. If one file only parsed as `latin-1`, that is a real property of the source and the next person must be told.

**How to think about it:** a named file format makes source differences visible and repeatable.

If you get stuck: read the error message and note which option name it names; search the Snowflake docs for "CREATE FILE FORMAT"; ask in Discord with the full error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/sql/d2/02_file_formats.sql`
- [ ] `students/DEx/week12/sql/d2/notes.md`

**Done when**

- [ ] The file format exists in Snowflake.
- [ ] Every option has a one-line reason in notes.
- [ ] The encoding per source is written down.

**Commit message:** `D2.2 define file formats for the four sources`

#### Task 3 - When is a Task the right tool? (ID `D2.3`)

##### 1. What We Are Doing

You will write a short answer in your notes file. The question is when a scheduled Snowflake Task is the right tool and when a person should run the load by hand. You work in VS Code.

##### 2. Why We Are Doing It

Someone has to decide what runs on a schedule and what a person runs by hand. Getting this wrong means a stale table that nobody notices.

##### 3. What to Learn First

- Snowflake docs search "Introduction to tasks". Skim only, about 15 minutes.

##### 4. Where to Find Resources

- Snowflake docs, https://docs.snowflake.com , search "Introduction to tasks".
- Search keywords: "Snowflake tasks", "scheduled run", "warehouse cost", "silent failure".

##### 5. Step-by-Step Guide

1. Append a paragraph to `students/DEx/week12/sql/d2/notes.md`.
2. Answer these questions in your own words, in one paragraph:
   - What does a scheduled run give you that typing `COPY INTO` yourself does not?
   - Which of our four sources arrives again and again on a known rhythm, and which arrives once or rarely?
   - What must be true before a scheduled run is safe (who pays for the warehouse time, who reads the error)?
   - If a scheduled run fails silently, which table is wrong, and who finds out?

**How to think about it:** Compare your recommendation with the arrival patterns and responsibilities you recorded.

If you get stuck: reread your Task 1 notes; make a two-column list of "arrives again" and "arrives once"; ask in Discord with your draft paragraph.

##### 6. What to Commit

**File to commit**

- [ ] `students/DEx/week12/sql/d2/notes.md`

**Done when**

- [ ] The paragraph recommends manual or scheduled loading for each source and explains the recommendation using its arrival pattern, cost owner and error owner.

**Commit message:** `D2.3 when a Snowflake Task is the right tool`

### Group 2 - Bronze Loads

#### Task 4 - Load all MCA files (ID `D3.1`)

##### 1. What We Are Doing

You will load all MCA RoC files into bronze tables. You write the SQL in VS Code, then run the loads in Snowflake. One bronze table per RoC office.

##### 2. Why We Are Doing It

Bronze is the raw copy of what the registry gave us. Keeping all columns as text means we never lose a row to a bad date or a comma in capital. We clean in silver, not here.

##### 3. What to Learn First

- Read the MCA data dictionary (published with the registry catalog on data.gov.in) before you decide your columns.
- Snowflake docs search "COPY INTO table" and read the examples.
- Then search "METADATA$FILENAME". About 40 minutes.

##### 4. Where to Find Resources

- Snowflake docs, https://docs.snowflake.com , search "COPY INTO table" and "METADATA$FILENAME".
- MCA data dictionary: published with the registry catalog on https://data.gov.in .
- Search keywords: "COPY INTO table", "METADATA$FILENAME", "PUT", "VARCHAR bronze".

##### 5. Step-by-Step Guide

1. Read the MCA data dictionary (published with the registry catalog on data.gov.in) before you decide your columns.
2. Create `students/DEx/week12/sql/d3/` and file `students/DEx/week12/sql/d3/01_mca_raw_loads.sql`.
3. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema. Starter:

```sql
CREATE OR REPLACE TABLE bronze_mca_roc_<name> (
  -- TODO: one VARCHAR column per field in the data dictionary
  cin               VARCHAR,
  company_name      VARCHAR,
  registration_date VARCHAR,
  -- TODO: status, class, category, capital columns, state, roc, address ...
  _source_file      VARCHAR,
  _loaded_at        TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP()
);

-- TODO: one row per field, in the same order as the file
COPY INTO bronze_mca_roc_<name>
  FROM (SELECT $1, $2, $3 /* ... */,
               METADATA$FILENAME, CURRENT_TIMESTAMP()
        FROM @stg_mca/<prefix>/<file>.csv)
  FILE_FORMAT = (FORMAT_NAME = ff_csv_header);
```

4. Before you run `PUT`, ask your program lead which upload client is approved for the cohort (for example the Snowflake CLI `snow`, or another client the program lead names). Set it up by following that client's install and sign-in instructions. Run `PUT` in that client on your laptop, from the folder that contains the file or with the full local path. Then run `COPY INTO` in Snowsight.
5. Create `students/DEx/week12/sql/d3/load_summary.md` with a row per file: file name, rows loaded, time taken.
6. One bronze table per RoC. Do not mix two offices in one table.

**How to think about it:** bronze keeps raw text. You clean in silver, not here.

If you get stuck: read the error message; run `LIST @stg_mca/<prefix>/` to check the file is on the stage; search the Snowflake docs for "COPY INTO table"; ask in Discord with the error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/sql/d3/01_mca_raw_loads.sql`
- [ ] `students/DEx/week12/sql/d3/load_summary.md`

**Done when**

- [ ] Each RoC file has its own bronze table.
- [ ] Every source-data column is `VARCHAR`; `_source_file` is `VARCHAR` and `_loaded_at` is `TIMESTAMP_NTZ`.
- [ ] A row count query returns rows.
- [ ] `load_summary.md` has one line per file.

**Commit message:** `D3.1 load MCA files into bronze with file name and load time`

#### Task 5 - Load the enrichment sources (ID `D3.2`)

##### 1. What We Are Doing

You will load IBBI, CDM and RBI files into bronze tables. You write the SQL in VS Code, then run it in Snowflake. Each table gets the same two metadata columns as Task 4.

##### 2. Why We Are Doing It

The insolvency list (IBBI), the state statistics (CDM) and the policy rate (RBI) turn a company record into a due-diligence answer. They get the same treatment as MCA.

##### 3. What to Learn First

Nothing new. You use what you learned in Task 4.

##### 4. Where to Find Resources

- Use the Task 4 steps and the Snowflake docs search "COPY INTO table" again.
- Search keywords: "COPY INTO table", "METADATA$FILENAME", "bronze load".

##### 5. Step-by-Step Guide

1. If you do not already have a CDM CSV, open `https://mcacdm.nic.in` in the browser, download or export the state statistics table, and save it as `data/raw/cdm/cdm_state_statistics.csv`. Record the export date in your notes. No earlier task creates this CSV.
2. In `students/DEx/week12/sql/d3/`, create `students/DEx/week12/sql/d3/02_enrichment_raw_loads.sql`.
3. Same role, warehouse, database and schema as Task 4.
4. Skeleton: three tables, three loads, the same two metadata columns.

```sql
CREATE OR REPLACE TABLE bronze_ibbi (
  -- TODO: cin, company_name, bench, start_date, admitted_claims
  _source_file VARCHAR,
  _loaded_at   TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP()
);

-- TODO: bronze_cdm and bronze_rbi, same pattern

COPY INTO bronze_ibbi
  FROM (SELECT $1, $2, $3, $4, $5, METADATA$FILENAME, CURRENT_TIMESTAMP()
        FROM @stg_ibbi/<prefix>/<file>.csv)
  FILE_FORMAT = (FORMAT_NAME = ff_csv_header);
```

5. Upload the IBBI file with your approved upload client from Task 4: run `PUT` to `@stg_ibbi/<prefix>/`. Run `LIST @stg_ibbi/<prefix>/` and check the file is there. Then run the IBBI `COPY INTO bronze_ibbi`.
6. Upload the CDM file with `PUT` to `@stg_cdm/<prefix>/`. Run `LIST @stg_cdm/<prefix>/` and check the file is there. Then run the CDM `COPY INTO bronze_cdm`.
7. Upload the RBI file with `PUT` to `@stg_rbi/<prefix>/`. Run `LIST @stg_rbi/<prefix>/` and check the file is there. Then run the RBI `COPY INTO bronze_rbi`.
8. Add each file to `students/DEx/week12/sql/d3/load_summary.md`.

**How to think about it:** the same metadata columns on every bronze table keep the trace the same across sources.

If you get stuck: compare the failing load with Task 4 line by line; check the file is on the stage with `LIST`; ask in Discord with the error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/sql/d3/02_enrichment_raw_loads.sql`
- [ ] `students/DEx/week12/sql/d3/load_summary.md`

**Done when**

- [ ] IBBI, CDM and RBI each have a bronze table.
- [ ] Each row shows its source file and load time.
- [ ] `load_summary.md` lists all three.

**Commit message:** `D3.2 load IBBI, CDM and RBI files into bronze`

#### Task 6 - Reconcile every table (ID `D3.3`)

##### 1. What We Are Doing

You will compare the row count in each bronze table with the data row count in its source file. You write the SQL in VS Code, run it in Snowflake, and write the results in `load_summary.md`.

##### 2. Why We Are Doing It

A number that does not match is the first thing a reviewer finds. A written explanation is worth more than a hidden fix.

##### 3. What to Learn First

Nothing new. You use what you learned in Task 4.

##### 4. Where to Find Resources

- Use your Task 4 files and the Snowflake docs search "COPY INTO table" again.
- Search keywords: "COUNT(*)", "UNION ALL", "reconciliation", "CSV row count".

##### 5. Step-by-Step Guide

1. Count the data rows in each file. Open the file in VS Code, or reuse your Python CSV reader from Week 3.
2. In `students/DEx/week12/sql/d3/`, create `03_reconciliation.sql`. One query, one row per table:

```sql
SELECT 'bronze_mca_roc_<name>' AS table_name, COUNT(*) AS rows_in_table
  FROM bronze_mca_roc_<name>
UNION ALL
SELECT 'bronze_ibbi', COUNT(*) FROM bronze_ibbi
-- TODO: add every bronze table
;
```

3. Run it in Snowflake.
4. Put the results and the file row counts side by side in `load_summary.md`. Every pair must match, or carry a written reason.

**How to think about it:** a written reason for a mismatch is better than a hidden fix.

If you get stuck: check whether you counted the header row; run the `COUNT(*)` query one table at a time; ask in Discord with both numbers.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/sql/d3/03_reconciliation.sql`
- [ ] `students/DEx/week12/sql/d3/load_summary.md`

**Done when**

- [ ] Every bronze table has one reconciliation line.
- [ ] No line says "does not match" without a reason next to it.

**Commit message:** `D3.3 reconcile every bronze table against its file`

**Why the file name and load timestamp matter:** they are the only way, at handover, to answer "which file did this wrong row come from". That is lineage, and it costs two columns.

### Group 3 - Teach-Back: the Load Path **[MILESTONE]**

#### Task 7 - Teach the load path (ID `D4.1`)

##### 1. What We Are Doing

You will teach the manual load path to a teammate and watch your teammate do it on a different RoC file. You work in VS Code to write notes and in Snowflake to do the demo. Do the demo live with your teammate, then write.

##### 2. Why We Are Doing It

In Week 15 the project is handed over. Another teammate must be able to repeat the load.

##### 3. What to Learn First

Nothing new. You use Tasks 4-6.

##### 4. Where to Find Resources

- Use your Task 4-6 files and your `load_summary.md`.
- Search keywords: "PUT", "LIST stage", "COPY INTO", "row count check".

##### 5. Step-by-Step Guide

1. Demo the six steps below to a teammate, talking out loud.
2. The teammate repeats the same six steps on a **different RoC file** while you watch and only answer questions.
3. Demo and check list:
   1. Put the file on the stage with `PUT`.
   2. Run `LIST @stg_mca/<prefix>/` and show the file is there.
   3. Run `COPY INTO` the bronze table.
   4. Compare the row count in the table with the row count in the file.
   5. Find `_source_file` and `_loaded_at` on a row you just loaded.
   6. Say what happens if you run the same `COPY INTO` twice.
4. Both of you write notes in your own week folder: where the other person got stuck, and what you had to explain twice.
5. Each of you commits your own notes file and opens a pull request. Put your partner's pull request link inside your notes.

**On Task 7:** the notes are the deliverable, not the load. Write where they got stuck and what you had to explain twice. That is the honest measure of whether the load path is documented well enough for the Week 15 handover.

**How to think about it:** the notes are the deliverable, not the load. Write where your partner got stuck and what you explained twice.

If you get stuck: use the six-step list as your script; reread Task 4 before the demo; ask in Discord with the step where your partner stopped.

##### 6. What to Commit

**File to commit**

- [ ] `students/DEx/week12/sql/d4/teachback_notes.md` (one in each student's folder)

**Done when**

- [ ] Your partner completed all six steps on a different file.
- [ ] Both notes files exist.
- [ ] Each notes file names at least one thing that had to be explained twice.

**Commit message:** `D4.1 teach-back notes for the manual load path`

### Group 4 - dbt Staging Models

**What is dbt, in plain words**

dbt is a tool that runs your `.sql` `SELECT` files in the right order and builds tables or views from them. Each file becomes a model. dbt also runs tests on the result. You write `SELECT`; dbt works out the order and stores the output in Snowflake. It keeps logic in Git, not only in someone's memory.

In this cohort, dbt means **dbt Core**. dbt Core runs from the VS Code terminal on your laptop. dbt Core is the default. dbt Projects on Snowflake is an optional extra, not the default.

**Where these files go**

Student practice: `students/DEx/week12/dbt/` with the same layout as the real project (`models/staging`, `models/marts`).
Team leader: create the team repo `dbt/` folder, add a line about it to the team repo `README.md`, mirror the reviewed files there, and then mirror them into `platform/dbt/` in the class repo.
Never commit `profiles.yml` or `.env`. They hold login values.

**How to run dbt Core (the cohort default)**

1. Open the VS Code terminal and activate the virtual environment: `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows).
2. `pip install dbt-snowflake`
3. `dbt --version` (if the command is not found, the environment is not active).
4. Create `~/.dbt/profiles.yml`. This is your user folder, **not** the repo. The profile name must match the `profile:` value in `dbt_project.yml`.

```yaml
my_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: "{{ env_var('SNOWFLAKE_ACCOUNT') }}"
      user: "{{ env_var('SNOWFLAKE_USER') }}"
      role: STUDENT_ROLE
      warehouse: STUDENT_WH
      database: <your_database>
      schema: <your_schema>
      threads: 4
      # Add the key-pair or PAT login setting your program lead gave you.
      # Check the Snowflake and dbt docs for the exact parameter spelling.
      # Never paste a password into code or into this file.
```

5. Put your account, user and login values in your `.env` file (never committed). Load them into the terminal before you run dbt. For example, export each required variable in the VS Code terminal (macOS/Linux: `export SNOWFLAKE_ACCOUNT="..."`; Windows PowerShell: `$env:SNOWFLAKE_ACCOUNT="..."`). Another way is the method your program lead gives. Check only that each required name exists, without printing its value (macOS/Linux: `test -n "$SNOWFLAKE_ACCOUNT" && echo SNOWFLAKE_ACCOUNT is set`; Windows PowerShell: `if ($env:SNOWFLAKE_ACCOUNT) { "SNOWFLAKE_ACCOUNT is set" }`). If a login fails with an MFA message, ask the program lead. Search the Snowflake docs for the title "Planning for the deprecation of single-factor password sign-ins".
6. From inside the dbt project folder: `dbt debug` (it must say the connection works), then `dbt run`, then `dbt test`.

**Optional extra:** dbt Projects on Snowflake can run the same `.sql` and `.yml` files in a Snowsight Workspace. It is not the cohort default. For the general idea, see https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake and https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake-using-workspaces .

#### Task 8 - Initialize the dbt project (ID `D5.1`)

##### 1. What We Are Doing

You will create the dbt project layout and declare the bronze tables as sources. You work in VS Code for files and in the VS Code terminal for dbt. Order: learn first, then build, then `dbt debug`.

##### 2. Why We Are Doing It

Bronze tables are raw text. Every model above bronze must read them the same way, once, in one place. That shared read is the source definition plus the staging models.

##### 3. What to Learn First

- dbt Learn, https://learn.getdbt.com , course "dbt Fundamentals" (free). Watch the modules on models, sources and tests. Skip the dbt Cloud UI parts.
- Then read dbt docs "About dbt projects" on https://docs.getdbt.com .
- About 2 hours. You do this once for Tasks 8-10.

##### 4. Where to Find Resources

- dbt Learn, https://learn.getdbt.com , course "dbt Fundamentals".
- dbt docs, https://docs.getdbt.com , "About dbt projects".
- Search keywords: "dbt project", "sources.yml", "dbt debug", "dbt models".

##### 5. Step-by-Step Guide

1. Create `students/DEx/week12/dbt/`, and inside it create `students/DEx/week12/dbt/models/staging/` and `students/DEx/week12/dbt/models/marts/`.
2. Add `students/DEx/week12/dbt/dbt_project.yml` with model paths `models`, seeds, snapshots, macros, tests. Staging models are views, marts are tables. Make sure the `profile:` name matches the profile name in `~/.dbt/profiles.yml`.
3. Add `students/DEx/week12/dbt/models/staging/sources.yml` for the bronze tables:

```yaml
version: 2

sources:
  - name: bronze
    database: <your_database>
    schema: <your_schema>
    tables:
      - name: bronze_mca_roc_<name>
      # TODO: the other MCA tables, then bronze_ibbi, bronze_cdm, bronze_rbi
```

4. Set up dbt Core from the section above. Stop when `dbt debug` passes.
5. Add `students/DEx/week12/dbt/README.md`: one line per folder, saying what belongs in it.
6. If you are the team leader, create the `dbt/` folder in the team repo and add a line to the team repo `README.md` that explains what is in it. After review, mirror the same reviewed files into the team repo `dbt/` folder and then into `platform/dbt/` in the class repo. Open the pull request there.

**How to think about it:** the source definition is the one shared read of bronze. Put it in one place.

If you get stuck: read the `dbt debug` output from the first error line; check that `profiles.yml` is not in the repo; search the dbt docs for "About dbt projects"; ask in Discord with the error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/dbt/dbt_project.yml`
- [ ] `students/DEx/week12/dbt/README.md`
- [ ] `students/DEx/week12/dbt/models/staging/sources.yml`

**Done when**

- [ ] `dbt debug` passes.
- [ ] The folder layout exists.
- [ ] The README explains the layout.
- [ ] `git status` shows no `profiles.yml` and no `.env`.

**Commit message:** `D5.1 initialize dbt project with bronze sources declared`

#### Task 9 - Write the MCA staging model (ID `D5.2`)

##### 1. What We Are Doing

You will write the MCA staging model and its tests. You work in VS Code to write the model and tests, then in the VS Code terminal to run `dbt run` and `dbt test`.

##### 2. Why We Are Doing It

Silver types and cleans. A reviewer comparing two companies in two states must see one spelling per state, and dates must be real dates. Bad CINs are flagged, never deleted.

##### 3. What to Learn First

- dbt docs "Sources" and "Add tests to your DAG" (search these exact titles on https://docs.getdbt.com ).
- About 30 minutes.

##### 4. Where to Find Resources

- dbt docs, https://docs.getdbt.com , search "Sources" and "Add tests to your DAG".
- Search keywords: "dbt sources", "dbt tests", "try_to_date", "state name map".

##### 5. Step-by-Step Guide

1. Create `students/DEx/week12/dbt/models/staging/stg_mca.sql`. The source CTE must use all three MCA bronze tables you declared in Task 8, with `UNION ALL` and an explicit column list that matches in every branch.
2. Skeleton below. Keep the CTE shape; fill the TODOs.

```sql
with source as (
    select
        cin,
        company_name,
        registration_date,
        -- TODO: status, class, category, capital columns, state, roc, address
        _source_file,
        _loaded_at
    from {{ source('bronze', 'bronze_mca_roc_<name1>') }}
    union all
    select
        cin,
        company_name,
        registration_date,
        -- TODO: status, class, category, capital columns, state, roc, address
        _source_file,
        _loaded_at
    from {{ source('bronze', 'bronze_mca_roc_<name2>') }}
    union all
    select
        cin,
        company_name,
        registration_date,
        -- TODO: status, class, category, capital columns, state, roc, address
        _source_file,
        _loaded_at
    from {{ source('bronze', 'bronze_mca_roc_<name3>') }}
),

cleaned as (
    select
        cin,
        -- TODO: flag bad CINs (a CIN is 21 characters), do not delete them
        company_name,
        -- TODO: parse the dates. More than one date format appears in the files:
        -- use try_to_date so a bad value becomes null instead of an error
        -- TODO: map the state name to one clean value using your Week 10 frequency map
        -- TODO: remove commas from capital, then cast to a number
        _source_file,
        _loaded_at
    from source
)

select * from cleaned
```

3. In `students/DEx/week12/dbt/models/staging/schema.yml`, add tests where they belong:

```yaml
version: 2

models:
  - name: stg_mca
    columns:
      - name: cin
        tests:
          - not_null
      # TODO: which column must be unique in this model? Add that test.
```

4. Run `dbt run` then `dbt test` from the dbt project folder in the VS Code terminal.

**On Task 9:** bad CINs are flagged, not deleted. Silver types and cleans; it does not decide what counts as a real company. That decision belongs in gold, where it is visible.

**How to think about it:** silver types and cleans. Bad CINs are flagged, not deleted. The decision about what counts as a real company belongs in gold, where it is visible.

If you get stuck: open `target/compiled/` and read the SQL dbt ran; check the indentation in `schema.yml`; search the dbt docs for "Add tests to your DAG"; ask in Discord with the error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/dbt/models/staging/stg_mca.sql`
- [ ] `students/DEx/week12/dbt/models/staging/schema.yml`

**Done when**

- [ ] `dbt run` builds `stg_mca`.
- [ ] `dbt test` runs.
- [ ] The state column has one spelling per state.
- [ ] Bad CINs appear as a flag column with the rows still present.

**Commit message:** `D5.2 MCA staging model with tests`

#### Task 10 - Write the remaining staging models (ID `D5.3`)

##### 1. What We Are Doing

You will write the IBBI, CDM and RBI staging models and add tests. You work in VS Code to write the models and tests, then in the VS Code terminal for `dbt run` and `dbt test`.

##### 2. Why We Are Doing It

The platform answers "is this company real, was it ever insolvent, how does it compare to its state, and what was the repo rate then". Each of those needs one clean staging model.

##### 3. What to Learn First

Nothing new. You use what you learned in Task 9.

##### 4. Where to Find Resources

- Use the dbt docs "Sources" and "Add tests to your DAG" again if needed.
- Search keywords: "dbt staging", "try_to_date", "lead window", "dbt tests".

##### 5. Step-by-Step Guide

1. Create `students/DEx/week12/dbt/models/staging/stg_ibbi.sql`, `students/DEx/week12/dbt/models/staging/stg_cdm.sql` and `students/DEx/week12/dbt/models/staging/stg_rbi.sql`, same discipline as Task 9.
2. The RBI file is a **step series**. A row exists only on the day the rate changes. So "no rows for this year" does not mean data is missing. Carry the rate forward:

```sql
with source as (
    select * from {{ source('bronze', 'bronze_rbi') }}
),

changes as (
    select
        -- TODO: parse the change date with try_to_date
        -- TODO: parse the rate
        row_number() over (order by <change date>) as change_order
    from source
),

carried as (
    select
        <change date> as valid_from,
        -- TODO: valid_to = the next change date (lead), null = still in force
        lead(<change date>) over (order by <change date>) as valid_to,
        <rate>
    from changes
)

select * from carried
```

3. The RBI file for this cohort ends in 2024. That is not a blocker. Carry the last rate forward. If you want to go further, add later change rows by hand from the RBI press releases and record the source link and date in `students/DEx/week12/dbt/rbi_notes.md`. For this week, write down the date range you found in the file in `students/DEx/week12/dbt/rbi_notes.md`.
4. Add `not_null` and `unique` tests in `students/DEx/week12/dbt/models/staging/schema.yml` where they belong.
5. Run `dbt run` first and fix any build errors. Then run `dbt test`. Paste the full output into `students/DEx/week12/dbt/test_results.md`.

**How to think about it:** a policy rate is a step series. A row exists only on a change day. Carry the latest rate forward until a newer row appears.

If you get stuck: open `target/compiled/` and read the SQL dbt ran; check the `lead()` order in the RBI model; search the dbt docs for "Add tests to your DAG"; ask in Discord with the error text.

##### 6. What to Commit

**Files to commit**

- [ ] `students/DEx/week12/dbt/models/staging/stg_ibbi.sql`
- [ ] `students/DEx/week12/dbt/models/staging/stg_cdm.sql`
- [ ] `students/DEx/week12/dbt/models/staging/stg_rbi.sql`
- [ ] `students/DEx/week12/dbt/models/staging/schema.yml`
- [ ] `students/DEx/week12/dbt/test_results.md`
- [ ] `students/DEx/week12/dbt/rbi_notes.md`

**Done when**

- [ ] All four staging models build.
- [ ] `dbt test` output is pasted in `students/DEx/week12/dbt/test_results.md`.
- [ ] `students/DEx/week12/dbt/rbi_notes.md` states the date range found and the source of any hand-added rows.

**Commit message:** `D5.3 staging models for IBBI, CDM and RBI with tests`

**If a dbt model runs but the table is empty,** open `target/compiled/` to read the SQL dbt actually ran. See `docs/10-troubleshooting.md`, dbt section.

## 6. What to Commit

**End of week checklist**

- [ ] Tasks 1-3 - stages with prefixes, file formats with justified options, the Tasks paragraph
- [ ] Tasks 4-6 - every source in bronze with file name and load timestamp, and a reconciliation line per table
- [ ] Task 7 - teach-back notes from both people, linked in one pair of pull requests
- [ ] Tasks 8-10 - dbt project with a README, four staging models, tests run with results pasted
- [ ] No `profiles.yml`, no `.env`, no data files in any commit
- [ ] If you are the team leader, the reviewed files are mirrored into `platform/` (or `delivery/`) in the class repo

**If you are short on time, cut in this order:** Task 10 (D5.3, do MCA only), then Task 3 (D2.3). Never cut Task 6 (D3.3) or Task 7 (D4.1). Unreconciled bronze makes every table above it wrong, and the teach-back is a milestone.

When your week is done, open one pull request for your week folder and follow the steps in `docs/03-student-guide.md`.

Next: `week13/problem_statement.md`.
