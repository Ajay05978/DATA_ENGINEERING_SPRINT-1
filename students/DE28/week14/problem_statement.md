# Week 14 - Orchestrate, break, fix

**Data Sprint 1 · Week 14 of 15 · Theme: schedule the pipeline, then break it on purpose and prove you can find it**

## 1. What We Are Doing

Read this whole file before you start. Then work through the task groups in order. This week you schedule the pipeline, add quality gates, review each other's work, break the pipeline on purpose, and write a blameless postmortem.

The task groups are:
- Group 1 - Quality Gates with Great Expectations: Task 1 (`D8.1`), Task 2 (`D8.2`), Task 3 (`D8.3`).
- Group 2 - Airflow Orchestration: Task 4 (`D9.1`), Task 5 (`D9.2`).
- Group 3 - Peer Review: Task 6 (`B4.1`), Task 7 (`B4.2`).
- Group 4 - Break, Fix and Postmortem: Task 8 (`B5.1`), Task 9 (`B5.2`).

**How your tasks are numbered:** the first letter is the group. B = Business and Delivery, S = SQL and Data Modeling, P = Python, D = Data Platform, A = Architecture. The ID (for example `D8.1`) goes in your commit message and in your pull request title.

## 2. Why We Are Doing It

The client is a fintech due-diligence company. Analysts check Indian companies: is it real and active (MCA), ever in insolvency (IBBI), how it compares to its state (MCA CDM), and the interest-rate backdrop (RBI).

A bad CIN or a status value nobody uses can give a wrong answer with no error. The quality gate stops bad data before it reaches gold. The Airflow DAG runs the monthly refresh in the right order. Peer review catches mistakes early. The postmortem shows the client what stops the same failure from happening again.

The RBI series is background context only. Our RBI data ends in 2024, and the last rate carries forward until a newer change appears. No new rows is normal, not missing data.

## 3. What to Learn First (Prerequisites)

### Before this week: what you must already have

| Check | Must already have | If it is missing |
|---|---|---|
| [ ] | Raw MCA tables in your own schema, names like `RAW_MCA_<ROC>`. They came from **week 3** (the practice data kit with the Snowsight wizard) and **week 6** (the proper stage + `COPY INTO` load). | Redo the week 6 load. Your ZIP files are still in `data/raw/mca/`. |
| [ ] | `RAW_IBBI_SAMPLE`, from **week 3**, or the real IBBI table from **week 9**. | Load the sample again with the week 3 steps. |
| [ ] | `dim_company` with `valid_from` and `valid_to`, and `fct_cirp_event`, from **week 11**. | Re-run your week 11 SCD2 work. |
| [ ] | A dbt Core project with staging models and marts that build, from **week 12**. | Open the VS Code terminal, activate `.venv`, and run `dbt build`. Read the first error. |
| [ ] | A working `.venv` in the repo, from **week 2** (task P0.4). Check: `source .venv/bin/activate` then `python --version`. | Ask the program lead in the cohort channel if the check fails. |
| [ ] | The Airflow URL from the program lead, and working access to deploy and run a DAG. | Ask the program lead in the cohort channel before Task 5. Do not start Task 5 until a DAG can run. |
| [ ] | At least one of your own pull requests open with actionable review feedback, so you can act on it in Task 7. If your original pull request is already merged, plan a follow-up pull request linked to it. | Ask the program lead in the cohort channel. |

Missing something? Do not skip the task. Ask the program lead in the cohort channel.

### By the end of this week you can

- Write expectation suites and a gate that stops bad data before it reaches gold
- Orchestrate the monthly refresh so the gate stops the pipeline before gold is touched
- Review someone's work usefully, and fix your own in response to feedback
- Find an injected failure from evidence and write a blameless postmortem

### Core concepts to review first

- **Orchestrator:** a tool that runs your steps on a schedule, in the right order, and keeps the logs. Airflow is ours.
- **DAG:** a list of steps with arrows. Arrow means "this step must finish before that one starts".
- **Quality gate:** a step that checks the data and stops the pipeline if the check fails. Nothing after the gate runs.
- **Blameless postmortem:** describe what the system allowed to happen, not who did it.
- **SCD2 checks:** the three gold checks are the SCD2 correctness checks from week 11, now automated.
- **Idempotency:** read only the idempotency section of the Airflow "Best Practices" page before Task 4.

> **Three words in plain language**
> **Orchestrator:** a tool that runs your steps on a schedule, in the right order, and keeps the logs. Airflow is ours.
> **DAG:** a list of steps with arrows. Arrow means "this step must finish before that one starts".
> **Quality gate:** a step that checks the data and stops the pipeline if the check fails. Nothing after the gate runs.

## 4. Where to Find Resources

> **Where you work**
> - **VS Code:** write files, save them, commit, open pull requests.
> - **Snowflake (Snowsight in the browser):** run SQL. Open Projects > Workspaces, pick role `STUDENT_ROLE` and warehouse `STUDENT_WH`, and work in your own schema.
> - **VS Code terminal:** run Python (with `.venv` activated) and Git commands.
> - **Airflow (browser):** the URL the program lead shared with you. You do not install Airflow yourself.
> - **dbt:** dbt Core, run in the VS Code terminal (installed in week 12). The optional extra, dbt Projects on Snowflake, is not needed this week.
> - Full paths use `DEx` = your own number. Example: DE12. So `students/DEx/week14/...` means `students/DE12/week14/...` for you.

### Your files this week

All of them live inside your own week folder.

| File | Path |
|---|---|
| Raw suite | `students/DEx/week14/quality/expectations/mca_suite.json` (or `.py`, see Task 1) |
| Quality notes | `students/DEx/week14/quality/notes.md` |
| Gold suite | `students/DEx/week14/quality/expectations/gold_suite.json` |
| Gold checks SQL | `students/DEx/week14/quality/gold_checks.sql` |
| Airflow DAG | `students/DEx/week14/airflow/monthly_refresh_dag.py` |
| Airflow notes | `students/DEx/week14/airflow/notes.md` |
| Review log | `students/DEx/week14/reviews/review_log.md` |
| Break-fix notes | `students/DEx/week14/postmortem/break_fix_notes.md` |
| Postmortem | `students/DEx/week14/postmortem/postmortem.md` |

Data files stay out of Git. Never commit a password or a connection string.

### Docs to read before you start

| If you need | Read this |
|---|---|
| The rules before you touch `platform/`, `airflow/` or `quality/` | Work in your own week folder. Only the team leader mirrors reviewed files into `platform/` by pull request. See `docs/03-student-guide.md` and `CONTRIBUTING.md`. |
| Git commands (the same ones as every week) | `docs/03-student-guide.md` |
| The meaning of a word | `docs/08-glossary.md` |
| Help when a tool breaks | `docs/10-troubleshooting.md` |
| What a tool is for | `docs/11-tools-and-technology.md` |

### Your week in order

This is your week in order. Learn first, then do.

| Step | Learn first (what and where) | Time | Then do |
|---|---|---|---|
| **1** | Great Expectations docs: "Try GX Core" quickstart, the Snowflake connection page, then the expectation gallery pages for `expect_column_value_lengths_to_equal` and `expect_column_values_to_be_in_set` | 60 min | Task 1, Task 2 |
| **2** | Airflow docs: "Fundamental Concepts" tutorial, then "Best Practices", idempotency section | 60 min | Task 4, Task 5 |
| **3** | dbt docs: search for "data tests" and skim the page | 10 min | Task 3 |
| **4** | GitHub Docs "About pull request reviews"; the in-file review guidance in Task 6; the in-file postmortem guidance in Task 9 | 55 min | Task 6, Task 7, Task 8, Task 9 |
| **5** | - | - | Cohort review: your gold gate results and your postmortem walkthrough |

### Search keywords if you get stuck

- Great Expectations GX Core quickstart
- Airflow Fundamental Concepts
- Airflow Best Practices idempotency
- dbt data tests
- GitHub pull request reviews
- blameless postmortem

## 5. Step-by-Step Guide

Work one task at a time. One task, one file, one commit. Use `docs/03-student-guide.md` for the Git commands and the pull request steps. Do not squash-merge.

> **Team project repo**
> Each team keeps its own project folder and Git repo. This week the team repo gets a `quality/` folder and an `airflow/` folder, each with a short README line. The team leader mirrors only reviewed files from the team repo into the class repo `platform/quality/` and `platform/orchestration/` by pull request. Everyone else keeps practice files in `students/DEx/week14/`. See `docs/03-student-guide.md` for Git and pull request steps.

> **How your script logs in**
> Your Python scripts and dbt need to sign in to Snowflake. The program lead sets the method for the cohort: key-pair authentication or a Programmatic Access Token (PAT). Snowflake requires MFA for people who sign in to Snowsight with a password, so scripts use key-pair or PAT instead. Never paste a password into code or into a file. If a script login fails with an MFA message, ask the program lead and use the docs page they give you for the exact parameter names.

### Group 1 - Quality Gates with Great Expectations

#### Task 1 - Write the raw expectations (ID `D8.1`)

##### 1. What We Are Doing

Write an expectation suite for one raw MCA table and run it. You work in VS Code first to write the suite file. Then you use the VS Code terminal with `.venv` activated to run it. Then you use VS Code again to commit. Your script signs in to Snowflake with the cohort login method (see the box above). Use Snowsight > Projects > Workspaces only if you want to look at the rows: role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own schema.

##### 2. Why We Are Doing It

Analysts answer due-diligence questions from these rows. A CIN that is 12 characters long, or a status value nobody uses, gives a wrong answer with no error.

##### 3. What to Learn First

- Great Expectations docs, "Try GX Core" quickstart (search that title on docs.greatexpectations.io), about 40 min.
- Great Expectations docs: the Snowflake connection page and the page for the cohort login method (key-pair or PAT). Install the extra dependencies the page lists, set the connection from environment variables, and make `main()` exit with a non-zero code when a check fails. This is the runnable entry point Airflow will call in Task 5.
- Then the expectation gallery pages for `expect_column_value_lengths_to_equal` and `expect_column_values_to_be_in_set`, about 20 min.
- Skip the hosting and cloud parts.

##### 4. Where to Find Resources

- Great Expectations docs, "Try GX Core" quickstart (search that title on docs.greatexpectations.io), about 40 min.
- Great Expectations docs: Snowflake connection page and the cohort login method page (search "Snowflake" on docs.greatexpectations.io).
- Expectation gallery pages for `expect_column_value_lengths_to_equal` and `expect_column_values_to_be_in_set`, about 20 min.
- Search keywords: "Try GX Core", "Great Expectations Snowflake connection", "expect_column_value_lengths_to_equal", "expect_column_values_to_be_in_set".

##### 5. Step-by-Step Guide

1. In VS Code, open the terminal and run `source .venv/bin/activate` (Windows: `.venv\Scripts\activate`).
2. Run `pip install great_expectations`. Wait for it to finish. Install the extra packages named on the GX Snowflake page.
3. In VS Code, right-click `students/DEx/week14` > New Folder, name it `quality`. Inside it make `expectations`.
4. Create the suite file (see **Save as**). Starter skeleton if your setup uses Python:

```python
# students/DEx/week14/quality/expectations/mca_suite.py
# Copy the connect and add-expectation lines from the GX quickstart
# and the GX Snowflake connection page.
import great_expectations as gx  # check the quickstart for the exact import

def main():
    # TODO 1: connect to one raw table, RAW_MCA_<ROC>, in your own schema
    # Use the GX Snowflake connection page and the cohort login method.
    # TODO 2: expect CIN length to equal 21
    # TODO 3: expect status values to be in your allowed list
    # TODO 4: expect no fully empty row
    # TODO 5: expect state names to be in your known list
    # TODO 6: save the run result, print it, and exit with code 1 if any check fails
    pass

if __name__ == "__main__":
    main()
```

5. Build reviewed allowed status and known state lists from the MCA data dictionary that comes with the data.gov.in catalog and from the documented normalization rules. Write the lists in `quality/notes.md` (or a small reference file you commit). Do not build them with `SELECT DISTINCT ... LIMIT 50` on the same rows you check: existing invalid values would pass, and `LIMIT 50` can omit valid values. Then validate the raw data against those reviewed lists.
6. Run it from the repo root: `python students/DEx/week14/quality/expectations/mca_suite.py`.
7. Write the run result in `students/DEx/week14/quality/notes.md`: how many rows checked, how many failed, one sentence on what you will fix.

**Save as:** `students/DEx/week14/quality/expectations/mca_suite.json`, or `mca_suite.py` if your setup writes Python. Say in your notes which one you used.

If you get stuck: read the full error text first; search for the exact expectation name on docs.greatexpectations.io; ask in Discord with the command and the error text.

How to think about it: start with checks you can see in the data. Build the allowed lists from the MCA data dictionary and the documented normalization rules, not from memory and not from the same rows you check.

##### 6. What to Commit

- [ ] `students/DEx/week14/quality/expectations/mca_suite.json` or `students/DEx/week14/quality/expectations/mca_suite.py`
- [ ] `students/DEx/week14/quality/notes.md`

Done when:
- [ ] The suite covers all four checks: CIN length, allowed status, no fully empty row, state in list, and the allowed lists are reviewed reference lists in `quality/notes.md`.
- [ ] A run result is pasted in `quality/notes.md`.
- [ ] The suite file and the notes file are committed.

Commit message: `D8.1 add raw MCA expectation suite`

#### Task 2 - Write the gold gate (ID `D8.2`)

##### 1. What We Are Doing

Write three checks against your gold tables and run them as a gate. You work in VS Code to write the gold suite. Then you use the VS Code terminal to run it. Then you use VS Code to commit. The tables are in Snowflake: role `STUDENT_ROLE`, warehouse `STUDENT_WH`, the database and schema where your week 12 dbt mart models were built. Use Snowsight > Projects > Workspaces to run SQL.

##### 2. Why We Are Doing It

The gold tables answer the client's questions. If a CIRP event points at a company that is not in `dim_company`, the dashboard shows a number that cannot be traced.

##### 3. What to Learn First

Nothing new. You use the GX quickstart from Task 1, and the SCD2 rules from week 11. Reread your week 11 notes for 5 minutes.

##### 4. Where to Find Resources

- Nothing new. Use the GX quickstart from Task 1.
- Reread your week 11 notes for 5 minutes.
- Search keywords: "SCD2 valid_from valid_to", "self join overlap check", "LEFT JOIN IS NULL", "Great Expectations gold gate".

##### 5. Step-by-Step Guide

1. Create `students/DEx/week14/quality/expectations/gold_suite.py` with three TODOs, one per check:

```python
# Check 1: every CIN in fct_cirp_event exists in dim_company
# Check 2: every valid_to is after its valid_from
# Check 3: no company has two periods that overlap
```

2. To find the bad rows while you build the check, write the SQL in VS Code and save it as `students/DEx/week14/quality/gold_checks.sql`. Then run it in Snowsight > Projects > Workspaces (role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own schema). For check 1 the idea is a `LEFT JOIN` plus `WHERE ... IS NULL` (Kudvenkat Part 12 "Joins" and Part 13 "Advanced or intelligent joins"). For check 3 the idea is a self join on the same company (Part 14 "Self join"). Watch for the idea, then write and run the Snowflake version. Keep the final working queries in the saved `gold_checks.sql` file.
3. Run the suite from the repo root: `python students/DEx/week14/quality/expectations/gold_suite.py`.
4. Paste the result of each of the three checks into `students/DEx/week14/quality/notes.md`.
5. If a check fails, decide: is the data wrong, or is the rule wrong? Never change the threshold to make it pass.

**Save as:** `students/DEx/week14/quality/expectations/gold_suite.json` (or `.py`), the SQL in `students/DEx/week14/quality/gold_checks.sql`, and the results in `students/DEx/week14/quality/notes.md`.

If you get stuck: read the SQL error or the Python error in full; search for "self join overlap check" or "LEFT JOIN IS NULL"; ask in Discord with the check number and the error text.

How to think about it: a gold gate is a rule about meaning. Ask what answer the table must support.

##### 6. What to Commit

- [ ] `students/DEx/week14/quality/expectations/gold_suite.json` or `students/DEx/week14/quality/expectations/gold_suite.py`
- [ ] `students/DEx/week14/quality/gold_checks.sql`
- [ ] `students/DEx/week14/quality/notes.md`

Done when:
- [ ] All three checks exist and have been run against your built marts.
- [ ] Each check has a pass or fail result pasted in the notes.
- [ ] Any fail has one sentence saying what you will fix.
- [ ] The final SQL queries are saved in `students/DEx/week14/quality/gold_checks.sql`.

Commit message: `D8.2 add gold layer gate`

#### Task 3 - GE versus dbt (ID `D8.3`)

##### 1. What We Are Doing

Write a short answer that compares Great Expectations and dbt tests. You work in VS Code. dbt here means dbt Core, the version you installed in week 12 and run in the VS Code terminal.

##### 2. Why We Are Doing It

The client will ask why there are two testing tools. You need a short, honest answer.

##### 3. What to Learn First

No video. Search the dbt docs (docs.getdbt.com) for "data tests" and skim the page, about 10 min. This is enough.

##### 4. Where to Find Resources

- dbt docs (docs.getdbt.com): search for "data tests" and skim the page, about 10 min.
- Search keywords: "dbt data tests", "Great Expectations vs dbt tests", "pipeline quality gate".

##### 5. Step-by-Step Guide

1. Think about these questions, then answer them in three sentences:
   - Where does each tool run: inside the dbt build, or as a separate step before gold?
   - How are results reported: dbt test output, or GX run results and data docs?
   - How does each tool connect to the pipeline and stop it before gold is published?
   - What would you lose if the project kept only one of the two?
2. Write exactly three sentences. No bullet lists.

**Save as:** append to `students/DEx/week14/quality/notes.md`.

If you get stuck: reread the dbt "data tests" page; keep the answer to three sentences; ask in Discord with your draft.

How to think about it: keep the answer to three sentences: one difference, one place each tool runs, one reason to keep both.

##### 6. What to Commit

- [ ] `students/DEx/week14/quality/notes.md`

Done when:
- [ ] The answer is three sentences long.
- [ ] It explains one practical difference supported by the documentation.
- [ ] It gives one reason the project runs both.

Commit message: `D8.3 compare Great Expectations and dbt tests`

> **On Task 2:** the three checks are the SCD2 correctness checks from week 11, now automated. That is the point: a rule you must remember to run by hand is a rule that one day does not get run.

> **When an expectation fails, fix the data or the rule, never the threshold.** Loosening a check until it passes is the failure this group exists to stop.

### Group 2 - Airflow Orchestration

#### Task 4 - Write the monthly refresh DAG (ID `D9.1`)

##### 1. What We Are Doing

Write an Airflow DAG for the monthly refresh and look at its graph. You work in VS Code to write the DAG file. Then you use the Airflow web page in your browser to load and look at it. Then you use VS Code to commit.

##### 2. Why We Are Doing It

The pipeline checks for available updates each month. IBBI updates are about quarterly. Our RBI data ends in 2024; carry the last rate forward. Scheduling the pipeline means nobody has to remember the order of the steps.

##### 3. What to Learn First

- Airflow docs (airflow.apache.org/docs/): the "Fundamental Concepts" tutorial, about 40 min.
- Then "Best Practices", read only the idempotency section, about 20 min.
- Skip the installation pages, you do not install Airflow.

##### 4. Where to Find Resources

- Airflow docs (airflow.apache.org/docs/): "Fundamental Concepts" tutorial, about 40 min.
- Airflow docs: "Best Practices", idempotency section, about 20 min.
- Search keywords: "Airflow Fundamental Concepts", "Airflow Best Practices idempotency", "Airflow DAG dependencies", "Airflow graph view".

##### 5. Step-by-Step Guide

1. In VS Code, make the folder `students/DEx/week14/airflow`.
2. Create the DAG file. Starter skeleton with seven placeholders:

```python
# students/DEx/week14/airflow/monthly_refresh_dag.py
import datetime
from airflow import DAG
from airflow.operators.empty import EmptyOperator  # check your Airflow version in the docs

with DAG(
    dag_id="monthly_refresh",
    start_date=datetime.datetime(2026, 1, 1),
    schedule="@monthly",
    catchup=False,
) as dag:
    prepare_snapshot = EmptyOperator(task_id="prepare_snapshot")  # TODO: real step
    load_bronze = EmptyOperator(task_id="load_bronze")            # TODO: real step
    validate_raw = EmptyOperator(task_id="validate_raw")          # TODO: real step
    build_staging = EmptyOperator(task_id="build_staging")        # TODO: real step
    build_candidate_marts = EmptyOperator(task_id="build_candidate_marts")  # TODO: real step
    validate_candidate_marts = EmptyOperator(task_id="validate_candidate_marts")  # TODO: real step
    publish_gold = EmptyOperator(task_id="publish_gold")          # TODO: real step

    prepare_snapshot >> load_bronze >> validate_raw
    validate_raw >> build_staging >> build_candidate_marts
    build_candidate_marts >> validate_candidate_marts >> publish_gold
```

3. Decide the real order. The gold tables must be published after candidate-mart validation passes, not before. `publish_gold` is the only task that writes the gold tables the dashboard reads. The candidate-mart validation checks the proposed marts before publication.
4. Describe or draw the graph in `students/DEx/week14/airflow/notes.md`. A few lines of text with arrows is enough. Add one line on RBI: our RBI file ends in 2024, and the last rate carries forward until a newer change appears, so no new rows is normal.
5. Copy the file into the Airflow environment the program lead set up, or upload it the way they told you, and look at the graph view. If you cannot load it or see the graph, ask the program lead before Task 5. Task 5 needs a real Airflow run.

**Save as:** `students/DEx/week14/airflow/monthly_refresh_dag.py` plus `students/DEx/week14/airflow/notes.md`.

If you get stuck: open the Airflow docs page "Fundamental Concepts" again; check the graph view for missing arrows; ask in Discord with the DAG file and the error text.

How to think about it: think of the DAG as a dependency map. Ask which task publishes gold, and make candidate-mart validation finish before it.

##### 6. What to Commit

- [ ] `students/DEx/week14/airflow/monthly_refresh_dag.py`
- [ ] `students/DEx/week14/airflow/notes.md`

Done when:
- [ ] The DAG has prepare, bronze load, raw validation, staging build, candidate-mart build, candidate-mart validation and gold publication, with arrows in a correct order.
- [ ] Gold publication runs only after candidate-mart validation.
- [ ] The dependency order is written in the notes.
- [ ] The graph shows in Airflow. If it does not, you have asked the program lead and recorded the answer.

Commit message: `D9.1 add monthly refresh DAG`

#### Task 5 - Make the gate stop the pipeline (ID `D9.2`)

##### 1. What We Are Doing

Change the DAG so a failed quality check stops the pipeline. You work in VS Code to edit the DAG and write notes. Then you use the Airflow web page in your browser to run it. Then you use VS Code to commit.

##### 2. Why We Are Doing It

A gate that logs a warning and lets the build continue is not a gate. The client needs to know that bad data never reaches the dashboard.

##### 3. What to Learn First

Nothing new. You use the Airflow best practices page from Task 4 and your Task 2 gate. Optional: if you want your gate to write a log file, watch Corey video #52: Logging Basics. Before you start, make sure you can deploy and run a DAG in Airflow. If you cannot, stop and ask the program lead. Task 5 needs a real failed Airflow run.

##### 4. Where to Find Resources

- Airflow "Best Practices" page from Task 4, idempotency section.
- Your Task 2 gate and its run results.
- Optional: Corey video #52 on the Corey Schafer playlist.
- Search keywords: "Airflow task fail stops downstream", "Airflow quality gate", "Airflow task log".

##### 5. Step-by-Step Guide

1. Make sure you can deploy and run a DAG in Airflow. If you cannot, stop and ask the program lead before continuing. Task 5 needs a real failed Airflow run.
2. Replace every `EmptyOperator` placeholder in `monthly_refresh_dag.py` with a real task. Pass the cohort connection configuration through the Airflow environment or connection the program lead shows you, not through the DAG file. Here is a small guided example for a validation task that calls your existing GX script and fails when the script fails:

```python
import os
import subprocess
import sys
from airflow.exceptions import AirflowException
from airflow.operators.python import PythonOperator

def run_checked_script(script_path, **context):
    env = os.environ.copy()
    # The Airflow environment must hold the same Snowflake key-pair or PAT
    # settings that your script already reads. Never put secrets in this file.
    result = subprocess.run(
        [sys.executable, script_path],
        env=env,
        capture_output=True,
        text=True,
    )
    print(result.stdout)
    if result.returncode != 0:
        print(result.stderr)
        raise AirflowException(f"quality check failed: {script_path}")
```

Use the same pattern for every real task. For the two validation tasks, call your Task 1 and Task 2 scripts:

```python
validate_raw = PythonOperator(
    task_id="validate_raw",
    python_callable=run_checked_script,
    op_kwargs={"script_path": "students/DEx/week14/quality/expectations/mca_suite.py"},
)

validate_candidate_marts = PythonOperator(
    task_id="validate_candidate_marts",
    python_callable=run_checked_script,
    op_kwargs={"script_path": "students/DEx/week14/quality/expectations/gold_suite.py"},
)
```

For load, staging, candidate-mart build and publish, call the real command you already use in the VS Code terminal. Replace `DEx` with your number.
3. Make the log line name the source file, for example `FAILED: raw_mca_karnataka.csv`.
4. Cause one deliberate failure. For example, load a small test row into a raw table that breaks one rule.
5. Run the DAG from the Airflow page. Take note of: the failed task, the log line, the tasks that did not run, and the row count in your gold table before and after. `publish_gold` must not run after a failed validation.
6. Remove the test row when you are done.
7. Paste the log line and the before/after gold row counts into `students/DEx/week14/airflow/notes.md`.

**Save as:** updated `students/DEx/week14/airflow/monthly_refresh_dag.py` and the pasted evidence in `students/DEx/week14/airflow/notes.md`.

If you get stuck: read the Airflow task log; check that the gate task raises an error, not only logs a warning; ask in Discord with the failed task ID and the log line.

How to think about it: a gate that only logs is not a gate. The failed task must stop the next tasks.

##### 6. What to Commit

- [ ] `students/DEx/week14/airflow/monthly_refresh_dag.py`
- [ ] `students/DEx/week14/airflow/notes.md`

Done when:
- [ ] Every `EmptyOperator` placeholder is replaced by a real task.
- [ ] The gate task shows as failed in Airflow.
- [ ] The tasks after the failed gate show as not run, including `publish_gold`.
- [ ] The log names the source file.
- [ ] The notes show the gold row count did not change.

Commit message: `D9.2 make the quality gate stop the pipeline`

> **On Task 5:** the deliberate failure run is the deliverable. A DAG with only a green run proves only a run with no failures. A run with no failures was never in doubt. Show the gate stopping the pipeline with gold untouched.

### Group 3 - Peer Review

#### Task 6 - Review two pull requests (ID `B4.1`)

##### 1. What We Are Doing

Review two pull requests from teammates and log the links and comments. You work in the browser, on GitHub.

##### 2. Why We Are Doing It

On a real delivery, another reviewer is the cheapest check there is. You also learn from other people's code.

##### 3. What to Learn First

- GitHub Docs, "About pull request reviews" (docs.github.com), 10 min.
- Read the review guidance in this task: point to a line, name the risk, ask a clear question. 10 min.
- Stop when you have enough to write one real comment.

##### 4. Where to Find Resources

- GitHub Docs, "About pull request reviews" (docs.github.com), 10 min.
- The review guidance in this task.
- Search keywords: "GitHub pull request reviews", "GitHub pull request comment", "review log".

##### 5. Step-by-Step Guide

1. Open the class repo on GitHub and find the pull requests of your teammates. Pick two that are not your own.
2. Read the changed files. Run the code or SQL if you can.
3. On each pull request, leave at least one real comment: a question, a spotted mistake, or a suggested improvement. "Looks good" does not count.
4. Copy both pull request links.
5. Write them in your review log. Use a small table:

```markdown
| PR link | What I commented | Reviewer reply |
|---|---|---|
| TODO | TODO | TODO |
```

**Save as:** `students/DEx/week14/reviews/review_log.md`.

If you get stuck: read the changed files first; use the pull request Files changed tab; ask in Discord for another reviewer if the change is large.

How to think about it: a useful review comment points to a line, names the risk, and asks a clear question.

##### 6. What to Commit

- [ ] `students/DEx/week14/reviews/review_log.md`

Done when:
- [ ] Two links from two different teammates are in the log.
- [ ] Each pull request has at least one real comment from you.

Commit message: `B4.1 log two pull request reviews`

#### Task 7 - Fix one piece of feedback (ID `B4.2`)

##### 1. What We Are Doing

Apply one review comment to your own work and reply to the reviewer. You work in VS Code to make the fix and commit. You use the browser (GitHub) to reply to the reviewer.

##### 2. Why We Are Doing It

A review only helps if the author acts on it. This is the loop a client team repeats every week.

##### 3. What to Learn First

Nothing new. You use the Git commands from `docs/03-student-guide.md`. Your pull request must be open with actionable feedback. If your original pull request is already merged, you will open a follow-up pull request and link it.

##### 4. Where to Find Resources

- `docs/03-student-guide.md` for the Git commands.
- GitHub Docs for pull request comments.
- Search keywords: "Git commit message", "GitHub reply to review comment", "pull request new commit".

##### 5. Step-by-Step Guide

1. Open one comment you received on your own work. Make sure the pull request is still open with actionable feedback. If the original pull request is merged, open a follow-up pull request and link the original in its description.
2. Make the fix in the original folder, not in week 14.
3. Commit and push to the open pull request. Write the original pull request number in the commit message.
4. Reply on the comment and say what you changed, in one or two sentences. If you opened a follow-up pull request, add its link to the reply or description.

**Save as:** the fix goes in its original folder. No new file in week 14.

If you get stuck: find the comment in the pull request Conversation tab; check your branch is the same one; ask in Discord with the pull request link.

How to think about it: the fix belongs where the mistake is. Do not move the fix into week 14 just to have a file there.

##### 6. What to Commit

- [ ] The fix file in its original folder, not in `students/DEx/week14/`
- [ ] The reply on the reviewer's comment in GitHub

Done when:
- [ ] The fix is pushed to the open pull request, or to a linked follow-up pull request if the original is merged.
- [ ] Your reply is posted on the reviewer's comment.
- [ ] The pull request shows the new commit.

Commit message: `B4.2 apply review feedback`

### Group 4 - Break, Fix and Postmortem

#### Task 8 - Find the injected failure (ID `B5.1`)

##### 1. What We Are Doing

Find the failure the program leads put into the project data or pipeline, and write the evidence trail. You work in the Airflow web page and GitHub (logs and runs) in the browser. You use Snowsight > Projects > Workspaces to run row counts and queries: role `STUDENT_ROLE`, warehouse `STUDENT_WH`, your own schema. You use VS Code to write the trail and commit.

##### 2. Why We Are Doing It

In due diligence, a broken number can reach the client. Being able to trace "what changed and when" from evidence is the skill, not guessing the answer.

##### 3. What to Learn First

Nothing new. You use row counts from week 6, dbt tests from week 12, and the Airflow runs from Task 5.

##### 4. Where to Find Resources

- Airflow run logs and GitHub runs in the browser.
- Snowflake row counts and queries.
- `RAW_MCA_PREV` from week 3 for comparison.
- Search keywords: "Airflow task log", "dbt test results", "row count before after", "data incident evidence trail".

##### 5. Step-by-Step Guide

1. The program leads will put a failure into the project data or pipeline. Ask in the cohort channel when it has been placed, if you were not told.
2. Start with the questions you can answer: which tables look wrong, and since when?
3. Check, in any order you like: Airflow run logs, dbt test results, Great Expectations run results, row counts per table, and a comparison with `RAW_MCA_PREV` from week 3.
4. Write the trail as you go, in order, with the time of each check and what it told you. Write down the wrong guesses too, and say what made you drop them.
5. Write one line: the cause, and the evidence that proves it.
6. Implement or coordinate the correction. If the failure is in project data you cannot change, ask the program lead to apply the correction and record who did it.
7. Rerun the failed checks: the Airflow run, dbt tests and GX suites as needed. Run the same checks that failed before.
8. Record the successful rerun result, with the time and the before/after row counts, in `break_fix_notes.md`. This is the evidence Task 9 uses for "How we fixed it".

**Save as:** `students/DEx/week14/postmortem/break_fix_notes.md`.

If you get stuck: start with row counts; compare with `RAW_MCA_PREV`; ask in Discord with the trail so far, not only the final guess.

How to think about it: a trace is a story told by evidence. Write each check and result before you make the next guess.

##### 6. What to Commit

- [ ] `students/DEx/week14/postmortem/break_fix_notes.md`

Done when:
- [ ] The trail is in time order and includes at least one wrong guess.
- [ ] Each step says what check you ran and what it showed.
- [ ] The trail includes a cause line with the evidence behind it.
- [ ] The trail records the correction and a successful rerun of the failed checks.

Commit message: `B5.1 trace the injected failure`

#### Task 9 - Write the postmortem (ID `B5.2`)

##### 1. What We Are Doing

Write a one-page blameless postmortem for the failure you traced in Task 8. You work in VS Code.

##### 2. Why We Are Doing It

The client will ask what stops this from happening again. A short written answer, with no blame, is what serious teams hand over.

##### 3. What to Learn First

Read the in-file postmortem guidance in this task and study your Task 8 trail for 25 min. A **blameless postmortem** means you describe what the system allowed to happen, not who did it.

##### 4. Where to Find Resources

- The in-file postmortem guidance in this task, about 25 min.
- Your Task 8 trail in `students/DEx/week14/postmortem/break_fix_notes.md`.
- Search keywords: "blameless postmortem", "postmortem prevention action", "postmortem evidence".

##### 5. Step-by-Step Guide

1. Create the postmortem file with these five headings:

```markdown
## What broke
## Why it happened
## How we found it
## How we fixed it
## One change that prevents it
```

2. Fill each heading in one or two short sentences. Use your Task 8 trail for "How we found it". Use the recorded correction and successful rerun for "How we fixed it".
3. Use no names of people, yours included.
4. Keep it to one page.

**Save as:** `students/DEx/week14/postmortem/postmortem.md`.

If you get stuck: use the Task 8 trail; keep each heading to two sentences; ask in Discord with the draft and the evidence link.

How to think about it: describe the system, not the person. Each heading answers one question.

##### 6. What to Commit

- [ ] `students/DEx/week14/postmortem/postmortem.md`

Done when:
- [ ] All five headings are filled.
- [ ] No person is named anywhere.
- [ ] The prevention section names one concrete change.
- [ ] It fits on one page.

Commit message: `B5.2 add blameless postmortem`

> **On Task 8:** write the trail as you go, including the wrong guesses. The order you checked things in is the part worth reviewing. Rebuilding it afterwards from memory produces fiction.

> **On Task 9:** no names. "The load was not verified before the merge ran" is a system fault. "X forgot to check" is not a postmortem, it is blame, and it teaches nobody anything.

## 6. What to Commit

End of week checklist:
- [ ] Tasks 1-2 - raw suite and gold gate, with run results pasted, and `gold_checks.sql` saved
- [ ] Task 3 - the GE versus dbt answer
- [ ] Tasks 4-5 - the monthly refresh DAG with correct dependencies, and a failure run showing the gate stopping it
- [ ] Tasks 6-7 - two real reviews logged, and one piece of feedback fixed with a reply
- [ ] Tasks 8-9 - evidence trail including wrong guesses and the successful rerun, and a blameless postmortem with one prevention
- [ ] Nothing secret, nothing large, nothing generated is in the repository

**If you are short on time, cut in this order:** Task 3 (D8.3), then Task 7 (B4.2), then the screenshot polish on Task 5. Never cut Task 5 (D9.2), Task 8 (B5.1), or Task 9 (B5.2). The gate, the evidence trail, and the postmortem are the point of the week.

Open one pull request for your week folder when you are done. See `docs/03-student-guide.md` for the steps.

Next: `week15/problem_statement.md`.
