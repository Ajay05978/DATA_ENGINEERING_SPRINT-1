# Week 8 - Big files and cleaner tooling

**Data Sprint 1 · Week 8 of 15 · Theme: read a large CSV one row at a time without loading the whole file, then make code a teammate can debug**

Read this whole file before you start. Then work through the task groups in order.

## 1. What We Are Doing

This week you do two things. First, you write a short design note that explains why the bronze layer is messy on purpose and the gold star schema is clean on purpose. Second, you learn to read a large CSV one row at a time without loading the whole file, and you make your Python code easier for a teammate to debug.

The task groups are:
- Group 1 - Normalization: Task 1 (`S8.1`), Task 2 (`S8.2`)
- Group 2 - Generators and Large Files: Task 3 (`P5.1`), Task 4 (`P5.2`), Task 5 (`P5.3`)
- Group 3 - Logging and Command Line Tools: Task 6 (`P6.1`), Task 7 (`P6.2`)

## 2. Why We Are Doing It

Our client is a fintech due-diligence company. Its analysts check Indian companies: is the company real and active (MCA), was it ever in insolvency (IBBI), how does it compare to its state (MCA CDM), and what was the interest-rate backdrop (RBI). The analysts must trust the numbers. So you must be able to explain, in plain words, why the raw bronze copy is messy on purpose and the gold star schema is clean on purpose.

The MCA registry is huge, about 31 lakh rows across the RoC files. A real due-diligence pipeline must read big files on a normal laptop without crashing. And a run may fail at 2am, so the log is the only thing a teammate has to debug it.

**The thing to watch this week:** the state frequency map in Task 4 (`P5.2`). It becomes the shared silver-layer cleaning rule in week 12. If your spellings are wrong or incomplete, the whole cohort's silver layer inherits it.

## 3. What to Learn First (Prerequisites)

### Before this week: what you must already have

Check these before Task 1. If something is missing, redo the small task that made it, or ask the program lead.

| Week | What you must already have | Where to find it |
|---|---|---|
| Week 2 | Three RoC ZIP files downloaded from data.gov.in and unzipped into `data/raw/mca/`. You know which of the three CSV files is the largest. | `data/raw/mca/` |
| Week 3 | Python tasks P1.1 to P1.3 written in `students/DEx/week3/python/p1/`. | `students/DEx/week3/python/p1/` |
| Week 3 | The practice kit tables in your own Snowflake schema: three `RAW_MCA_<ROC>` tables, `RAW_IBBI_SAMPLE`, and `RAW_MCA_PREV`. | Your own Snowflake schema |
| Week 5 | Your `UNION ALL` practice SQL, so you remember how to stack the three RoC tables. | Your week 5 SQL files |
| Week 7 | Your RBI download script (task P4.2) in `students/DEx/week7/python/p4/`, and a downloaded RBI file in `data/raw/rbi/` with the pull date in the file name. | `students/DEx/week7/python/p4/` and `data/raw/rbi/` |
| Every week | Your repo is cloned, your `.venv` virtual environment exists, and you can activate it. | Your cloned repo and the VS Code terminal |

### By the end of this week you can

- Explain why bronze preserves raw data and gold uses a denormalized star schema
- Read a large CSV one row at a time without loading the whole file
- Produce logs someone else could debug a failed run from

### Core concepts to review first

- **Normalization:** splitting data into tables so each fact is stored once. Bronze keeps raw repeated values; gold uses a star schema (fact table plus dimension tables).
- **Denormalized star schema:** a reporting shape where dimensions are joined to one fact table. This is the Kimball idea you will meet again in week 12.
- **Memory (RAM):** your laptop's fast working space. If a file is bigger than that space, loading the whole file at once stops your program.
- **Generator and `yield`:** a function that gives you one row at a time instead of all rows at once.
- **Logging:** writing messages with a timestamp and a level (INFO, WARNING, ERROR) so a failed run can be debugged later.
- **Command line arguments (`argparse`):** letting a tool take inputs such as an output folder and a date when you run it.

## 4. Where to Find Resources

> **Where you work (same every week)**
> 1. VS Code with your cloned repo is your home base. You write, save and commit files there.
> 2. Snowflake (Snowsight in the browser) is where SQL runs. Write the SQL in VS Code, then open Snowsight > Projects > Workspaces, paste it and run it.
> 3. The VS Code terminal runs your Python and your Git commands.
> 4. In every path, `DEx` is your own number. If you are DE12, then `students/DEx/week8/...` means `students/DE12/week8/...`.
> 5. Data files never go into Git. Only code, SQL, notes and small text files.

> **Team project repo**
> Only the team leader copies or syncs this week's reviewed files from the team repo into the class repository's `delivery/` or `platform/` folder. Everyone keeps practice files in `students/DEx/week8/`.

### Docs to open

Git commands: `docs/03-student-guide.md`. Client story and sources: `docs/01-project-brief.md`. Unknown word: `docs/08-glossary.md`. Broken tool: `docs/10-troubleshooting.md`. What a tool is for: `docs/11-tools-and-technology.md`. Each task names the exact file path to commit to, inside this same week folder.

| If you need... | Open |
|---|---|
| The Git commit and pull request loop | `docs/03-student-guide.md` |
| Client story and data sources | `docs/01-project-brief.md` |
| The meaning of a word | `docs/08-glossary.md` |
| A fix for a broken tool | `docs/10-troubleshooting.md` |
| What a tool is for | `docs/11-tools-and-technology.md` |

### Your week in order

| Step | LEARN (with time estimate) | DO |
|---|---|---|
| **1** | Kudvenkat Part 52, Part 53, Part 54 (about 30 min) | Tasks 1-2 - normalization write-up, pivot |
| **2** | Corey video #36 "Generators - How to use them and the benefits you receive" (about 20 min) | Tasks 3-5 - generator, frequency map, memory explanation |
| **3** | Corey video #52 "Logging Basics - Logging to Files, Setting Levels, and Formatting" (about 15 min) | Task 6 - logging |
| **4** | Python docs `argparse` tutorial, Corey video #56 "if \_\_name\_\_ == '\_\_main\_\_'" repeated (about 25 min) | Task 7 - CLI tool |
| **5** | Nothing new | Cohort review: state spelling reconciliation, then the checklist |

### Search keywords if you get stuck

- `python generator yield large file`
- `python logging basicConfig format`
- `argparse add_argument required`
- `Snowflake PIVOT`
- `python csv reader newline encoding`
- `MemoryError python read large csv`

## 5. Step-by-Step Guide

How your tasks are numbered. The ID tells you the topic: B = business and delivery, S = SQL and data modeling, P = Python, D = data platform, A = architecture diagrams. The ID goes in your commit message and your pull request title, for example `P5.2 state frequency map`. The words "Task 1", "Task 2" only show the order inside this week.

How to work: one task, one file, one commit. Then a pull request to the class repo. Never squash-merge. The full Git loop is in `docs/03-student-guide.md`.

### Group 1 - Normalization

Foundation link: Kudvenkat Part 52 (database normalization), Part 53 (second and third normal form), Part 54 (pivot).

#### Task 1 - Explain the layer choices (ID `S8.1`)

##### 1. What We Are Doing
You write a short Markdown notes file with three paragraphs. Each paragraph explains one design choice about the bronze and gold layers. You work in VS Code.

##### 2. Why We Are Doing It
Our client pays analysts to check Indian companies. Analysts must trust the numbers, so you must be able to explain why the raw bronze copy is messy on purpose and the gold star schema is clean on purpose.

##### 3. What to Learn First
- Kudvenkat Part 52 "Database normalization" and Part 53 "Second and third normal form", about 20 min in total.
- You do not need to run the SQL in these two videos. Watch for the idea, then write in your own words. Skip the SQL Server screens.
- Plain words: normalization means storing each fact once, in one place.

##### 4. Where to Find Resources
- Kudvenkat playlist: https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB - Part 52, Part 53 (about 20 min).
- Search keywords: `database normalization simple explanation`, `star schema fact dimension`, `bronze silver gold data layer`.
- Glossary for words: `docs/08-glossary.md`.

##### 5. Step-by-Step Guide
1. In VS Code, open your week folder `students/DEx/week8/`.
2. Right-click the folder > New Folder > type `design`.
3. Right-click `design` > New File > type `normalization_notes.md`.
4. Write three short paragraphs, one per heading below. Use plain words a client would accept.
   ```markdown
   ## Bronze is not normalized
   TODO: what a raw copy of the MCA data looks like, and which problems we accept for now.

   ## Gold is a denormalized star
   TODO: why the fact and dimension tables are shaped this way for reporting.

   ## Which problem each choice solves
   TODO: one problem for bronze, one problem for gold.
   ```
5. Keep each paragraph to 3-5 sentences. No copied text.
6. Answer the question "which problem does each choice solve?" in your own words, without copying the video.

If you get stuck: read one paragraph out loud and check it names a layer (bronze or gold) and a problem. Search `star schema simple example`. Ask in Discord and paste the sentence that feels wrong.

How to think about it: bronze is a copy of the source, so it keeps the mess. Gold is built for analysts to report on, so it is shaped cleanly. You are explaining the trade-off, not writing SQL.

##### 6. What to Commit
- [ ] `students/DEx/week8/design/normalization_notes.md`
- [ ] Done when: three paragraphs exist, each names its layer, and each states the problem it solves.
- [ ] Commit is pushed and a pull request is open (see `docs/03-student-guide.md`).
- [ ] Commit message: `S8.1 explain bronze vs gold normalization`

#### Task 2 - Pivot company counts by state (ID `S8.2`)

##### 1. What We Are Doing
You write one SQL query that shows one row per state, with a column for each company status. You write the `.sql` file in VS Code and run it in Snowflake (Snowsight Workspaces).

##### 2. Why We Are Doing It
Analysts compare a company to its state. This query turns long rows into one row per state with a column per status, which is the shape a dashboard needs.

##### 3. What to Learn First
- Kudvenkat Part 54 "Pivot", about 15 min. Skip the SQL Server syntax details.
- Snowflake has its own `PIVOT` keyword too. Search the Snowflake docs for "pivot" if you want to try it. The CASE-based way below is enough to start.
- `COUNT(CASE WHEN ... THEN 1 END)` counts only the rows that match the condition.

##### 4. Where to Find Resources
- Kudvenkat playlist: https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB - Part 54 (about 15 min).
- Snowflake docs: https://docs.snowflake.com - search for "pivot".
- Search keywords: `sql count case when pivot`, `Snowflake PIVOT example`, `group by state count status`.
- Reminder of your practice kit tables: `students/DEx/week3/` and `docs/04-week-map.md`.

##### 5. Step-by-Step Guide
1. In VS Code, right-click `students/DEx/week8/` > New Folder > `sql`.
2. Right-click `students/DEx/week8/sql/` > New Folder > `s8`.
3. Right-click `s8` > New File > `01_pivot_states.sql`.
4. First check the actual status spellings. Run this in Snowsight and look at the result:
   ```sql
   SELECT DISTINCT status FROM RAW_MCA_<ROC1>;
   ```
5. Start from this skeleton and fill the TODOs. Replace `<ROC1>`, `<ROC2>`, `<ROC3>` with your three table names from week 3:
   ```sql
   WITH all_roc AS (
       SELECT state, status FROM RAW_MCA_<ROC1>
       UNION ALL
       SELECT state, status FROM RAW_MCA_<ROC2>
       UNION ALL
       SELECT state, status FROM RAW_MCA_<ROC3>
   )
   SELECT state,
          -- TODO: COUNT with CASE for the Active status
          -- TODO: COUNT with CASE for the Strike Off status
          -- TODO: COUNT with CASE for the Under Liquidation status
   FROM all_roc
   GROUP BY state
   ORDER BY 2 DESC;  -- TODO: sort by the column you want to inspect
   ```
6. Paste it into a Snowsight Workspace and run it. Fix errors until it runs. In Snowsight set role `STUDENT_ROLE`, warehouse `STUDENT_WH`, the database the program lead gave you, and your own schema.
7. Copy the final working SQL back into your `.sql` file in VS Code. Answer the question "which state has the highest Strike Off column?" in your commit note or in the pull request comment.

If you get stuck: read the error message line number and look at that line. Run `SELECT DISTINCT status FROM RAW_MCA_<ROC1>;` again and copy the exact spellings into your CASE lines. Search `sql count case when`. Ask in Discord with the full error text.

##### 6. What to Commit
- [ ] `students/DEx/week8/sql/s8/01_pivot_states.sql`
- [ ] Done when: the query runs in Snowflake, returns one row per state, and shows three status columns.
- [ ] Done when: you named the top Strike Off state (in the commit note or the pull request comment).
- [ ] Commit message: `S8.2 pivot company counts by state`

### Group 2 - Generators and Large Files

"Memory" here means your laptop's RAM, the fast working space. When a file is bigger than that space, loading the whole file at once stops your program.

#### Task 3 - Write a row generator (ID `P5.1`)

##### 1. What We Are Doing
You write a Python function that gives you one row at a time from a big CSV file, and you count rows and empty capital values in one pass. You write the file in VS Code and run it in the VS Code terminal.

##### 2. Why We Are Doing It
The MCA registry is huge. Real due-diligence pipelines must read big files without crashing on a normal laptop.

##### 3. What to Learn First
- Corey video #36 "Generators - How to use them and the benefits you receive", about 20 min.
- Watch the idea of `yield`. Skip the generator-expression tricks at the end if they confuse you. You only need a simple `yield` loop today.
- `csv.reader` gives you one row per call, so you never hold the whole file.

##### 4. Where to Find Resources
- Corey Schafer playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU - Corey video #36 "Generators - How to use them and the benefits you receive" (about 20 min).
- `csv` module docs: https://docs.python.org/3/library/csv.html
- Optional later read: Real Python article "Introduction to Python Generators" at https://realpython.com
- Search keywords: `python yield generator csv`, `python read large csv memory`, `csv reader newline=""`.

##### 5. Step-by-Step Guide
1. Create the folder `students/DEx/week8/python/p5/`.
2. Create `row_generator.py` with this skeleton. Use the largest RoC CSV you downloaded in week 2.
   Read the header once with `next(rows)`, identify the capital column from the MCA data dictionary, and exclude the header from the counting loop:
   ```python
   import csv
   from pathlib import Path

   FILE = Path("data/raw/mca/<your_largest_file>.csv")

   def iter_rows(path):
       """Yield one row at a time. Never load the whole file."""
       # TODO: open the file with newline="" and encoding="utf-8", errors="replace"
       # TODO: make a csv.reader
       # TODO: yield each row
       ...

   rows = iter_rows(FILE)
   header = next(rows)  # read the header once
   # TODO: find the capital column index from header, using the MCA data dictionary
   # capital_idx = header.index("<capital column name from the data dictionary>")

   total = 0
   empty_capital = 0
   for row in rows:
       # TODO: total = total + 1
       # TODO: count rows where row[capital_idx] is empty
       ...

   print("rows:", total, "empty capital:", empty_capital)
   ```
3. Activate your virtual environment in the VS Code terminal (`source .venv/bin/activate` on macOS/Linux, `.venv\Scripts\activate` on Windows).
4. From the repo root run:
   ```
   python students/DEx/week8/python/p5/row_generator.py
   ```
5. Write the two counts, the file name and the run date into `students/DEx/week8/python/p5/notes.md`.

If you get stuck: check the file name in `FILE` matches the real file in `data/raw/mca/`. Print the header (`print(header)`) to see the column names. Read the error message for the line number. Search `python csv reader`. Ask in Discord with the error text.

##### 6. What to Commit
- [ ] `students/DEx/week8/python/p5/row_generator.py`
- [ ] `students/DEx/week8/python/p5/notes.md` (the two counts, the file name, the run date)
- [ ] Done when: the script prints a row count and an empty-capital count in one pass.
- [ ] Done when: it uses `yield` and does not call `readlines()` or `list(csv.reader(...))`.
- [ ] Commit message: `P5.1 stream large RoC file with a generator`

#### Task 4 - Build the state frequency map (ID `P5.2`)

##### 1. What We Are Doing
You count how many times each state spelling appears in the MCA file, and you print the top twenty spellings. You write the file in VS Code and run it in the VS Code terminal.

##### 2. Why We Are Doing It
State names in the MCA data are written several ways. This map is the evidence for the cleaning rules the whole cohort uses in week 12.

##### 3. What to Learn First
- Nothing new. You use what you learned in Task 3.
- `collections.Counter` is a dict that counts things for you.
- `repr()` shows a string with quotes, so trailing spaces become visible.

##### 4. Where to Find Resources
- Your own Task 3 file: `students/DEx/week8/python/p5/row_generator.py`
- Python docs for `csv`: https://docs.python.org/3/library/csv.html
- Search keywords: `python Counter most_common`, `python repr string spaces`, `write csv python`.
- Data rules: `data/raw/mca/` is ignored by Git, so output there is never committed.

##### 5. Step-by-Step Guide
1. Create `students/DEx/week8/python/p5/state_frequency.py`.
2. Before you import, go back to `row_generator.py` and put Task 3's counting and output code inside a `main()` function. Call it under `if __name__ == "__main__":`. This stops the counting loop from running when you import `iter_rows`.
3. At the top of `state_frequency.py`, define `FILE` with the same largest RoC CSV path you used in Task 3. Add `from pathlib import Path` if needed:
   ```python
   from pathlib import Path
   FILE = Path("data/raw/mca/<your_largest_file>.csv")
   ```
4. Import your `iter_rows` from `row_generator.py`, or copy the function into this file.
5. Read the header once, identify the state column from the MCA data dictionary, and exclude the header from the counting loop. Build a Python `dict` where the key is the state spelling exactly as it appears in the file, and the value is the row count. Skeleton:
   ```python
   from collections import Counter
   # TODO: import iter_rows (or paste it)

   rows = iter_rows(FILE)
   header = next(rows)  # read the header once
   # TODO: find the state column index from header, using the MCA data dictionary
   # state_idx = header.index("<state column name from the data dictionary>")

   counts = Counter()
   for row in rows:
       # TODO: read row[state_idx] and add 1 to that spelling
       ...

   for spelling, n in counts.most_common(20):
       print(repr(spelling), n)
   ```
6. Print the top twenty entries with `repr()` so trailing spaces are visible.
7. Also write the full map to a local CSV at `data/raw/mca/state_frequency.csv`. That folder is ignored by Git, so the output is not committed.
8. Copy the top twenty lines into `students/DEx/week8/python/p5/notes.md` in a small table: spelling, count, and your guess at the clean state name.
9. Run from the repo root: `python students/DEx/week8/python/p5/state_frequency.py`

If you get stuck: check your import line, or paste `iter_rows` into the file to remove the import problem. Look at the raw file once in VS Code to see which column number is the state. Search `python Counter most_common`. Ask in Discord with the first five lines of your output.

##### 6. What to Commit
- [ ] `students/DEx/week8/python/p5/state_frequency.py`
- [ ] The top twenty table in `students/DEx/week8/python/p5/notes.md`
- [ ] Done when: the script lists the twenty most frequent spellings, or all spellings if fewer than twenty exist.
- [ ] Done when: the top spellings are in the notes, and the CSV output sits outside Git at `data/raw/mca/state_frequency.csv`.
- [ ] Commit message: `P5.2 state spelling frequency map`

#### Task 5 - Explain the memory win (ID `P5.3`)

##### 1. What We Are Doing
You add three sentences to your notes file that explain how the generator and your week 3 script use memory differently as a CSV file grows. You work in VS Code.

##### 2. Why We Are Doing It
A client or a reviewer will ask why one script handles a large file and another does not. You need a short, correct answer.

##### 3. What to Learn First
- Real Python article "Introduction to Python Generators". Read only the part that explains lazy evaluation and memory, about 10 min.
- Plain words: lazy evaluation means you do the work only when the next row is asked for.

##### 4. Where to Find Resources
- Real Python: https://realpython.com - search for "Introduction to Python Generators".
- Your own files: `students/DEx/week3/python/p1/` (week 3 script) and `students/DEx/week8/python/p5/row_generator.py`.
- Search keywords: `python generator lazy evaluation memory`, `python list vs generator memory`.
- Glossary: `docs/08-glossary.md`.

##### 5. Step-by-Step Guide
1. Open `students/DEx/week8/python/p5/notes.md`.
2. Add a heading `## Why the generator handles a big file`.
3. Write exactly three sentences. Answer these questions in your own words:
   - What does your week 3 script keep in memory while it works?
   - What does the generator keep in memory instead?
   - What happens to each script as the file grows?

If you get stuck: read your Task 3 code again and look at what is stored in a variable. Search `python generator lazy evaluation memory`. Ask in Discord and paste one sentence you are unsure about.

How to think about it: Compare the variables and file-reading loops in your two scripts. Explain how their memory use changes as the file grows.

##### 6. What to Commit
- [ ] The three sentences in `students/DEx/week8/python/p5/notes.md`
- [ ] Done when: there are three sentences, they mention memory at least once each, and they compare the P1 approach with the generator.
- [ ] Commit message: `P5.3 explain generator memory win`

> **If your machine freezes or you see `MemoryError`,** you are still loading the whole file. That is the exact failure this group exists to fix. See `docs/10-troubleshooting.md`, Python section.

### Group 3 - Logging and Command Line Tools

#### Task 6 - Replace prints with logging (ID `P6.1`)

##### 1. What We Are Doing
You add logging setup to your week 7 RBI script and to your week 8 Python scripts, and you replace every `print()` with a log line that has a timestamp and a level. You edit in VS Code, then run in the VS Code terminal.

##### 2. Why We Are Doing It
A due-diligence run may fail at 2am. The log is the only thing a teammate has. Print output with no timestamp and no level cannot be debugged.

##### 3. What to Learn First
- Corey video #52 "Logging Basics - Logging to Files, Setting Levels, and Formatting", about 15 min.
- Python `logging` HOWTO: read the first half and stop at "Logging from multiple modules".
- Levels you need: `logging.info` for progress, `logging.warning` for a skipped row, `logging.error` for a failure.

##### 4. Where to Find Resources
- Corey Schafer playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU - Corey video #52 "Logging Basics - Logging to Files, Setting Levels, and Formatting" (about 15 min).
- https://docs.python.org/3/howto/logging.html (first half only)
- https://docs.python.org/3/library/logging.html
- Optional: Corey video #53 "Logging Advanced - Loggers, Handlers, and Formatters".
- Search keywords: `python logging basicConfig format`, `python logging levels`, `logging warning vs error`.
- Troubleshooting: `docs/10-troubleshooting.md`

##### 5. Step-by-Step Guide
1. Open your week 7 RBI script in `students/DEx/week7/python/p4/` (the file you wrote for task P4.2; use the exact name you gave it).
2. Open `students/DEx/week8/python/p5/row_generator.py` and `state_frequency.py`.
3. In each file, add this near the top:
   ```python
   import logging

   logging.basicConfig(
       level=logging.INFO,
       format="%(asctime)s %(levelname)s %(message)s",
   )
   ```
4. Replace each `print(...)`:
   - `logging.info(...)` for progress, for example the file name and the row count reached.
   - `logging.warning(...)` for a row you skipped, and say why.
   - `logging.error(...)` for a failure.
5. Bad example: `logging.error("Error")`. Good example: `logging.warning("row %s skipped, CIN length %s, file %s", n, len(cin), path.name)`.
6. Run each script from the repo root, for example `python students/DEx/week8/python/p5/row_generator.py`, and check the timestamps appear.

If you get stuck: check the `logging.basicConfig(...)` block is before the first log call. If no lines appear, check the level is `logging.INFO`. Read the traceback line number. Search `python logging basicConfig`. Ask in Discord with the output text.

How to think about it: the test of a log line is whether a teammate could find the failing row from it at 2am without your help. "Error" is not a log line. "WARNING row 41822 skipped, CIN length 19, file ROC_KERALA.csv" is.

##### 6. What to Commit
- [ ] Your updated week 7 RBI script in `students/DEx/week7/python/p4/`
- [ ] `students/DEx/week8/python/p5/row_generator.py`
- [ ] `students/DEx/week8/python/p5/state_frequency.py`
- [ ] Done when: no `print()` is left in those scripts.
- [ ] Done when: every log line has a timestamp and a level, and skip messages name the row and the reason.
- [ ] Done when: the week 7 pull request is referenced in your commit message.
- [ ] Commit message: `P6.1 replace prints with logging in P4 and P5 scripts; refs #<week7_PR_number>`

Replace `<week7_PR_number>` with the number of your week 7 pull request.

#### Task 7 - Make the RBI download a CLI tool (ID `P6.2`)

##### 1. What We Are Doing
You wrap your week 7 RBI download in a small command line tool. It takes an output folder and a pull date, checks the date format, and saves the file with the date in its name. You write the file in VS Code and run it in the VS Code terminal.

##### 2. Why We Are Doing It
A tool that takes an output folder and a date can be scheduled later, and the pull date ends up in the file name for the whole team to see.

##### 3. What to Learn First
- The Python `argparse` tutorial, about 20 min. Read up to "The add_argument() method".
- There is no Corey video for argparse in our list.
- `date.fromisoformat` turns `"YYYY-MM-DD"` into a date, or raises an error if the text is wrong.
- `if __name__ == "__main__":` is what makes a file run as a tool.

##### 4. Where to Find Resources
- `argparse`: https://docs.python.org/3/library/argparse.html (to "The add_argument() method", about 20 min)
- Corey Schafer playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU - repeat Corey video #56 "Python Tutorial: if \_\_name\_\_ == '\_\_main\_\_'" (about 8 min).
- RBI portal DBIE: https://data.rbi.org.in
- RBI press releases: https://www.rbi.org.in
- Search keywords: `python argparse add_argument required`, `python date fromisoformat`, `python argparse help message`, `python create folder pathlib`.
- Troubleshooting: `docs/10-troubleshooting.md`

##### 5. Step-by-Step Guide
1. Create the folder `students/DEx/week8/python/p6/`.
2. Before you build the CLI, open your week 7 RBI script and move the download or local-file handling code into a callable function, for example `def download_rbi(out_dir, pull_date):`. It must return the saved path or the local file path. Then import that function into `rbi_cli.py`, or copy it into the file.
3. Create `rbi_cli.py`. Skeleton:
   ```python
   import argparse
   from datetime import date
   from pathlib import Path

   def parse_args():
       parser = argparse.ArgumentParser(description="Download RBI policy rate data")
       parser.add_argument("--out", required=True, help="folder to save the file, e.g. data/raw/rbi")
       parser.add_argument("--date", required=True, help="pull date, YYYY-MM-DD")
       return parser.parse_args()

   def main():
       args = parse_args()
       out_dir = Path(args.out)
       # TODO: check the date format with date.fromisoformat, give a clear message if it is wrong
       # TODO: create the folder if it does not exist
       # TODO: call your week 7 download function, or your local-file copy function
       # TODO: build the file name with the pull date inside it
       # TODO: log an INFO line with the saved path

   if __name__ == "__main__":
       main()
   ```
4. Remember from week 7: the RBI portal DBIE at https://data.rbi.org.in is a JavaScript site, so a script may not be able to download the file. If there is no direct link, keep the download by hand into `data/raw/rbi/` and let the CLI read or copy that local file. In that case, select the existing local RBI file explicitly, copy it to the requested output folder with the pull date in its name, and record the source link, pull date and last data date in `students/DEx/week8/python/p6/notes.md`. The RBI data we have ends in 2024, and that is fine. The project uses RBI as background only.
5. Add a helpful message for bad input, for example a wrong date format.
6. Run it from the repo root:
   ```
   python students/DEx/week8/python/p6/rbi_cli.py --out data/raw/rbi --date 2026-10-15
   ```
7. Put the example run and the sample output in `students/DEx/week8/python/p6/notes.md`.

If you get stuck: run the tool with `--help` first and check both arguments appear. If the date check fails, print the raw string you passed. Read the error message and the line number. Search `python argparse add_argument required`. Ask in Discord.

How to think about it: a good tool takes its inputs from the command line, not from code you edit each time. The date is an input, so it goes in the file name.

##### 6. What to Commit
- [ ] `students/DEx/week8/python/p6/rbi_cli.py`
- [ ] `students/DEx/week8/python/p6/notes.md` (the example run and sample output)
- [ ] Done when: `--help` prints both arguments.
- [ ] Done when: a wrong date gives a clear message, and a correct run saves a file with the date in the name.
- [ ] Commit message: `P6.2 RBI download as a CLI tool with argparse`

## 6. What to Commit

End of week checklist:

- [ ] Tasks 1-2 - the three-paragraph normalization write-up and the pivot with its answer
- [ ] Tasks 3-5 - generator, state frequency map with top twenty, memory explanation
- [ ] Tasks 6-7 - logging with timestamps and levels, working CLI with example invocations
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] No CSV, PDF, or ZIP anywhere in your commits

**If you are short on time, cut in this order:** Task 2 (S8.2), then Task 7 (P6.2). Never cut Task 4 (P5.2). The frequency map feeds week 12's silver cleaning rules.

When your week is finished, check that each task's commit is pushed and its pull request is open, with the task ID in the title. Never squash-merge.

Next: `week9/problem_statement.md`.
