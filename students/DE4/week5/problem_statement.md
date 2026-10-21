# Week 5 - Missing values, messy strings

**Data Sprint 1 · Week 5 of 15 · Theme: handle what is missing and clean what is messy, so the data says what it means**

Read this whole file before you start. Then work through the task groups in order.

## 1. What We Are Doing

This week you learn to show missing values honestly, compare two monthly snapshots, and clean messy text like state names and dates. You write SQL files in VS Code and run them in Snowflake. This is new for most people. Go slowly, one task at a time.

You work in two task groups:

- **Group 1 - Missing Values and Set Operations:** Task 1 (`S4.1`), Task 2 (`S4.2`), Task 3 (`S4.3`), Task 4 (`S4.4`).
- **Group 2 - SQL Functions: String, Date, Math:** Task 5 (`S5.1`), Task 6 (`S5.2`), Task 7 (`S5.3`), Task 8 (`S5.4`), Task 9 (`S5.5`).

## 2. Why We Are Doing It

The client is a fintech due-diligence company. Its analysts check Indian companies: is the company real and active (MCA), was it ever in insolvency (IBBI), how does it compare to its state (MCA CDM), and what was the interest-rate backdrop (RBI).

This week the data we already loaded stops being clean-looking. Some paid-up capital boxes are empty. State names come in several spellings. Registration dates come in more than one format. If we store 0 where the client should read "not reported", a due-diligence check gives a wrong answer. If we clean state names badly, the comparison with MCA CDM state totals breaks. So this week is about honesty and consistency.

**Milestone note:** there is no milestone marker this week. But Task 3 (`S4.3`) is the same comparison the SCD2 logic performs in week 11. Keep the query and the counts. Task 6 (`S5.2`) becomes the shared silver-layer cleaning rule in week 12, so write it so a teammate can read it.

## 3. What to Learn First (Prerequisites)

### Before this week: what you must already have

This week uses tables you built in week 3. Check them before Task 1.

| Required item | When it was created | How to check it |
|---|---|---|
| Role `STUDENT_ROLE`, warehouse `STUDENT_WH`, and your own schema inside the database the program lead gave you | Week 1 setup, task `G0.2` | Sign in to Snowflake and check the role and warehouse at the top of the Workspace. |
| `RAW_MCA_<ROC>` three times, one per RoC ZIP file you loaded | Week 3, Step 0 practice data kit | Run `SELECT COUNT(*) FROM RAW_MCA_ROC1;` and the same for ROC2 and ROC3. |
| `RAW_MCA_PREV`, the "last month" practice copy of one RoC table, with a few rows changed and a few deleted | Week 3, Step 0 | Run `SELECT COUNT(*) FROM RAW_MCA_PREV;` and look at a few changed rows. |
| `RAW_IBBI_SAMPLE`, 20 to 30 rows you typed from the IBBI PDF. Task 8 needs it. | Week 3, Step 0 | Run `SELECT COUNT(*) FROM RAW_IBBI_SAMPLE;` (should be between 20 and 30). |
| Your week 3 folder with `students/DEx/week3/sql/s2/00_practice_kit.sql` in it | Week 3 | Open the file in VS Code. |

**If a table is missing:** open `students/DEx/week3/sql/s2/00_practice_kit.sql` in VS Code, copy the parts you need, run them in Snowflake, and check the row counts again. If the file is not there, ask your teammate or the program lead before you start Task 1. Do not skip a table.

**Python is paused this week.** There are no P tasks. Use the free time to finish any Python task from weeks 2, 3 or 4 that you did not complete. Optional revision: Corey video #28 "Python Tutorial: CSV Module - How to Read, Parse, and Write CSV Files", Corey video #31 "Python Tutorial: Using Try/Except Blocks for Error Handling", Corey video #25 "Python Tutorial: File Objects - Reading and Writing to Files". Run scripts from the repo root after `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows).

### By the end of this week you can

- Display missing values without inventing them, and defend that rule in writing
- Compare two monthly snapshots with EXCEPT and INTERSECT
- Clean state names and parse dates safely, so one bad row does not fail the whole query

### Core concepts to review first

- **NULL** means "no value was reported". It is not 0 and it is not an empty string.
- **COALESCE** returns the first value that is not NULL. **NULLIF** turns one value into NULL when a condition matches.
- **UNION ALL** keeps every row. **UNION** removes duplicate rows.
- **EXCEPT** (also called **MINUS** in Snowflake, same job) returns rows in the first query that are not in the second. **INTERSECT** returns rows in both.
- **TRY_TO_DATE** and **TRY_TO_NUMBER** parse text safely. If the text is bad, they return NULL instead of failing the query.
- **DATEDIFF counts boundaries, not full units.** So age in completed years needs care. Task 9 asks you to find the fix.

> **Why "missing stays missing".** The client checks Indian companies. Suppose a company's paid-up capital column is empty in the MCA file. If we store 0, the client reads "this company reported zero capital". That is a different, and wrong, fact. An analyst may then reject a healthy company or misread a ratio. So an empty value must stay empty (NULL), and our reports must show it as "Not reported", never as 0. Every SQL task this week uses your own schema, with role `STUDENT_ROLE` and warehouse `STUDENT_WH`. Run `SELECT * FROM <table> LIMIT 5;` first to see the exact column names in your own tables.

## 4. Where to Find Resources

> **Where you work (same rule every week)**
> - **VS Code** = you write and save files here, and you commit here.
> - **Snowflake (Snowsight, in the browser)** = you run SQL here. Open Snowsight > Projects > Workspaces, paste your SQL, pick the role and warehouse, run it. Snowflake is where the data lives; VS Code is where your work is saved.
> - **VS Code terminal** = you run Git commands, and Python when a week has Python tasks.
> - Full paths use `DEx`. Replace `DEx` with your own number, for example `DE12`. So `students/DEx/week5/...` becomes `students/DE12/week5/...` for you.
> - Your Snowflake sign-in uses MFA, the second step you set up in week 1.

| You want to... | Read this |
|---|---|
| Know how to commit and open a pull request | `docs/03-student-guide.md` |
| Remember why the client cares | `docs/01-project-brief.md` |
| Look up a word | `docs/08-glossary.md` |
| Fix a broken tool | `docs/10-troubleshooting.md` |

Git commands: `docs/03-student-guide.md` (one page, read it once and come back when a Git step feels unclear). Client story and sources: `docs/01-project-brief.md`. Unknown word: `docs/08-glossary.md`. Broken tool: `docs/10-troubleshooting.md`. What a tool is for: `docs/11-tools-and-technology.md`.

**How your tasks are numbered.** The letter tells you the skill: B = Business and Delivery, S = SQL and Data Modeling, P = Python, D = Data Platform, A = Architecture diagrams. The number after the letter is the task ID (for example `S4.1`). Put the task ID at the start of every commit message and every pull request title.

### Your week in order

| Step | Learn first (exact parts, time) | Then do |
|---|---|---|
| **1** | Kudvenkat Part 15 "Different ways to replace NULL" and Part 16 "Coalesce function" (about 25 min). Skip the SQL Server install and SSMS clicks. | Tasks 1 and 2 - show missing capital, UNION vs UNION ALL |
| **2** | Kudvenkat "SQL Server except operator", "Difference between except and not in", "Intersect operator", "Difference between union intersect and except" (about 30 min). Then search the Snowflake docs for "set operators". | Tasks 3 and 4 - snapshot comparison, the missing-stays-missing rule |
| **3** | Kudvenkat Part 22 "Built in string functions", Part 23 "LEFT, RIGHT, CHARINDEX and SUBSTRING", Part 24 "Replicate, Space, Patindex, Replace and Stuff" (about 35 min). | Tasks 5 and 6 - CIN length, cleaned states |
| **4** | Kudvenkat Part 25 "DateTime functions", Part 26 "IsDate, Day, Month, Year, DateName", Part 27 "DatePart, DateAdd and DateDiff", Part 28 "Cast and Convert", Part 29 "Mathematical functions" (about 50 min). | Tasks 7, 8 and 9 - dates, recovery rate, company age |
| **5** | Nothing new. Reread your own Task 4 notes. | Cohort review: defend the NULL rule out loud |

The videos come from the Kudvenkat playlist: https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB

> **Kudvenkat teaches SQL Server. We use Snowflake.** Watch for the idea, then type the Snowflake version and run it in Snowflake.
> - `ISNULL()` becomes `IFNULL()` or `COALESCE()`.
> - `LEN()` becomes `LENGTH()`.
> - `GETDATE()` becomes `CURRENT_TIMESTAMP()`.
> - `TOP 10` becomes `LIMIT 10`.
> - Cast and convert become `CAST` or the `::` operator. For safe parsing use `TRY_TO_DATE` and `TRY_CAST`.
> - `DATEDIFF` argument order is `DATEDIFF(part, start, end)`.
> - `DATEDIFF` counts boundaries, not full units. So age in completed years needs care. Task 9 asks you to find the fix.

### Search keywords if you get stuck

- Snowflake COALESCE NULLIF
- Snowflake EXCEPT MINUS INTERSECT
- Snowflake TRY_TO_DATE format
- Snowflake DATEDIFF completed years
- Snowflake CASE statement

## 5. Step-by-Step Guide

How to work: one task, one file, one commit. Do the tasks in order. Save every file inside your own week folder, `students/DEx/week5/...`, and commit from VS Code. The Git steps are in `docs/03-student-guide.md`.

### Group 1 - Missing Values and Set Operations

Foundation link: Kudvenkat Part 15 (replace NULL), Part 16 (COALESCE), Part 17 (union and union all), and the unnumbered videos "SQL Server except operator", "Difference between except and not in", "Intersect operator", "Difference between union intersect and except". Snowflake has both `EXCEPT` and `MINUS`, and they do the same job.

#### Task 1 - Show missing capital (ID `S4.1`)

##### 1. What We Are Doing

Find the rows where paid-up capital is empty, count them, and display them with the label "Not reported". You write the file in VS Code first, then run it in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

The client must see "Not reported" and never a fake 0, or a due-diligence check gives a wrong answer.

##### 3. What to Learn First

- Kudvenkat Part 15 "Different ways to replace NULL" and Part 16 "Coalesce function". About 25 minutes. Skip the SQL Server install and the SSMS clicks.
- `ISNULL()` in SQL Server becomes `IFNULL()` or `COALESCE()` in Snowflake.
- `COALESCE` returns the first value that is not NULL. `NULLIF` turns a value into NULL when it matches.
- `TRIM` removes spaces at both ends of a text value.

##### 4. Where to Find Resources

- Kudvenkat Part 15 "Different ways to replace NULL" and Part 16 "Coalesce function" (about 25 min).
- Search the Snowflake docs for "COALESCE".
- Search keywords: Snowflake COALESCE NULLIF, Snowflake TRIM empty string, Snowflake NULL handling.

##### 5. Step-by-Step Guide

1. In VS Code, open your week 5 folder and make the file `students/DEx/week5/sql/s4/01_null_capital.sql`.
2. In Snowflake, run `SELECT * FROM RAW_MCA_<ROC> LIMIT 5;` to see the exact name of the paid-up capital column in your table.
3. Write two queries in the file: one counts rows where that column is NULL or empty, one shows those rows with the label "Not reported" in a new output column. Do not change the stored data.
4. Starter skeleton:
   ```sql
   -- 1) how many rows have no paid up capital?
   SELECT COUNT(*) AS missing_rows
   FROM RAW_MCA_ROC1
   WHERE <PAID_UP_CAPITAL_COL> IS NULL
      OR TRIM(<PAID_UP_CAPITAL_COL>) = '';   -- TODO: check if empty strings exist too

   -- 2) show them without inventing a value
   SELECT
       CIN,
       /* TODO: build the display expression with COALESCE and NULLIF.
          The label for a missing value must be 'Not reported'. */
       <DISPLAY_EXPRESSION> AS paid_up_capital_display
   FROM RAW_MCA_ROC1
   WHERE /* TODO: same filter as above */
   LIMIT 50;
   ```
5. Run both queries in Snowflake. Fix errors until both run. Copy the final working SQL back into the VS Code file.

If you get stuck: read the error text and find which line it points to. Check that `<PAID_UP_CAPITAL_COL>` is the exact column name from your `SELECT * ... LIMIT 5`. Search "Snowflake COALESCE". Ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s4/01_null_capital.sql`
- [ ] Both queries run in Snowflake
- [ ] The count is written as a comment at the top of the file
- [ ] No stored value was updated

Commit message: `S4.1 show missing paid up capital as Not reported`

#### Task 2 - UNION ALL vs UNION (ID `S4.2`)

##### 1. What We Are Doing

Run the same combine of your three RoC tables twice, once with `UNION ALL` and once with `UNION`, and compare the row counts. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

The three RoC files must become one list for the client. You need to know which combine keeps every row.

##### 3. What to Learn First

- Kudvenkat Part 17 "Union and union all". About 12 minutes.
- `UNION ALL` keeps every row. `UNION` removes duplicate rows.

##### 4. Where to Find Resources

- Kudvenkat Part 17 "Union and union all" (about 12 min).
- Search the Snowflake docs for "set operators".
- Search keywords: Snowflake UNION ALL, Snowflake set operators.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week5/sql/s4/02_union_all_vs_union.sql`.
2. Write the CIN and company name query over your three `RAW_MCA_<ROC>` tables twice: once with `UNION ALL`, once with `UNION`. Use `COUNT(*)` around each one.
3. Starter skeleton:
   ```sql
   SELECT COUNT(*) AS rows_union_all FROM (
       SELECT CIN, <COMPANY_NAME_COL> FROM RAW_MCA_ROC1
       UNION ALL
       SELECT CIN, <COMPANY_NAME_COL> FROM RAW_MCA_ROC2
       UNION ALL
       SELECT CIN, <COMPANY_NAME_COL> FROM RAW_MCA_ROC3
   );

   -- TODO: same query with UNION instead of UNION ALL
   ```
4. Run both in Snowflake and write the two counts plus your two sentences into `students/DEx/week5/sql/s4/notes.md`.

If you get stuck: read the error text first. Check the column names and the number of columns in each part of the union. Search "Snowflake UNION ALL". Ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s4/02_union_all_vs_union.sql`
- [ ] `students/DEx/week5/sql/s4/notes.md`
- [ ] Both counts are in the notes file
- [ ] Your two sentences say what happened to duplicate rows
- [ ] The pull request shows both files changed

Commit message: `S4.2 compare UNION ALL and UNION row counts`

#### Task 3 - Compare two snapshots (ID `S4.3`)

##### 1. What We Are Doing

Compare `RAW_MCA_PREV` with the current RoC table to find CINs only in the practice previous copy, CINs only in the current table, and CINs in both. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

The client asks "what changed since last month". Set operations answer it exactly, and week 11 reuses this idea.

##### 3. What to Learn First

- The unnumbered videos "SQL Server except operator", "Difference between except and not in", "Intersect operator", "Difference between union intersect and except". About 30 minutes.
- `EXCEPT` returns rows in the first query that are not in the second. `INTERSECT` returns rows in both.
- Snowflake also has `MINUS`, which does the same job as `EXCEPT`.

##### 4. Where to Find Resources

- Kudvenkat "SQL Server except operator", "Difference between except and not in", "Intersect operator", "Difference between union intersect and except" (about 30 min).
- Search the Snowflake docs for "set operators".
- Search keywords: Snowflake EXCEPT INTERSECT, Snowflake MINUS.

##### 5. Step-by-Step Guide

1. Pick the RoC table you copied in week 3 to make `RAW_MCA_PREV`. That pair is your two snapshots.
2. Create `students/DEx/week5/sql/s4/03_except_intersect.sql`.
3. Query 1: CINs only in the practice previous copy (use `EXCEPT`). Query 2: CINs only in the current table (the reverse `EXCEPT`), to find the practice rows you deleted from the previous copy. Query 3: CINs in both (use `INTERSECT`). Add a `COUNT(*)` wrapper around each to get the numbers.
4. Starter skeleton:
   ```sql
   -- CINs only in the practice previous copy
   SELECT CIN FROM RAW_MCA_PREV
   EXCEPT
   SELECT CIN FROM RAW_MCA_ROC1;

   -- TODO: wrap the query above in SELECT COUNT(*) FROM ( ... ) to get the number

   -- CINs only in the current table (the practice rows you deleted from RAW_MCA_PREV)
   SELECT CIN FROM RAW_MCA_ROC1
   EXCEPT
   SELECT CIN FROM RAW_MCA_PREV;

   -- TODO: repeat with INTERSECT for CINs present in both months
   ```
5. Run all three, write the three counts as comments in the file, and note which rows you changed or deleted in week 3. The rows you deleted from `RAW_MCA_PREV` should appear in the reverse comparison, because they are still in the current table. Check that direction in your result.

If you get stuck: read the error text. Check that both queries select the same number of columns. Search "Snowflake EXCEPT INTERSECT". Ask in Discord with the error text.

How to think about it: a snapshot is one month's picture. Comparing two pictures tells you what moved. Do not merge them into one table here.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s4/03_except_intersect.sql`
- [ ] All three queries run
- [ ] The count of CINs only in the practice previous copy is in the file
- [ ] The reverse count (only in current) is in the file
- [ ] You can point at the week 3 rows that caused each result

Commit message: `S4.3 compare snapshots with EXCEPT and INTERSECT`

**Note:** Task 3 (S4.3) is the same comparison the SCD2 logic performs in week 11. Keep the query and the counts.

#### Task 4 - Defend the missing-stays-missing rule (ID `S4.4`)

##### 1. What We Are Doing

Write a short paragraph in plain English that explains why an empty value stays empty. You work in VS Code only. No SQL to run.

##### 2. Why We Are Doing It

The client will ask why some boxes are blank. A written rule keeps the whole team consistent, including in week 12.

##### 3. What to Learn First

Nothing new. You use the "Why missing stays missing" note above and your result from Task 1.

##### 4. Where to Find Resources

- The "Why missing stays missing" callout in this file.
- Your Task 1 result in `students/DEx/week5/sql/s4/01_null_capital.sql`.
- `docs/01-project-brief.md` for the client context.
- Search keywords: NULL vs zero meaning, data quality missing values, data quality rules.

##### 5. Step-by-Step Guide

1. Open `students/DEx/week5/sql/s4/notes.md` in VS Code and add a heading `## Missing stays missing`.
2. Write one paragraph in plain English. Answer these guiding questions, in your own words:
   - What does an empty paid-up capital mean about the company, and what does 0 mean?
   - Which client decision could change if we stored 0 instead?
   - Where in our pipeline could a 0 sneak in by accident (for example a sum, a cast, a spreadsheet export)?
   - How should the dashboard show a missing value?
3. Keep it under 150 words. Short sentences.

If you get stuck: reread the "Why missing stays missing" callout. Use one real row from Task 1 as your example. Ask in Discord and paste your paragraph for feedback.

How to think about it: you are not repeating the rule. You are showing one real client decision that would go wrong.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s4/notes.md`
- [ ] The paragraph is in the notes file
- [ ] It names one concrete client decision
- [ ] It does not just repeat the rule

Commit message: `S4.4 write the missing stays missing rule`

### Group 2 - SQL Functions: String, Date, Math

Foundation link: Kudvenkat Parts 22, 23, 24 (strings), Parts 25, 26, 27 (dates), Part 28 (cast and convert), Part 29 (math). Remember the Snowflake translations: `LENGTH()`, `CURRENT_TIMESTAMP()`, `TRY_TO_DATE`, `DATEDIFF(part, start, end)`.

#### Task 5 - Check CIN length (ID `S5.1`)

##### 1. What We Are Doing

List the CINs whose length is not 21 characters, and count them. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

A CIN is the company's key, exactly 21 characters. A wrong length means a broken join later.

##### 3. What to Learn First

- Kudvenkat Part 22 and Part 23 (string functions). About 25 minutes.
- `LEN()` becomes `LENGTH()` in Snowflake.
- `TRIM` removes spaces at both ends before you count characters.

##### 4. Where to Find Resources

- Kudvenkat Part 22 "Built in string functions" and Part 23 "LEFT, RIGHT, CHARINDEX and SUBSTRING" (about 25 min).
- Search the Snowflake docs for "LENGTH".
- Search keywords: Snowflake LENGTH string, Snowflake TRIM.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week5/sql/s5/01_cin_length_check.sql`.
2. Write one query that lists CINs whose length is not 21, with the trimmed length and the company name. Add a count. The check uses trimmed CINs.
3. Starter skeleton:
   ```sql
   SELECT CIN, <COMPANY_NAME_COL>, LENGTH(TRIM(CIN)) AS cin_length
   FROM RAW_MCA_ROC1
   WHERE LENGTH(TRIM(CIN)) <> 21   -- TODO: also count how many rows this is
   LIMIT 100;
   ```
4. Run it. Decide: is NULL CIN also a problem? Add a second query if you think so, and say why in a comment.

If you get stuck: read the error text. Check the exact column names with `SELECT * ... LIMIT 5`. Search "Snowflake LENGTH string". Ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s5/01_cin_length_check.sql`
- [ ] The query runs
- [ ] You have the count and the list in the file
- [ ] You know whether NULL CIN exists in your table

Commit message: `S5.1 check CIN length is 21 characters`

#### Task 6 - Clean the state column (ID `S5.2`)

##### 1. What We Are Doing

Uppercase and trim the state column, then map the spellings you actually see to one standard name with a `CASE` expression. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

State names come in several spellings. The comparison with MCA CDM state totals only works if each state has one name.

##### 3. What to Learn First

- Kudvenkat Part 24 "Replicate, Space, Patindex, Replace and Stuff". About 12 minutes. For `UPPER`, `TRIM` and `CASE`, search the Snowflake docs for "UPPER", "TRIM" and "CASE".
- `CASE` returns one value per condition. Missing stays missing: map an empty state to NULL, not to a real state name.

##### 4. Where to Find Resources

- Kudvenkat Part 24 "Replicate, Space, Patindex, Replace and Stuff" (about 12 min).
- Search the Snowflake docs for "CASE".
- Search keywords: Snowflake CASE expression, Snowflake UPPER TRIM.

##### 5. Step-by-Step Guide

1. First look at the data: `SELECT <STATE_COL>, COUNT(*) FROM RAW_MCA_ROC1 GROUP BY <STATE_COL> ORDER BY 2 DESC LIMIT 50;`
2. Create `students/DEx/week5/sql/s5/02_clean_states.sql`.
3. Write a `CASE` mapping that uppercases and trims the state, then maps the variants you can see to one standard name. Only map variants that you actually saw in the output.
4. Starter skeleton:
   ```sql
   SELECT
       UPPER(TRIM(<STATE_COL>)) AS state_before,
       CASE UPPER(TRIM(<STATE_COL>))
           /* TODO: add WHEN lines only for variants you saw in step 1.
              Shape: WHEN 'OLD SPELLING' THEN 'STANDARD NAME' */
           WHEN '' THEN NULL          -- missing stays missing
           ELSE UPPER(TRIM(<STATE_COL>))
       END AS state_after,
       COUNT(*) AS companies
   FROM RAW_MCA_ROC1
   GROUP BY 1, 2   -- TODO: check this groups the way you expect
   ORDER BY companies DESC;
   ```
5. Run it. Fix the grouping so the counts add up to your table row count.

If you get stuck: read the error text. Run the step 1 query again and look at the exact spellings. Search "Snowflake CASE expression". Ask in Discord with the error text.

How to think about it: every mapping line is a small claim about the data. Only claim what you saw.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s5/02_clean_states.sql`
- [ ] The query runs
- [ ] Before and after counts are both visible
- [ ] No missing state became a real state name
- [ ] Every mapping has a comment

Commit message: `S5.2 clean state names with CASE mapping`

#### Task 7 - Parse registration dates (ID `S5.3`)

##### 1. What We Are Doing

Look at the date strings, then parse them with `TRY_TO_DATE` so a bad row returns NULL instead of failing the query. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

Registration date drives company age and the "repo rate on the day of incorporation" question. One bad row must not kill the query.

##### 3. What to Learn First

- Kudvenkat Part 25, Part 26, Part 27 (date functions) and Part 28 "Cast and Convert". About 35 minutes.
- In Snowflake, use `TRY_TO_DATE` for safe parsing, not `TO_DATE`.
- If two formats appear, use `COALESCE` over two `TRY_TO_DATE` calls.

##### 4. Where to Find Resources

- Kudvenkat Part 25 "DateTime functions", Part 26 "IsDate, Day, Month, Year, DateName", Part 27 "DatePart, DateAdd and DateDiff", Part 28 "Cast and Convert" (about 35 min).
- Search the Snowflake docs for "TRY_TO_DATE".
- Search keywords: Snowflake TRY_TO_DATE format, Snowflake date format elements.

##### 5. Step-by-Step Guide

1. Look at 20 sample date strings: `SELECT DISTINCT <REG_DATE_COL> FROM RAW_MCA_ROC1 LIMIT 20;` Write down the formats you see.
2. Create `students/DEx/week5/sql/s5/03_parse_dates.sql`.
3. Write a parsed column using `TRY_TO_DATE(<REG_DATE_COL>, '<format>')`. If you see two formats, use `COALESCE` over two `TRY_TO_DATE` calls.
4. Starter skeleton:
   ```sql
   SELECT
       CIN,
       <REG_DATE_COL> AS date_as_text,
       TRY_TO_DATE(<REG_DATE_COL>, 'DD-MM-YYYY') AS date_parsed  -- TODO: second format with COALESCE if needed
   FROM RAW_MCA_ROC1
   LIMIT 100;
   ```
5. Run a count of rows where the text is not NULL but the parsed date is NULL. Put that count and up to five example rows into `students/DEx/week5/sql/s5/notes.md`. If there are no failing rows, write zero.

If you get stuck: read the error text. Compare your format string with the exact text you saw in step 1. Search "Snowflake TRY_TO_DATE format". Ask in Discord with the raw text and the error text.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s5/03_parse_dates.sql`
- [ ] `students/DEx/week5/sql/s5/notes.md`
- [ ] The parse query runs with no error on any row
- [ ] The failure count is in the notes file
- [ ] List up to five failing examples with their raw text; if there are none, record zero.

Commit message: `S5.3 parse registration dates with TRY_TO_DATE`

#### Task 8 - Compute recovery rates (ID `S5.4`)

##### 1. What We Are Doing

Convert the two IBBI amount columns from text to numbers and compute a recovery percentage for each case. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

The client wants to see how much of the admitted claims insolvency cases actually recover. It is a headline number in the dashboard.

##### 3. What to Learn First

- Kudvenkat Part 29 "Mathematical functions" and Part 28 (cast). About 15 minutes.
- Reopen Part 15 if you need `COALESCE` again.
- `TRY_TO_NUMBER` parses text to a number safely. Use an explicit scale, for example `TRY_TO_NUMBER(text_col, 38, 2)`, so decimal amounts are not rounded. Remove commas with `REPLACE` before conversion.
- `NULLIF(x, 0)` stops a divide-by-zero.
- If the source text has other formatting, clean it first or leave the value NULL. Do not guess.

##### 4. Where to Find Resources

- Kudvenkat Part 29 "Mathematical functions" and Part 28 "Cast and Convert" (about 15 min).
- Search the Snowflake docs for "TRY_TO_NUMBER".
- Search keywords: Snowflake TRY_TO_NUMBER, Snowflake NULLIF divide by zero.

##### 5. Step-by-Step Guide

1. Before you compute: open the IBBI PDF and find the realizable amount for each of your 20 to 30 rows. If `RAW_IBBI_SAMPLE` has no realizable amount column, add a text column, for example `REALIZABLE_AMOUNT`. Type each amount into its row. If the PDF shows no amount, leave it NULL. Never type 0.
2. Look at your data first: `SELECT * FROM RAW_IBBI_SAMPLE LIMIT 5;` Note that the amounts are text because you loaded them as VARCHAR.
3. Create `students/DEx/week5/sql/s5/04_recovery_rate.sql`.
4. Convert the two amount columns with `TRY_TO_NUMBER` and the same explicit scale. Compute `realizable / admitted * 100`, rounded to one decimal.
5. Starter skeleton:
   ```sql
   SELECT
       CIN,
       <COMPANY_NAME_COL>,
       TRY_TO_NUMBER(REPLACE(<ADMITTED_CLAIMS_COL>, ',', ''), 38, 2) AS admitted,
       TRY_TO_NUMBER(REPLACE(<REALIZABLE_COL>, ',', ''), 38, 2)     AS realizable,
       /* TODO: write the recovery percentage here */
       ROUND( <YOUR_RECOVERY_EXPRESSION> , 1) AS recovery_pct
   FROM RAW_IBBI_SAMPLE
   WHERE /* TODO: both amounts present, and admitted is not zero */
   ORDER BY recovery_pct ASC
   LIMIT 10;
   ```
6. Run it. Add a comment saying what your query does when a case has no realizable amount.

If you get stuck: read the error text. Check the exact column names in your `RAW_IBBI_SAMPLE` with the `SELECT * ... LIMIT 5` step. Search "Snowflake TRY_TO_NUMBER". Ask in Discord with the error text.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s5/04_recovery_rate.sql`
- [ ] The query runs
- [ ] Only cases with both amounts appear
- [ ] The top ten lowest rates are shown
- [ ] The zero-claim case is handled and explained in a comment

Commit message: `S5.4 compute recovery rate percentage per case`

#### Task 9 - Compute company age (ID `S5.5`)

##### 1. What We Are Doing

Write two age columns, one with plain `DATEDIFF(year, ...)` and one corrected to completed years, then compare them. You write in VS Code first, then run in Snowflake (Snowsight > Projects > Workspaces).

##### 2. Why We Are Doing It

Age in completed years is a client filter ("companies older than 5 years"). An off-by-one year gives a wrong list.

##### 3. What to Learn First

- Kudvenkat Part 27 "DatePart, DateAdd and DateDiff". About 15 minutes. Watch how he explains what `DATEDIFF` counts.
- `DATEDIFF` counts the year boundaries crossed, not the full years lived, so it can be one too high.
- Use `CURRENT_TIMESTAMP()` in Snowflake where he uses `GETDATE()`.

##### 4. Where to Find Resources

- Kudvenkat Part 27 "DatePart, DateAdd and DateDiff" (about 15 min).
- Reopen Part 28 "Cast and Convert" if the date parsing feels unclear.
- Search keywords: Snowflake DATEDIFF completed years, Snowflake CURRENT_TIMESTAMP, Snowflake date parts.

##### 5. Step-by-Step Guide

1. Create `students/DEx/week5/sql/s5/05_company_age.sql`. Use the complete date parsing expression you built and checked in Task 7. Do not hardcode a new date format here.
2. Write two columns: `DATEDIFF(year, ...)` and a corrected age in completed years. Hint: `DATEDIFF` counts the year boundaries crossed, not the full years lived, so it can be one too high.
3. Starter skeleton:
   ```sql
   SELECT
       CIN,
       <REG_DATE_COL> AS date_as_text,
       DATEDIFF(year, /* TODO: copy your complete date parsing expression from Task 7 */, CURRENT_TIMESTAMP()) AS age_diff_years,
       /* TODO: corrected age in completed years, using the same parsed date */
       AS age_completed_years
   FROM RAW_MCA_ROC1
   LIMIT 5;
   ```
4. Run it. Then find a row whose incorporation month and day are later than today's month and day this year, so the anniversary has not yet occurred. If no such row exists in your table, write that in the notes. Write in `students/DEx/week5/sql/s5/notes.md` why the two columns differ, using that row as the example. If you found no such row, say so and explain what that means.

If you get stuck: read the error text. Check the argument order `DATEDIFF(part, start, end)`. Search "Snowflake DATEDIFF completed years". Ask in Discord with your rows.

How to think about it: an age in completed years needs a check on month and day, not just the year number.

##### 6. What to Commit

- [ ] `students/DEx/week5/sql/s5/05_company_age.sql`
- [ ] `students/DEx/week5/sql/s5/notes.md`
- [ ] Both columns run, and you found a row where they differ (or recorded that none exists)
- [ ] The notes explain the difference using a real row whose anniversary has not yet occurred this year, or say that no such row exists
- [ ] You can say which column you would trust in a report

Commit message: `S5.5 company age in completed years`

**Note:** the CASE mapping in Task 6 (S5.2) becomes the shared silver-layer cleaning rule in week 12. Write it so a teammate can read it.

## 6. What to Commit

End of week checklist:

- [ ] Tasks 1-3: SQL files and required counts; Task 4: written NULL rule
- [ ] Tasks 5-9 - five queries, each with its answer written down
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] You can say out loud why bronze columns are VARCHAR and why missing stays missing

**If you are short on time, cut in this order:** Task 8 (S5.4), then Task 9 (S5.5), then Task 2 (S4.2). Never cut Task 4 (S4.4). The written rule cannot be added back later.

> **Team project repo.** Only the team leader copies or syncs this week's reviewed files from the team repo into the class repo `delivery/` or `platform/` folder and opens a pull request. Everyone keeps practice files in `students/DEx/week5/`.

When your tasks are committed, open one pull request to the class repo and do not squash-merge. The steps are in `docs/03-student-guide.md`.

Next: `week6/problem_statement.md`.
