# Task List

Every task in the sprint, in the order you do them. This file is built from the week files. Task IDs stay the same for the whole project and go in your commit message and pull request title.

Letters: G = Getting started, B = Business and Delivery, S = SQL and Data Modeling, P = Python, D = Data Platform, A = Architecture diagrams.

## Week 1 - Set up your tools and understand the ask

### Set up your tools

| ID | Task | What you do |
|---|---|---|
| `G0.1` | First commit and pull request | You make your first commit and your first pull request (PR) |
| `G0.2` | Snowflake first login and first query | You sign in to Snowflake for the first time, set up MFA, open a Workspace, and run a small check query |
| `G0.3` | Connect VS Code to Snowflake and run a .sql file | You install the official Snowflake extension in VS Code, sign in, and run a `.sql` file from VS Code |
| `G0.4` | Create your project folder on your computer | You create a practice team project folder on your computer |

### Discovery Brief **[MILESTONE]**

| ID | Task | What you do |
|---|---|---|
| `B1.1` | Write the discovery brief | You write a one-page discovery brief in your own words |
| `B1.2` | List the open questions | You add exactly five questions to the same discovery brief |
| `A1.1` | Draw the whole system on one page | You draw the whole system as one Mermaid diagram |

### Source Summaries and the Client Ask

| ID | Task | What you do |
|---|---|---|
| `B2.1` | Summarise the four sources | You visit the four data source websites, then write a short summary of each one |
| `B2.2` | Write the onboarding paragraph | You write one paragraph, 6 to 10 sentences, that a new teammate could read on their first day |

## Week 2 - First tables and first Python

### Databases and Tables

| ID | Task | What you do |
|---|---|---|
| `S1.1` | Create the database and schemas | You set up your isolated database with three schemas: BRONZE, SILVER and GOLD |
| `S1.2` | Create the raw MCA table | You create one table in BRONZE that matches your MCA CSV file, with one text column per CSV field |
| `S1.3` | List the non-empty columns | You list, in your notes file, the columns a real company record cannot do without, and you say why for each one |

### Python From Zero

| ID | Task | What you do |
|---|---|---|
| `P0.1` | Hello Python, variables and strings | You write your first Python script |
| `P0.2` | Lists and dictionaries | You write a script that holds a list of five Indian state names, a dictionary for one company with `cin`, `name` and `state`, and a small number variable |
| `P0.3` | Decisions and loops | You write a script that loops over your list of states, prints only the states that start with the letter K, and counts them |
| `P0.4` | Functions and imports | You write a function `count_long_names(names, min_length)` that counts how many names are long enough, and a `main` block that calls it |

## Week 3 - Python reads files; trust the data

### Python Basics: Files and Functions

| ID | Task | What you do |
|---|---|---|
| `P1.1` | Count rows in a CSV | You write a small Python script that opens a CSV file and counts its data rows |
| `P1.2` | Check the headers | You write a Python function that returns the column names of a CSV file, then compare the headers of two files |
| `P1.3` | Filter rows by state | You write a script that reads one CSV, keeps only the rows for one state, and writes a new CSV |

### Data Integrity: Constraints

| ID | Task | What you do |
|---|---|---|
| `S2.1` | Find empty CINs | You write and run a query that counts rows with an empty or `NULL` CIN |
| `S2.2` | Find duplicate CINs | You write a `GROUP BY` query that finds CINs that appear more than once |
| `S2.3` | Enforce one company, one CIN | You write a short note that says how the team will enforce the rule "one company, one CIN" |

## Week 4 - Queries, joins, and checking data with Python

### Queries, Grouping and Joins

| ID | Task | What you do |
|---|---|---|
| `S3.1` | Count companies by status | You write a GROUP BY query that counts companies by status |
| `S3.2` | Count active companies by state | You write a query that counts active companies by state and returns the top five |
| `S3.3` | Join IBBI to the registry | You write a LEFT JOIN that counts matched and unmatched rows between `RAW_IBBI_SAMPLE` and one RoC table |
| `S3.4` | Explain the unmatched CINs | You list unmatched IBBI CINs and write a two-sentence explanation of why they do not match |
| `S3.5` | Compare two monthly snapshots | You write a join between two snapshots: a current RoC table and `RAW_MCA_PREV`, joined on CIN, to find rows whose status changed |

### Type Hints and Data Validation

| ID | Task | What you do |
|---|---|---|
| `P2.1` | Add type hints | You add type hints to the three Python scripts from week 3 |
| `P2.2` | Write a validator by hand | You write a Python function that checks one insolvency record and returns a list of error messages |
| `P2.3` | Rewrite the validator with pydantic | You rewrite the validator as a pydantic model and compare it with your hand-written version |

## Week 5 - Missing values, messy strings

### Missing Values and Set Operations

| ID | Task | What you do |
|---|---|---|
| `S4.1` | Show missing capital | Find the rows where paid-up capital is empty, count them, and display them with the label "Not reported" |
| `S4.2` | UNION ALL vs UNION | Run the same combine of your three RoC tables twice, once with `UNION ALL` and once with `UNION`, and compare the row counts |
| `S4.3` | Compare two snapshots | Compare `RAW_MCA_PREV` with the current RoC table to find CINs only in the practice previous copy, CINs only in the current table, and CINs in both |
| `S4.4` | Defend the missing-stays-missing rule | Write a short paragraph in plain English that explains why an empty value stays empty |

### SQL Functions: String, Date, Math

| ID | Task | What you do |
|---|---|---|
| `S5.1` | Check CIN length | List the CINs whose length is not 21 characters, and count them |
| `S5.2` | Clean the state column | Uppercase and trim the state column, then map the spellings you actually see to one standard name with a `CASE` expression |
| `S5.3` | Parse registration dates | Look at the date strings, then parse them with `TRY_TO_DATE` so a bad row returns NULL instead of failing the query |
| `S5.4` | Compute recovery rates | Convert the two IBBI amount columns from text to numbers and compute a recovery percentage for each case |
| `S5.5` | Compute company age | Write two age columns, one with plain `DATEDIFF(year, ...)` and one corrected to completed years, then compare them |

## Week 6 - First load, and the design begins

### First Snowflake Load [MILESTONE]

| ID | Task | What you do |
|---|---|---|
| `P3.1` | Create the stage and file format | You create a named internal stage and a named CSV file format in your own Snowflake schema |
| `P3.2` | Load the first file | You upload one RoC CSV to the stage and run `COPY INTO` to load it into a raw table |
| `P3.3` | Load the remaining files | You load every remaining RoC file with the same proven pattern |

### Design Record, ERD and Architecture

| ID | Task | What you do |
|---|---|---|
| `B3.1` | Draw the star schema ERD | You draw the gold layer star schema as an ERD in a Markdown file with a Mermaid diagram |
| `B3.2` | Write the grain statements | You add a "Grain" section to your ERD file |
| `B3.3` | Write the modelling decision record | You write a short decision record for the gold layer design |

### Pipeline Flow Diagram

| ID | Task | What you do |
|---|---|---|
| `A2.1` | Draw the pipeline flow | You draw the pipeline flow in a Markdown file with a Mermaid flowchart |

## Week 7 - Readable SQL and the agreed model

### Views and CTEs

| ID | Task | What you do |
|---|---|---|
| `S6.1` | Rewrite the join as CTEs | Rewrite your S3.3 insolvency join as two CTEs |
| `S6.2` | Create the current-company view | Create a Snowflake view `v_company_current` that returns one current row per company |
| `S6.3` | Find above-average strike-off states | Find states whose strike-off rate this month is above the overall sample average |

### Star Schema Design [MILESTONE]

| ID | Task | What you do |
|---|---|---|
| `S7.1` | Write the gold grain statements | Write one grain sentence for each gold table: `dim_company`, `fct_cirp_event`, `dim_state`, and `dim_date` |
| `S7.2` | Mark dim_company columns as SCD 1 or 2 | List every planned column in `dim_company` and mark it Type 1, Type 2, or Key / version metadata |
| `S7.3` | Draw the final star schema | Draw the final star schema for the gold layer in a Mermaid code block |

### Retry Logic and Context Managers

| ID | Task | What you do |
|---|---|---|
| `P4.1` | Write a retry decorator | Write a retry decorator with a waiting gap that doubles |
| `P4.2` | Download the RBI file | Open the RBI data site, then save the RBI policy rate file by hand if needed |
| `P4.3` | Close files safely | Rewrite file handling in your earlier Python scripts to use `with open(...) as f:` |

## Week 8 - Big files and cleaner tooling

### Normalization

| ID | Task | What you do |
|---|---|---|
| `S8.1` | Explain the layer choices | You write a short Markdown notes file with three paragraphs |
| `S8.2` | Pivot company counts by state | You write one SQL query that shows one row per state, with a column for each company status |

### Generators and Large Files

| ID | Task | What you do |
|---|---|---|
| `P5.1` | Write a row generator | You write a Python function that gives you one row at a time from a big CSV file, and you count rows and empty capital values in one pass |
| `P5.2` | Build the state frequency map | You count how many times each state spelling appears in the MCA file, and you print the top twenty spellings |
| `P5.3` | Explain the memory win | You add three sentences to your notes file that explain how the generator and your week 3 script use memory differently as a CSV file grows |

### Logging and Command Line Tools

| ID | Task | What you do |
|---|---|---|
| `P6.1` | Replace prints with logging | You add logging setup to your week 7 RBI script and to your week 8 Python scripts, and you replace every `print()` with a log line that has a timestamp and a level |
| `P6.2` | Make the RBI download a CLI tool | You wrap your week 7 RBI download in a small command line tool |

## Week 9 - Extractors and the design sign-off

### Object Oriented Extractors

| ID | Task | What you do |
|---|---|---|
| `P7.1` | Write the IBBI PDF extractor | You will write a Python script that opens the IBBI insolvency PDF, extracts the table rows, validates them, and saves a clean CSV |
| `P7.2` | Write the CDM portal extractor | You will write a Python script that reads the MCA CDM state statistics table from the web page and saves the numbers as a clean CSV |
| `P7.3` | Refactor behind one base class | You will move your three extractors (RBI, IBBI, CDM) behind one base class, so each source has the same three steps: pull, validate, save |

### Design Review from the Python Side [MILESTONE]

| ID | Task | What you do |
|---|---|---|
| `P8.1` | Write the pipeline risk paragraph | You will write one short paragraph that names a real risk your Python extractors create for the pipeline design, and say what the design must change |

### Design Review Update

| ID | Task | What you do |
|---|---|---|
| `A3.1` | Update the pipeline flow diagram | You will copy your week 8 pipeline flow diagram into this week and update it with what the real extractors showed you, then write one line per change in a notes file |

## Week 10 - Window functions and tests

### Window Functions

| ID | Task | What you do |
|---|---|---|
| `S9.1` | RANK vs DENSE_RANK | You rank states by their count of active companies |
| `S9.2` | Month over month change | You compute month over month change in new company registrations |
| `S9.3` | Capital deciles | You split companies inside one state into ten groups by paid up capital |
| `S9.4` | Latest row per company | You practise keeping only the latest row for each CIN |

### Testing with pytest

| ID | Task | What you do |
|---|---|---|
| `P9.1` | Test the validator | You write pytest tests for the MCA validator |
| `P9.2` | Test the PDF parser offline | You test the IBBI PDF parser with a saved text sample, not with a live PDF download |
| `P9.3` | Run the tests in CI | You find out which CI workflow runs student tests, and you record proof that your tests ran in CI |

## Week 11 - History that survives

### Slowly Changing Dimensions (SCD2) [MILESTONE]

| ID | Task | What you do |
|---|---|---|
| `S10.1` | Detect the changes | You write a SQL query in VS Code that finds what changed between this month's table and last month's practice table, matched on CIN |
| `S10.2` | Apply the changes with MERGE | You write a MERGE statement in VS Code and run it in Snowflake on a test copy of `dim_company` |
| `S10.3` | Answer a point in time question | You write one SQL query in VS Code and run it in Snowflake |

### Parallel Downloads

| ID | Task | What you do |
|---|---|---|
| `P10.1` | Download in parallel | You write a Python script in VS Code that downloads files in a sequential loop, then in parallel with `ThreadPoolExecutor` |
| `P10.2` | Explain why threads help | You work in VS Code only |

### Python and Snowflake Together

| ID | Task | What you do |
|---|---|---|
| `P11.1` | Connect Python to Snowflake | You work in VS Code to write a Python script and a `.env` file |
| `P11.2` | Write the load verifier | You write a Python script in VS Code that compares a file row count with a table row count |

### Technical Brief [MILESTONE]

| ID | Task | What you do |
|---|---|---|
| `D1.1` | Write the technical brief | You work in VS Code only |
| `D1.2` | Draw the platform architecture | You work in VS Code only |

## Week 12 - The shared platform begins

### Stages and File Formats

| ID | Task | What you do |
|---|---|---|
| `D2.1` | Create the stages | You will create one Snowflake stage for each of the four sources |
| `D2.2` | Define the file formats | You will create named Snowflake file formats for the sources |
| `D2.3` | When is a Task the right tool? | You will write a short answer in your notes file |

### Bronze Loads

| ID | Task | What you do |
|---|---|---|
| `D3.1` | Load all MCA files | You will load all MCA RoC files into bronze tables |
| `D3.2` | Load the enrichment sources | You will load IBBI, CDM and RBI files into bronze tables |
| `D3.3` | Reconcile every table | You will compare the row count in each bronze table with the data row count in its source file |

### Teach-Back: the Load Path **[MILESTONE]**

| ID | Task | What you do |
|---|---|---|
| `D4.1` | Teach the load path | You will teach the manual load path to a teammate and watch your teammate do it on a different RoC file |

### dbt Staging Models

| ID | Task | What you do |
|---|---|---|
| `D5.1` | Initialize the dbt project | You will create the dbt project layout and declare the bronze tables as sources |
| `D5.2` | Write the MCA staging model | You will write the MCA staging model and its tests |
| `D5.3` | Write the remaining staging models | You will write the IBBI, CDM and RBI staging models and add tests |

## Week 13 - Gold layer and the monthly refresh

### Layer Contracts

| ID | Task | What you do |
|---|---|---|
| `D6.1` | Write the layer contract | You write a short contract for the bronze, silver and gold layers |
| `D6.2` | Map every source column | You make one map from each source column to its final gold home, or to a clear reason for dropping it |

### Gold Layer with dbt

| ID | Task | What you do |
|---|---|---|
| `D7.1` | Build dim_date | You build a date dimension model called dim_date |
| `D7.2` | Build SCD2 dim_company | You build an SCD2 company dimension |
| `D7.3` | Build the fact table and dim_state | You build the fact table for CIRP events and a dim_state table |

### The Monthly Refresh **[MILESTONE]**

| ID | Task | What you do |
|---|---|---|
| `P12.1` | Write the snapshot preparation script | You write a script that prepares the monthly snapshot |
| `P12.2` | Run the full refresh | You run the full monthly refresh end to end |

## Week 14 - Orchestrate, break, fix

### Quality Gates with Great Expectations

| ID | Task | What you do |
|---|---|---|
| `D8.1` | Write the raw expectations | Write an expectation suite for one raw MCA table and run it |
| `D8.2` | Write the gold gate | Write three checks against your gold tables and run them as a gate |
| `D8.3` | GE versus dbt | Write a short answer that compares Great Expectations and dbt tests |

### Airflow Orchestration

| ID | Task | What you do |
|---|---|---|
| `D9.1` | Write the monthly refresh DAG | Write an Airflow DAG for the monthly refresh and look at its graph |
| `D9.2` | Make the gate stop the pipeline | Change the DAG so a failed quality check stops the pipeline |

### Peer Review

| ID | Task | What you do |
|---|---|---|
| `B4.1` | Review two pull requests | Review two pull requests from teammates and log the links and comments |
| `B4.2` | Fix one piece of feedback | Apply one review comment to your own work and reply to the reviewer |

### Break, Fix and Postmortem

| ID | Task | What you do |
|---|---|---|
| `B5.1` | Find the injected failure | Find the failure the program leads put into the project data or pipeline, and write the evidence trail |
| `B5.2` | Write the postmortem | Write a one-page blameless postmortem for the failure you traced in Task 8 |

## Week 15 - Present and hand over

### Dashboard

| ID | Task | What you do |
|---|---|---|
| `B6.1` | Build the dashboard | Build the Metabase dashboard with four views |
| `B6.2` | Write the plain language labels | Write plain language labels and short explanations for the four dashboard views |

### Stakeholder Delivery

| ID | Task | What you do |
|---|---|---|
| `B7.1` | Write the presentation script | Write a five minute client presentation script |
| `B7.2` | Rehearse and log the hard questions | Ask a peer to play the client |

### Final Architecture for Handover

| ID | Task | What you do |
|---|---|---|
| `A4.1` | Draw the final architecture | Draw the final architecture and final star schema for handover |

### Project Handover [MILESTONE]

| ID | Task | What you do |
|---|---|---|
| `B8.1` | Write the runbook | Write the runbook |
| `B8.2` | Write the final README | Write the final README for the repo |
| `D10.1` | Run the clean handover test | Run the clean handover test |
| `D10.2` | Archive the project | Archive the project on GitHub |

