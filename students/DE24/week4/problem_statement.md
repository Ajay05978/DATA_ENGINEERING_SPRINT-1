# Week 4 - Queries, joins, and checking data with Python

**Data Sprint 1 | Week 4 of 15 | Theme: answer questions with SQL joins, then check records with Python**

Read this whole file before you start. Then work through the task groups in order.

## 1. What We Are Doing

This week you answer business questions with SQL: counts, groups, joins, and a join between two monthly snapshots on CIN. You join the IBBI sample to the MCA registry and explain the rows that do not match. Then you check records with Python: type hints, a hand-written validator, and a pydantic validator.
Groups: Group 1 Queries, Grouping and Joins (`S3.1`, `S3.2`, `S3.3`, `S3.4`, `S3.5`). Group 2 Type Hints and Data Validation (`P2.1`, `P2.2`, `P2.3`).

## 2. Why We Are Doing It

The client is a fintech due-diligence company. Its analysts check Indian companies: is the company real and active (MCA), has it ever been in insolvency (IBBI), and how does it compare to its state (MCA CDM). This week you learn to join two sources on CIN, and you learn to validate a record before the pipeline trusts it. These skills protect the client from false matches and wrong counts.

**The thing to watch this week:** Everyone's IBBI to MCA join leaves unmatched rows. The count is easy. The explanation is the work. You will be asked about it at the cohort review. There is no milestone this week.

## 3. What to Learn First (Prerequisites)

**Before this week: what you must already have**

| You need this | It came from | If it is missing |
|---|---|---|
| Your cloned repo with your own folder `students/DEx/` and your week folders | Week 1 setup | See `docs/03-student-guide.md`, then clone again |
| Three RoC ZIP files downloaded and unzipped into `data/raw/mca/`, with the download date noted | Week 2, the step "Download your practice data" | Open `students/DEx/week2/problem_statement.md` and do that step now |
| Practice tables in Snowflake: `RAW_MCA_<ROC>` (three tables), `RAW_IBBI_SAMPLE`, and `RAW_MCA_PREV`, in your own schema | Week 3 practice data kit | Open `students/DEx/week3/problem_statement.md` and do the practice kit step now |
| Python warm-up scripts from week 2 (`P0.1` to `P0.4`) and the three file-reading scripts from week 3 (`P1.1`, `P1.2`, `P1.3`) in `students/DEx/week2/python/` and `students/DEx/week3/python/p1/` | Weeks 2 and 3 | Redo those tasks |
| A working `.venv` with `pydantic` installed | Setup, `docs/02-tools-setup.md` | Redo the setup steps, then run `pip install pydantic` |
| Snowflake login: role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your database, your own schema | Given by the program lead | Ask the program lead. Do not guess names |

**By the end of this week you can**

- Answer a business question with GROUP BY, not with a spreadsheet
- Join two sources on CIN and explain the rows that do not match
- Compare two monthly tables with a join between snapshots
- Add type hints to a Python function
- Validate a record instead of trusting it, first by hand and then with pydantic

**Core concepts to review first**

- SELECT with WHERE, GROUP BY, HAVING, ORDER BY, LIMIT.
- LEFT JOIN and how to find unmatched rows with `IS NULL`.
- Joining two snapshot tables on the same key (CIN) to find changes.
- Python try/except and datetime parsing.
- Type hints for function inputs and return values.
- pydantic models for validation.

You use the practice tables from week 3. The proper repeatable load (stage, file format, COPY INTO, row-count check) comes in week 6.

## 4. Where to Find Resources

**Before you start**

| If you need... | Read this |
|---|---|
| Setup steps and Git commands | `docs/03-student-guide.md` |
| Client story and the four sources | `docs/01-project-brief.md` |
| A word you do not know | `docs/08-glossary.md` |
| A broken tool | `docs/10-troubleshooting.md` |
| What a tool is for | `docs/11-tools-and-technology.md` |
| SQL Server vs Snowflake differences | `docs/09-resources.md` |

**How your tasks are numbered:** the letter is the area (`S` = SQL, `P` = Python, `B` = business, `D` = data platform, `A` = diagrams). The first number is the program group number. The number after the dot is the task. So `S3.4` means SQL group 3, task 4. Put the ID in your commit message and in your pull request title.

**Where you work (reminder)**

> - **VS Code** (your cloned repo): you write files here, save them here, and commit here.
> - **VS Code terminal**: you run Git and Python commands here. For Python, activate `.venv` first.
> - **Snowflake (Snowsight, in the browser)**: you run SQL here. Use Projects > Workspaces. The data lives in Snowflake, not on your laptop.
> - The loop is: write the `.sql` file in VS Code, paste it into a Snowsight Workspace to run it, then save the final working version back in VS Code.
> - In every path below, `DEx` means your own number. If you are DE12, then `students/DEx/week4/` means `students/DE12/week4/`.

**Your week in order:**

| Step | Learn first (with time) | Then do |
|---|---|---|
| **1** | Kudvenkat Part 10 "Select statement" (about 12 min), Part 11 "Group by" (about 12 min), and the unnumbered video "Difference between where and having" (about 10 min) | Tasks 1-2 - status and state counts |
| **2** | Kudvenkat Part 12 "Joins" and Part 13 "Advanced or intelligent joins" (about 30 min), Part 14 "Self join" (about 15 min) | Tasks 3-5 - the IBBI join, the unmatched list, the status-change snapshot join |
| **3** | Corey video #155 "Type Hints - From Basic Annotations to Advanced Generics", first part only. Optional: Corey video #154. Python docs `typing` page, introduction sections only (about 25 min) | Task 6 - type hints |
| **4** | Corey video #31 "Using Try/Except Blocks for Error Handling" (about 15 min), Corey video #24 "Datetime Module - How to work with Dates, Times, Timedeltas, and Timezones" (about 25 min) | Task 7 - validator by hand |
| **5** | Corey video #157 "Python Pydantic Tutorial: Complete Data Validation Course", first parts only. Take two ideas: BaseModel and validation errors. pydantic docs, "Models" page (about 30 min) | Task 8 - pydantic validator, then cohort review |

**Search keywords for this week:** Snowflake GROUP BY status, SQL LEFT JOIN unmatched CIN, Python type hints function, pydantic BaseModel validation error

## 5. Step-by-Step Guide

Work one task at a time. One task, one file, one commit. Git basics (add, commit, push, pull request) are in `docs/03-student-guide.md`. We do not repeat them every week. The task IDs keep their program names (`S3.x`, `P2.x`). The group numbers below are just the order in this week.

### Group 1 - Queries, Grouping and Joins

Foundation link: Kudvenkat Part 10 to Part 14 (SELECT, GROUP BY, joins, self join).

#### Task 1 - Count companies by status (ID `S3.1`)

##### 1. What We Are Doing

You write a GROUP BY query that counts companies by status. You work in VS Code to write the file, and in Snowflake (Snowsight) to run it. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema.

##### 2. Why We Are Doing It

"How many companies are still active?" is the first number the client asks for. A GROUP BY gives that number, and it can be repeated by anyone.

##### 3. What to Learn First

- Kudvenkat Part 10 "Select statement" (about 12 min).
- Kudvenkat Part 11 "Group by" (about 12 min).
- The unnumbered video "Difference between where and having" (about 10 min).
- About 35 min in total.

##### 4. Where to Find Resources

- Kudvenkat Part 10 "Select statement" (about 12 min).
- Kudvenkat Part 11 "Group by" (about 12 min).
- The unnumbered video "Difference between where and having" (about 10 min).
- Search keywords: Snowflake GROUP BY status, SQL ORDER BY count DESC, Snowflake COUNT rows per group.

##### 5. Step-by-Step Guide

1. In VS Code, open your week 4 folder `students/DEx/week4/`. Right-click it > New Folder, name it `sql`. Then right-click `sql` > New Folder, name it `s3`. You now have `students/DEx/week4/sql/s3/`.
2. Right-click the `s3` folder > New File, name it `01_status_counts.sql`.
3. Start here:
   ```sql
   SELECT
       STATUS,
       -- TODO: how many rows per status?
   FROM RAW_MCA_<roc>
   -- TODO: group the rows by the status column
   -- TODO: order from the most common status to the least common
   ;
   ```
4. Run it in a Snowsight Workspace.
5. Look at the status texts: some may have extra spaces or odd capitalisation. Say this in your notes.
6. You loaded three RoC tables in week 3. For this task, use one table only, and say in notes which one you used. Stacking the three tables with `UNION ALL` comes in week 5.
7. Write the answer table in `students/DEx/week4/sql/s3/notes.md` (create the file).

If you get stuck: read the error message, search "Snowflake GROUP BY status", and check that every column in SELECT is either grouped or aggregated. Ask in Discord with the query and the error text.

##### 6. What to Commit

- [ ] `students/DEx/week4/sql/s3/01_status_counts.sql`
- [ ] `students/DEx/week4/sql/s3/notes.md`

Done when:
- [ ] The query runs.
- [ ] The answer table is in notes.
- [ ] Notes say which table you used and how you handled extra spaces in status.

Commit message: `S3.1 count companies by status`

#### Task 2 - Count active companies by state (ID `S3.2`)

##### 1. What We Are Doing

You write a query that counts active companies by state and returns the top five. You work in VS Code to write, and in Snowflake (Snowsight) to run. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema.

##### 2. Why We Are Doing It

The client compares a company with other companies in its state. This task builds that state-level count for the first time.

##### 3. What to Learn First

Nothing new. You reuse Part 10, Part 11 and "Difference between where and having" from Task 1.

##### 4. Where to Find Resources

- Reuse Kudvenkat Part 10 "Select statement", Part 11 "Group by", and "Difference between where and having" from Task 1.
- Search keywords: Snowflake WHERE active status GROUP BY state, SQL LIMIT top rows, Snowflake TRIM state names.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week4/sql/s3/02_active_by_state.sql`.
2. Take the exact "active" status text from the Task 1 output. Do not type it from memory.
3. Start here:
   ```sql
   SELECT
       STATE,
       -- TODO: count the companies
   FROM RAW_MCA_<roc>
   -- TODO: keep only the active rows
   -- TODO: group by state
   -- TODO: order by the count, biggest first
   LIMIT 5;
   ```
4. Run it and save the working version.
5. Write the states and counts (up to five) in `students/DEx/week4/sql/s3/notes.md`. Also note any state names written in more than one way (for example with extra spaces or a different spelling).

If you get stuck: check the exact status text from Task 1, search "Snowflake WHERE active status GROUP BY state", and read the error line. Ask in Discord with the query and the error text.

##### 6. What to Commit

- [ ] `students/DEx/week4/sql/s3/02_active_by_state.sql`
- [ ] `students/DEx/week4/sql/s3/notes.md`

Done when:
- [ ] The query returns up to five states.
- [ ] The states and counts (up to five) are in notes.
- [ ] Notes describe observed state-name problems, or state that none were found.

Commit message: `S3.2 active companies by state`

#### Task 3 - Join IBBI to the registry (ID `S3.3`)

##### 1. What We Are Doing

You write a LEFT JOIN that counts matched and unmatched rows between `RAW_IBBI_SAMPLE` and one RoC table. You work in VS Code to write, and in Snowflake (Snowsight) to run. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema. This task uses `RAW_IBBI_SAMPLE` from week 3.

##### 2. Why We Are Doing It

IBBI tells the client a company went into insolvency. The registry tells the client the company exists. Joining them on CIN is how the platform proves that the company in the IBBI list is a real company.

##### 3. What to Learn First

- Kudvenkat Part 12 "Joins" and Part 13 "Advanced or intelligent joins" (about 30 min).
- Kudvenkat teaches SQL Server. Watch for the idea, then write Snowflake SQL. Use `LIMIT` instead of `TOP`, and `IFNULL` or `COALESCE` instead of `ISNULL`. You may ask AI to translate a query or explain an error, but you must understand the result.

##### 4. Where to Find Resources

- Kudvenkat Part 12 "Joins" and Part 13 "Advanced or intelligent joins" (about 30 min).
- Search keywords: Snowflake LEFT JOIN count matched unmatched, SQL join on CIN, Snowflake COUNT(*) vs COUNT(column).

##### 5. Step-by-Step Guide

1. Create `students/DEx/week4/sql/s3/03_cirp_join_counts.sql`.
2. Your IBBI sample has 20 to 30 rows. Your RoC tables are big. First run a separate detail query with `LIMIT 10` to see that the join works:
   ```sql
   SELECT
       i.CIN,
       m.CIN AS registry_cin
   FROM RAW_IBBI_SAMPLE i
   LEFT JOIN RAW_MCA_<roc> m
       ON i.CIN = m.CIN
   LIMIT 10;
   ```
3. Then run the count query below with no limit. A `LIMIT` on a count query limits the result rows, not the rows that are inspected or joined.
4. Start here and fill in the TODOs:
   ```sql
   SELECT
       COUNT(*) AS ibbi_rows,
       -- TODO: count the rows where the registry side matched
       -- TODO: count the rows where the registry side did not match
   FROM RAW_IBBI_SAMPLE i
   LEFT JOIN RAW_MCA_<roc> m
       ON i.CIN = m.CIN;
   ```
5. Join one RoC table at a time. Say in notes which table you joined.
6. Write both counts in `students/DEx/week4/sql/s3/notes.md`.

If you get stuck: read the error message, search "Snowflake LEFT JOIN count matched unmatched", and check that both tables have a CIN column. Ask in Discord with the query and the error text.

##### 6. What to Commit

- [ ] `students/DEx/week4/sql/s3/03_cirp_join_counts.sql`
- [ ] `students/DEx/week4/sql/s3/notes.md`

Done when:
- [ ] The query runs.
- [ ] Matched plus unmatched equals your IBBI sample row count.
- [ ] Both counts are in notes.

Commit message: `S3.3 join IBBI to registry on CIN`

#### Task 4 - Explain the unmatched CINs (ID `S3.4`)

##### 1. What We Are Doing

You list unmatched IBBI CINs and write a two-sentence explanation of why they do not match. You work in VS Code to write, in Snowflake (Snowsight) to run, then in VS Code to write the explanation. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema.

##### 2. Why We Are Doing It

Unmatched rows are the part of the work clients ask about. A number is not an answer; the reason is the answer.

##### 3. What to Learn First

Nothing new. Reuse Part 12. Optional refresher: Kudvenkat Part 13 "Advanced or intelligent joins" (about 15 min), and review your pattern `LEFT JOIN ... WHERE ... IS NULL` (the rows where the right side is NULL are the unmatched rows).

##### 4. Where to Find Resources

- Reuse Kudvenkat Part 12 "Joins".
- Optional refresher: Kudvenkat Part 13 "Advanced or intelligent joins" (about 15 min).
- Search keywords: Snowflake LEFT JOIN IS NULL, SQL unmatched rows, Snowflake compare CIN strings.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week4/sql/s3/04_unmatched_cins.sql`.
2. Start here:
   ```sql
   SELECT
       i.CIN,
       i.COMPANY_NAME
       -- TODO: any other IBBI columns you want to show
   FROM RAW_IBBI_SAMPLE i
   LEFT JOIN RAW_MCA_<roc> m
       ON i.CIN = m.CIN
   -- TODO: keep only the rows where the registry side is missing
   LIMIT 10;
   ```
3. Run it. Compare one unmatched CIN character by character with the registry CINs.
4. Run the same query once for each of your three RoC tables, so you see the unmatched list against each table.
5. Before you write the explanation: check each unmatched CIN against all three loaded RoC tables. For example, run `SELECT CIN FROM RAW_MCA_<roc> WHERE CIN = '<the unmatched CIN>'` once per table. A CIN missing from one table may be present in another.
6. Check the coverage of the tables you queried and the CIN formats. Use evidence from your checks in your explanation.
7. In `students/DEx/week4/sql/s3/notes.md`, add the heading `S3.4 - Why some CINs do not match` and write **two sentences**.

If you get stuck: check the `IS NULL` condition, search "Snowflake LEFT JOIN IS NULL", and compare one CIN by eye for spaces or length. Ask in Discord with one example CIN and the error text.

How to think about it: Check the coverage of the tables you queried and the CIN formats. Use evidence from your checks in your explanation.

##### 6. What to Commit

- [ ] `students/DEx/week4/sql/s3/04_unmatched_cins.sql`
- [ ] `students/DEx/week4/sql/s3/notes.md`

Done when:
- [ ] The query returns up to ten CINs with company names.
- [ ] Each unmatched CIN is checked against all three loaded RoC tables.
- [ ] The two sentences are in your own words.
- [ ] You can say the explanation out loud.

Commit message: `S3.4 list and explain unmatched CINs`

#### Task 5 - Compare two monthly snapshots (ID `S3.5`)

##### 1. What We Are Doing

You write a join between two snapshots: a current RoC table and `RAW_MCA_PREV`, joined on CIN, to find rows whose status changed. You work in VS Code to write, and in Snowflake (Snowsight) to run. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema. This task uses `RAW_MCA_PREV` from week 3.

##### 2. Why We Are Doing It

A company that was Active last month and Strike Off this month is a big signal in due diligence. The client wants to see that change, not just the current status.

##### 3. What to Learn First

- Kudvenkat Part 14 "Self join" (about 15 min). Same alias idea: two tables, two aliases, joined on CIN.

##### 4. Where to Find Resources

- Kudvenkat Part 14 "Self join" (about 15 min).
- Search keywords: SQL compare two tables status change, Snowflake join two tables on CIN, Snowflake compare snapshots.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week4/sql/s3/05_status_change_selfjoin.sql`.
2. Join the current table to `RAW_MCA_PREV` on CIN, so one row shows both months. Start here:
   ```sql
   SELECT
       m.CIN,
       -- TODO: the company name
       p.STATUS AS status_last_month,
       m.STATUS AS status_this_month
   FROM RAW_MCA_<roc> m
   -- TODO: join to RAW_MCA_PREV on CIN
   WHERE
       -- TODO: status last month is the active text
       -- TODO: AND status this month is the strike-off text
       ;
   ```
3. Take the exact status texts from the Task 1 output. Run the query.
4. Add a second query in the same file that counts those rows.
5. Check your answer against `RAW_MCA_PREV` rows you changed in week 3. Did the query find them?

If you get stuck: check the table aliases `m` and `p`, search "SQL compare two tables status change", and compare the exact status text with Task 1. Ask in Discord with the query and the error text.

**Note:** Task 5 (S3.5) is the first time you compute a change between two months. Week 11 turns exactly this into the SCD2 logic, so keep the query.

##### 6. What to Commit

- [ ] `students/DEx/week4/sql/s3/05_status_change_selfjoin.sql`
- [ ] `students/DEx/week4/sql/s3/notes.md`

Done when:
- [ ] The query returns rows with two different status values.
- [ ] The count is written in `students/DEx/week4/sql/s3/notes.md`.
- [ ] You can say whether your changed rows appeared.

Commit message: `S3.5 compare two snapshots with a join on CIN`

### Group 2 - Type Hints and Data Validation

### Warm-up - What is a type hint? (no task ID, not graded)

A type hint is a short label on a function. It says what type goes in and what type comes out. It does not change how Python runs the function. It helps the reader and the editor.

This is new for most people. Go slowly. Look at this 3-line example:

```python
def add_vat(amount: float) -> float:
    vat_amount = amount * 0.2
    return amount + vat_amount
```

`amount: float` means the input should be a float. `-> float` means the function returns a float.

#### Task 6 - Add type hints (ID `P2.1`)

##### 1. What We Are Doing

You add type hints to the three Python scripts from week 3. You work in VS Code to edit the files, and in the VS Code terminal to run them. Activate `.venv` first: `source .venv/bin/activate` on macOS/Linux, `.venv\Scripts\activate` on Windows.

##### 2. Why We Are Doing It

Later scripts are called by other people and by the pipeline. A type hint tells the reader what goes in and what comes out, without reading the whole function.

##### 3. What to Learn First

- Type hint syntax: input types, and return type with `->`.
- The `Path` type from `pathlib`.
- Corey video #155 "Type Hints - From Basic Annotations to Advanced Generics", first part only.
- Optional: Corey video #154 "Type Hinting vs Type Checking vs Data Validation".
- Python docs `typing` page, introduction sections only (about 25 min).

##### 4. Where to Find Resources

- Corey video #155 "Type Hints - From Basic Annotations to Advanced Generics", first part only.
- Optional: Corey video #154 "Type Hinting vs Type Checking vs Data Validation".
- Python docs `typing` page: https://docs.python.org/3/library/typing.html , introduction sections only (about 25 min).
- Search keywords: Python type hints function return type, Python typing Path int, Python def main None.

##### 5. Step-by-Step Guide

1. Open the three scripts in `students/DEx/week3/python/p1/`.
2. Add a type hint to every function: input types and return type. Copy the shape below:
   ```python
   from pathlib import Path

   def count_rows(csv_path: Path) -> int:
       """Return the number of data rows in the CSV."""
       # TODO: reuse your week 3 code here, do not change the logic
       ...

   def main() -> None:
       ...

   if __name__ == "__main__":
       main()
   ```
3. Run each script again from the repo root, for example:
   `python students/DEx/week3/python/p1/<script_name>.py`
4. Check that the output is the same as before your edit.

If you get stuck: read the Python error, search "Python type hints function return type", and check that every function has `->` before the colon. Ask in Discord with the traceback text.

##### 6. What to Commit

- [ ] `students/DEx/week3/python/p1/` (the three scripts from P1.1, P1.2, P1.3)

Done when:
- [ ] Every function has input types and a return type.
- [ ] All three scripts run without errors.
- [ ] The output is unchanged.

Commit message: `P2.1 add type hints to P1 scripts (see P1 pull request)`

**The shared record contract (read this before Task 7 and Task 8)**

Both validators, the hand-written one and the pydantic one, must use the same record shape and the same rules. A record is a Python dict with exactly these five keys:

| Key | Type | Rule |
|---|---|---|
| `cin` | text | exactly 21 characters, no spaces, not empty |
| `company_name` | text | not empty |
| `bench` | text | not empty |
| `start_date` | text | exactly the format `YYYY-MM-DD`, for example `2024-01-31`, and it must be a real date |
| `admitted_claims` | number | a number, zero or more, never negative, not text |

- A missing key or an empty value is an error.
- These rules are the same in Task 7 (by hand) and Task 8 (pydantic). The pydantic `date` type accepts the `YYYY-MM-DD` format.
- Use fake data only. Do not copy real company data.

#### Task 7 - Write a validator by hand (ID `P2.2`)

##### 1. What We Are Doing

You write a Python function that checks one insolvency record and returns a list of error messages. You work in VS Code to write, and in the VS Code terminal to run. `.venv` activated.

##### 2. Why We Are Doing It

IBBI rows are typed from PDFs by people, so mistakes happen. A validator returns clear errors instead of crashing, so a load can report the bad row and carry on. The client then knows which rows to fix.

##### 3. What to Learn First

- Corey video #31 "Using Try/Except Blocks for Error Handling" (about 15 min).
- Corey video #24 "Datetime Module - How to work with Dates, Times, Timedeltas, and Timezones" (about 25 min).
- About 40 min in total.

##### 4. Where to Find Resources

- Corey video #31 "Using Try/Except Blocks for Error Handling" (about 15 min).
- Corey video #24 "Datetime Module - How to work with Dates, Times, Timedeltas, and Timezones" (about 25 min).
- Search keywords: Python try except datetime parse, Python validate record errors list, Python fake CIN 21 characters.

##### 5. Step-by-Step Guide

1. In VS Code, create the folder for this task. Right-click `students/DEx/week4` > New Folder, name it `python`. Then right-click `python` > New Folder, name it `p2`. You now have `students/DEx/week4/python/p2/`.
2. Create the file `students/DEx/week4/python/p2/record_validator.py`.
3. Read the shared record contract above. Your records must use its exact keys and its rules.
4. Write five valid records that follow the contract.
5. Write five invalid records, with a different error in each one. Use these five: a CIN that is too short, a start date that does not parse, an admitted claims value that is not a number, a missing field, and one more of your choice.
6. Invent fake CINs of 21 characters, in the same format you saw in the data. Do not copy real company data.
7. Write one function that takes one record and returns a list of error messages. An empty list means the record is valid. Use try/except so the function never crashes.
8. Print the result for every record in your test set.
9. Run it from the repo root: `python students/DEx/week4/python/p2/record_validator.py`
10. Copy the output into `students/DEx/week4/python/p2/notes.md`.
11. Starter shape:
   ```python
   from datetime import datetime

   def validate_record(record: dict) -> list[str]:
       """Return a list of error messages. An empty list means the record is valid."""
       errors: list[str] = []
       # TODO: check the CIN is present and 21 characters long
       # TODO: try to parse the start date, add an error if it fails
       # TODO: check that admitted claims is a number
       return errors

   if __name__ == "__main__":
       records = [
           # TODO: five valid records, then five invalid ones, each with a different mistake
       ]
       for r in records:
           print(validate_record(r))
   ```
If you get stuck: read the traceback, search "Python try except datetime parse", and check that the function returns `errors` at the end. Ask in Discord with the traceback and your test record.

##### 6. What to Commit

- [ ] `students/DEx/week4/python/p2/record_validator.py`
- [ ] `students/DEx/week4/python/p2/notes.md`

Done when:
- [ ] The script runs with no traceback.
- [ ] All five invalid records get at least one clear message that names the field.
- [ ] All five valid records return an empty list.
- [ ] The output is saved in notes.

Commit message: `P2.2 hand-written insolvency record validator`

#### Task 8 - Rewrite the validator with pydantic (ID `P2.3`)

##### 1. What We Are Doing

You rewrite the validator as a pydantic model and compare it with your hand-written version. You work in VS Code to write, and in the VS Code terminal to run. `.venv` activated.

##### 2. Why We Are Doing It

pydantic is the library the later pipeline uses for data contracts. You compare two ways of validating the same row, so you can explain the choice to the client.

##### 3. What to Learn First

- Corey video #157 "Python Pydantic Tutorial: Complete Data Validation Course", first parts only. Take two ideas: `BaseModel` and validation errors.
- pydantic docs, "Models" page at https://docs.pydantic.dev (about 30 min).
- Read the field types and the validation error section.

##### 4. Where to Find Resources

- Corey video #157 "Python Pydantic Tutorial: Complete Data Validation Course", first parts only.
- pydantic docs, "Models" page: https://docs.pydantic.dev (about 30 min).
- Search keywords: pydantic BaseModel validation error, pydantic field types int date, pydantic model from dict.

##### 5. Step-by-Step Guide

1. Check pydantic is installed. It is already in your setup; run `pip install pydantic` again only if the import fails.
2. In VS Code create `students/DEx/week4/python/p2/pydantic_validator.py`.
3. Define one model for an insolvency record: CIN, company name, bench, start date, admitted claims. Use the shared record contract above: the same five keys, the same `YYYY-MM-DD` date format, the same CIN length, and the same admitted-claims rules. Use the field types and constraints the Models page shows.
4. Reuse your test set from Task 7. Build the model from each record inside a try/except and print the errors pydantic gives.
5. Run it from the repo root: `python students/DEx/week4/python/p2/pydantic_validator.py`
6. In `students/DEx/week4/python/p2/notes.md`, add `P2.3 - pydantic vs by hand` and list two things pydantic gave you that your own version did not.

If you get stuck: read the pydantic error, search "pydantic BaseModel validation error", and check the field names match your record keys. Ask in Discord with the traceback text.

How to think about it: Compare what you had to write by hand with what the model declares. Focus on reusable rules and readable errors.

**Note:** this validator is reused in week 9 (P7 extractors) and week 10 (P9 tests). Write it as if someone else will call it.

##### 6. What to Commit

- [ ] `students/DEx/week4/python/p2/pydantic_validator.py`
- [ ] `students/DEx/week4/python/p2/notes.md`

Done when:
- [ ] The model rejects the same bad records.
- [ ] The error messages are readable.
- [ ] The two differences are written in notes.
- [ ] The script runs from the repo root.

Commit message: `P2.3 rewrite the validator with pydantic`

## 6. What to Commit

- [ ] Tasks 1-5 - five queries (`S3.1` to `S3.5`), each with its answer written in notes, not just the SQL
- [ ] Tasks 6-8 - type hints on the week 3 scripts, hand-written validator, pydantic version, and the comparison
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] Your unmatched CIN explanation is written in your own words and you can say it out loud
- [ ] No data files and no secrets in the commits

**If you are short on time, cut in this order:** Task 2 (S3.2). Never cut Tasks 3-4 (S3.3, S3.4), Task 8 (P2.3) or the notes files. The join and its unmatched rows are the point of the week, and the validator comes back in weeks 9 and 10.

When the week is done, open a pull request to the class repo and do not squash-merge; see `docs/03-student-guide.md`.

Next: `students/DEx/week5/problem_statement.md`.
