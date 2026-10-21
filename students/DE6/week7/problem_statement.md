# Week 7 - Readable SQL and the agreed model

**Data Sprint 1 · Week 7 of 15 · Theme: write SQL someone else can review, then agree the shape of the gold layer**

## 1. What We Are Doing

This week you make your SQL easier to review, agree the shape of the gold layer, and make your Python downloads and file handling safer. Read this whole file before you start. Then work through the task groups in order.

The task groups are:
- Group 1 - Views and CTEs: Tasks 1-3 (`S6.1`, `S6.2`, `S6.3`)
- Group 2 - Star Schema Design [MILESTONE]: Tasks 4-6 (`S7.1`, `S7.2`, `S7.3`)
- Group 3 - Retry Logic and Context Managers: Tasks 7-9 (`P4.1`, `P4.2`, `P4.3`)

## 2. Why We Are Doing It

The client uses the platform for due diligence on Indian companies. Analysts must be able to trust and check your insolvency query, see one current row per company, compare a company with its state, and use agreed gold table names. The Python work matters too: downloads fail, files can stay open after an error, and the pipeline must keep running without a person rerunning steps by hand.

**The milestone this week: S7 Star Schema Design.** Table names, column names, and the grain of the fact table get agreed cohort-wide. Everything in weeks 11 and 13 is built on the names decided here, so a private naming choice becomes everyone's problem later.

## 3. What to Learn First (Prerequisites)

### Before this week: what you must already have

| What you need | Where to look or what to do if missing |
|---|---|
| Your week 3 practice kit in Snowflake: `RAW_MCA_<ROC>` tables (one per RoC file), `RAW_IBBI_SAMPLE`, and `RAW_MCA_PREV`. | Redo the "Step 0 - practice data kit" in `week3/problem_statement.md`. |
| Your S3.3 insolvency join query (IBBI rows joined to company rows). | Redo Task S3.3 in `week4/problem_statement.md`. You rewrite this query in Task 1 this week. |
| Your week 3 Python file handling and CSV scripts (the P1 group). | Redo the P1 tasks in `week3/problem_statement.md`. You rewrite their file handling in Task 9. |
| Your week 6 proper load files working, if you finished them. | If not, the week 3 wizard tables are enough for this week. |
| The three MCA ZIPs unzipped into `data/raw/mca/`. | See week 2, "Download your practice data". |
| Your `.venv` created and `requests` installed (`pip install requests` in the VS Code terminal with `.venv` active). | Create `.venv` again and install `requests`. |

### By the end of this week you can

- Rewrite a nested query as CTEs a teammate can read without asking you questions.
- State the grain of a table in one sentence, and know why that sentence matters.
- Design a star schema and defend it against the alternatives.
- Write code that survives a failing network call instead of crashing.

### Core concepts to review first

- CTE (common table expression): a named block of SQL before the final `SELECT`, using `WITH`.
- View: a saved query in Snowflake that you can `SELECT` from.
- Grain: one sentence that says what one row in a table means.
- Star schema: a fact table in the centre and dimension tables around it.
- SCD Type 1 and Type 2: Type 1 overwrites the old value. Type 2 keeps an old version row and a new version row.
- Mermaid `erDiagram`: a way to draw a table relationship diagram inside a Markdown file.
- Python decorator: a function that wraps another function.
- Context manager: a `with` block that closes a file even when an error happens.
- Timeout: a limit on how long a network call can wait.

## 4. Where to Find Resources

### Where you work

- **VS Code** is your home base. You write and save every file here, and you commit here.
- **Snowflake (Snowsight, in the browser)** is where SQL runs. Open Snowsight > Projects > Workspaces, paste your `.sql` file, pick the role and warehouse, run it.
- **VS Code terminal** is where Python runs and where Git commands run.
- Python runs from the **repo root** with `.venv` activated: `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows).
- In every path below, `DEx` means your own number, for example `DE12`. So `students/DEx/week7/` is `students/DE12/week7/` for you.

### Docs table

| What you need | Where to look |
|---|---|
| Git commands and pull requests | `docs/03-student-guide.md` |
| Client story and data sources | `docs/01-project-brief.md` |
| An unknown word | `docs/08-glossary.md` |
| A tool that will not work | `docs/10-troubleshooting.md` |
| What a tool is for | `docs/11-tools-and-technology.md` |

**How your tasks are numbered:** the letters tell you the area. `S` = SQL and data modeling, `P` = Python, `B` = business and delivery, `D` = data platform, `A` = architecture diagrams. The ID goes in your commit message and your pull request title, for example `S6.1 rewrite insolvency join as two CTEs`.

### Your week in order

Learn first, then do.

| Step | LEARN first (time estimate) | DO |
|---|---|---|
| **1** | Kudvenkat Part 39 "Views" (20 min) and Part 48 "Derived tables and common table expressions" plus Part 49 "CTE" (45 min) | Tasks 1-2 - CTE rewrite, current view |
| **2** | Kudvenkat Part 59 "Subqueries" (15 min) and the unnumbered "Difference between where and having" (10 min) | Task 3 - above-average states |
| **3** | On https://www.kimballgroup.com search the technique pages for "Slowly Changing Dimensions" (Types 1 and 2) and "Grain" (40 min). Mermaid basics at https://mermaid.js.org (15 min). Skip the rest. | Tasks 4-6 - grain, SCD types, final diagram with two reviewers |
| **4** | Corey video #37 "Decorators - Dynamically Alter The Functionality Of Your Functions" plus #38 "Decorators With Arguments" (55 min), and Corey video #100 "Python Requests Tutorial: Request Web Pages, Download Images, POST Data, Read JSON, and More" (40 min) | Tasks 7-8 - retry decorator, RBI download |
| **5** | Corey video #49 "Context Managers - Efficiently Managing Resources" (25 min) | Task 9 - context managers, then the cohort review and star schema sign-off |

### Search keywords for students who get stuck

- Snowflake CTE WITH clause
- Snowflake CREATE OR REPLACE VIEW
- Kimball grain statement
- SCD Type 1 and Type 2
- Python retry decorator
- RBI DBIE repo rate step series

## 5. Step-by-Step Guide

Work in small steps. Do one task, in one file, then one commit. Use `docs/03-student-guide.md` for Git and pull request steps. Never squash-merge. This is new for most people. Go slowly.

**Git in one line:** Make one commit per task. When the week's work is ready, open one pull request containing those commits. Details are in `docs/03-student-guide.md`. Never squash-merge.

**Team project repo:** if you are the team leader, copy or sync reviewed project files from the team repo into the class repo's `delivery/` or `platform/` folder and open a pull request. Everyone keeps practice files in `students/DEx/week7/`.

### Group 1 - Views and CTEs

A CTE (common table expression) is a named block of SQL you put before the final `SELECT`, using the word `WITH`. You build the query in readable steps instead of nesting one query inside another.

#### Task 1 - Rewrite the join as CTEs (ID `S6.1`)

##### 1. What We Are Doing

Rewrite your S3.3 insolvency join as two CTEs. The first CTE filters the IBBI rows you need. The second CTE joins those rows to the company table. Work in VS Code first to write the file, then run it in Snowflake.

##### 2. Why We Are Doing It

The analysts must be able to trust and check your insolvency query. A query written in readable steps can be reviewed in minutes.

##### 3. What to Learn First

- Kudvenkat Part 48 "Derived tables and common table expressions" and Part 49 "CTE", about 45 min together.
- He uses SQL Server. Type the Snowflake version in a Workspace and run it.
- Part 50 "Updatable CTE" is optional.

##### 4. Where to Find Resources

In the Kudvenkat playlist (https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB): Part 48 "Derived tables and common table expressions" and Part 49 "CTE", about 45 min together. Part 50 "Updatable CTE" is optional.

Search keywords: `Snowflake WITH clause`, `Snowflake CTE syntax`, `rewrite nested query as CTE`.

##### 5. Step-by-Step Guide

1. In VS Code, create `students/DEx/week7/` if it is not there. Inside it create `sql/`. Inside `sql/`, create `s6/`.
2. Right-click `s6` > New File > `01_cte_rewrite.sql`.
3. Paste this starter and fill the TODOs with your own S3.3 query:
   ```sql
   -- S6.1 Rewrite of the S3.3 insolvency join as two CTEs
   -- In Snowflake: role STUDENT_ROLE, warehouse STUDENT_WH, database YOUR_DB, your own schema
   WITH filtered_ibbi AS (
       -- TODO: first step, only the IBBI rows you need
       SELECT ...
       FROM ...
       WHERE ...
   ),
   joined AS (
       -- TODO: second step, join filtered_ibbi to the company table
       SELECT ...
       FROM filtered_ibbi f
       JOIN ...
   )
   SELECT *
   FROM joined;
   ```
4. Open Snowsight in the browser > Projects > Workspaces. Open a SQL file there. Set role `STUDENT_ROLE`, warehouse `STUDENT_WH`, database the one the program lead gave you, and your own schema.
5. Paste the file, run it. Fix errors until it runs.
6. Write one sentence at the top of the file, as a comment, saying why this version is easier to review.
7. Copy the final working SQL back into the VS Code file and save.

If you get stuck:
- Read the error text first. It often names the line with the missing comma or wrong table name.
- Search the Snowflake docs for `WITH clause` or ask in Discord with the full error text.
- Compare your query to the starter. Check that every CTE has a name and a comma between CTEs.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/sql/s6/01_cte_rewrite.sql`

Done when:
- [ ] The file has two CTEs, one that filters and one that joins.
- [ ] The query runs in Snowflake without error.
- [ ] The one-sentence reason is in the file.

Commit message: `S6.1 rewrite insolvency join as two CTEs`

#### Task 2 - Create the current-company view (ID `S6.2`)

##### 1. What We Are Doing

Create a Snowflake view `v_company_current` that returns one current row per company. Use your week 3 practice tables for now. Your practice kit has two snapshots. `RAW_MCA_PREV` is the older practice snapshot. Your `RAW_MCA_<ROC>` table is the newer one. Give each snapshot an explicit order number: `1` for `RAW_MCA_PREV` and `2` for `RAW_MCA_<ROC>`. Write and save the SQL in VS Code. Run it in a Snowflake Workspace, then save the final working version in VS Code.

##### 2. Why We Are Doing It

The platform must show one current row per company, not every snapshot. Analysts ask "what is this company's status now".

##### 3. What to Learn First

- Kudvenkat Part 39 "Views" (20 min).
- Part 40 "Updatable views" is optional, skip it if you are short on time.

##### 4. Where to Find Resources

In the Kudvenkat playlist (https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB): Part 39 "Views" (20 min). Part 40 "Updatable views" is optional.

Search keywords: `Snowflake CREATE OR REPLACE VIEW`, `Snowflake latest row per group`, `Snowflake view`.

##### 5. Step-by-Step Guide

1. In VS Code create `students/DEx/week7/sql/s6/02_current_view.sql`.
2. Before you write the query, decide the selection method. Put both snapshots into one CTE with `snapshot_order`: `1` for `RAW_MCA_PREV` (older) and `2` for `RAW_MCA_<ROC>` (newer). Then keep only the row with the highest `snapshot_order` for each `cin`. Write this rule as a comment, then fill in the query.
3. Starter to fill in:
   ```sql
   -- S6.2 v_company_current: one row per company, newest practice snapshot
   -- Role STUDENT_ROLE, warehouse STUDENT_WH, database YOUR_DB, your own schema
   CREATE OR REPLACE VIEW v_company_current AS
   WITH all_versions AS (
       -- older practice snapshot
       SELECT ..., 1 AS snapshot_order, 'prev' AS snapshot_label FROM RAW_MCA_PREV
       UNION ALL
       -- newer practice snapshot
       SELECT ..., 2 AS snapshot_order, 'current_practice' AS snapshot_label FROM RAW_MCA_<ROC>
   )
   SELECT ...
   FROM all_versions
   -- TODO: a rule that keeps only the row with the highest snapshot_order for each CIN
   ;
   ```
4. Run it in Snowflake. Then test it: `SELECT COUNT(*) FROM v_company_current;` and compare with a plain count of one snapshot.
5. Note in the file: this practice view uses your week 3 practice tables for now. Real monthly history arrives in week 11.
6. Answer in `students/DEx/week7/sql/s6/notes.md`: which gold layer table will this view eventually mirror?

If you get stuck:
- Check that the view name is spelled the same in `CREATE` and in your test `SELECT`.
- Read the error for the line with `UNION ALL`. Both parts need the same number of columns and similar types.
- Ask in Discord with the `CREATE VIEW` error text and the part you think is wrong.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/sql/s6/02_current_view.sql`
- [ ] `students/DEx/week7/sql/s6/notes.md`

Done when:
- [ ] The view exists in your schema and a `SELECT` on it returns one row per CIN.
- [ ] The file matches what ran in Snowflake.
- [ ] The gold table answer is in `notes.md`.

Commit message: `S6.2 create v_company_current view`

#### Task 3 - Find above-average strike-off states (ID `S6.3`)

##### 1. What We Are Doing

Find states whose strike-off rate this month is above the overall sample average. Build it with a state rate CTE and an overall sample rate CTE. Use `UNION ALL` across all three of your practice RoC tables. This is a practice-sample comparison, not a national figure. Work in VS Code to write the file, then run it in Snowflake.

##### 2. Why We Are Doing It

The client wants to see how a company compares to its state. Finding states above the overall sample average is a practice version of that comparison. It uses only the three RoC files in your practice kit, so it does not establish a national average.

##### 3. What to Learn First

- Kudvenkat Part 59 "Subqueries" (15 min).
- The unnumbered video "Difference between where and having" (10 min).
- Watch for the idea of comparing a group value to a single overall value.

##### 4. Where to Find Resources

In the Kudvenkat playlist (https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB): Part 59 "Subqueries" (15 min) and the unnumbered video "Difference between where and having" (10 min).

Search keywords: `Snowflake CTE aggregate`, `SQL above average by group`, `Snowflake CROSS JOIN`.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week7/sql/s6/03_above_avg_states.sql`.
2. Starter to fill in:
   ```sql
   -- S6.3 States whose strike-off rate this month is above the overall sample average
   -- Role STUDENT_ROLE, warehouse STUDENT_WH, database YOUR_DB, your own schema
   WITH all_companies AS (
       -- TODO: select the same columns from all three practice RoC tables
       SELECT ... FROM RAW_MCA_<ROC_1>
       UNION ALL
       SELECT ... FROM RAW_MCA_<ROC_2>
       UNION ALL
       SELECT ... FROM RAW_MCA_<ROC_3>
   ),
   state_rate AS (
       -- TODO: per state, count strike-off rows and count all rows, then the rate
       SELECT state_name,
              COUNT(*) AS total_companies
       FROM all_companies
       GROUP BY state_name
   ),
   overall_rate AS (
       -- TODO: the same rate for the whole practice sample, one row
       SELECT ...
       FROM all_companies
   )
   SELECT ...
   FROM state_rate s
   CROSS JOIN overall_rate o
   -- TODO: keep only states above the overall sample rate
   ORDER BY ...;
   ```
3. Run it in Snowflake. Check the row count and read a few rows. This is a practice sample from three RoC files, so do not call the result national.
4. Look at the class channel and use the same column names the cohort agreed on, not a private name.

If you get stuck:
- Check that the state column name exists in your tables. Run `SELECT * FROM RAW_MCA_<ROC_1> LIMIT 10;` to see real column names. Repeat for the other two tables.
- Read the error for the aggregate or `GROUP BY` line.
- Ask in Discord with the query part and the error text. Say which column names you used.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/sql/s6/03_above_avg_states.sql`

Done when:
- [ ] The query runs and returns a list of states.
- [ ] Each row shows the state rate and the overall sample rate.
- [ ] The SQL in VS Code is the version that ran.

Commit message: `S6.3 above-average strike-off states with a CTE`

### Group 2 - Star Schema Design [MILESTONE]

#### Task 4 - Write the gold grain statements (ID `S7.1`)

##### 1. What We Are Doing

Write one grain sentence for each gold table: `dim_company`, `fct_cirp_event`, `dim_state`, and `dim_date`. A grain sentence says what one row means. Work in VS Code only.

##### 2. Why We Are Doing It

The whole platform in weeks 11 and 13 is built on these tables. The grain says what one row means, and it stops double counting later.

##### 3. What to Learn First

- On https://www.kimballgroup.com read the technique pages on "Grain" and on star schemas, about 30 min.
- Skip the pages on data vault and on unstructured data.

##### 4. Where to Find Resources

On https://www.kimballgroup.com search the technique pages for "Grain" and star schemas, about 30 min.

Search keywords: `Kimball grain statement`, `fact table grain`, `star schema grain`.

##### 5. Step-by-Step Guide

1. In VS Code, right-click `students/DEx/week7/` > New Folder > `design`.
2. Right-click `design` > New File > `grain_statements.md`.
3. Use this skeleton and fill in each line, one sentence per table:
   ```markdown
   # Gold grain statements

   ## dim_company
   One row in this table represents ...

   ## fct_cirp_event
   One row in this table represents ...

   ## dim_state
   One row in this table represents ...

   ## dim_date
   One row in this table represents ...
   ```
4. Check each sentence with the rule in "How to think about it". Keep each sentence short. No answers are given here, this is a design decision you own.

How to think about it:
State exactly what one row represents. Check that two rows cannot represent the same entity or event at the same point in time.

If you get stuck:
- Reread the Kimball page on "Grain".
- Ask: what event or entity does one row count?
- Ask in Discord with your draft sentence and say which word feels unclear.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/design/grain_statements.md`

Done when:
- [ ] All four tables have a sentence.
- [ ] Every sentence starts "One row in this table represents".
- [ ] Each sentence passes the check: two rows cannot represent the same entity or event at the same point in time.

Commit message: `S7.1 gold grain statements`

#### Task 5 - Mark dim_company columns as SCD 1 or 2 (ID `S7.2`)

##### 1. What We Are Doing

List every planned column in `dim_company` and mark it Type 1, Type 2, or Key / version metadata. Add a one-line reason for each column. Work in VS Code only.

##### 2. Why We Are Doing It

The client asked "what was this company's status on 1 March". That question needs at least one column that keeps history.

##### 3. What to Learn First

- On https://www.kimballgroup.com read the "Slowly Changing Dimensions" technique pages, Type 1 and Type 2, about 25 min.
- A key or version metadata column is not a changing business attribute. It identifies a row or marks a version, so it needs its own label in the table.
- Skip Type 3 and Type 6 for now.

##### 4. Where to Find Resources

On https://www.kimballgroup.com search the technique pages for "Slowly Changing Dimensions", Type 1 and Type 2, about 25 min.

Search keywords: `SCD Type 1 Type 2`, `Kimball SCD`, `dim_company Type 2`.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week7/design/dim_company_columns.md`.
2. List every column you plan for `dim_company`. Use this table:
   ```markdown
   # dim_company columns and SCD type

   | Column | Type 1, Type 2, or Key / version metadata | Reason (one line) |
   |---|---|---|
   | company_sk | TODO | TODO |
   | cin | TODO | TODO |
   | ... | ... | ... |
   ```
3. For each column decide: Type 1 overwrites the old value and loses history. Type 2 keeps an old version row and a new version row. A key or version metadata column (for example `company_sk` or `cin`) is marked `Key / version metadata`, not Type 1 or Type 2.
4. Add one closing line: which changing descriptive column or columns must be Type 2, and which client question needs it.

How to think about it:
Start from the client question: what was this company's status on 1 March. That answer needs history, so find the column or columns that change over time.

If you get stuck:
- Read the Type 1 and Type 2 definitions again on https://www.kimballgroup.com.
- Ask: does the client need the old value, or only the current value?
- Ask in Discord with your table and say which column is hard to classify.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/design/dim_company_columns.md`

Done when:
- [ ] Every planned column is listed.
- [ ] Every row has a label (Type 1, Type 2, or Key / version metadata) and a short reason.
- [ ] At least one changing descriptive column is marked Type 2 with a reason that names the client question.

Commit message: `S7.2 mark dim_company columns as SCD 1 or 2`

#### Task 6 - Draw the final star schema (ID `S7.3`)

##### 1. What We Are Doing

Draw the final star schema for the gold layer in a Mermaid code block. Then ask two teammates to review it and record their names and the date. Work in VS Code, then review in class chat or a short call.

##### 2. Why We Are Doing It

This is the milestone. Table names and column names get agreed cohort-wide here, so weeks 11 and 13 can start without rework.

##### 3. What to Learn First

- Mermaid basics at https://mermaid.js.org, about 15 min: how to write an `erDiagram`, define table columns, and connect tables.
- Nothing else new.

##### 4. Where to Find Resources

Mermaid basics at https://mermaid.js.org, about 15 min.

Search keywords: `Mermaid erDiagram`, `Mermaid markdown GitHub`, `star schema diagram Mermaid`.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week7/design/star_schema_final.md`.
2. Put your diagram in a Mermaid code block. Starter:
   ```mermaid
   erDiagram
       dim_company ||--o{ fct_cirp_event : "company_sk"
       dim_state   ||--o{ dim_company : "state_sk"
       dim_date    ||--o{ fct_cirp_event : "date_sk"
   ```
   Add the columns you agreed on to each table, and add the other dimensions you planned.
3. Ask two teammates to review the file. Give them the review checklist below.
4. Record both names in the file, plus the date.
5. Fix what the review finds, then commit. A reviewer comment that you acted on is worth more than a "looks good".

**Review checklist for your two teammates (each answer yes or no):**

1. Does every table in the diagram have a grain statement in `design/grain_statements.md`?
2. Do the column names in the diagram match the names in `dim_company_columns.md`?
3. Does every fact table reach each dimension through a key column, with no missing link?
4. Can the reviewer point to the Type 2 column and say which client question it answers?
5. Is each dimension stored at its own grain, with no event rows mixed in?

How to think about it:
Names agreed now save rework in weeks 11 and 13. The diagram is not only a picture. It is the shared contract for table names and column names.

If you get stuck:
- Open the file on GitHub to see if Mermaid renders. Check the `erDiagram` keyword and the relationship arrows.
- Compare column names with `dim_company_columns.md`.
- Ask in Discord with the Mermaid block and the error text.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/design/star_schema_final.md`

Done when:
- [ ] The diagram renders on GitHub.
- [ ] Two teammate names and the review date are in the file.
- [ ] Every "no" from the review is fixed or explained.

Commit message: `S7.3 final star schema diagram, reviewed by two teammates`

**On Task 5:** Type 1 overwrites and loses history. Type 2 keeps a version row. Keys and version metadata columns are not Type 1 or Type 2 business attributes. The client asked "what was this company's status on 1 March", so at least one changing descriptive column must be Type 2, and you must be able to say which and why. Week 11 and week 13 build exactly what you write here.

### Group 3 - Retry Logic and Context Managers

#### Task 7 - Write a retry decorator (ID `P4.1`)

##### 1. What We Are Doing

Write a retry decorator with a waiting gap that doubles. Use it on a small test function that fails twice and succeeds on the third try. Work in VS Code and run it in the VS Code terminal from the repo root.

##### 2. Why We Are Doing It

Downloads fail. A short retry with a growing wait keeps the pipeline running instead of making a person rerun it by hand.

##### 3. What to Learn First

- Corey video #37 "Decorators - Dynamically Alter The Functionality Of Your Functions" and #38 "Decorators With Arguments", about 55 min.
- Watch for the idea of a function that wraps another function.
- Skip his Flask and Django videos.

##### 4. Where to Find Resources

In the Corey Schafer playlist (https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU): #37 "Decorators - Dynamically Alter The Functionality Of Your Functions" and #38 "Decorators With Arguments", about 55 min.

Search keywords: `Python retry decorator`, `functools wraps`, `Python decorator with arguments`.

##### 5. Step-by-Step Guide

1. In VS Code, right-click `students/DEx/week7/` > New Folder > `python`, then inside it a folder `p4`.
2. Create `students/DEx/week7/python/p4/retry_decorator.py`. Starter:
   ```python
   """P4.1 A retry decorator with a waiting gap that doubles."""
   import time
   from functools import wraps


   def retry(max_tries=3, base_wait=1):
       def decorator(func):
           @wraps(func)
           def wrapper(*args, **kwargs):
               # TODO: loop up to max_tries times
               # TODO: call func and return the result when it works
               # TODO: when it fails, wait, then double the wait, then try again
               # TODO: after the last try, let the error out
               ...
           return wrapper
       return decorator


   @retry(max_tries=3, base_wait=1)
   def flaky():
       # TODO: fail on the first two calls, succeed on the third
       ...


   if __name__ == "__main__":
       print(flaky())
   ```
3. Run it from the repo root:
   ```
   python students/DEx/week7/python/p4/retry_decorator.py
   ```
4. Copy the terminal output into `students/DEx/week7/python/p4/notes.md`, so the proof of three tries is saved.

If you get stuck:
- Read the error line. Check that `retry` returns `decorator`, and `decorator` returns `wrapper`.
- Print the wait value inside the loop to see if it doubles.
- Ask in Discord with your code and the error text.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/python/p4/retry_decorator.py`
- [ ] `students/DEx/week7/python/p4/notes.md`

Done when:
- [ ] The script runs and shows three attempts before success.
- [ ] The wait grows each time (print the wait to see it).
- [ ] The output is saved in `notes.md`.

Commit message: `P4.1 retry decorator with doubling wait`

#### Task 8 - Download the RBI file (ID `P4.2`)

##### 1. What We Are Doing

Open the RBI data site, then save the RBI policy rate file by hand if needed. Write a Python script that reads the file and prints its date range, the source link, and the pull date. Work in the browser first, then in VS Code.

##### 2. Why We Are Doing It

The platform answers "what was the repo rate when this company was incorporated". A policy rate is a step series, so the table needs the rate rows and the dates they started.

##### 3. What to Learn First

- Corey video #100 "Python Requests Tutorial: Request Web Pages, Download Images, POST Data, Read JSON, and More" (40 min).
- Also read the note below. It matters more than the video.

##### 4. Where to Find Resources

In the Corey Schafer playlist (https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU): #100 "Python Requests Tutorial: Request Web Pages, Download Images, POST Data, Read JSON, and More" (40 min).

Use the RBI data portal at https://data.rbi.org.in and the RBI press releases at https://www.rbi.org.in.

Search keywords: `RBI DBIE repo rate`, `Python requests timeout`, `RBI policy repo rate download`.

##### 5. Step-by-Step Guide

1. Open https://data.rbi.org.in in the browser. The RBI data portal is DBIE (Database on Indian Economy). It is a JavaScript site, so a script may not be able to download a file from it directly.
2. Look for the policy repo rate series. If there is no direct file link, download the file by hand, once. Save it untouched into `data/raw/rbi/` and put the pull date in the file name, for example `repo_rate_2026-10-14.csv`.
3. Create `students/DEx/week7/python/p4/rbi_download.py`. Starter:
   ```python
   """P4.2 Read the RBI policy rate file and record its date range."""
   from pathlib import Path

   # from retry_decorator import retry  # or copy your decorator here

   RAW_DIR = Path("data/raw/rbi")


   def fetch_page(url: str) -> str:
       # TODO: requests.get(url, timeout=10), stop on a bad status, return the text
       # TODO: decorate this function with your retry decorator
       ...


   def main() -> None:
       # TODO: call fetch_page with a timeout, for example fetch_page("https://www.rbi.org.in")
       # TODO: find the newest file in RAW_DIR
       # TODO: print the first date and the last date found in the file
       # TODO: print the source link and today's date
       ...


   if __name__ == "__main__":
       main()
   ```
4. In `main()`, call the decorated `fetch_page` with a timeout, for example `fetch_page("https://www.rbi.org.in")`. The request must run. It must report failure after the retries are exhausted. The RBI file download itself can be the hand-saved local file.
5. Run it from the repo root:
   ```
   python students/DEx/week7/python/p4/rbi_download.py
   ```
6. Write in `students/DEx/week7/python/p4/notes.md`: the file name you saved, the source link, the pull date, and the date range inside the file.

**Note on the rate series:**

1. A policy rate row exists only on the day the rate changes.
2. Our cohort data ends in 2024. That is fine: this project uses the RBI data as available, ending 2024.
3. A file with no rows after 2024 does not mean data is missing. Use "carry forward": the latest rate applies until a newer row appears.
4. Keep a `valid_from` date, and a `valid_to` date or an open end for the last row.
5. Record the last date found in the file in your notes.
6. Do not copy rate numbers from news summaries and do not guess. Check https://www.rbi.org.in and the DBIE series.

**Optional stretch, not required:** You can add later rate changes by hand from the RBI press releases at https://www.rbi.org.in, into a small reference table or a dbt seed, with the source link and the date.

If you get stuck:
- Check that the file exists in `data/raw/rbi/` and that the name matches the one in your script.
- Print `RAW_DIR` and the file list to see what the script sees.
- Ask in Discord with the file name, the date range, and the error text.

##### 6. What to Commit

Files to commit:
- [ ] `students/DEx/week7/python/p4/rbi_download.py`
- [ ] `students/DEx/week7/python/p4/notes.md`

Do not commit the data file. The data file stays in `data/raw/rbi/` and is never committed.

Done when:
- [ ] The script prints the date range of the file and the source link.
- [ ] `main()` calls the decorated `fetch_page` with a timeout, and the request runs.
- [ ] When the request keeps failing, the script reports failure after the retries are exhausted.
- [ ] It runs without crashing twice in a row.
- [ ] The note lists file name, source, pull date and date range.

Commit message: `P4.2 RBI policy rate download with retry`

#### Task 9 - Close files safely (ID `P4.3`)

##### 1. What We Are Doing

Rewrite file handling in your earlier Python scripts to use `with open(...) as f:`. This closes files even when an error happens. Work in VS Code and run each updated script from the repo root.

##### 2. Why We Are Doing It

A file left open can lock or corrupt data, and a crash in the middle of a write leaves a half file. Context managers close files even when something fails.

##### 3. What to Learn First

- Corey video #49 "Context Managers - Efficiently Managing Resources" (25 min).
- Watch for the `with` statement and the idea of cleanup that always runs.

##### 4. Where to Find Resources

In the Corey Schafer playlist (https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU): #49 "Context Managers - Efficiently Managing Resources" (25 min).

Search keywords: `Python with open`, `context manager file`, `Python file close exception`.

##### 5. Step-by-Step Guide

1. Open both sets of earlier Python scripts: your week 3 P1 scripts in `students/DEx/week3/python/p1/` and your week 4 P2 scripts in `students/DEx/week4/python/p2/`. Open the CSV readers you wrote too.
2. Rewrite the file handling with `with open(...) as f:` instead of `f = open(...)`. Starter pattern:
   ```python
   # before: f = open(path); text = f.read(); f.close()
   with open(path, "r", encoding="utf-8") as f:
       text = f.read()
   # TODO: repeat this for every open() in the old scripts
   ```
3. Run each updated script from the repo root, for example:
   ```
   python students/DEx/week3/python/p1/<your_script>.py
   python students/DEx/week4/python/p2/<your_script>.py
   ```
4. Write one sentence in `students/DEx/week7/python/p4/notes.md` saying what problem this prevents.

If you get stuck:
- Search the old script for `open(` and check every match.
- Run one script at a time and read the first error line.
- Ask in Discord with the script path and the error text.

##### 6. What to Commit

Files to commit:
- [ ] The updated earlier scripts under `students/DEx/week3/python/p1/` and `students/DEx/week4/python/p2/`.
- [ ] `students/DEx/week7/python/p4/notes.md`

Done when:
- [ ] No plain `open()` without `with` is left in those scripts.
- [ ] Each script still runs.
- [ ] The one-sentence note is saved.

Commit message: `P4.3 use context managers for file handling`

**Note:** every network call in this project needs a timeout. A download that hangs forever with no timeout is the failure Task 7 exists to prevent, and `data/raw/` is gitignored, so the downloaded file must never appear in your commit.

## 6. What to Commit

End of week checklist:
- [ ] Tasks 1-3 - CTE rewrite, current view, above-average states
- [ ] Tasks 4-6 - grain statements, dim_company columns with SCD types, final diagram with two named reviewers
- [ ] Tasks 7-9 - retry decorator with proof it retried, RBI download, context managers
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] Your table and column names match what the cohort agreed, not what you wrote first

If you are short on time, cut in this order: Task 3 (S6.3), then Task 9 (P4.3). Never cut Tasks 4-6 (S7). It is the milestone, and weeks 11 and 13 cannot start without it.

When the checklist is ready, open one pull request for your week. The steps are in `docs/03-student-guide.md`.

Next: `week8/problem_statement.md`.
