# Week 3 - Python reads files; trust the data

**Data Sprint 1 · Week 3 of 15 · Theme: read real files in Python, then check the data before you trust it**

Read this whole file before you start anything. Then work through the steps in order. Every task is small: do one task, save one file, make one commit, then start the next task.

## 1. What We Are Doing

This week you write your first real Python. You open a file, count its rows, check its headers, and write a filtered copy of a CSV. You also load your practice data into Snowflake with the web wizard, then check the data for empty and duplicate CINs before anyone reports a number from it.

The steps and task groups this week:

- **Warm-up - your first files** (no task ID, not graded)
- **Group 1 - Python Basics: Files and Functions**: Tasks 1 to 3 (`P1.1`, `P1.2`, `P1.3`)
- **Step 0 - Build your practice data kit** (no task ID, not graded)
- **Group 2 - Data Integrity: Constraints**: Tasks 4 to 6 (`S2.1`, `S2.2`, `S2.3`)

## 2. Why We Are Doing It

The client is a fintech company that does due diligence on Indian companies. Its analysts check four things: is the company real and active (MCA), was it ever in insolvency (IBBI), how does it compare with its state (MCA CDM), and what was the interest-rate backdrop (RBI). Over 15 weeks we build one Snowflake data platform (bronze, silver, gold) that answers those questions, and it ends in a Metabase dashboard.

This week you stop trusting files and start checking them. Before we report "how many companies are active", we must know how many rows exist, whether the two files have the same columns, and whether the same company appears twice. A wrong count in a due-diligence report can cost the client real money.

There is no milestone this week. The first milestone (Discovery Brief) is done, and the next one is **First Snowflake Load** in week 6. The practice load you do this week is a warm-up for that milestone, not a replacement for it.

This is new for most people. Go slowly. One task at a time.

## 3. What to Learn First (Prerequisites)

**Before this week: what you must already have**

| You need this | It came from | If it is missing |
|---|---|---|
| Your cloned repo with your own folder `students/DEx/week3/` | Week 1 setup | See `docs/03-student-guide.md`, then clone again |
| The Python warm-up scripts `P0.1` to `P0.4` (in your week 2 folder) | Week 2 | Redo those small scripts first. This week assumes you have run a Python file before |
| Three RoC ZIP files downloaded and unzipped into `data/raw/mca/`, with the download date noted in `data/raw/mca/download_notes.txt` | Week 2, the step "Download your practice data" | Open `students/DEx/week2/problem_statement.md` and do that step now |
| Your Snowflake database with the `BRONZE`, `SILVER` and `GOLD` schemas and your raw MCA table (`S1.1` to `S1.3`) | Week 2 | Redo `S1.1` to `S1.3`. This week's SQL uses that database |
| A working `.venv` | Setup, `docs/02-tools-setup.md` | Redo the setup steps |
| Snowflake login: role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your database, your own schema | Given by the program lead | Ask the program lead. Do not guess names |
| The Snowflake extension for VS Code, or a Workspace in Snowsight | Week 1 setup | Open https://docs.snowflake.com/en/user-guide/vscode-ext and follow the page, or use a Workspace in Snowsight |

**By the end of this week you can**

- Read a CSV in Python with functions that each do one job
- Count rows and compare headers, so a broken download is caught early
- Write a new CSV from a filtered set of rows
- Load files into Snowflake with the web wizard, and say what that wizard load cannot do
- Find empty and duplicate CINs, and write down what the team will do about them

**Core concepts to review first**

- `open()` with the `with` block, and the `csv` module.
- Functions that return a value, and `if __name__ == "__main__"`.
- `sys.argv`: how you pass a file name to a script.
- Snowflake: what a schema is, and loading a file with the web wizard. All columns stay `VARCHAR` at this stage.
- `NULL` and empty text (`''`) are not the same thing.
- `GROUP BY` with `HAVING`, to keep only the groups that appear more than once.

## 4. Where to Find Resources

**Where you work**

> - **VS Code** (your cloned repo): you write files here, save them here, and commit here.
> - **VS Code terminal**: you run Git and Python commands here. Activate `.venv` first.
> - **Snowflake (Snowsight, in the browser)**: you run SQL here. The data lives in Snowflake, not on your laptop. The SQL editor is a **Workspace** (Snowsight > Projects > Workspaces).
> - **VS Code with the Snowflake extension**: you can also run a saved `.sql` file from VS Code. Open or create a "Snowflake SQL file", then run it from the Command Palette with `Snowflake: Execute All Statements`.
> - The loop is: write the `.sql` file in VS Code, run it in a Workspace or with the extension, then save the final working version back in VS Code.
> - In every path below, `DEx` means your own number. If you are DE12, then `students/DEx/week3/` means `students/DE12/week3/`.
> - At your first login Snowflake asks for a second sign-in step (MFA). Set it up with your authenticator app. Never put a password in a file.

**Docs to keep open**

| If you need... | Read this |
|---|---|
| Setup steps and Git commands | `docs/03-student-guide.md` |
| Client story and the four sources | `docs/01-project-brief.md` |
| A word you do not know | `docs/08-glossary.md` |
| A broken tool | `docs/10-troubleshooting.md` |
| What a tool is for and why we use it | `docs/11-tools-and-technology.md` |
| Videos and playlists for this week | `docs/09-resources.md` |
| Where this week sits in the course | `docs/04-week-map.md` |

**Your week in order**

| Step | Learn first (with time) | Then do |
|---|---|---|
| **1** | Corey video #25 "Python Tutorial: File Objects - Reading and Writing to Files", Corey video #28 "Python Tutorial: CSV Module - How to Read, Parse, and Write CSV Files", Corey video #127 "Python Tutorial: Zip Files - Creating and Extracting Zip Archives" (about 1 hour together) | The warm-up, then Tasks 1, 2 and 3 |
| **2** | In the Snowflake docs at https://docs.snowflake.com, search for "load data using the web interface" and read that page (about 15 min) | Step 0 practice data kit |
| **3** | Kudvenkat Part 9 "Unique key constraint" (about 10 min), Part 11 "Group by" (about 12 min), Part 15 "Different ways to replace NULL" (about 10 min), Part 16 "Coalesce function" (about 10 min) | Tasks 4, 5 and 6 |

Kudvenkat teaches Microsoft SQL Server. We use Snowflake. Watch for the idea, type the Snowflake version, and run it in Snowflake. The difference that matters this week: Snowflake declares a `UNIQUE` key but does not enforce it, and `NOT NULL` is enforced. The full list is in `docs/09-resources.md`.

**Search keywords if you get stuck this week**

- "python csv reader count rows"
- "python csv read header row"
- "Snowflake load data using the web interface"
- "Snowflake GROUP BY HAVING count duplicates"
- "SQL find duplicate CIN"
- "python csv writer example"

## 5. Step-by-Step Guide

How to work: one task, one file, one commit. Finish a task, save the file, commit it, then start the next task. The full Git steps (branch, add, commit, push, pull request) are in `docs/03-student-guide.md` section 4. Never squash-merge.

### Warm-up - Read your first file (no task ID, not graded)

##### 1. What We Are Doing
You create a tiny text file yourself and print it with Python. Then you create a 3-row CSV yourself and print it row by row. You work in VS Code, and you run the scripts in the VS Code terminal. Only after this works do you touch the real RoC files.

##### 2. Why We Are Doing It
The RoC files are large and messy. A file you made yourself lets you see exactly what the code does, before a government file confuses you. This is the same trick the team uses before every real load.

##### 3. What to Learn First
- `open()` and the `with` block.
- `csv.reader` gives you one row at a time, as a list of text values.
- The first row of a CSV is usually the header, not data.

##### 4. Where to Find Resources
- Corey video #25: "Python Tutorial: File Objects - Reading and Writing to Files"
- Corey video #28: "Python Tutorial: CSV Module - How to Read, Parse, and Write CSV Files"
- Corey video #127: "Python Tutorial: Zip Files - Creating and Extracting Zip Archives" (for the RoC ZIP files)
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU
- Search keywords: "python open file read", "python csv reader for loop", "python with open newline".

##### 5. Step-by-Step Guide
1. In VS Code, create the folder `data/raw/practice/` at the repo root. It sits inside `data/raw/`, which `.gitignore` already excludes, so nothing here is ever committed.
2. Create `data/raw/practice/hello.txt` and type three lines of any text you like. Save it.
3. Create `students/DEx/week3/python/warmup/first_read.py` and type this:

   ```python
   with open("data/raw/practice/hello.txt", encoding="utf-8") as f:
       text = f.read()
   print(text)
   ```

4. Run it from the repo root, with `.venv` active: `python students/DEx/week3/python/warmup/first_read.py`
5. Create `data/raw/practice/tiny.csv`. Type a header line and three rows yourself, for example `name,city` and three made-up rows. Do not use real company data.
6. Create `students/DEx/week3/python/warmup/first_csv.py` and type this:

   ```python
   import csv

   with open("data/raw/practice/tiny.csv", newline="", encoding="utf-8") as f:
       reader = csv.reader(f)
       for row in reader:
           print(row)
   ```

7. Run it: `python students/DEx/week3/python/warmup/first_csv.py`. You should see one printed list per line of the file, the header first.
8. Only now move to the real RoC files. If a ZIP is still zipped, unzip it first (Corey video #127), then keep the CSV in `data/raw/mca/`.

**How to think about it:** read the output line by line. Each printed list is one row of your file. The header is row number one, so it is not a data row.

If you get stuck:
- `FileNotFoundError` means the path is wrong. Run the script from the repo root, not from inside a subfolder.
- Print `row` inside the loop and look at the shape: a list of text values, one per column.
- Search "python csv reader for loop", or ask in Discord with the error text.

##### 6. What to Commit
- [ ] `students/DEx/week3/python/warmup/first_read.py` (replace `DEx` with your number)
- [ ] `students/DEx/week3/python/warmup/first_csv.py`
- [ ] Done when: both scripts run and print your own text and your own rows
- [ ] Done when: the practice files stay in `data/raw/practice/` and are not committed

Commit message: `warm-up read a text file and a small CSV`

### Group 1 - Python Basics: Files and Functions

Run every Python command from the repo root, with your virtual environment active: `source .venv/bin/activate` on macOS or Linux, `.venv\Scripts\activate` on Windows.

#### Task 1 - Count rows in a CSV (ID `P1.1`)

##### 1. What We Are Doing
You write a small Python script that opens a CSV file and counts its data rows. You write the script in VS Code and run it in the VS Code terminal.

##### 2. Why We Are Doing It
Before we trust any file, we count it. Row counts are how you catch a broken download or a half-loaded file later in the project.

##### 3. What to Learn First
- Reading a file in Python with `open()`.
- The `csv` module and `csv.reader`.
- Functions, and the difference between the header row and the data rows.
- `sys.argv`, to pass a file name to the script.

##### 4. Where to Find Resources
- Corey video #1: "Python Tutorial for Beginners 1: Install and Setup for Mac and Windows" (skip the install if it is already done)
- Corey video #5: "Python Tutorial for Beginners 5: Dictionaries - Working with Key-Value Pairs"
- Corey video #8: "Python Tutorial for Beginners 8: Functions"
- Corey video #9: "Python Tutorial for Beginners 9: Import Modules and Exploring The Standard Library"
- Corey video #105 "VENV (Mac & Linux)" and #106 "VENV (Windows)" for the virtual environment. Use these two, not the older #14 video.
- Corey video #25 "File Objects - Reading and Writing to Files" and #28 "CSV Module - How to Read, Parse, and Write CSV Files" (watch again if you need to)
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU. About 2 hours. Skip the rest of the playlist for now.
- Search keywords: "python csv reader", "python count rows csv", "python sys.argv example", "python open file newline".

##### 5. Step-by-Step Guide
1. In VS Code, create `students/DEx/week3/python/p1/row_counter.py`.
2. Start from this skeleton and fill in the TODO:

   ```python
   import csv
   import sys

   def count_rows(path):
       with open(path, newline="", encoding="utf-8") as f:
           reader = csv.reader(f)
           # TODO: count the data rows (hint: the first row is the header)
           return 0

   if __name__ == "__main__":
       path = sys.argv[1]   # the file name you type after the script name
       print(path, count_rows(path))
   ```

3. **What each line does.** Read this before you edit anything.
   - `import csv` brings in the Python module that knows how to read CSV files.
   - `import sys` brings in the module that lets you read what you typed on the command line.
   - `def count_rows(path):` starts a function named `count_rows`. It takes one input, called `path`, which is the file name.
   - `with open(path, newline="", encoding="utf-8") as f:` opens that file as UTF-8 text and gives it the short name `f`. The `with` block closes the file by itself when the block ends, even if there is an error.
   - `reader = csv.reader(f)` makes a reader. Each time you ask it, it returns the next row as a list of text values.
   - The `# TODO` line is where your loop goes. The first row you get is the header, so it is not a data row.
   - `return 0` sends a number back to whoever called the function. Right now it is a placeholder, so your job is to return the real count.
   - `if __name__ == "__main__":` runs the lines below only when you run this file directly, not when another file imports it.
   - `path = sys.argv[1]` takes the first thing you typed after the script name.
   - `print(path, count_rows(path))` prints the file name and the number the function returned.
4. Run it on three different files, one at a time:

   ```
   python students/DEx/week3/python/p1/row_counter.py data/raw/mca/<your_file>.csv
   ```

5. Create `students/DEx/week3/python/p1/notes.md` and paste the three outputs.

If you get stuck:
- Read the error text. `FileNotFoundError` means the path is wrong.
- Print the first row inside the function to see what the reader returns.
- Search "python count rows csv", or ask in Discord with the error text.

##### 6. What to Commit
- [ ] `students/DEx/week3/python/p1/row_counter.py` (replace `DEx` with your number)
- [ ] `students/DEx/week3/python/p1/notes.md`
- [ ] Done when: the script runs on three files without an error
- [ ] Done when: the three counts are in the notes
- [ ] Done when: a pull request is open

Commit message: `P1.1 count rows in one RoC csv`

#### Task 2 - Check the headers (ID `P1.2`)

##### 1. What We Are Doing
You write a Python function that returns the column names of a CSV file, then compare the headers of two files. You write in VS Code and run in the VS Code terminal.

##### 2. Why We Are Doing It
Two RoC files must have the same columns before we load both into one table in week 6. The header line is how you prove it.

##### 3. What to Learn First
- Lists in Python, and how to compare two lists.
- Functions that return a value.
- `if __name__ == "__main__"`.

##### 4. Where to Find Resources
- Corey video #4: "Python Tutorial for Beginners 4: Lists, Tuples, and Sets"
- Corey video #8: "Python Tutorial for Beginners 8: Functions"
- Corey video #56: "Python Tutorial: if \_\_name\_\_ == '\_\_main\_\_'"
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU. About 45 minutes.
- Search keywords: "python compare two lists", "python csv read header row", "python if name main".

##### 5. Step-by-Step Guide
1. Create `students/DEx/week3/python/p1/header_check.py`.
2. Start from this skeleton:

   ```python
   import csv

   def get_headers(path):
       with open(path, newline="", encoding="utf-8") as f:
           reader = csv.reader(f)
           # TODO: return the first row as a list of column names
           return []

   if __name__ == "__main__":
       file_a = "data/raw/mca/<your_first_file>.csv"
       file_b = "data/raw/mca/<your_second_file>.csv"
       # TODO: compare the two lists and print "match" or "no match"
   ```

3. Run it: `python students/DEx/week3/python/p1/header_check.py`
4. Write the result in `students/DEx/week3/python/p1/notes.md`.

If you get stuck:
- Print both lists to see the difference with your own eyes.
- Read the error text. A wrong path gives `FileNotFoundError`.
- Search "python compare two lists", or ask in Discord with the two headers pasted.

##### 6. What to Commit
- [ ] `students/DEx/week3/python/p1/header_check.py` (replace `DEx` with your number)
- [ ] `students/DEx/week3/python/p1/notes.md` (the result)
- [ ] Done when: the function returns the column names
- [ ] Done when: the script prints match or no match
- [ ] Done when: the result is in the notes
- [ ] Done when: the pull request is updated

Commit message: `P1.2 check CSV headers of two RoC files`

#### Task 3 - Filter rows by state (ID `P1.3`)

##### 1. What We Are Doing
You write a script that reads one CSV, keeps only the rows for one state, and writes a new CSV. You write in VS Code and run in the VS Code terminal.

##### 2. Why We Are Doing It
Analysts slice by state all the time. This is your first "read one file, write another" script, the same shape every extractor in this project uses.

##### 3. What to Learn First
- `if` statements and text comparison.
- Finding a named column in the header row.
- Writing a CSV file with `csv.writer`.

##### 4. Where to Find Resources
- Corey video #2: "Python Tutorial for Beginners 2: Strings - Working with Textual Data"
- Corey video #6: "Python Tutorial for Beginners 6: Conditionals and Booleans - If, Else, and Elif Statements"
- Corey video #28: "Python Tutorial: CSV Module - How to Read, Parse, and Write CSV Files"
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU. About 45 minutes.
- Search keywords: "python csv writer example", "python filter rows csv", "python string strip compare".

##### 5. Step-by-Step Guide
1. Create `students/DEx/week3/python/p1/state_filter.py`.
2. Start from this skeleton:

   ```python
   import csv
   import sys

   def filter_by_state(in_path, out_path, state):
       # TODO: find the state column in the header row
       # TODO: write the header, then only the rows where state matches
       pass

   if __name__ == "__main__":
       in_path = sys.argv[1]
       out_path = sys.argv[2]
       state = sys.argv[3]
       filter_by_state(in_path, out_path, state)
   ```

3. Run it on two files with the state `Kerala`:

   ```
   python students/DEx/week3/python/p1/state_filter.py data/raw/mca/<file>.csv data/raw/mca/kerala_<file>.csv Kerala
   ```

4. Keep the new CSV under `data/raw/mca/` so no data file ends up in a commit.
5. Check the output CSV against the input CSV:
   - The output header must match the input header.
   - Every output data row must have the chosen state.
   - The output data row count must equal the number of input rows that match the chosen state.
6. Write in `students/DEx/week3/python/p1/notes.md`: for both runs, the input file name, the output file name, the input row count, and the output row count.

If you get stuck:
- Open the output CSV and look at it. If it is empty, your state text did not match.
- Print the state value from one row to see the exact spelling and spaces.
- Search "python csv writer example", or ask in Discord with the row count you got.

##### 6. What to Commit
- [ ] `students/DEx/week3/python/p1/state_filter.py` (replace `DEx` with your number)
- [ ] `students/DEx/week3/python/p1/notes.md` with both output file names and their input/output row counts
- [ ] Done when: both runs produce a new CSV
- [ ] Done when: the output header matches the input header, every output data row matches the chosen state, and the output count equals the number of matching input rows
- [ ] Done when: the pull request is updated

Commit message: `P1.3 filter rows by state`

> **If Python stops with an encoding error:** stop and check the encoding of the source file. Do not switch to `latin-1` just to make the error go away: `latin-1` accepts every byte value, so it can silently corrupt text. Ask the program lead if you are unsure. When the encoding is confirmed, use it in `open()` and record the confirmed encoding in `students/DEx/week3/python/p1/notes.md`.

### Step 0 - Build your practice data kit (no task ID, not graded)

This step is **practice only**. The proper, repeatable load (stage, file format, `COPY INTO`, row-count check) is taught in week 6, in the task group **First Snowflake Load**. Until then you need tables you can query now, so you build a small kit by hand with the web wizard. The three RoC ZIP files you use here are the ones you downloaded in week 2.

##### 1. What We Are Doing
You build small practice tables in Snowflake so the tasks after this one have data to query. You work in VS Code first to save the SQL and the IBBI sample CSV, then in Snowflake (Snowsight) to load and run. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and your own schema.

##### 2. Why We Are Doing It
Tasks 4 and 5 need real tables in Snowflake. Without this kit you cannot test a query on real rows. The kit also gives you a "last month" copy, so week 4 has something to compare with a self join (`S3.5`) and week 5 has something to find with set operations.

##### 3. What to Learn First
- Search the Snowflake docs at https://docs.snowflake.com for "load data using the web interface" and read that page (about 15 min).
- Nothing else new.

##### 4. Where to Find Resources
- Snowflake docs: https://docs.snowflake.com. Search for "load data using the web interface" (about 15 min).
- Search keywords: Snowflake load CSV wizard, Snowsight create table from file, Snowflake USE ROLE USE WAREHOUSE.

##### 5. Step-by-Step Guide
1. In Snowsight, pick role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your database, your own schema.
2. Load the first RoC CSV from `data/raw/mca/` with the wizard: Snowsight > Create > Table > From file (or "Load data into a table"; the menu names can differ a little).
3. In the wizard, set **all columns to VARCHAR** where the wizard allows it.
4. Name the table `RAW_MCA_<ROC>`, using a short RoC code you can remember.
5. After loading, run `SELECT * FROM RAW_MCA_<ROC> LIMIT 5;` and read the real column names. The SQL examples in this file use names like `CIN` and `STATUS`; your table may call the status column something slightly different (for example a name that contains STATUS). Always use the names you see in your own table.
6. Repeat steps 2 to 5 for the other two RoC files. Use a different `<ROC>` code for each table.
7. Get the IBBI PDF: go to https://ibbi.gov.in. Find the CIRP PDF you will use, download it by hand, and save it under `data/raw/ibbi/` with the pull date in the file name. If you are unsure which list to use, ask the program lead.
8. Hand-build `RAW_IBBI_SAMPLE`. In VS Code create the file `data/raw/ibbi/ibbi_sample.csv` (this folder is ignored by Git, so the file is never committed). Header row: `CIN,COMPANY_NAME,BENCH,START_DATE,ADMITTED_CLAIMS`. Type 20 to 30 rows from the IBBI PDF you saved in step 7, and note the pull date in `students/DEx/week3/sql/s2/notes.md`. Load it with the same wizard.
9. Make the "last month" copy. Before you run `UPDATE` or `DELETE`, know what they do. `UPDATE` changes values in existing rows. `DELETE` removes rows. `WHERE` selects which rows are affected. First write a `SELECT` with the same `WHERE`, look at the rows, and only then change or delete them. In VS Code create `students/DEx/week3/sql/s2/00_practice_kit.sql` and put these statements in it, then run them in Snowsight and correct the file afterwards:
   ```sql
   -- TODO: replace <your_database>, <your_schema> and the table names
   USE ROLE STUDENT_ROLE;
   USE WAREHOUSE STUDENT_WH;
   USE DATABASE <your_database>;
   USE SCHEMA <your_schema>;

   CREATE TABLE RAW_MCA_PREV AS
   SELECT * FROM RAW_MCA_<roc>;      -- TODO: pick ONE of your three RoC tables

   -- Preview the rows you will change first:
   SELECT * FROM RAW_MCA_PREV
   WHERE CIN IN ('...', '...');      -- TODO: use CINs you can find again

   UPDATE RAW_MCA_PREV
   SET STATUS = '...'                 -- TODO: change the status on a few rows
   WHERE CIN IN ('...', '...');       -- TODO: use CINs you can find again

   DELETE FROM RAW_MCA_PREV
   WHERE CIN IN ('...', '...');       -- TODO: delete a few rows
   ```
10. Write in `students/DEx/week3/sql/s2/notes.md`: each table name, its row count, the IBBI pull date, and which rows you changed or deleted in `RAW_MCA_PREV`.

**How to think about it:** this kit is practice. The real repeatable load comes in week 6. Say so in your notes, so nobody thinks this is the final pipeline.

If you get stuck:
- Read the wizard error text, search "load data using the web interface" in the Snowflake docs, and check that role, warehouse, database and schema are all set.
- If the wizard shows the wrong data types, look for the column type setting and change it to text (`VARCHAR`) before the load.
- Ask in Discord with the exact error text.

##### 6. What to Commit
- [ ] `students/DEx/week3/sql/s2/00_practice_kit.sql` (replace `DEx` with your number)
- [ ] `students/DEx/week3/sql/s2/notes.md`
- [ ] The CSV stays in `data/raw/ibbi/ibbi_sample.csv` and is not committed
- [ ] Done when: the three `RAW_MCA` tables exist and you wrote their row counts
- [ ] Done when: `RAW_IBBI_SAMPLE` has 20 to 30 rows
- [ ] Done when: `RAW_MCA_PREV` has fewer rows than its source and at least one changed status
- [ ] Done when: both files are saved

Commit message: `S2.0 practice data kit`

### Group 2 - Data Integrity: Constraints

Foundation link: Kudvenkat Part 4 to Part 9 (constraints, identity, unique keys). Snowflake does not enforce `UNIQUE` or `CHECK`. See `docs/09-resources.md` for the full difference list.

#### Task 4 - Find empty CINs (ID `S2.1`)

##### 1. What We Are Doing
You write and run a query that counts rows with an empty or `NULL` CIN. You work in VS Code to write the file, then in Snowflake to run it, either in a Workspace (Snowsight > Projects > Workspaces) or from VS Code with the Snowflake extension (`Snowflake: Execute All Statements`). Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema.

##### 2. Why We Are Doing It
The client's first question is "is this company real?". A row with no CIN can never be joined to anything, so we must know how many such rows exist before we trust any join.

##### 3. What to Learn First
- Kudvenkat Part 9 "Unique key constraint" (about 10 min).
- Kudvenkat Part 15 "Different ways to replace NULL" and Part 16 "Coalesce function" (about 20 min).
- Watch for the idea. Note that Snowflake declares a `UNIQUE` key but does not enforce it.
- About 30 min in total.

##### 4. Where to Find Resources
- Kudvenkat Part 9 "Unique key constraint" (about 10 min), on https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB
- Kudvenkat Part 15 "Different ways to replace NULL" (about 10 min)
- Kudvenkat Part 16 "Coalesce function" (about 10 min)
- Search keywords: Snowflake check NULL empty string, Snowflake TRIM CIN, SQL count NULL values.

##### 5. Step-by-Step Guide
1. In VS Code, open your week 3 folder, right-click the `sql/s2` folder > New File, name it `01_null_cin_check.sql`.
2. Paste this starter and fill in the TODOs:
   ```sql
   USE ROLE STUDENT_ROLE;
   USE WAREHOUSE STUDENT_WH;
   USE DATABASE <your_database>;   -- TODO: the database the program lead gave you
   USE SCHEMA <your_schema>;       -- TODO: your own schema

   SELECT
       -- TODO: a count, with a clear alias
   FROM RAW_MCA_<roc>              -- TODO: one of your three RoC tables
   WHERE
       -- TODO: CIN IS NULL, OR CIN is empty after TRIM
       ;
   ```
3. Run the SQL in Snowflake: open a Workspace and paste it, or use the Snowflake extension in VS Code. Run it one statement at a time.
4. Put the final working version back in the VS Code file.
5. Write in `students/DEx/week3/sql/s2/notes.md`: the table name, the total row count of that table, and the count of empty CINs.

If you get stuck:
- Read the SQL error line number. It usually points at the missing part.
- Search "Snowflake check NULL empty string", and check that `RAW_MCA_<roc>` is the real table name you created.
- Ask in Discord with the SQL and the error text.

##### 6. What to Commit
- [ ] `students/DEx/week3/sql/s2/01_null_cin_check.sql` (replace `DEx` with your number)
- [ ] `students/DEx/week3/sql/s2/notes.md`
- [ ] Done when: the query runs with no error
- [ ] Done when: the count is written down next to the total row count
- [ ] Done when: you can say what share of rows is affected

Commit message: `S2.1 find empty CINs`

#### Task 5 - Find duplicate CINs (ID `S2.2`)

##### 1. What We Are Doing
You write a `GROUP BY` query that finds CINs that appear more than once. You work in VS Code to write, and in Snowflake to run, either in a Workspace or with the Snowflake extension. Use role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own database and schema.

##### 2. Why We Are Doing It
If the same company appears twice, the client's report counts it twice and the due-diligence numbers are wrong. We check this before any reporting.

##### 3. What to Learn First
- Kudvenkat Part 11 "Group by" (about 12 min). This is your first `GROUP BY`.
- Just get the idea of grouping rows this time.

##### 4. Where to Find Resources
- Kudvenkat Part 11 "Group by" (about 12 min), on https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB
- Search keywords: Snowflake GROUP BY HAVING count duplicates, SQL find duplicate CIN, Snowflake ORDER BY count DESC.

##### 5. Step-by-Step Guide
1. Create `students/DEx/week3/sql/s2/02_duplicate_cin.sql`.
2. Start from this shape and fill in the TODOs:
   ```sql
   SELECT
       CIN,
       -- TODO: how many rows share this CIN?
   FROM RAW_MCA_<roc>
   WHERE CIN IS NOT NULL AND TRIM(CIN) <> ''
   GROUP BY CIN
   -- TODO: keep only the CINs that appear more than once
   -- TODO: order by the count, biggest first
   LIMIT 10;
   ```
3. Run it in Snowflake. This output is ten examples only. It does not give the total.
4. Run the same grouped query without `LIMIT 10` inside a count to get the total number of CIN groups that appear more than once. For example, put your grouped query inside `SELECT COUNT(*) FROM ( ... );` and run it. Save both statements in the same `.sql` file.
5. If you get zero rows, write that down too. Zero is a real answer.
6. Save the working version in VS Code.

If you get stuck:
- Read the error message, then search "Snowflake GROUP BY HAVING count duplicates".
- Check that the `GROUP BY` column list matches the `SELECT` list.
- Ask in Discord with the query and the error text.

##### 6. What to Commit
- [ ] `students/DEx/week3/sql/s2/02_duplicate_cin.sql` (replace `DEx` with your number)
- [ ] `students/DEx/week3/sql/s2/notes.md`
- [ ] Done when: the `LIMIT 10` query returns at most ten example CINs with their counts
- [ ] Done when: the uncapped count query gives the total number of CIN groups that appear more than once
- [ ] Done when: you wrote in notes which table you used, the total number of CIN groups that appear more than once, and the ten example CINs

Commit message: `S2.2 find duplicate CINs`

#### Task 6 - Enforce one company, one CIN (ID `S2.3`)

##### 1. What We Are Doing
You write a short note that says how the team will enforce the rule "one company, one CIN". You work in VS Code only. This is a writing task, with no SQL to run.

##### 2. Why We Are Doing It
The platform must never report one company twice. If the database will not enforce the rule, the team needs a written plan for who or what checks it.

##### 3. What to Learn First
Nothing new. You use Part 9 from Task 4.

##### 4. Where to Find Resources
- No new resource. Reuse Kudvenkat Part 9 "Unique key constraint" from Task 4.
- Search keywords: Snowflake UNIQUE constraint not enforced, dbt unique test, SQL check duplicate CIN after load.

##### 5. Step-by-Step Guide
1. Open `students/DEx/week3/sql/s2/notes.md` (or create it if it is missing).
2. Add the heading `S2.3 - One company, one CIN`.
3. Answer in **three sentences in total**. Use these questions to build the sentences, not as extra answers:
   - Snowflake does not enforce `UNIQUE`. Which SQL from Task 5 (`S2.2`) proves the rule still holds after a load?
   - What should the pipeline do when that check fails?
   - Where do you write the rule down so the next person finds it?
4. Optional preview, not required: dbt tests arrive in week 12 and can check this rule automatically. You do not need to use them now.

**How to think about it:** a rule that is not enforced by the database needs a check, a failure action, and a written owner. Write those three ideas in three sentences.

If you get stuck:
- Reread the questions and write one sentence per idea.
- Check that the three sentences cover check, fail action, and where the rule is written.
- Ask in Discord with your three sentences.

##### 6. What to Commit
- [ ] `students/DEx/week3/sql/s2/notes.md` (append; replace `DEx` with your number)
- [ ] Done when: the note has exactly three sentences
- [ ] Done when: the words UNIQUE, check (or test) and fail appear
- [ ] Done when: you can say the three sentences out loud without reading

Commit message: `S2.3 enforce one company one CIN`

> **NOT NULL and empty are not the same thing.** `NOT NULL` stops a row being saved when the value is missing. An empty text value (`''`) is not missing, so Snowflake will accept it. Decide which of the two rules you want, and write your choice in the notes.

## 6. What to Commit

End of week checklist:

- [ ] Warm-up - two small scripts that read your own text file and your own 3-row CSV
- [ ] Tasks 1 to 3 - three scripts that run, with output and row counts pasted in notes
- [ ] Step 0 - the practice tables, their row counts, the IBBI pull date, and the rows you changed in notes
- [ ] Tasks 4 to 6 - both integrity queries with counts, plus the three-sentence note
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] No data file, no credential, and no `.venv` anywhere in your commits

All files go inside `students/DEx/week3/` (replace `DEx` with your number). Data files stay in `data/raw/`.

**If you are short on time, cut in this order:** Task 3 (`P1.3`), then Task 6 (`S2.3`). Never cut Tasks 1 and 2 (`P1.1`, `P1.2`) or Tasks 4 and 5 (`S2.1`, `S2.2`), and never cut the notes files. The row count, the header check and the duplicate check are the point of the week.

When your week is done, open one pull request on GitHub for your work and update it as you go (`docs/03-student-guide.md` section 4 shows how). Do not squash-merge.

Next: `week4/problem_statement.md`.
