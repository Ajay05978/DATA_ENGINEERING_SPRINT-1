# Week 2 - First tables and first Python

**Data Sprint 1 · Week 2 of 15 · Theme: make your first Snowflake database and table, then write your first tiny Python scripts**

Read this whole file before you start anything. Then work through the task groups in order. Every task is small: do one task, save one file, make one commit, then start the next task.

## 1. What We Are Doing

This week you touch Snowflake for real: you set up your isolated database with three schemas, create a raw table for the MCA company data, and write down which columns must never be empty. Then you start Python from zero with four tiny scripts, so the later data work is not your first contact with code.

The task groups this week:

- **Group 1 - Databases and Tables**: download your practice data (unnumbered step), then Tasks 1 to 3 (`S1.1`, `S1.2`, `S1.3`)
- **Group 2 - Python From Zero**: Tasks 4 to 7 (`P0.1`, `P0.2`, `P0.3`, `P0.4`)

Plan: the SQL tasks first, Monday to Wednesday. The Python tasks Thursday and Friday.

## 2. Why We Are Doing It

The client is a fintech company that does due diligence on Indian companies. Its analysts check four things: is the company real and active (MCA), was it ever in insolvency (IBBI), how does it compare with its state (MCA CDM), and what was the interest-rate backdrop (RBI). We build one Snowflake data platform that answers those questions, in three layers: bronze (raw data), silver (cleaned data), gold (the tables the dashboard reads). It ends in a Metabase dashboard.

This week you build the first two pieces of that platform. The database and the schemas are the shape every later table sits inside. The raw MCA table is where the company data lands, unchanged, so you can always check a value against the original file. The Python scripts are small on purpose: every later extractor in this project uses the same few pieces (variables, lists, loops, functions, imports).

This week has no milestone. The first Snowflake load milestone comes in week 6, and the practice data you download this week is used all the way there.

## 3. What to Learn First (Prerequisites)

**Before this week: what you must already have**

| You need | Where |
|---|---|
| The client story and the four data sources | `docs/01-project-brief.md` |
| Git, Python, and Snowflake working | `docs/02-tools-setup.md` |
| Fork, clone, commit, pull request commands | `docs/03-student-guide.md` |
| A word you do not know | `docs/08-glossary.md` |
| This week's reading and videos | `docs/09-resources.md` |
| A tool that will not work | `docs/10-troubleshooting.md` |
| What a tool is for and why we use it | `docs/11-tools-and-technology.md` |

If one of these is not done, go back to the week 1 setup tasks first, then come here.

**By the end of this week you can**

- Set up your isolated database with three schemas in Snowflake, and say what each schema holds
- Create a raw table with one text column per CSV field, and say why every column is text at this stage
- List the columns a real company record cannot do without, and give a reason for each one
- Write and run a tiny Python script with variables, strings, a list, a dictionary, an `if`, a `for` loop, a function and one import
- Run Python from the VS Code terminal with your virtual environment active

**Core concepts to review first**

- **How your tasks are numbered.** Each week numbers its tasks 1 to N in the order you should do them. Every task also keeps a stable ID like `S1.1`. That ID goes in your commit message and in your pull request title, so anyone can find the task behind any commit.
- **The places you work.** You write files in VS Code. You run SQL in Snowflake. You run Python in the VS Code terminal. The full list is in section 4.
- **What "your week folder" means.** It is `students/DEx/week2/` inside your cloned repo. `DEx` is your own student number. If you are DE12, your folder is `students/DE12/week2/`. Every path in this file uses `DEx`; replace it with your number.
- **The one exception.** Downloaded data files never go in your week folder. They go to `data/raw/...` at the repo root, and `.gitignore` already excludes them.
- **One task, one file, one commit.** Small commits are easy to read and easy to review. Never squash-merge.
- **One week branch, one weekly pull request.** Keep all your task commits for this week on one branch. Open one pull request for the week and update it as you add commits.
- **Python is new for most people. Go slowly. The scripts are tiny on purpose.**

**You do not clean any data this week.** That is deliberate. Cleaning starts in week 3.

## 4. Where to Find Resources

**Where you work**

You work in four places. Each task tells you which one to use.

1. **VS Code** - your home base. You write and save every file here, inside your own week folder. You also commit here.
2. **VS Code terminal** - you run Git commands and Python commands here. For Python, activate your `.venv` first (`source .venv/bin/activate` on macOS or Linux, `.venv\Scripts\activate` on Windows).
3. **Snowflake** - you run SQL here. Two ways, both fine:
   - the **Snowflake extension in VS Code**, if your setup task `G0.3` worked for you. Open the `.sql` file and run it with the command "Snowflake: Execute All Statements" from the Command Palette.
   - a **Snowflake Workspace** in the browser: Snowsight > Projects > Workspaces, then create a SQL file and run it.

   Snowflake is where the data lives. It is not where you save your work.
4. **The web** - you download data by hand from the source websites into `data/raw/...` at the repo root.

**The one rule:** write and save every file in VS Code in your own week folder. Then run SQL in Snowflake and Python in the VS Code terminal. When the SQL works, make sure your saved `.sql` file holds the final working version.

Example: if you are DE12, your first SQL file goes to `students/DE12/week2/sql/s1/01_create_database.sql`.

**Docs to keep open this week**

| Doc | Use it for |
|---|---|
| `docs/01-project-brief.md` | the client story, the four sources, where to download MCA data (section 2.1) |
| `docs/02-tools-setup.md` | setup checks for Git, Python and Snowflake |
| `docs/03-student-guide.md` | branch, commit and pull request steps (section 4) |
| `docs/08-glossary.md` | any word you do not know |
| `docs/09-resources.md` | the videos, and the SQL Server to Snowflake differences |
| `docs/10-troubleshooting.md` | a tool that will not work |
| `docs/11-tools-and-technology.md` | what a tool is for and why we use it |

**Your week in order**

| Day | Learn first (exact items) | Then do | Time |
|---|---|---|---|
| **1** | Kudvenkat Part 2 "Creating, altering and dropping a database" (about 12 minutes), on https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB. Skip the SSMS clicks. In the Snowflake docs (https://docs.snowflake.com), search for "Key Concepts and Architecture". Ask the program lead for your isolated database name. | Download your practice data, then Task 1 | about 1.5 hours |
| **2** | Kudvenkat Part 3 "Creating and working with tables" (about 15 minutes). In the Snowflake docs, search for "CREATE TABLE". The MCA data dictionary published with the Company Master Data catalog on https://data.gov.in | Tasks 2 and 3 | about 2 hours |
| **3** | Corey video #1 "Python Tutorial for Beginners 1: Install and Setup for Mac and Windows" (just check it is installed), #2 "Strings - Working with Textual Data", #3 "Integers and Floats - Working with Numeric Data", and #105 "VENV (Mac & Linux)" or #106 "VENV (Windows)" (about 10 minutes). Set up and activate `.venv` before your first Python command. | Task 4 (P0.1) | about 1.5 hours |
| **4** | Corey #4 "Lists, Tuples, and Sets", #5 "Dictionaries - Working with Key-Value Pairs", #6 "Conditionals and Booleans - If, Else, and Elif Statements", #7 "Loops and Iterations - For/While Loops". Optional: #19 "Slicing Lists and Strings" | Tasks 5 and 6 (P0.2, P0.3) | about 2 hours |
| **5** | Corey #8 "Functions", #9 "Import Modules and Exploring The Standard Library", #56 "if \_\_name\_\_ == '\_\_main\_\_'" | Task 7 (P0.4) | about 2 hours |

The Corey Schafer playlist is here: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU. The number (#N) is the position of the video in the playlist. Watch only the videos listed for this week.

The Kudvenkat playlist teaches Microsoft SQL Server, an old tool. We use Snowflake. So: watch for the idea, type the Snowflake version yourself, and run it in Snowflake. The differences that matter are listed in `docs/09-resources.md`. A few you will meet this week: `IDENTITY` becomes `AUTOINCREMENT`, `TOP 10` becomes `LIMIT 10`, `ISNULL()` becomes `IFNULL()` or `COALESCE()`, `GETDATE()` becomes `CURRENT_TIMESTAMP()`, and `LEN()` becomes `LENGTH()`.

**Using an AI assistant:** you may ask an AI assistant to translate a SQL Server query from the video into Snowflake SQL, and to explain an error message. Read the answer and make sure you can explain it yourself. Do not paste a finished deliverable.

**Search keywords if you get stuck this week**

- "Snowflake CREATE DATABASE CREATE SCHEMA"
- "Snowflake CREATE TABLE VARCHAR"
- "MCA Company Master Data data.gov.in"
- "python print variable example"
- "python list and dictionary example"
- "python function with parameters"

## 5. Step-by-Step Guide

How to work: one task, one file, one commit. Finish a task, save the file, commit it, then start the next task. Keep all your task commits for this week on one week branch, and open one weekly pull request. The full Git steps are in `docs/03-student-guide.md` section 4. Short version, run in the VS Code terminal from the repo root:

```
git status
git checkout -b week2-work
git add students/DEx/week2/sql/s1/01_create_database.sql
git commit -m "S1.1 create database and bronze silver gold schemas"
git push -u origin week2-work
```

After the push, open one pull request for the whole week on GitHub. For each later task, stay on the same `week2-work` branch and run only `git add`, `git commit`, and `git push`. The rule for all 15 weeks: **one task, one file, one commit.** Never squash-merge.

### Download your practice data (do this before Task 1)

This step has no task ID. Do it once, at the start of the week.

1. Open a browser and go to https://data.gov.in. Follow the clicks in `docs/01-project-brief.md` section 2.1 exactly.
2. Find the Company Master Data catalog. It has one ZIP file per RoC office. Download **three** ZIP files, any three offices.
3. Unzip each one. Each ZIP holds one CSV file.
4. In VS Code, put the three CSV files in `data/raw/mca/` at the repo root. This folder is in `.gitignore`, so data never goes into a commit.
5. Create `data/raw/mca/download_notes.txt` and write today's date and the three file names.
6. You open the header of one of these files in Task 2. The same three files come back in week 3, where the Python tasks read them. You load them into Snowflake in week 3 with the practice data kit. The proper repeatable load (stage, file format, `COPY INTO`, row count check) is taught in week 6.

> **Why a practice download now:** the real load is a later skill, and we do not want you to wait for it. These three files let you practise tables and Python on real data today.

### Group 1 - Databases and Tables

#### Task 1 - Create the database and schemas (ID `S1.1`)

##### 1. What We Are Doing
You set up your isolated database with three schemas: BRONZE, SILVER and GOLD. You write the SQL in VS Code, then run it in Snowflake, either with the Snowflake extension in VS Code or in a Snowsight Workspace.

##### 2. Why We Are Doing It
The client's data platform needs three layers. BRONZE holds raw data as it arrived, SILVER holds cleaned data, GOLD holds the tables the dashboard reads. Every later table in the project sits inside one of these three schemas.

##### 3. What to Learn First
- What a database and a schema are.
- The `CREATE DATABASE` and `CREATE SCHEMA` statements.
- How to pick a role and a warehouse in Snowflake.
- Use the isolated database name the program lead gives you. Do not use a shared database.

##### 4. Where to Find Resources
- Kudvenkat Part 2 "Creating, altering and dropping a database" (about 12 minutes), on https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB. Skip the SSMS clicks, keep the `CREATE DATABASE` idea, then write the Snowflake version yourself.
- In the Snowflake docs (https://docs.snowflake.com), search for "Key Concepts and Architecture".
- Search keywords: "Snowflake CREATE DATABASE", "Snowflake CREATE SCHEMA", "Snowsight choose role warehouse", "Snowflake bronze silver gold".

##### 5. Step-by-Step Guide
1. Before you write SQL, ask the program lead for your own isolated database name. Write it down. If the program lead already created it, use it and do not create a new database. If the program lead tells you to create it yourself, the name must include your student number, for example `DE12_WEEK2`.
2. In VS Code, create `students/DEx/week2/sql/s1/01_create_database.sql` inside your week folder.
3. Write this SQL. Replace `<your_isolated_database_name>` with the name from step 1, and remove the angle brackets:

   ```sql
   CREATE DATABASE IF NOT EXISTS <your_isolated_database_name>;
   USE DATABASE <your_isolated_database_name>;

   CREATE SCHEMA IF NOT EXISTS BRONZE;
   CREATE SCHEMA IF NOT EXISTS SILVER;
   CREATE SCHEMA IF NOT EXISTS GOLD;

   -- BRONZE: write one sentence on what it will hold.
   -- SILVER: write one sentence on what it will hold.
   -- GOLD: write one sentence on what it will hold.
   ```

4. Save the file in VS Code.
5. Open Snowflake. If your setup task `G0.3` worked, use the Snowflake extension in VS Code: open your saved `.sql` file and run it with "Snowflake: Execute All Statements" from the Command Palette. If not, open Snowsight in the browser > Projects > Workspaces, create a SQL file, and paste the SQL there. At the top, set your role and warehouse to the ones the program lead gave you (usually STUDENT_ROLE and STUDENT_WH).
6. Run the SQL. Then put the final working SQL back into your VS Code file and save. Never put a password in a file.
7. In the same SQL file, finish the three comments so each schema has one sentence saying what it will hold.

**How to think about it:** A database contains schemas. Each schema contains tables. BRONZE, SILVER and GOLD are the three schemas every table you build will use.

If you get stuck:
- Read the Snowflake error text. It often names the missing role or warehouse.
- Check the role and warehouse boxes at the top again.
- Search "Snowflake CREATE SCHEMA" in the Snowflake docs, or ask in Discord with the full error text.

##### 6. What to Commit
- [ ] `students/DEx/week2/sql/s1/01_create_database.sql` (replace `DEx` with your number)
- [ ] Done when: the three schemas exist in your isolated database
- [ ] Done when: the SQL file has one comment for each schema
- [ ] Done when: the weekly pull request is open

Commit message: `S1.1 create database and bronze silver gold schemas`

#### Task 2 - Create the raw MCA table (ID `S1.2`)

##### 1. What We Are Doing
You create one table in BRONZE that matches your MCA CSV file, with one text column per CSV field. You open the CSV in VS Code, write the SQL in VS Code, and run it in Snowflake.

##### 2. Why We Are Doing It
BRONZE must hold the MCA company data exactly as it arrived, so we can always check a value against the original file. This is also the table the practice data kit loads in week 3.

##### 3. What to Learn First
- What a table and a column are.
- The `CREATE TABLE` statement and the `VARCHAR` type.
- How to read a CSV header line.
- How to choose a short uppercase suffix for `<ROC_NAME>` and use the same table name in week 3.

##### 4. Where to Find Resources
- Kudvenkat Part 3 "Creating and working with tables" (about 15 minutes), on https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB. Watch for the idea, then type the Snowflake version.
- In the Snowflake docs (https://docs.snowflake.com), search for "CREATE TABLE".
- The MCA data dictionary published with the catalog on https://data.gov.in.
- Search keywords: "Snowflake CREATE TABLE VARCHAR", "MCA data dictionary", "Snowflake SELECT LIMIT", "CSV header line".

##### 5. Step-by-Step Guide
1. In VS Code, open one of the RoC CSV files you downloaded into `data/raw/mca/`. Look at the first line: that is the header, and it holds the field names.
2. Read the MCA data dictionary published with the catalog on https://data.gov.in to check what each field means.
3. Choose your RoC suffix. Use the RoC office name from the CSV file name, in upper case, with no spaces. For example, if the file name includes `MUMBAI`, use `MUMBAI`. Do not type `<ROC_NAME>` or the angle brackets in your final SQL. Write the final table name down so you use it again in week 3.
4. Create `students/DEx/week2/sql/s1/02_create_raw_mca.sql` in your week folder. Here is a skeleton with three example columns only, you add the rest:

   ```sql
   USE DATABASE <your_isolated_database_name>;
   USE SCHEMA BRONZE;

   -- Table name: BRONZE.RAW_MCA_<ROC_NAME>. Replace <ROC_NAME> with your suffix.
   CREATE TABLE IF NOT EXISTS RAW_MCA_<ROC_NAME> (
       cin               VARCHAR,
       company_name      VARCHAR,
       registration_date VARCHAR
       -- TODO: one VARCHAR column per field in your CSV header
       -- Why every column is VARCHAR at this stage: write your sentence here.
   );
   ```

5. Replace every `<ROC_NAME>` with your suffix. Replace `<your_isolated_database_name>` with your database name, and remove the angle brackets. At the top of the file, add a comment with the final table name, for example `-- Table: BRONZE.RAW_MCA_MUMBAI`.
6. Save the file. Run it in Snowflake. Check with `SELECT * FROM BRONZE.RAW_MCA_<ROC_NAME> LIMIT 5;` after you replace `<ROC_NAME>` with your suffix.
7. Add one SQL comment in the same file saying why every column is VARCHAR at this stage. Do not create a separate notes file for this task.
8. Copy the final working SQL back into your VS Code file and save it.

If you get stuck:
- Count the columns in the CSV header and count the columns in your SQL. They must be the same.
- Read the error text: a missing comma is the most common problem.
- Search "Snowflake CREATE TABLE VARCHAR" or ask in Discord with your SQL and the error text.

##### 6. What to Commit
- [ ] `students/DEx/week2/sql/s1/02_create_raw_mca.sql` (replace `DEx` with your number)
- [ ] Done when: the table exists in BRONZE with your final table name
- [ ] Done when: it has one VARCHAR column per CSV field
- [ ] Done when: the SQL file has the sentence saying why every column is VARCHAR
- [ ] Done when: the weekly pull request is updated

Commit message: `S1.2 create raw MCA table in bronze`

#### Task 3 - List the non-empty columns (ID `S1.3`)

##### 1. What We Are Doing
You list, in your notes file, the columns a real company record cannot do without, and you say why for each one. You work in VS Code.

##### 2. Why We Are Doing It
A row without the company identity is not a real company record. Saying which fields those are is the first data quality rule of the project, and the tests in later weeks check exactly these columns.

##### 3. What to Learn First
Nothing new. You use what you learned in Task 2. Note one Snowflake rule: Snowflake enforces NOT NULL but it does not enforce UNIQUE.

##### 4. Where to Find Resources
- Your own `students/DEx/week2/sql/s1/02_create_raw_mca.sql` and the CSV header.
- In the Snowflake docs (https://docs.snowflake.com), search for "constraints".
- Search keywords: "NOT NULL vs empty string SQL", "Snowflake constraints not enforced", "data quality required fields".

##### 5. Step-by-Step Guide
1. Open `students/DEx/week2/sql/s1/notes.md`. If it does not exist, create it.
2. Add a heading `## Columns that must never be empty`.
3. Go through your table column by column. List the columns a real company record cannot do without.
4. Write one sentence per column saying why. Do not change the table this week.

**How to think about it:** ask "can I still find this company in the registry without this value?" If the answer is no, the column must not be empty.

If you get stuck:
- Start with the company identity fields and go outwards.
- Check the MCA data dictionary on https://data.gov.in for what a field means.
- Ask in Discord and paste your list for a second opinion.

##### 6. What to Commit
- [ ] `students/DEx/week2/sql/s1/notes.md` (replace `DEx` with your number)
- [ ] Done when: the list is there
- [ ] Done when: each column has a reason
- [ ] Done when: the weekly pull request is updated

Commit message: `S1.3 list columns that must not be empty`

> **NOT NULL and empty are not the same thing.** NOT NULL stops a row being saved when the value is missing. An empty text value (`''`) is not missing, so Snowflake will accept it. Decide which of the two rules you want, and write your choice in the notes.

### Group 2 - Python From Zero

**Python is new for most people. Go slowly. The scripts are tiny on purpose.**

Task 4 shows how to set up and activate your virtual environment before your first Python command. Run every Python command from the repo root, with your virtual environment active: `source .venv/bin/activate` on macOS or Linux, `.venv\Scripts\activate` on Windows. Your prompt shows `(.venv)` when it is active.

#### Task 4 - Hello Python, variables and strings (ID `P0.1`)

##### 1. What We Are Doing
You write your first Python script. It stores a name and a company name in variables and prints them, with the length of the company name and its upper case version. Before your first Python command, you set up and activate your virtual environment. You write the file in VS Code and run it in the VS Code terminal.

##### 2. Why We Are Doing It
Every script in this project holds text values: a company name, a CIN, a state. Variables and strings are the base of all of it.

##### 3. What to Learn First
- What a variable is: a name that holds a value.
- Text (a string) in quotes, and joining text with `print()`.
- `len()` for length and `.upper()` for upper case.
- A virtual environment (`.venv`) is a private folder for your project's Python packages. Activate it before you run Python.

##### 4. Where to Find Resources
- Corey #1 "Python Tutorial for Beginners 1: Install and Setup for Mac and Windows" (about 10 minutes, only to check that Python is installed).
- Corey #2 "Strings - Working with Textual Data" (about 20 minutes).
- Corey #3 "Integers and Floats - Working with Numeric Data" (about 15 minutes).
- Corey #105 "VENV (Mac & Linux)" or #106 "VENV (Windows)" (about 10 minutes). Watch before your first Python command.
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU
- Search keywords: "python print variable", "python len string", "python upper lower case".

##### 5. Step-by-Step Guide
1. In VS Code, open your week folder `students/DEx/week2` and create a folder `python`, then a folder `p0` inside it.
2. Create the file `students/DEx/week2/python/p0/hello.py`.
3. Type this starter yourself. Do not copy and paste it, typing is how your hands learn:

   ```python
   name = "your name here"
   company = "tata consultancy services"

   print("Hello,", name)
   print("Company:", company)
   print("Letters in company name:", len(company))
   print("Upper case:", company.upper())
   ```

4. Before your first Python command, set up your virtual environment. Watch Corey #105 or #106. If you do not have `.venv` in the repo root yet, create it as shown in the video. Then activate it from the repo root with `source .venv/bin/activate` on macOS or Linux, or `.venv\Scripts\activate` on Windows. Your prompt shows `(.venv)` when it is active.
5. In the VS Code terminal, from the repo root, with `.venv` active, check Python:

   ```
   python --version
   ```

6. Run your script:

   ```
   python students/DEx/week2/python/p0/hello.py
   ```

7. **Exercise:** change `name` to your own name. Change `company` to a company name from your MCA CSV file. Add one more variable for your state and print it in upper case.

If you get stuck:
- Read the last line of the error message. It usually names the file and the line.
- Print the variable just before the line that fails, to see its value.
- Search the exact error text, or ask in Discord and paste the error text.

##### 6. What to Commit
- [ ] `students/DEx/week2/python/p0/hello.py` (replace `DEx` with your number)
- [ ] Done when: `python --version` prints a version with `.venv` active
- [ ] Done when: the script runs with no error
- [ ] Done when: your name, the company name, the length and the upper case line all print
- [ ] Done when: the weekly pull request is updated

Commit message: `P0.1 first script with variables and strings`

#### Task 5 - Lists and dictionaries (ID `P0.2`)

##### 1. What We Are Doing
You write a script that holds a list of five Indian state names, a dictionary for one company with `cin`, `name` and `state`, and a small number variable. You also do one arithmetic calculation. You work in VS Code and run in the VS Code terminal.

##### 2. Why We Are Doing It
A list holds many values in order, a dictionary holds named values. Later scripts hold lists of files and dictionaries of company records, so you meet both now. Numbers are used for counts and totals.

##### 3. What to Learn First
- Lists: square brackets, items separated by commas, counted from position 0.
- Dictionaries: curly braces, `"key": value` pairs, read with `company["name"]`.
- `len()` works on both.
- Numbers: `int` and `float`, and arithmetic with `+`, `-`, `*`, `/`.

##### 4. Where to Find Resources
- Corey #4 "Lists, Tuples, and Sets" (about 20 minutes).
- Corey #5 "Dictionaries - Working with Key-Value Pairs" (about 15 minutes).
- Corey #3 "Integers and Floats - Working with Numeric Data" (about 15 minutes, if you need a reminder on numbers).
- Optional: Corey #19 "Slicing Lists and Strings" (about 15 minutes).
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU
- Search keywords: "python list example", "python dictionary example", "python dictionary get key".

##### 5. Step-by-Step Guide
1. In VS Code, create `students/DEx/week2/python/p0/company_collections.py` inside `students/DEx/week2`.
2. Type this starter and fill in the real values:

   ```python
   states = ["Kerala", "Karnataka", "Tamil Nadu", "Maharashtra", "Gujarat"]

   company = {
       "cin": "<a CIN from your CSV file>",
       "name": "<a company name from your CSV file>",
       "state": "<its state>",
   }

   number_of_states = 5
   total_with_one_more = number_of_states + 1

   print(states)
   print("How many states:", len(states))
   print(company["name"], "is in", company["state"])
   print(company)
   print("States plus one:", total_with_one_more)
   ```

3. Run it from the repo root:

   ```
   python students/DEx/week2/python/p0/company_collections.py
   ```

4. **Exercise:** add a sixth state to the list. Print only the first state in the list. Add one more key to the dictionary, for example `"status"`, and print it. Change `number_of_states` to 6 and print `number_of_states * 2`.

If you get stuck:
- Read the last line of the error message. `KeyError` means the key name you asked for is not in the dictionary.
- Print the whole list or dictionary to see what is really inside.
- Search "python dictionary example", or ask in Discord and paste the error text.

##### 6. What to Commit
- [ ] `students/DEx/week2/python/p0/company_collections.py` (replace `DEx` with your number)
- [ ] Done when: the list has six states, and the first state prints
- [ ] Done when: the dictionary has `cin`, `name` and `state`, and the added key prints
- [ ] Done when: the numeric arithmetic prints a result
- [ ] Done when: the weekly pull request is updated

Commit message: `P0.2 lists and dictionaries`

#### Task 6 - Decisions and loops (ID `P0.3`)

##### 1. What We Are Doing
You write a script that loops over your list of states, prints only the states that start with the letter K, and counts them. You also add an `else` branch that handles the states that do not match. You work in VS Code and run in the VS Code terminal.

##### 2. Why We Are Doing It
Filtering and counting rows is most of data work. A loop plus an `if` keeps only the rows you want, and a counter tells you how many you kept. An `else` branch lets you handle the rows you did not keep.

##### 3. What to Learn First
- `if` and `else`: `if` runs when the test is true, `else` runs when the test is false.
- `if` and `==`: compare two values.
- `for item in my_list:` repeat a block once per item.
- `.startswith("K")` to test the first letter.
- Indentation: Python uses spaces to know which lines belong to the `if`, the `else`, and the loop.

##### 4. Where to Find Resources
- Corey #6 "Conditionals and Booleans - If, Else, and Elif Statements" (about 20 minutes).
- Corey #7 "Loops and Iterations - For/While Loops" (about 20 minutes).
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU
- Search keywords: "python for loop list", "python if else startswith", "python count items in list", "python indentation error".

##### 5. Step-by-Step Guide
1. In VS Code, create `students/DEx/week2/python/p0/loops.py` inside `students/DEx/week2`.
2. Type this starter:

   ```python
   states = ["Kerala", "Karnataka", "Tamil Nadu", "Maharashtra", "Gujarat"]

   count = 0
   for state in states:
       if state.startswith("K"):
           print(state)
           count = count + 1

   print("States starting with K:", count)
   ```

3. Run it from the repo root:

   ```
   python students/DEx/week2/python/p0/loops.py
   ```

4. **Exercise:** change the letter to `M` and run again. Then add an `else` block that prints `Not a K state:` and the state name for every nonmatching state. Finally add a second count that counts all states, not only the matching ones, and print both numbers.

If you get stuck:
- Read the last line of the error message. `IndentationError` means the spaces at the start of a line are wrong.
- Print the state inside the loop before the `if`, to see exactly what the loop sees.
- Search "python indentation error", or ask in Discord and paste the error text.

##### 6. What to Commit
- [ ] `students/DEx/week2/python/p0/loops.py` (replace `DEx` with your number)
- [ ] Done when: only the matching states print from the `if` block
- [ ] Done when: the `else` block prints for nonmatching states
- [ ] Done when: the count prints and is correct
- [ ] Done when: the weekly pull request is updated

Commit message: `P0.3 filter a list and count matches`

#### Task 7 - Functions and imports (ID `P0.4`)

##### 1. What We Are Doing
You write a function `count_long_names(names, min_length)` that counts how many names are long enough, and a `main` block that calls it. You also import one module, `math` or `os`, and use it once. You work in VS Code and run in the VS Code terminal.

##### 2. Why We Are Doing It
Every script in this project is built from small functions that each do one job, plus a `main` block that runs them in order. That shape is how later extractors and checks are written.

##### 3. What to Learn First
- `def` to define a function, `return` to send a value back.
- Parameters: the values you pass into a function.
- `import os` and `import math`, and using one thing from them.
- `if __name__ == "__main__":` to run the `main` function only when the file is run directly.

##### 4. Where to Find Resources
- Corey #8 "Functions" (about 20 minutes).
- Corey #9 "Import Modules and Exploring The Standard Library" (about 20 minutes).
- Corey #56 "if \_\_name\_\_ == '\_\_main\_\_'" (about 10 minutes).
- Playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU
- Search keywords: "python function return value", "python import module", "python if name main".

##### 5. Step-by-Step Guide
1. In VS Code, create `students/DEx/week2/python/p0/functions.py` inside `students/DEx/week2`.
2. Type this starter and finish the TODO:

   ```python
   import os

   def count_long_names(names, min_length):
       count = 0
       for name in names:
           # TODO: add 1 to count when the name is at least min_length characters long
           pass
       return count

   def main():
       companies = ["Tata", "Infosys Limited", "Wipro", "Reliance Industries Limited"]
       print("Long names:", count_long_names(companies, 10))
       print("Current folder:", os.getcwd())

   if __name__ == "__main__":
       main()
   ```

3. Run it from the repo root, with `.venv` active:

   ```
   python students/DEx/week2/python/p0/functions.py
   ```

4. **Exercise:** add one more company name to `companies`. Change the second argument in `count_long_names(companies, 10)` from `10` to `5` and run again. Say out loud what the function does in one sentence.
5. Test your function with three cases: a list with no matching names, a list where all names match, and a name exactly `min_length` characters long. Use names from your list and write the expected number in a comment.

If you get stuck:
- Read the last line of the error message. It usually names the function and the line.
- Print `count` inside the loop to see it grow.
- Search "python function return value", or ask in Discord and paste the error text.

##### 6. What to Commit
- [ ] `students/DEx/week2/python/p0/functions.py` (replace `DEx` with your number)
- [ ] Done when: the function returns a number, not `None`
- [ ] Done when: the `main` block runs and prints both lines
- [ ] Done when: the import is used at least once
- [ ] Done when: the no-match case returns `0`
- [ ] Done when: the all-match case returns the full count
- [ ] Done when: a name exactly `min_length` characters long is counted
- [ ] Done when: the weekly pull request is updated

Commit message: `P0.4 functions, main block and an import`

## 6. What to Commit

End of week checklist:

- [ ] Task 1 (S1.1) - isolated database, three schemas, and comments explaining each schema
- [ ] Task 2 (S1.2) - raw MCA table in BRONZE, one VARCHAR column per CSV field, and the VARCHAR comment
- [ ] Task 3 (S1.3) - the list of columns that must never be empty, with a reason for each
- [ ] Tasks 4 to 7 (P0.1 to P0.4) - four tiny Python scripts that run from the VS Code terminal
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] No data file, no credential, and no `.venv` anywhere in your commits

All files go inside `students/DEx/week2/` (replace `DEx` with your number). Data files stay in `data/raw/mca/` and are never committed.

**If you are short on time, do not cut Tasks 6 and 7 (P0.3 and P0.4).** The week 3 file-reading tasks need loops and functions. You may skip only the extra exercises inside Tasks 4 to 7. Never cut Tasks 1 to 3 (S1). The database, the table and the notes are the base you build on in week 3, and the written reasoning cannot be added back later.

When your week is done, your one weekly pull request on GitHub should contain all your commits. Update it as you go (`docs/03-student-guide.md` section 4 shows how).

Next: `week3/problem_statement.md`.
