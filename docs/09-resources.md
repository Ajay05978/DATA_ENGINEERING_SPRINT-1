# Resources

**Named sources, mapped to the week that needs them.** Every entry says what to watch or read and which week needs it. All sources here are free.

## 1. What We Are Doing

This guide helps you find the named source for your week. It lists the SQL videos, Python videos, docs, and data links that the sprint uses. It also tells you what to skip.

The guide has these parts:
- The learning table for all 15 weeks
- The main SQL series, Kudvenkat, with exact parts
- The Python video list and what to skip
- Snowflake and SQL Server differences
- dbt, data modelling, diagrams, data quality, orchestration, Git, and review sources
- The real data sources you will download from
- What not to use

## 2. Why We Are Doing It

You work on a due diligence project for Indian companies. You check if a company is real and active, if it was ever in insolvency, how it compares to its state, and the interest-rate backdrop.

The tools are new. Random blog posts and old forum answers often disagree with current Snowflake and dbt. That costs time. This guide gives you one named source per topic. It also tells you where to look when you get stuck.

You must explain your work at the cohort review. Use an AI assistant to understand an error or a concept, not to write your deliverable. If you cannot rebuild the thing from memory, it is not yours.

## 3. What to Learn First (Prerequisites)

### Before this guide: what you must already have

| You need | Why |
|---|---|
| Your DEx number, for example DE12 | All week paths use `students/DEx/weekN/` |
| The class repo cloned in VS Code | You write, save, and commit there |
| Your Snowflake login, role, and warehouse from the program lead | You run SQL in Snowflake |
| Python and `.venv` when a week uses Python | You run Python in the VS Code terminal |
| `docs/02-tools-setup.md` and `docs/03-student-guide.md` open | Setup help and the work rules |

### By the end of this guide you can

- Find the source for your week.
- Tell a SQL video from a Python video.
- Know the main Snowflake and SQL Server differences.
- Know where downloaded data goes and that it is never committed.
- Know which sources to avoid.

### Core concepts to review first

- One task, one file, one commit. Then open one pull request.
- VS Code is your main work place. You save files there and commit there.
- SQL is written in VS Code as `.sql` files and run in Snowflake.
- Python runs in the VS Code terminal with `.venv` activated.
- Downloaded data goes in `data/raw/...`. Never commit data or secrets.
- Your week problem statement is the final word.

## 4. Where to Find Resources

### Where you work

- VS Code: your main work place. You write and save files, and you commit from the VS Code terminal.
- Snowflake: where data lives and SQL runs. Use Snowsight in the browser. Projects > Workspaces. Select the role and warehouse, then run.
- Terminal: for Python and Git commands. Activate `.venv` before Python work.
- Browser: for Snowsight, GitHub, and the data sources.
- `DEx` means your number, for example DE12.

### Repo docs you will use

| File | Use it for |
|---|---|
| `docs/02-tools-setup.md` | Setup help for week 1 |
| `docs/03-student-guide.md` | The student work rules |
| `docs/01-project-brief.md` | Client story, star schema, Medallion layers |
| `docs/05-task-list.md` | Task IDs and delivery order |
| `docs/08-glossary.md` | NULL and set terms; view, CTE, subquery |
| `CONTRIBUTING.md` | This repo's branch, commit, and pull request rules |

### This week's learning - one table for all 15 weeks

Open the row for your week. "None new" means you reuse an earlier video. Your week problem statement is the final word.

| Week | Kudvenkat (SQL) | Corey Schafer (Python) | Other reading |
|---|---|---|---|
| W1 | None new | Setup help only: #110 Visual Studio Code (Mac) setup or #111 Visual Studio Code (Windows) setup; #105 VENV (Mac & Linux) or #106 VENV (Windows) | Pro Git chapters 1-2; `docs/02-tools-setup.md`; `docs/03-student-guide.md`; `CONTRIBUTING.md` |
| W2 | Part 2 Creating, altering and dropping a database; Part 3 Creating and working with tables | #1-#9 Beginners 1-9; #14 virtualenv optional | Python docs tutorial sections 4 and 5; MCA data dictionary |
| W3 | Part 4 Adding a default constraint; Part 5 Cascading referential integrity constraint; Part 6 Adding a check constraint; Part 7 Identity column; Part 8 optional; Part 9 Unique key constraint; Part 15 Different ways to replace NULL and Part 16 Coalesce function only as needed for S2.x | #25 File Objects - Reading and Writing to Files; #28 CSV Module - How to Read, Parse, and Write CSV Files; #127 Zip Files - Creating and Extracting Zip Archives; #56 if __name__ == '__main__' | Python docs `csv`; `docs/08-glossary.md` for NULL |
| W4 | Part 10 Select statement; Part 11 Group by; Part 12 Joins; Part 13 Advanced or intelligent joins; Part 14 Self join; "Difference between where and having" | #31 Using Try/Except Blocks for Error Handling; #24 Datetime Module - How to work with Dates, Times, Timedeltas, and Timezones; #154 Type Hinting vs Type Checking vs Data Validation; #155 Type Hints - From Basic Annotations to Advanced Generics; #157 Python Pydantic Tutorial: Complete Data Validation Course | Python docs `typing`; pydantic docs |
| W5 | Part 15 Different ways to replace NULL; Part 16 Coalesce function; Part 17 Union and union all; "SQL Server except operator"; "Difference between except and not in"; "Intersect operator"; "Difference between union intersect and except"; Parts 22-29 | None new | `docs/08-glossary.md` for NULL and set terms |
| W6 | None new | None new | Snowflake docs: search "Loading Data"; Kimball Group: fact tables, dimension tables, grain; `docs/01-project-brief.md`: Medallion layers; your week 6 problem statement: Medallion and ADR |
| W7 | Parts 39, 40, 48, 49, 59, 60 | #37 Decorators - Dynamically Alter The Functionality Of Your Functions; #38 Decorators With Arguments; #49 Context Managers - Efficiently Managing Resources; #100 Python Requests Tutorial: Request Web Pages, Download Images, POST Data, Read JSON, and More | `docs/08-glossary.md` for view, CTE, subquery |
| W8 | Parts 52-54 | #36 Generators - How to use them and the benefits you receive; #52 Logging Basics - Logging to Files, Setting Levels, and Formatting; #53 Logging Advanced - Loggers, Handlers, and Formatters | Python docs logging HOWTO; Python docs argparse tutorial |
| W9 | None new | #40 OOP Tutorial 1: Classes and Instances; #41 OOP Tutorial 2: Class Variables; #43 OOP Tutorial 4: Inheritance - Creating Subclasses; #42, #44, #45 optional; #46 Web Scraping with BeautifulSoup and Requests; #47 Working with JSON Data using the json Module | pdfplumber README; BeautifulSoup docs quick start; Python docs `json` |
| W10 | Window function videos: "Over clause in SQL Server"; "Row Number function"; "Rank and Dense Rank"; "Difference between rank dense rank and row number"; "Calculate running total"; "NTILE function"; "Lead and Lag functions" | None new | pytest docs; Real Python testing article, pytest section |
| W11 | Part 69 Merge in SQL Server | #124 Python Threading Tutorial; #146 Securely Manage Passwords and API Keys with DotEnv; #54 Hiding Passwords and Secret Keys in Environment Variables (Mac & Linux); #55 Hiding Passwords and Secret Keys in Environment Variables (Windows) | Snowflake docs: Python Connector; Real Python threading article |
| W12 | None new | None new | dbt Docs: about projects, sources, tests, snapshots, materializations; dbt Learn: dbt Fundamentals; Snowflake dbt Projects on Snowflake. Ask the program lead which path is default. |
| W13 | None new | None new | Great Expectations docs: quickstart and expectation gallery |
| W14 | None new | None new | Airflow docs: Fundamental Concepts and Best Practices |
| W15 | None new | None new | Metabase docs: Asking questions and Dashboards |

### The main SQL series: Kudvenkat SQL Server tutorial

The SQL track follows one series: "SQL Server tutorial for beginners" by Kudvenkat. Use this playlist: https://www.youtube.com/playlist?list=PL08903FB7ACA1C2FB

Kudvenkat teaches Microsoft SQL Server. We use Snowflake. Watch for the idea, type the Snowflake version, and run it in Snowflake.

Rule: Kudvenkat is old SQL Server. Watch for the idea. If a query does not run in Snowflake, you may ask an AI assistant to convert it to Snowflake SQL. You must read the converted query and explain it. Do not paste a finished deliverable.

Skip stored procedures, triggers, indexes, cursors, deadlocks, transaction isolation, dynamic SQL, and ASP.NET parts.

### Skip these Kudvenkat videos

- SQL Server install and SSMS clicks.
- Stored procedures, triggers, indexes, cursors, deadlocks, transaction isolation, dynamic SQL, and ASP.NET parts.
- Functions you create yourself (Parts 30-33).
- Temp tables.
- Error handling.
- Transactions.
- Locking and deadlocks.
- GUID.
- Sequences, except a short idea in S1 and S2.
- OFFSET FETCH.

| Part | Exact title | Use in this sprint |
|---|---|---|
| Part 1 | Connecting to SQL Server using SSMS | Watch first 5 min for server and database idea. Skip SSMS clicks. W1 setup if useful. |
| Part 2 | Creating, altering and dropping a database | Week 2 |
| Part 3 | Creating and working with tables | Week 2 |
| Part 4 | Adding a default constraint | Week 3 |
| Part 5 | Cascading referential integrity constraint | Week 3. Snowflake declares foreign keys but does not enforce them. |
| Part 6 | Adding a check constraint | Week 3 |
| Part 7 | Identity column | Week 3 |
| Part 8 | How to get the last generated identity column value | Optional |
| Part 9 | Unique key constraint | Week 3 |
| Part 10 | Select statement | Week 4 |
| Part 11 | Group by | Week 4 |
| Part 12 | Joins | Week 4 |
| Part 13 | Advanced or intelligent joins | Week 4 |
| Part 14 | Self join | Week 4 |
| Part 15 | Different ways to replace NULL | Week 3 only as needed for S2.x. Week 5 main use. |
| Part 16 | Coalesce function | Week 3 only as needed for S2.x. Week 5 main use. |
| Part 17 | Union and union all | Week 5 |
| unnumbered | "SQL Server except operator"; "Difference between except and not in"; "Intersect operator"; "Difference between union intersect and except"; "Difference between where and having" | Week 4 and week 5 |
| Part 22 | Built in string functions | Week 5 |
| Part 23 | LEFT, RIGHT, CHARINDEX and SUBSTRING | Week 5 |
| Part 24 | Replicate, Space, Patindex, Replace and Stuff | Week 5 |
| Part 25 | DateTime functions | Week 5 |
| Part 26 | IsDate, Day, Month, Year, DateName | Week 5 |
| Part 27 | DatePart, DateAdd and DateDiff | Week 5. Snowflake order is `DATEDIFF(part, start, end)`. |
| Part 28 | Cast and Convert | Week 5. Use `CAST` or `::`. Use `TRY_TO_DATE` or `TRY_CAST` for safe parsing. |
| Part 29 | Mathematical functions | Week 5 |
| Part 39 | Views | Week 7 |
| Part 40 | Updatable views | Week 7 |
| Parts 41-42 | Indexed views; view limitations | Optional |
| Part 48 | Derived tables and common table expressions | Week 7 |
| Part 49 | CTE | Week 7 |
| Part 50 | Updatable CTE | Optional |
| Part 51 | Recursive CTE | Optional |
| Part 52 | Database normalization | Week 8 |
| Part 53 | Second and third normal form | Week 8 |
| Part 54 | Pivot | Week 8 |
| unnumbered | "UNPIVOT in SQL Server" | Optional |
| Part 59 | Subqueries | Week 7 |
| Part 60 | Correlated subquery | Week 7 |
| Part 69 | Merge in SQL Server | Week 11. Snowflake MERGE uses the same WHEN MATCHED and WHEN NOT MATCHED idea. |
| unnumbered | "Over clause in SQL Server"; "Row Number function"; "Rank and Dense Rank"; "Difference between rank dense rank and row number"; "Calculate running total"; "NTILE function"; "Lead and Lag functions"; "FIRST VALUE function"; "LAST VALUE function" | Week 10 |

Earlier versions of this file had wrong Kudvenkat part numbers. Use this table. The week numbers here are the new 15-week numbers. If your week problem statement points to a different number, tell the program lead.

### Which Corey Schafer videos to skip

Corey Schafer Python playlist: https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU

Corey now has videos for type hints (#154 and #155), pydantic (#157), threading (#124), and pandas (#128-#138). He does not have a verified pytest video in this playlist. Use pytest docs for tests. For argparse, use the Python docs tutorial.

Skip these video numbers: #10-#12, #15-#17, #22, #23, #26, #27, #29, #32-#34, #39, #48, #50, #51, #58, #59, #61-#99, #101-#104, #107, #109, #112-#123, #126, #139-#145, #147, #148, #150, #151, #156, #158.

This list covers all Flask and Django videos, the Sublime, Atom, and Eclipse setup videos, Anaconda, Pipenv, Pillow, Jupyter, SQLite, interview prep, Easter egg, random module, automate renaming, itertools, custom domain and HTTPS videos, and "Calculate Number of Days...".

These videos exist and are useful, but are optional for this sprint:

- #19 Slicing Lists and Strings
- #20 Comprehensions - How they work and why you should be using them
- #21 Sorting Lists, Tuples, and Objects
- "Python Tutorial: String Formatting - Advanced Operations..."
- #35 F-Strings - How to Use Them and Advanced String Formatting
- "Python Tutorial: OS Module"
- "Python Tutorial: Namedtuple"
- #76 Sets - Set Methods and Operations
- #90 Iterators and Iterables
- #57 Unit Testing Your Code with the unittest Module. Watch for the idea of a test, then use pytest docs.
- #42, #44, #45 OOP Tutorial 3, 5, and 6. #40, #41, and #43 are required in week 9.

### Snowflake vs SQL Server differences

When the video and Snowflake disagree, run it in Snowflake and note the difference in your notes file.

| Kudvenkat shows | In Snowflake |
|---|---|
| `IDENTITY` columns | Use `AUTOINCREMENT` |
| Enforced `UNIQUE` and `CHECK` | Declared but not enforced. `NOT NULL` is enforced. |
| `PRIMARY KEY` and `FOREIGN KEY` | Declared but not enforced |
| `ISNULL()` | `IFNULL()` or `COALESCE()` |
| `GETDATE()` | `CURRENT_TIMESTAMP()` |
| `TOP 10` | `LIMIT 10` |
| `MINUS` not available | `EXCEPT` and `MINUS` both work |
| `DATEDIFF` argument order | `DATEDIFF(part, start, end)` |
| `CAST` and `CONVERT` | `CAST` or `::`. Use `TRY_TO_DATE` or `TRY_CAST` for safe parsing. |
| `LEN()` | `LENGTH()` |

### Snowflake

Read the Snowflake docs for load work, not blog posts. Use https://docs.snowflake.com and search for the exact title.

| Source | What exactly | Feeds |
|---|---|---|
| Snowflake docs | Search "Key Concepts and Architecture". Read the storage and compute separation. | Week 1 setup, D track |
| Snowflake docs | Search "Loading Data". Read stages, then file formats, then `COPY INTO`. | Week 3 practice kit, week 6 load |
| Snowflake docs | Search "COPY INTO <table>". Read `ON_ERROR` options and load metadata columns. | Week 6 |
| Snowflake docs | Search "load data using the web interface". Use the Snowsight wizard. Menu names can differ. | Week 3 |
| Snowflake docs | Search "Python Connector". Connect, execute, close. | Week 11 |

### Python

| Source | What exactly | Feeds |
|---|---|---|
| Corey Schafer playlist | #1-#9 Beginners 1-9 | Week 2 |
| Python docs tutorial https://docs.python.org/3/tutorial/ | Sections 4 and 5: control flow, then data structures | Week 2 |
| Corey Schafer | #25 File Objects - Reading and Writing to Files; #28 CSV Module - How to Read, Parse, and Write CSV Files; #56 if __name__ == '__main__'; #127 Zip Files - Creating and Extracting Zip Archives | Week 3 |
| Python docs `csv` https://docs.python.org/3/library/csv.html | Read and write CSV files | Week 3 |
| Corey Schafer | #31 Using Try/Except Blocks for Error Handling | Week 4 |
| Corey Schafer | #24 Datetime Module - How to work with Dates, Times, Timedeltas, and Timezones | Week 4 |
| Corey Schafer | #154 Type Hinting vs Type Checking vs Data Validation; #155 Type Hints - From Basic Annotations to Advanced Generics | Week 4 |
| Python docs `typing` https://docs.python.org/3/library/typing.html | Function annotations | Week 4 |
| pydantic docs https://docs.pydantic.dev | Models page: model definition and validation errors | Week 4 |
| Corey Schafer | #157 Python Pydantic Tutorial: Complete Data Validation Course | Week 4 |
| Corey Schafer | #37 Decorators - Dynamically Alter The Functionality Of Your Functions; #38 Decorators With Arguments | Week 7 |
| Corey Schafer | #49 Context Managers - Efficiently Managing Resources | Week 7 |
| Corey Schafer | #100 Python Requests Tutorial: Request Web Pages, Download Images, POST Data, Read JSON, and More | Week 7 |
| Corey Schafer | #36 Generators - How to use them and the benefits you receive | Week 8 |
| Corey Schafer | #52 Logging Basics - Logging to Files, Setting Levels, and Formatting; #53 Logging Advanced - Loggers, Handlers, and Formatters | Week 8 |
| Python docs logging https://docs.python.org/3/howto/logging.html and https://docs.python.org/3/library/logging.html | Basic configuration and levels | Week 8 |
| Python docs argparse https://docs.python.org/3/library/argparse.html | Positional and optional arguments, help text | Week 8 |
| Corey Schafer | #40 OOP Tutorial 1: Classes and Instances; #41 OOP Tutorial 2: Class Variables; #43 OOP Tutorial 4: Inheritance - Creating Subclasses. #42, #44, #45 optional. | Week 9 |
| Beautiful Soup docs https://www.crummy.com/software/BeautifulSoup/bs4/doc/ | Quick start: find a table, iterate rows | Week 9 |
| pdfplumber https://pypi.org/project/pdfplumber/ | `extract_table` and its quirks | Week 9 |
| Corey Schafer | #47 Working with JSON Data using the json Module | Week 9 |
| pytest docs https://docs.pytest.org | Write and run a test, use fixtures | Week 10 |
| Real Python https://realpython.com | Testing article, pytest section | Week 10 |
| Corey Schafer | #54 Hiding Passwords and Secret Keys in Environment Variables (Mac & Linux); #55 Hiding Passwords and Secret Keys in Environment Variables (Windows); #146 Securely Manage Passwords and API Keys with DotEnv | Week 11 |
| Real Python https://realpython.com | Threading article | Week 11 |
| Corey Schafer | #124 Python Threading Tutorial | Week 11 |
| Snowflake docs | Search "Python Connector" | Week 11 |

### dbt

dbt arrives in week 12. For this cohort, the default is dbt Core (Path A). The program lead confirms this. dbt Projects on Snowflake is an optional extra.

Path A: dbt Core on your laptop in VS Code. Install `dbt-snowflake`. Use a local `profiles.yml` that is never committed. Run `dbt run` and `dbt test` in the VS Code terminal.

Path B: dbt Projects on Snowflake. Open a Workspace in Snowsight and create or connect a dbt project. Run the project or a single model from the editor. If you use Path B, run `dbt deps` inside the workspace and enable secondary roles.

Both paths use the same `.sql` and `.yml` files from the repo's `platform/dbt` folder layout.

| Source | What exactly | Feeds |
|---|---|---|
| dbt Docs https://docs.getdbt.com | About dbt projects, sources, add tests, snapshots, materializations | Week 12 |
| dbt Learn https://learn.getdbt.com | dbt Fundamentals. Free. Skip dbt Cloud UI parts if you use dbt Core. | Week 12 |
| Snowflake docs https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake | dbt Projects on Snowflake overview | Week 12 |
| Snowflake docs https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake-using-workspaces | Using dbt in a Snowflake Workspace | Week 12 |

### Data modelling

| Source | What exactly | Feeds |
|---|---|---|
| Kimball Group https://www.kimballgroup.com | Fact tables, dimension tables, and grain. Grain means what one row means. | Week 6, week 10, week 12 |
| Kimball Group https://www.kimballgroup.com | Slowly changing dimensions, types 1, 2, and 3. Type 2 is SCD2. | Week 10, week 11, week 12 |
| `docs/01-project-brief.md` | Star schema and Medallion layers: bronze, silver, gold | Week 6, week 12 |
| Your week 6 problem statement | Medallion and ADR (architecture decision record) | Week 6 |

### Diagrams and architecture

| Source | What exactly | Feeds |
|---|---|---|
| Mermaid docs https://mermaid.js.org | Flowcharts: nodes, edges, subgraphs in plain Markdown | A track, D track |
| GitHub Docs https://docs.github.com | Search "Mermaid support in Markdown". Diagrams render in `.md` files, issues, and pull requests. | A track, D track |
| Your program lead | ADR template. An ADR is a short record of one design decision. | Week 6, B track |

Mermaid is text, so a review can comment on it line by line. A committed diagram also shows up in pull request diffs. It is saved in the repo, not only drawn in a meeting.

### Data quality and orchestration

| Source | What exactly | Feeds |
|---|---|---|
| Great Expectations docs https://docs.greatexpectations.io | Try GX Core quickstart: expectations, suites, validation results | Week 13 |
| Great Expectations docs https://docs.greatexpectations.io | Expectation gallery: look up `expect_column_value_lengths_to_equal` and `expect_column_values_to_be_in_set` | Week 13 |
| Airflow docs https://airflow.apache.org/docs/ | Fundamental Concepts: DAGs (a graph of tasks and their order), tasks, dependencies with `>>` | Week 14 |
| Airflow docs https://airflow.apache.org/docs/ | Best Practices. Read the idempotency section. Idempotency means you can run it again without changing the result. | Week 14 |
| Metabase docs https://www.metabase.com/docs/latest/ | Asking questions and Dashboards | Week 15 |

### Git and reviewing

| Source | What exactly | Feeds |
|---|---|---|
| Pro Git https://git-scm.com/book/en/v2 | Chapters 1 and 2: basics, then branching | Week 1, all weeks |
| GitHub Docs https://docs.github.com | Fork, branch, pull request | Week 1 |
| GitHub Docs https://docs.github.com | Pull request reviews. Leave a comment on a line. The class repo never squash-merges. | All weeks |
| Chris Beams https://cbea.ms/git-commit/ | How to Write a Git Commit Message, the seven rules | All weeks |
| `CONTRIBUTING.md` | This repo's branch, commit, and pull request rules | Week 1, all weeks |

### The data sources themselves

Bookmark these. You will return to them all sprint.

| Source | Where | Format | Used by |
|---|---|---|---|
| MCA company registry | https://data.gov.in, Company Master Data catalog | CSV per RoC office, inside ZIP. RoC means Registrar of Companies. | Week 2 download, week 3 practice kit load, S and D tracks |
| IBBI insolvency data | https://ibbi.gov.in | PDF | Week 9, D track |
| MCA CDM statistics | https://mcacdm.nic.in | HTML tables | Week 9, D track |
| RBI policy rates | https://data.rbi.org.in, the DBIE portal (Database on Indian Economy) | CSV or HTML | Week 7, D track |
| MCA data dictionary | Published with the registry catalog on https://data.gov.in | PDF | Week 2, every column decision |

The RBI policy rate is a step series. A step series means the value changes only at some dates. A row exists only on the day the rate changes. The cohort file ends in 2024. That is OK. The project uses RBI as background only: "RBI data as available, ending 2024." Carry the last rate forward. The last row has no end date. Record the last date in the file in your notes. Check RBI press releases at https://www.rbi.org.in. Optional stretch, not required: add later rate changes by hand from press releases and record the source link and date.

Read the MCA data dictionary in week 2. Most week 3 and week 4 column confusion is answered there.

Downloaded data goes in `data/raw/...` and is never committed.

### Reading for the business track

| Source | What exactly | Feeds |
|---|---|---|
| `docs/01-project-brief.md` | Client story, the four questions, and the final product | B track |
| `docs/05-task-list.md` | Task IDs and delivery order | B track |
| One RBI press release at https://www.rbi.org.in | Skim one. See how rate decisions are announced. | B track, week 7 context |
| One IBBI page at https://ibbi.gov.in | Skim one CIRP list. CIRP means corporate insolvency resolution process. | B track, week 9 context |

### Search keywords if you get stuck

- Snowflake `COPY INTO`
- Snowflake load data using the web interface
- Kudvenkat part numbers
- Corey Schafer CSV Module
- pytest fixtures
- dbt projects on Snowflake

## 5. Step-by-Step Guide

Work in small steps. When you do a task, use one task, one file, one commit. See `docs/03-student-guide.md`.

### How to use this file

Do not read ahead. Open the row for your week. Do the reading in study time, then do the tasks in build time.

Your week problem statement is the final word. If they disagree, tell the program lead so this file can be fixed.

In paths, `DEx` means your own number, for example DE12. All week files go inside your folder: `students/DEx/weekN/`.

Write SQL in VS Code as `.sql` files, then run it in Snowsight. Write and commit in VS Code. Snowsight Workspaces can pull from a public Git repo, but cannot push back.

Run Python in the VS Code terminal with `.venv` activated.

Downloaded data goes in `data/raw/...`. Never commit data files or secrets.

### Check it worked

- [ ] You can name the row for your week in the 15-week table.
- [ ] You know where to write SQL and where to run it.
- [ ] You know where downloaded data goes.
- [ ] You know which sources not to use.

### What not to use

Use an AI assistant to understand an error or a concept, not to write your deliverable. You must explain every line at the cohort review. If you cannot rebuild the thing from memory, it is not yours.

Avoid tutorials that use a toy dataset, meaning a small fake dataset. Our data has commas inside quoted fields, several spellings of one state, and dates in different formats. A tutorial built on a clean five-row CSV teaches habits that fail in week 3.

Avoid old blog or forum answers for Snowflake and dbt. Both tools changed. Use the official docs for anything version-sensitive.

## 6. What to Commit

This guide is reading material. It does not add a task file to your week folder. When you finish this guide, these points must be true:

- [ ] You found your week in the 15-week table.
- [ ] You bookmarked the data sources table.
- [ ] You know your DEx number and your week folder path.
- [ ] You know SQL is written in VS Code and run in Snowsight.
- [ ] You know Python runs in the VS Code terminal with `.venv` activated.
- [ ] You know downloaded data goes in `data/raw/...` and is never committed.
- [ ] You know the sources in "What not to use".
- [ ] You know the week problem statement is the final word.

No commit is needed for reading this guide. When you do a task, follow `docs/03-student-guide.md` and use one task, one file, one commit.
