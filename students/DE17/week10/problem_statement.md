# Week 10 - Window functions and tests

**Data Sprint 1 · Week 10 of 15 · Theme: Compare rows with window functions and check Python behaviour with automated tests**

## 1. What We Are Doing

This week you compare rows to their neighbours with SQL window functions. Then you check your Python code with pytest tests that run offline. Read this whole file before you start, then work through the task groups in order.

Task groups this week:
- Group 1 - Window Functions: Task 1 (`S9.1`), Task 2 (`S9.2`), Task 3 (`S9.3`), Task 4 (`S9.4`)
- Group 2 - Testing with pytest: Task 5 (`P9.1`), Task 6 (`P9.2`), Task 7 (`P9.3`)

## 2. Why We Are Doing It

The client is a fintech due-diligence company. Its analysts check Indian companies, compare states, watch company creation trends, and need the current version of each company record. Window functions help you answer those questions in SQL. Pytest tests protect the validator and the IBBI parser, so bad rows do not quietly reach the platform and make the dashboard wrong.

There is no milestone this week. The next milestone is the SCD2 Build in week 11, and Task 4 (`S9.4`) is the pattern that build uses. Keep that file.

## 3. What to Learn First (Prerequisites)

### Before this week: what you must already have

| You need | Came from | If it is missing |
|---|---|---|
| `data/raw/mca/` files and the practice kit tables `RAW_MCA_<ROC>`, `RAW_IBBI_SAMPLE`, `RAW_MCA_PREV` in your own schema | week 3, Step 0 | redo the Snowsight wizard load from week 3 |
| A cleaned registration date (task `S5.3`) | week 5 | re-run your week 5 SQL and save the cleaned date as a view you can reuse |
| Bronze MCA tables for all RoC files, loaded with stage + COPY INTO | week 6, task group First Snowflake Load | redo week 6. This week needs real rows, see the note below |
| The P2 validator script and its validation function (task group `P2`) | week 4 | ask the program lead, then finish that task first |
| The IBBI PDF extractor with its parsing function | week 9 | finish week 9 first. Task 6 tests that function |

### By the end of this week you can

- Rank, compare to the previous row, split into deciles, and pick the latest row per key
- Say why ROW_NUMBER and not RANK is correct when you need exactly one row
- Check your validator with tests that run offline, every time, and explain how CI runs tests on a pull request

### Core concepts to review first

- Window functions: `OVER`, `PARTITION BY`, `ORDER BY`
- Ranking functions: `ROW_NUMBER`, `RANK`, `DENSE_RANK`
- Moving values between rows: `LAG`, `NTILE`
- Dates in SQL: `DATE_TRUNC('month', ...)`
- pytest: test files, test functions that start with `test_`, and fixtures
- CI: a machine runs your tests when you push a commit or open a pull request

## 4. Where to Find Resources

### Where you work

- **VS Code** is your home base. You write and save every file here. Git runs in the VS Code terminal.
- **Snowflake (Snowsight in the browser)** is where you run SQL: Projects > Workspaces. Pick your role and warehouse, then run.
- Sign in to Snowflake with MFA (an authenticator app on your phone). Snowflake now asks for this second step when a person signs in with a password.
- **VS Code terminal** is where you run Python, with `.venv` activated: `source .venv/bin/activate` (Windows: `.venv\Scripts\activate`).
- **Paths** look like `students/DEx/week10/sql/s9/...`. Replace `DEx` with your own number, e.g. `DE12`.
- One task, one file, one commit, then update your pull request. Never squash-merge. No data files and no secrets in a commit.

Git is the same every week. The steps are in `docs/03-student-guide.md`, so we do not repeat them here.

### Docs to read when you need them

| If you need... | Read this |
|---|---|
| Git commands (clone, commit, push, pull request) | `docs/03-student-guide.md` |
| The client story and the data sources | `docs/01-project-brief.md` |
| A word you do not know | `docs/08-glossary.md` |
| Help with a broken tool | `docs/10-troubleshooting.md` |
| What a tool is for | `docs/11-tools-and-technology.md` |

Each task names the exact file path to commit to, inside this same week folder.

### Your week in order

| Step | LEARN (time estimate) | DO |
|---|---|---|
| **1** | Kudvenkat "Over clause in SQL Server" and "Row Number function" (about 30 min) | Task 1 - RANK vs DENSE_RANK |
| **2** | Kudvenkat "Rank and Dense Rank", "Difference between rank dense rank and row number", "Lead and Lag functions" (about 40 min) | Task 2 - month over month change |
| **3** | Kudvenkat "NTILE function". Optional: "Calculate running total", "FIRST VALUE function" (about 30 min) | Tasks 3-4 - deciles, latest row per company |
| **4** | pytest docs, "Get started" page, and Real Python "Getting Started With Testing in Python", pytest part only (about 60 min) | Task 5 - validator tests |
| **5** | Corey video #57: "Unit Testing Your Code with the unittest Module", for the idea of a test only (we use pytest), plus `CONTRIBUTING.md` in the repo (about 40 min) | Tasks 6-7 - parser fixture, tests in CI |

Last step: the cohort review, a window function walkthrough.

### Search keywords for students who get stuck

- `Snowflake window functions`
- `RANK DENSE_RANK ROW_NUMBER`
- `Snowflake LAG function`
- `Snowflake NTILE`
- `pytest fixtures`
- `GitHub Actions pull request checks`

## 5. Step-by-Step Guide

**How to work:** work one task at a time: one task, one main file, one commit. Some tasks also save a short note or fixture, as listed in the task. Git steps are in `docs/03-student-guide.md`.

> **Team project repo:** only the team leader copies or syncs this week's reviewed project files from the team repo into the class repo `delivery/` or `platform/` folder, then opens a pull request. Everyone keeps practice files in `students/DEx/week10/`.

**How your tasks are numbered:** the letter tells you the type. B = business and delivery, S = SQL and data modelling, P = Python, D = data platform, A = architecture diagrams. The number after the letter is a label, not the week number. So `S9.1` to `S9.4` and `P9.1` to `P9.3` all belong to week 10. Put the task ID in your commit message and in your pull request title.

### Group 1 - Window Functions

**Learn first:** the window function videos in the Kudvenkat playlist: "Over clause in SQL Server", "Row Number function", "Rank and Dense Rank", "Difference between rank dense rank and row number", "NTILE function", "Lead and Lag functions". He teaches SQL Server, so type the Snowflake version yourself and run it in Snowflake. Skip stored procedures, triggers, indexes and cursors.

> **Note: this week uses the real loaded data for Tasks 1-3.** The week 3 practice kit is too small for month over month change and for deciles. Use the bronze tables you loaded properly in week 6, with all RoC files, and the cleaned registration date from `S5.3`. This is the first week the real loaded data is used. Task 4 is the exception: it uses `RAW_MCA_PREV` as a practice comparison, because real monthly snapshots arrive in week 11.

For every task below: database = the one the program lead gave you, schema = your own schema, role = `STUDENT_ROLE`, warehouse = `STUDENT_WH`. Check the real column names in your table before you write the query.

#### Task 1 - RANK vs DENSE_RANK (ID `S9.1`)

##### 1. What We Are Doing

You rank states by their count of active companies. You work in VS Code to write the SQL file, then in Snowflake (Snowsight) to run it. Write first, run second.

##### 2. Why We Are Doing It

Analysts want to know which states have the most active companies, and ties matter when you compare states. RANK and DENSE_RANK handle ties differently, so you must know which one your report shows.

##### 3. What to Learn First

- Kudvenkat "Over clause in SQL Server" and "Row Number function"
- Kudvenkat "Rank and Dense Rank" and "Difference between rank dense rank and row number"
- Skip the parts about SQL Server performance

##### 4. Where to Find Resources

- Kudvenkat "Over clause in SQL Server" and "Row Number function", then "Rank and Dense Rank" and "Difference between rank dense rank and row number" (about 30 min)
- Search keywords: `Snowflake RANK DENSE_RANK`, `Snowflake OVER PARTITION BY`, `COUNT GROUP BY state`

##### 5. Step-by-Step Guide

1. In VS Code, right-click `students/DEx/week10/sql/s9` > New File. If the folders do not exist, create them first.
2. Name it `01_state_rankings.sql` and paste this starter.
3. Run Step A first to see the real `status` values. Your filter must match them exactly.
4. Finish the query: one row per state, the count of active companies, then RANK and DENSE_RANK over that count.
5. Check whether any states have the same active company count (a tie). Write in a SQL comment whether ties exist. If ties exist, find a pair of states where RANK and DENSE_RANK differ, and write two sentences saying why. If there are no ties, say so, then use a small synthetic tied-count example in the same file to show the difference, and write two sentences.
6. Run it in Snowflake, then copy the final working version back into your VS Code file.

```sql
-- students/DEx/week10/sql/s9/01_state_rankings.sql
-- S9.1 Rank states by active company count
USE ROLE STUDENT_ROLE;
USE WAREHOUSE STUDENT_WH;
USE DATABASE <your_database>;
USE SCHEMA <your_schema>;

-- Step A: what status values really exist?
SELECT status, COUNT(*) AS n
FROM <your_bronze_mca_table>
GROUP BY status
ORDER BY n DESC;

-- Step B: one row per state with the active company count,
-- then RANK() and DENSE_RANK() over that count. TODO.
```

**How to think about it:** look for states with the same active company count. If the real data has no ties, the two rank columns look the same. Use a small synthetic tied-count example to see the difference.

If you get stuck: read the error message; run Step A first and compare the `status` values exactly; search `Snowflake RANK DENSE_RANK`; ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week10/sql/s9/01_state_rankings.sql`
- [ ] Done when: the query runs; you see both rank columns; the file says whether real ties exist; if no ties exist, the file has a small synthetic tied-count example; the file has your two-sentence explanation as a comment; you committed.
- [ ] Commit message: `S9.1 rank states by active company count`

#### Task 2 - Month over month change (ID `S9.2`)

##### 1. What We Are Doing

You compute month over month change in new company registrations. You work in VS Code to write the SQL file, then in Snowflake (Snowsight) to run it.

##### 2. Why We Are Doing It

A due-diligence client watches whether company creation is rising or falling. Month over month change turns a list of months into that trend.

##### 3. What to Learn First

- Kudvenkat "Lead and Lag functions" (about 20 min)
- Optional: Kudvenkat "Calculate running total" if the idea of a running total is unclear

##### 4. Where to Find Resources

- Kudvenkat "Lead and Lag functions" (about 20 min)
- Optional: Kudvenkat "Calculate running total"
- Search keywords: `Snowflake LAG function`, `Snowflake DATE_TRUNC month`, `month over month SQL`

##### 5. Step-by-Step Guide

1. In VS Code create `students/DEx/week10/sql/s9/02_mom_registrations.sql`.
2. Use the cleaned registration date from `S5.3` inside `DATE_TRUNC('month', ...)` to get one row per month.
3. Check for missing months. A `GROUP BY` leaves out months with zero registrations. Before you use `LAG`, build a month series from the first to the last month and `LEFT JOIN` your counts, so every month appears with `new_companies = 0` when it has no rows.
4. Add `LAG(...)` to bring the previous month's count onto the same row.
5. Compute the change and the change as a percentage. Decide what to show for the first month, which has no previous row.
6. Run it in Snowflake. Find the month with the biggest drop and write the answer in a comment at the top of the file.

```sql
-- students/DEx/week10/sql/s9/02_mom_registrations.sql
-- S9.2 Month over month change in new registrations
USE ROLE STUDENT_ROLE;
USE WAREHOUSE STUDENT_WH;
USE DATABASE <your_database>;
USE SCHEMA <your_schema>;

WITH monthly_counts AS (
    SELECT DATE_TRUNC('month', <clean_reg_date>) AS reg_month,
           COUNT(*) AS new_companies
    FROM <your_bronze_mca_table>
    WHERE <clean_reg_date> IS NOT NULL
    GROUP BY 1
),
monthly AS (
    -- TODO: build a month series from the first to the last month,
    -- then LEFT JOIN monthly_counts so missing months appear with 0
    SELECT reg_month, new_companies
    FROM monthly_counts
)
SELECT reg_month,
       new_companies,
       LAG(new_companies) OVER (ORDER BY reg_month) AS prev_month
       -- TODO: add the change and the change in percent
FROM monthly
ORDER BY reg_month;
```

**How to think about it:** the first month has no previous row. Decide whether to show NULL, 0, or a blank, and write down why in a comment.

If you get stuck: check that the cleaned date column name matches your table; run the CTE followed by `SELECT * FROM monthly;` first; search `Snowflake LAG function`; ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week10/sql/s9/02_mom_registrations.sql`
- [ ] Done when: one row per month with no gaps; missing months show zero; the previous month is beside it; the biggest drop is written down; the query runs in Snowflake.
- [ ] Commit message: `S9.2 month over month change in registrations`

#### Task 3 - Capital deciles (ID `S9.3`)

##### 1. What We Are Doing

You split companies inside one state into ten groups by paid up capital. You work in VS Code to write the SQL file, then in Snowflake (Snowsight) to run it.

##### 2. Why We Are Doing It

Comparing paid up capital across a state is easier in ten equal groups than in one long list. Deciles show whether capital is spread out or sitting in a few large companies.

##### 3. What to Learn First

- Kudvenkat "NTILE function" (about 15 min)
- Optional: Kudvenkat "FIRST VALUE function" if you want the biggest company in each group

##### 4. Where to Find Resources

- Kudvenkat "NTILE function" (about 15 min)
- Optional: Kudvenkat "FIRST VALUE function"
- Search keywords: `Snowflake NTILE`, `Snowflake PARTITION BY state`, `SQL deciles`

##### 5. Step-by-Step Guide

1. In VS Code create `students/DEx/week10/sql/s9/03_capital_deciles.sql`.
2. Check how the state name is really written before you filter. Try `SELECT DISTINCT state FROM ... LIMIT 50`.
3. Check the data type of `<paid_up_capital>`. Run `DESCRIBE TABLE <your_bronze_mca_table>;` or look at a few rows. If it is text and contains commas, convert it to a number before you rank: `TRY_TO_NUMBER(REPLACE(CAST(<paid_up_capital> AS VARCHAR), ',', ''))`. Use that same numeric value in the window and in the output.
4. Deciding what to do with missing capital is part of the task. Choose and be able to defend it.
5. Add your answer in a comment at the top of the file.

```sql
-- students/DEx/week10/sql/s9/03_capital_deciles.sql
-- S9.3 Deciles of paid up capital within each state
USE ROLE STUDENT_ROLE;
USE WAREHOUSE STUDENT_WH;
USE DATABASE <your_database>;
USE SCHEMA <your_schema>;

WITH deciles AS (
    SELECT state,
           cin,
           TRY_TO_NUMBER(REPLACE(CAST(<paid_up_capital> AS VARCHAR), ',', '')) AS capital
           -- TODO: add a NTILE(10) window over each state,
           -- ordered by capital from highest to lowest, named decile
    FROM <your_bronze_mca_table>
    -- TODO: decide what to do with rows where capital is NULL
)
-- TODO: write the final SELECT to show one row per decile for the state
-- you found, with the number of companies in each decile.
```

**How to think about it:** NULL paid up capital is a data decision. Choose a rule, apply it consistently, and write down why. If capital is text with commas, rank the numeric value, not the text.

If you get stuck: run `SELECT DISTINCT state` to check the spelling; check how many rows have NULL capital; check the capital column type; search `Snowflake NTILE`; ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week10/sql/s9/03_capital_deciles.sql`
- [ ] Done when: you get ten decile rows for Maharashtra plus your count for the top decile; you checked the capital type and ranked the numeric value, not the text; you wrote down what you did with NULL capital; the query runs in Snowflake.
- [ ] Commit message: `S9.3 paid up capital deciles by state`

#### Task 4 - Latest row per company (ID `S9.4`)

##### 1. What We Are Doing

You practise keeping only the latest row for each CIN. This is a practice comparison, not a real monthly snapshot. You work in VS Code to write the SQL file and the note, then in Snowflake (Snowsight) to run the SQL.

##### 2. Why We Are Doing It

The same CIN appears once per snapshot, and the client wants the current version only. This is the pattern we reuse next week, in week 11, when we build the history table. In this task `RAW_MCA_PREV` is a synthetic practice copy. Real monthly snapshots arrive in week 11.

##### 3. What to Learn First

Nothing new. You use what you learned in Task 1: "Row Number function" and "Rank and Dense Rank".

##### 4. Where to Find Resources

- Reuse Kudvenkat "Row Number function" and "Rank and Dense Rank" from Task 1
- Search keywords: `Snowflake ROW_NUMBER partition`, `latest row per key SQL`, `UNION ALL matching columns`, `Snowflake TRY_TO_NUMBER`

##### 5. Step-by-Step Guide

1. This task is a practice comparison, not real snapshots. Use the current bronze table for the same RoC office that `RAW_MCA_PREV` was copied from. Add one synthetic ordering date to each side (for example `2024-01-01` and `2024-02-01`) and write "synthetic practice date" in a comment. Real monthly snapshots arrive in week 11.
2. Use the same explicit column list and compatible casts on both sides of `UNION ALL`. Start from the skeleton and change the column names to your real ones. Do not use `SELECT *`.
3. Number the rows per CIN with `ROW_NUMBER()`, newest synthetic practice date first, and keep only number 1.
4. Write one sentence in `notes.md` saying why ROW_NUMBER and not RANK gives exactly one row per CIN.
5. Run it in Snowflake. A CIN that you know changed between the two tables is a good check.

```sql
-- students/DEx/week10/sql/s9/04_latest_per_company.sql
-- S9.4 Latest row per CIN
USE ROLE STUDENT_ROLE;
USE WAREHOUSE STUDENT_WH;
USE DATABASE <your_database>;
USE SCHEMA <your_schema>;

-- Use the RoC table that RAW_MCA_PREV was copied from.
-- The two dates are synthetic ordering labels only. They are not real snapshot dates.
-- Real monthly snapshots arrive in week 11.

WITH snapshots AS (
    SELECT
        TO_DATE('2024-01-01') AS snapshot_date, -- synthetic practice date, not a real snapshot
        cin,
        CAST(<company_name> AS VARCHAR) AS company_name,
        CAST(status AS VARCHAR) AS status,
        TRY_TO_DATE(<registration_date>) AS registration_date,
        CAST(state AS VARCHAR) AS state,
        TRY_TO_NUMBER(REPLACE(CAST(<paid_up_capital> AS VARCHAR), ',', '')) AS paid_up_capital
    FROM <matching_source_roc_table>

    UNION ALL

    SELECT
        TO_DATE('2024-02-01') AS snapshot_date, -- synthetic practice date, not a real snapshot
        cin,
        CAST(company_name AS VARCHAR) AS company_name,
        CAST(status AS VARCHAR) AS status,
        TRY_TO_DATE(registration_date) AS registration_date,
        CAST(state AS VARCHAR) AS state,
        TRY_TO_NUMBER(REPLACE(CAST(paid_up_capital AS VARCHAR), ',', '')) AS paid_up_capital
    FROM RAW_MCA_PREV
),
numbered AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY cin ORDER BY snapshot_date DESC) AS rn
    FROM snapshots
)
SELECT * FROM numbered WHERE rn = 1;
```

**How to think about it:** focus on what happens when two rows for the same CIN have the same snapshot date. Write one sentence about the number each function gives in that case.

If you get stuck: check that both sides of the `UNION ALL` have the same column list and compatible types; check the synthetic practice dates; search `Snowflake ROW_NUMBER partition`; ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week10/sql/s9/04_latest_per_company.sql`
- [ ] `students/DEx/week10/sql/s9/notes.md`
- [ ] Done when: every CIN appears once in the result; `notes.md` has the one-sentence answer, your synthetic practice dates, and the note that real snapshots arrive in week 11; the file is committed.
- [ ] Commit message: `S9.4 latest row per CIN with row_number`

> **Note:** Task 3 has a NULL trap. Paid up capital is missing for many companies, and where those rows land in the deciles changes the answer. Say what you did with them.

> **Note:** Task 4 is the pattern the SCD2 build uses in week 11 to find the current version row. Keep it. It uses a practice comparison only, because real monthly snapshots arrive in week 11.

### Group 2 - Testing with pytest

**Learn first:** the pytest docs "Get started" page (https://docs.pytest.org) and Real Python "Getting Started With Testing in Python", pytest part only (about 60 min). Corey video #57 "Unit Testing Your Code with the unittest Module" is only for the idea of a test; we use pytest, so skip his unittest code. For the CI words, search `CONTRIBUTING.md` in the repo.

A test file is Python that calls your function and checks the result. It runs offline and it runs the same way every time.

#### Task 5 - Test the validator (ID `P9.1`)

##### 1. What We Are Doing

You write pytest tests for the MCA validator. You work in VS Code to write the test file, then in the VS Code terminal (with `.venv` active) to run pytest.

##### 2. Why We Are Doing It

The validator decides which MCA rows are allowed into the platform. A bad rule quietly lets bad CINs and bad dates through, and the dashboard is wrong from day one.

##### 3. What to Learn First

- pytest docs "Get started"
- Real Python pytest article, pytest part only
- Skip the parts about mocking and coverage

##### 4. Where to Find Resources

- pytest docs "Get started" page (https://docs.pytest.org)
- Real Python "Getting Started With Testing in Python", pytest part only (about 60 min)
- Search keywords: `pytest test file naming`, `pytest assert`, `pytest import sys.path`

##### 5. Step-by-Step Guide

1. In VS Code create the folder `students/DEx/week10/python/p9/tests` and inside it `test_validator.py`.
2. Import your validation function from the P2 task (week 4). Start with `from validator import validate_record` and change the module and function names to your real ones.
3. If Python cannot find your validator, add the folder that holds it to `sys.path` at the top of the test file, or see `docs/10-troubleshooting.md`.
4. Write at least five tests: one good record, bad CINs, bad dates, bad amounts. Every test must call `validate_record` and assert an expected result. A test body with only `pass` does not count.
5. Before the first test run, install pytest inside `.venv`. In the VS Code terminal run:
   ```
   source .venv/bin/activate
   python -m pip install pytest
   python -m pytest --version
   ```
   On Windows use `.venv\Scripts\activate`. You should see a pytest version number.
6. Run the tests from the repo root:
   ```
   python -m pytest students/DEx/week10/python/p9/tests -v
   ```
7. After all tests pass, deliberately change one expected value so one test fails. Run pytest again and look at the failure. Restore the value and run again to see all pass.
8. Paste the passing output into `students/DEx/week10/python/p9/notes.md`.

```python
# students/DEx/week10/python/p9/tests/test_validator.py
import pytest
from validator import validate_record  # TODO: your real module and function name

def test_good_record_passes():
    record = {}  # TODO: one record you know is valid
    assert validate_record(record) is True  # TODO: check the real return value

def test_bad_cin_is_rejected():
    record = {}  # TODO: one record with a CIN rule broken in one clear way
    assert validate_record(record) is False  # TODO: check the real return value

def test_bad_date_is_rejected():
    record = {}  # TODO
    assert validate_record(record) is False  # TODO: check the real return value

def test_bad_amount_is_rejected():
    record = {}  # TODO
    assert validate_record(record) is False  # TODO: check the real return value
```

If you get stuck: check that the test file name and test function names start with `test_`; check the import path to your validator; read `docs/10-troubleshooting.md`; run pytest from the repo root and ask in Discord with the full output.

##### 6. What to Commit

- [ ] `students/DEx/week10/python/p9/tests/test_validator.py`
- [ ] `students/DEx/week10/python/p9/notes.md`
- [ ] Done when: at least five tests exist; every test calls `validate_record` and asserts an expected result; no test body is only `pass`; you changed one expected value, saw one test fail, then restored it; `python -m pytest ... -v` shows all tests passed; the output is pasted in `notes.md`.
- [ ] Commit message: `P9.1 pytest tests for the MCA validator`

#### Task 6 - Test the PDF parser offline (ID `P9.2`)

##### 1. What We Are Doing

You test the IBBI PDF parser with a saved text sample, not with a live PDF download. You work in VS Code to write the test and the fixture, then in the VS Code terminal to run pytest.

##### 2. Why We Are Doing It

The IBBI extractor must give the same rows on every run. A test with a saved text sample proves the parsing logic alone, with no PDF download and no network.

##### 3. What to Learn First

Nothing new. You reuse the pytest basics from Task 5. For the parser function, look at your own week 9 code.

##### 4. Where to Find Resources

- Reuse the pytest docs "Get started" page (https://docs.pytest.org) and the Real Python pytest article from Task 5
- Search keywords: `pytest fixture`, `pytest read text file`, `offline unit test parser`

##### 5. Step-by-Step Guide

1. Create `students/DEx/week10/python/p9/tests/test_ibbi_parser.py`.
2. Create `students/DEx/week10/python/p9/tests/fixtures/ibbi_sample_text.txt`. Paste 10-20 lines of PDF text you already extracted from an IBBI PDF. This small text file is allowed in Git; the real PDF is not.
3. Import your parsing function from week 9. Call it with the fixture text, not with a file download.
4. Check the number of rows and one or two field values you can verify by eye. Every test must call `parse_ibbi_text` and assert an expected result. A test body with only `pass` does not count.
5. Run `python -m pytest students/DEx/week10/python/p9/tests -v` from the repo root.

```python
# students/DEx/week10/python/p9/tests/test_ibbi_parser.py
from pathlib import Path
import pytest

from ibbi_extractor import parse_ibbi_text  # TODO: your real module and function name

FIXTURE = Path(__file__).parent / "fixtures" / "ibbi_sample_text.txt"

@pytest.fixture
def raw_text():
    return FIXTURE.read_text(encoding="utf-8")

def test_extracts_expected_number_of_rows(raw_text):
    rows = parse_ibbi_text(raw_text)
    expected_count = 0  # TODO: the real expected count
    assert len(rows) == expected_count

def test_extracts_a_known_cin(raw_text):
    rows = parse_ibbi_text(raw_text)
    expected_cin = "TODO"  # TODO: one CIN you can see in the fixture
    assert any(row["cin"] == expected_cin for row in rows)  # TODO: check the field name
```

If you get stuck: check the fixture path with `Path(__file__).parent`; open the fixture and count the expected rows by eye; search `pytest fixture`; ask in Discord with the full error text.

##### 6. What to Commit

- [ ] `students/DEx/week10/python/p9/tests/test_ibbi_parser.py`
- [ ] `students/DEx/week10/python/p9/tests/fixtures/ibbi_sample_text.txt`
- [ ] Done when: the tests pass with no internet connection; every test calls the parser and asserts an expected result; no test body is only `pass`; the fixture file is small and committed; no PDF file is committed.
- [ ] Commit message: `P9.2 offline test for the IBBI parser`

#### Task 7 - Run the tests in CI (ID `P9.3`)

##### 1. What We Are Doing

You find out which CI workflow runs student tests, and you record proof that your tests ran in CI. You work in VS Code to write your notes, in the VS Code terminal to run pytest, and in GitHub in the browser to look at the checks on your pull request.

##### 2. Why We Are Doing It

CI means the tests run on their own when someone opens a pull request, so a broken validator is caught before it reaches the class repo.

##### 3. What to Learn First

Read `CONTRIBUTING.md` in the repo (about 15 min). Three plain sentences on CI: CI is a machine that runs your tests for you. It runs when you push a commit or open a pull request. If a test fails, GitHub shows a red cross, and nobody has to remember to run anything.

##### 4. Where to Find Resources

- `CONTRIBUTING.md` in the repo (about 15 min)
- The `.github/workflows` folder in the repo
- Search keywords: `GitHub Actions pull request checks`, `GitHub Actions workflow path filter`, `pytest in GitHub Actions`

##### 5. Step-by-Step Guide

1. Look at the `.github/workflows` folder in the repo. The repo already has workflow files there.
2. Automated checks may run only on the `platform/` and `delivery/` folders by default. Read `CONTRIBUTING.md`.
3. Ask the program lead which workflow runs student tests. Do not write a new workflow file on your own.
4. If a workflow exists for student folders, make sure your tests are picked up, push, and open your pull request. Wait for the check and record the link to the successful CI run in `notes.md`.
5. If no workflow runs student folders, run pytest at the repo root, copy the terminal output, and mark it in `notes.md` as "temporary local evidence only". Write "Task 7 pending: CI workflow for student folders not enabled yet". Ask the program lead to enable one. Do not mark Task 7 as done.
6. When the program lead enables a workflow, push again and record the successful CI run link in `notes.md`.
7. The proof of completion is the successful CI run link, not the local pytest output.

**How to think about it:** CI is a machine that runs the tests for you. You only need to find which workflow covers your folder, then show proof from that workflow. A local run is temporary evidence only.

If you get stuck: read `CONTRIBUTING.md` again; ask the program lead which workflow covers student folders; run pytest at the repo root and paste the output as temporary evidence; ask in Discord with the check name or error text.

##### 6. What to Commit

- [ ] `students/DEx/week10/python/p9/notes.md` with proof and a short note on what you found
- [ ] Done when: a successful CI run for your pull request is recorded in `notes.md` with the check link or check name.
- [ ] If no workflow exists yet, `notes.md` says "Task 7 pending" and shows temporary local output. This does not complete the task.
- [ ] No secrets and no new invented workflow are committed.
- [ ] Only edit a workflow file if the program lead names the exact file.
- [ ] Commit message: `P9.3 run pytest tests in CI`

> **On Task 6:** a test that downloads a PDF is not a test. It is a network call that fails on a bad connection. The fixture is a few lines of saved text committed next to the test, which is why it is allowed in Git while real source files are not.

> **If pytest says `no tests ran`:** the file name or the function name does not start with `test_`. See `docs/10-troubleshooting.md`.

## 6. What to Commit

### End of week checklist

- [ ] Tasks 1-4: four queries, each with the answer written down, plus the RANK and ROW_NUMBER explanations
- [ ] Task 5: five or more validator tests, every test asserts an expected result, a broken expectation seen to fail then restored, and a passing run pasted
- [ ] Task 6: an offline parser fixture and tests that call the parser and assert expected results
- [ ] Task 7: a recorded successful CI run for your pull request, or marked pending with temporary local output in `notes.md`
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] You can explain, out loud, the difference between RANK, DENSE_RANK, and ROW_NUMBER
- [ ] Team leaders only: reviewed files copied or synced from the team repo into `delivery/` or `platform/` in the class repo, then a pull request

**If you are short on time, cut in this order:** Task 3 (`S9.3`), then Task 6 (`P9.2`). Never cut Task 4 (`S9.4`). It is next week's SCD2 pattern.

Open your pull request when your tasks are ready, following `docs/03-student-guide.md`, and never squash-merge.

Next: `week11/problem_statement.md`.
