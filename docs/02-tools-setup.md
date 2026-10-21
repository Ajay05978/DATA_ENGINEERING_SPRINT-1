# Tools Setup

**Set up every tool before week 1 opens. Budget about two hours.**

The full click-by-click walkthrough for Git, VS Code, Python venv, Snowflake with MFA, and the Snowflake extension in VS Code is in `docs/14-vscode-snowflake-setup-guide.md`. This file is the short list of tools and the checklist.

## 1. What We Are Doing

This guide helps you install and check all the tools for Data Sprint 1. Do this before week 1 opens. Budget about two hours. Use `docs/14-vscode-snowflake-setup-guide.md` for the full click-by-click steps. Use this page to see the tool list and to check that setup is done.

Everything on this page is free. If a step asks for a credit card, stop and ask in Discord before you type any card number.

This is new for most people. Go slowly.

You will set up:
- Git and GitHub
- Python and a virtual environment
- VS Code
- Snowflake
- The Snowflake extension in VS Code
- Your fork and clone of the class repository
- Local data folders
- A note about tools that arrive later

### How your two workplaces fit together

You will use two places almost every day. Learn the split now, because it saves time later.

| Place | What you do there |
|---|---|
| VS Code on your laptop | You write, save and commit files. You also run Python and Git here. The Snowflake extension lets you run a `.sql` file here too. |
| Snowflake in the browser (Snowsight) | You run SQL here and you look at the data. Use **Projects > Workspaces**. |
| The web | You download the source data from data.gov.in, IBBI, MCA CDM and RBI. |
| Git and GitHub | The link. They carry your files between your laptop and the class repository. |

**The one rule:** files are written and committed in VS Code. SQL is run in Snowflake.

Why the rule matters:

- Snowflake is where the data lives. It is not where your work is saved.
- VS Code is your main place to work. Your cloned repo lives there, so every file you make can be committed.
- After you get some SQL working in Snowflake, copy the final working version back into the `.sql` file in VS Code, then commit it.

### A note on Snowsight Workspaces

Snowsight is the name of the Snowflake web interface. A Workspace is a place inside Snowflake where you can write and run SQL. The SQL editor is now **Projects > Workspaces**. Legacy Worksheets are being removed (docs: June 22, 2026).

A Workspace can also connect to a Git repository, and this is useful for reading files. But a connected public repository can only be pulled, not pushed back (this is in the Snowflake docs).

So for this sprint: write and commit in VS Code. Use Snowflake to run your SQL. Do not try to push from Snowflake.

## 2. Why We Are Doing It

In the client project you check Indian companies for due diligence. You save every piece of work as a file in Git, and you run SQL in Snowflake. This setup makes that work repeatable and safe.

Your commits are also a record of your work. Employers can look at this history. A clean setup now means fewer problems in week 1 and later.

### What you are installing and why

| Tool | What it is | Why this sprint needs it |
|---|---|---|
| Git | Version control. It records every change to every file. | Every task ends in a commit. This history is also what employers look at. |
| A GitHub account | Where the shared repository lives | Pull requests, reviews, and your contribution graph |
| Python 3.11 or newer | A programming language | Most source files need code before they can be loaded |
| VS Code | A code editor with a terminal built in | SQL, Python and Markdown in one place |
| Snowflake | A cloud data warehouse | The platform lives here. All SQL runs here. |
| The Snowflake Extension for Visual Studio Code | An official VS Code add-on from Snowflake | Lets you run a `.sql` file from VS Code |
| DBeaver (optional) | A desktop SQL client | Useful if you prefer a desktop tool to the Snowflake web page |
| dbt Core | A transformation tool | Arrives in week 12. dbt Core is the cohort default. Listed here so nothing surprises you. |

The full story of every tool, including the ones that arrive in weeks 12 to 15, is in `docs/11-tools-and-technology.md`.

## 3. What to Learn First (Prerequisites)

Before you start, you need:

- A computer where you can install software.
- A terminal. This is a text window where you type commands. VS Code has one built in.
- About two hours.
- No credit card. All tools here are free.

You do not need to know Git, Python or SQL before this guide. This guide starts from zero.

Core concepts to review first:

- **Terminal:** a text window where you type commands such as `git` or `python`.
- **Git:** the tool that records changes to your files.
- **Repository:** a project folder that Git tracks.
- **Fork and clone:** a fork is your own copy on GitHub. A clone is a copy on your laptop.
- **Branch:** a separate line of work. You make one branch per week.
- **Virtual environment:** a private copy of Python for one project.
- **Snowflake and Snowsight:** Snowflake is the cloud data warehouse. Snowsight is the web page where you use it.
- **Workspace:** the SQL editor area in Snowsight, under **Projects > Workspaces**.
- **Role and warehouse:** the permissions and the compute you pick in Snowflake.
- **MFA:** a second sign-in step, usually a code from an authenticator app. MFA means multi-factor authentication.

## 4. Where to Find Resources

You work in VS Code, a terminal, Snowflake in the browser, and the web. The split is in section 1.

### Docs in this repo

| Document | What it gives you |
|---|---|
| `docs/14-vscode-snowflake-setup-guide.md` | The full click-by-click walkthrough for Git, VS Code, Python venv, Snowflake with MFA, and the Snowflake extension in VS Code. |
| `docs/03-student-guide.md` | The full explanation of fork, clone, branch and every Git command. See section 1. |
| `docs/11-tools-and-technology.md` | The full story of every tool, including tools that arrive in weeks 12 to 15. |

### Which video or page for setup help

Watch only what you need. Skip everything else for now.

| Problem | Watch or read | Time |
|---|---|---|
| Installing Python on your machine | Corey video #1: "Python Tutorial for Beginners 1: Install and Setup for Mac and Windows", on https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU | about 15 min |
| Using the built-in `venv` module | Corey video #105 (Mac and Linux) or #106 (Windows): VENV built-in venv module, same playlist | short video |
| Git and GitHub basics, commits, branches, remotes | Pro Git, chapters 1 and 2, https://git-scm.com/book/en/v2 | about 60 min, or read the parts you need |
| Writing a good commit message | Chris Beams, https://cbea.ms/git-commit/ | about 5 min |
| Snowflake sign in, MFA and Workspaces | Search the Snowflake docs at https://docs.snowflake.com for "Snowsight navigation" and "Planning for the deprecation of single-factor password sign-ins" | about 10 min |
| Snowflake extension in VS Code | Search the Snowflake docs at https://docs.snowflake.com for "Snowflake Extension for Visual Studio Code" | about 10 min |

For Corey's playlist, skip the Windows/Mac settings talk once your own Python runs. For Pro Git, chapter 2 gives you the daily commands, the rest can wait.

### Search keywords if you get stuck

- install Git on Windows
- GitHub personal access token
- Python venv activate Windows
- Snowflake MFA setup
- Snowsight Workspaces
- Snowflake extension VS Code

## 5. Step-by-Step Guide

Work in order. The full click-by-click steps are in `docs/14-vscode-snowflake-setup-guide.md`. This page keeps the short checks. Each section ends with a **Check it worked** block. Do not move on until it passes. For full Git commands, read `docs/03-student-guide.md`, section 1.

### 1. Git

Git is the tool that records your work. You run it from a terminal.

Check if Git is installed:

```bash
git --version
```

If that prints a version number, you already have Git. If it prints nothing, or offers to install developer tools, accept the offer, or install from https://git-scm.com. On Windows, use **Git Bash** as your terminal. During install, choose **main** as the default branch name.

#### Configure Git, once

Your commits only show on your GitHub contribution graph if the email in your commits matches a verified email on your GitHub account.

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

**Check it worked**

```bash
git config --global --list
```

You should see your name, your email, and `init.defaultbranch=main`.

### 2. GitHub account

GitHub is the website that hosts the class repository.

1. Create an account on GitHub if you do not have one. Help pages: https://docs.github.com
2. Open Settings, then Emails. Confirm the email you just put in the Git config is listed and verified.
3. Open Settings, then Profile. Put your real name. This repository becomes something you show people.

#### Authentication for pushing

GitHub does not accept your account password from the command line. Pick one option.

**Option A: HTTPS with a personal access token (simplest).**

Settings > Developer settings > Personal access tokens > Tokens (classic) > Generate new token. Tick the `repo` scope. Copy the token somewhere safe. GitHub shows it once. When Git asks for a password, paste the token.

**Option B: SSH key (nicer long term).**

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
cat ~/.ssh/id_ed25519.pub
```

Copy the whole output. Then GitHub > Settings > SSH and GPG keys > New SSH key > paste.

**Check it worked**

```bash
ssh -T git@github.com     # Option B: GitHub greets you by username
```

With Option A you will find out at your first `git push`. That is fine.

### 3. Python

You need Python **3.11 or newer**.

```bash
python3 --version
```

If it is missing or older:

- **macOS:** `brew install python@3.12`
- **Windows:** install from https://www.python.org/downloads/ and **tick "Add python.exe to PATH"** on the first installer screen. This one checkbox causes most Windows setup problems in week 1.
- **Linux:** `sudo apt install python3 python3-pip python3-venv`

#### The virtual environment

A virtual environment is a private copy of Python for one project. A library you install for this sprint then cannot break anything else on your machine.

Use the built-in `venv` module. Corey video #105 (Mac and Linux) or #106 (Windows) shows it.

Do this part **after** you clone the repo in section 6. You need the repo folder first.

```bash
cd path/to/Data-Sprint-1
python3 -m venv .venv
source .venv/bin/activate     # macOS / Linux
.venv\Scripts\activate        # Windows PowerShell
```

Your prompt now starts with `(.venv)`. Run the activate line again in every new terminal.

Install what weeks 2 to 11 need:

```bash
pip install --upgrade pip
pip install pandas pydantic requests pdfplumber beautifulsoup4 lxml pytest python-dotenv snowflake-connector-python
```

`pytest` is used in week 10. The Snowflake connector is used in week 11.

**Check it worked**

```bash
python -c "import pandas, pydantic, requests, pdfplumber, bs4, snowflake.connector; print('all imports fine')"
```

`.venv/` is already in `.gitignore`. Never commit it.

### 4. VS Code

Install from https://code.visualstudio.com, then add these extensions. Open the Extensions panel and search by name.

| Extension | Why |
|---|---|
| Python (Microsoft) | Running and debugging Python |
| Jupyter (Microsoft) | Only if you like notebooks for exploring. Committed work stays as `.py` and `.sql` files. |
| SQLFluff | The same SQL linter that CI runs, so you see problems before you push |
| Markdown All in One | Your notes and written answers are all Markdown |
| GitLens (optional) | Makes Git history easier to read |
| Snowflake Extension for Visual Studio Code | Official Snowflake extension. It can run a `.sql` file from VS Code. |

Two settings worth changing now: turn on **Format on Save**, and set **Files: Insert Final Newline**. Both keep your diffs small and easy to review.

#### Open your repo folder

Do this after you clone in section 6.

1. In VS Code, choose **File > Open Folder**.
2. Pick the cloned folder `Data-Sprint-1`.
3. If VS Code asks "Do you trust the authors", choose yes.

The file list on the left now shows the repo. That list is called the Explorer.

#### Open the terminal

Choose **Terminal > New Terminal**. A panel opens at the bottom, already inside the repo folder.

Type `pwd` (macOS/Linux) or `cd` (Windows) to confirm you are in the right folder.

#### Select the Python interpreter from `.venv`

1. Press `Ctrl+Shift+P` on Windows/Linux, or `Cmd+Shift+P` on macOS. This opens the Command Palette.
2. Type `Python: Select Interpreter` and press Enter.
3. Pick the line that ends with `.venv`. If you do not see it, close and reopen VS Code, then try again.

#### Create a new file inside a folder

1. In the Explorer, right-click the folder you want.
2. Choose **New File**.
3. Type the file name with the extension, for example `notes.md` or `01_create_database.sql`.
4. Press Enter, then type or paste the content, then save with `Ctrl+S` (Windows/Linux) or `Cmd+S` (macOS).

#### Snowflake extension in VS Code

Open the Extensions panel and install the official Snowflake extension, the one with the Snowflake badge. Sign in with your account identifier, username, and password, or SSO or key-pair. Open a `.sql` file and run it with the Command Palette command **Snowflake: Execute All Statements**. Exact menu names may change. If they differ, follow the docs page.

**Check it worked:** the terminal prompt shows `(.venv)` and the bottom status bar of VS Code shows the `.venv` interpreter.

### 5. Snowflake

Snowflake is the cloud data warehouse. All the data lives there and all the SQL runs there. You reach it through a browser page called Snowsight.

The program lead gives you these values:

- an **account URL**
- a **username**
- a **first-time password**
- a **role** (usually `STUDENT_ROLE`)
- a **warehouse** (usually `STUDENT_WH`)
- a **database**

#### First login, step by step

1. Open the account URL in your browser.
2. Sign in with the username and the first-time password from the program lead.
3. Change your password when Snowflake asks. Store the new password in a password manager, not in a file.
4. Set up MFA when Snowflake asks. Use an authenticator app. MFA is a second sign-in step. Snowflake requires MFA when human users sign in with a password to Snowsight. Final enforcement date in the docs: October 2026.
5. Pick your role and your warehouse at the top of the screen. Look for two small pickers near the top left area. If you cannot find them, search the Snowflake docs for "Snowsight navigation".
6. Open **Projects > Workspaces**. Legacy Worksheets are being removed (docs: June 22, 2026). Use a Workspace. Create a SQL file.
7. Paste this and run it:

```sql
SELECT CURRENT_USER(), CURRENT_ROLE(), CURRENT_WAREHOUSE(), CURRENT_DATABASE();
```

#### A short tour of Snowsight

- **Top of the screen:** the role picker and the warehouse picker. Check them every time a query behaves strangely.
- **Projects > Workspaces:** a work area for SQL files. You pick the role and warehouse at the top, then run all or run the selection. A Workspace can also connect to a Git repository. Remember: a public repo can only be pulled, so we commit from VS Code.
- **Snowflake Extension for Visual Studio Code:** lets you run a `.sql` file from VS Code.

**Check it worked:** four values come back and none of them are NULL. If the warehouse is NULL, run `USE WAREHOUSE STUDENT_WH;` and run the check again.

#### When it fails

Work through this list before you ask for help.

- **The page does not load.** Check the account URL. Copy it again from the program lead's message instead of typing it.
- **The login is rejected.** On the first login you must change the password and set up MFA. If that step did not happen, ask the program lead to reset it.
- **`CURRENT_WAREHOUSE()` is NULL.** Run `USE WAREHOUSE STUDENT_WH;` then run the check query again.
- **`CURRENT_ROLE()` is not your role.** Run `USE ROLE STUDENT_ROLE;` then run the check query again.
- **`CURRENT_DATABASE()` is NULL.** Run `USE DATABASE <your database>;` using the name the program lead gave you.
- **You get an insufficient privileges error.** You are probably touching something you did not create. Work in your own schema and try again.
- **A Python script or dbt login fails with an MFA message.** Ask the program lead. Do not paste your password into code. The safe ways are key-pair authentication or a Programmatic Access Token (PAT). A PAT is a token you use instead of a password in a script. A key pair is a private key on your laptop and a public key on your Snowflake user.

For anything else, post in Discord with the exact query and the exact error text.

#### Rules that keep the shared account alive

- **Suspend when done.** The warehouse suspends itself, but do not leave a big query running when you close your laptop.
- **Use `LIMIT` while exploring.** The MCA registry has about 31 lakh rows. A `SELECT *` with no limit is slow and burns shared credits.
- **Work in your own schema** when a task does not say otherwise. Never drop or overwrite anything you did not create.
- **Never put your password in a file.** Not in SQL, not in Python, not in a notes file. Credentials go in environment variables. Task P11.1 in week 11 teaches this properly.

#### Environment variables for Python later

Create a file called `.env` in the repo root. Confirm `.env` is already in `.gitignore` before you save anything in it.

```
SNOWFLAKE_ACCOUNT=your_account_identifier
SNOWFLAKE_USER=your_user
SNOWFLAKE_PASSWORD=your_password
SNOWFLAKE_ROLE=STUDENT_ROLE
SNOWFLAKE_WAREHOUSE=STUDENT_WH
SNOWFLAKE_DATABASE=your_database
```

The Snowflake connector is used in week 11. For Python scripts and dbt on a laptop, use key-pair authentication or a PAT. The program lead tells you which one the cohort uses.

### 6. Fork and clone the repository

A fork is your own copy of the class repository on GitHub. A clone is a copy of that fork on your laptop.

The full explanation of every command is in `docs/03-student-guide.md`, section 1. The short version:

```bash
# 1. Fork on GitHub: open the class repository, click Fork, top right.
# 2. Clone your fork:
git clone https://github.com/YOUR_USERNAME/Data-Sprint-1.git
cd Data-Sprint-1

# 3. Add the class repository as a second remote called upstream:
git remote add upstream https://github.com/StartwithDot/Data-Sprint-1.git
git remote -v
```

**Check it worked:** `git remote -v` shows `origin` pointing at your fork and `upstream` pointing at the class repository.

#### Work on a branch for each week

Do not work on `main`. Make one branch per week, so your work is easy to review and easy to fix.

```bash
git checkout -b DEx_week1
```

**What is `DEx`?** It is your own student number. If you are student 12, type `git checkout -b DE12_week1`. If you are student 7, type `git checkout -b DE7_week1`. Every place in the docs that says `DEx`, replace it with your own number.

Next week you make a fresh branch from an updated `main`, for example `DE12_week2`. Never squash-merge a pull request.

**Check it worked:** `git branch` shows your new branch with a `*` next to it.

### 7. Data folders on your machine

Downloaded source files never go into Git. They live in a local folder that `.gitignore` already excludes.

```bash
mkdir -p data/raw/mca data/raw/ibbi data/raw/cdm data/raw/rbi
```

**Check it worked**

```bash
git status
```

`data/` must not appear in the output. If it does, stop and ask in Discord before you commit anything.

### 8. Tools that arrive later in the sprint

Do not install these now. They are listed so nothing surprises you. Each one arrives in the week that needs it.

| Tool | Arrives | Install then with |
|---|---|---|
| dbt Core | week 12, task group dbt Staging Models | `pip install dbt-snowflake` into `.venv`. Connection details go in `~/.dbt/profiles.yml`, never committed. |
| Great Expectations | week 14, task group Quality Gates | `pip install great_expectations` |
| Apache Airflow | week 14, task group Airflow Orchestration | The program lead shares the URL of the prepared environment |
| Metabase | week 15, task group Dashboard | Hosted by the program. You get a login. |

#### dbt, in two possible ways

For this cohort, **dbt Core is the default**. You install `dbt-snowflake` into your `.venv` and run `dbt run` and `dbt test` in the VS Code terminal. Your connection details sit in `~/.dbt/profiles.yml`, and this file is never committed.

dbt Projects on Snowflake in a Workspace is an optional extra. It uses the same `.sql` and `.yml` files from the `platform/dbt` folder in the repo, and you run it from the Workspace. Docs for the optional path: https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake and https://docs.snowflake.com/en/user-guide/data-engineering/dbt-projects-on-snowflake-using-workspaces

General dbt learning: https://docs.getdbt.com and https://learn.getdbt.com

#### Snowflake CLI (optional)

Snowflake CLI (command name `snow`) is optional. The program lead says if you need it.

What each tool is for, and why we chose it, is in `docs/11-tools-and-technology.md`.

## 6. What to Commit

You do not commit setup files. Setup files stay on your laptop. Before you finish setup, these points must be true. Do not commit `.venv`, `.env`, or data files.

Setup checklist:

- [ ] `git config --global --list` shows your name and your GitHub-verified email
- [ ] GitHub account exists, and pushing works with a token or an SSH key
- [ ] `python3 --version` is 3.11 or newer
- [ ] `.venv` created and activated, and the import check prints `all imports fine`
- [ ] VS Code installed with the Python, SQLFluff and Snowflake extensions, and the `.venv` interpreter selected
- [ ] Snowflake login works, MFA is set up, and `SELECT CURRENT_USER(), CURRENT_ROLE(), CURRENT_WAREHOUSE(), CURRENT_DATABASE()` returns values
- [ ] You can run a `.sql` file in a Snowsight Workspace
- [ ] You can run a `.sql` file from VS Code with the Snowflake extension
- [ ] Repository forked, cloned, `upstream` remote added, and your week 1 branch created
- [ ] `data/raw/...` folders created locally and invisible to `git status`
- [ ] `.env` created, and `.env` confirmed present in `.gitignore`

If something on this list will not work after two honest attempts, post in Discord with the exact command you ran and the exact error text. Setup problems are the most common reason a student loses week 1, and they are the fastest thing for another person to fix.

When all boxes are ticked, your setup is done.

Next: `docs/03-student-guide.md`.
