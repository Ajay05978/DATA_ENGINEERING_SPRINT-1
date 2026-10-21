# VS Code, Git and Snowflake Setup Guide

## 1. What We Are Doing

You will set up VS Code, Git, GitHub, Python and Snowflake.
You will save your work on your computer and run SQL in Snowflake.

This guide supports these week 1 tasks:

- `G0.1`: Git, GitHub, fork, clone, first commit and pull request.
- `G0.2`: Snowflake login and first query.
- `G0.3`: Connect VS Code to Snowflake and run a `.sql` file.
- `G0.4`: Create your own project folder and repository for team-project practice.

Python exercises start in week 2.
This guide includes Python installation so your computer is ready.

## 2. Why We Are Doing It

Your client checks Indian companies before making business decisions.
The team will use MCA, IBBI, MCA CDM and RBI data.

You need one clear place to save code, notes and project files.
Git records changes, and GitHub lets the team review them.
Snowflake stores the data and runs your SQL.

This is new for most people.
Go slowly and check each part before you continue.

## 3. What to Learn First (Prerequisites)

| Before you start | What you need |
|---|---|
| Computer | Windows, macOS or Linux |
| Internet | Access to the installation sites and class resources |
| Class repository | Its URL from the program lead |
| Student number | Your assigned number, such as `DE12` |
| Snowflake access | Account URL, username and first password |
| Snowflake context | Your role, warehouse, database and own schema |

Replace `DEx` with your student number in every path.
For example, `students/DEx/week1/` becomes `students/DE12/week1/`.

Learn these words first:

- A terminal accepts commands you type.
- A repository is a project whose changes Git tracks.
- A virtual environment keeps Python packages separate for a project.
- A role controls what you can access in Snowflake.
- A warehouse provides computing power for Snowflake queries.
- A schema groups tables and other database objects.
- MFA means a second sign-in step after your password.

By the end, you can open your project, save files, run a query and submit a pull request.

## 4. Where to Find Resources

| Week 1 task | Sections in this guide |
|---|---|
| `G0.1` | 5.2, 5.3, 5.4, 5.8, 5.9 and 6 |
| `G0.2` | 5.6 and 6 |
| `G0.3` | 5.7 and 6 |
| `G0.4` | 5.1, 5.3, 5.4, 5.10 and 6 |

> **Where you work**
>
> VS Code is your home base.
> Write and save files in your cloned repository.
> Run Git and Python commands in the VS Code terminal.
> Run SQL through the Snowflake extension or Snowsight Workspaces.
> `DEx` means your assigned student number.

| Resource | Use it for |
|---|---|
| [docs/00-START-HERE.md](00-START-HERE.md) | Course starting point |
| [docs/02-tools-setup.md](02-tools-setup.md) | Shared setup instructions |
| [docs/03-student-guide.md](03-student-guide.md) | Tasks, commits and pull requests |
| [docs/09-resources.md](09-resources.md) | Learning resources |
| [docs/10-troubleshooting.md](10-troubleshooting.md) | Help with errors |
| [docs/11-tools-and-technology.md](11-tools-and-technology.md) | How the tools fit together |
| [CONTRIBUTING.md](../CONTRIBUTING.md) | Repository contribution rules |

Use the [Corey Schafer playlist](https://www.youtube.com/playlist?list=PL-osiE80TeTt2d9bfVyTiXJA-UTHn6WwU).
The numbers below are playlist positions.

| Order | Resource | When to use it |
|---|---|---|
| 1 | [Git book](https://git-scm.com/book/en/v2), [GitHub docs](https://docs.github.com) | Install, fork and clone |
| 2 | Corey video #110: Visual Studio Code (Mac) setup | macOS setup |
| 3 | Corey video #111: Visual Studio Code (Windows) setup | Windows setup |
| 4 | Corey video #1: Beginners 1 Install and Setup | Python installation |
| 5 | Corey video #105: VENV (Mac & Linux), or #106: VENV (Windows) | Built-in `venv` |
| 6 | Corey video #13: pip, optional | Package installation |
| 7 | [Snowflake extension docs](https://docs.snowflake.com/en/user-guide/vscode-ext) | VS Code connection |
| 8 | Corey video #146: Securely Manage Passwords and API Keys with DotEnv | `.env` |
| 9 | Corey video #54 and #55: Hiding Passwords and Secret Keys in Environment Variables (Mac and Linux / Windows) | Environment variables |

Video times are not supplied here.
Check the playlist for each video's length.

Search keywords: `Git fork clone`, `VS Code open folder`, `Python venv`, `Snowflake Workspaces`, `Snowflake MFA`.

## 5. Step-by-Step Guide

Do one task, save its file and make one commit.
Follow [docs/03-student-guide.md](03-student-guide.md).
Use the setup file paths below unless your task gives another path.

### 5.1 The big picture

| Tool or place | What it is for |
|---|---|
| VS Code | Write, save and review project files |
| Terminal | Run Git, Python and later dbt commands |
| Snowflake | Store data and run SQL |
| GitHub | Share repositories and review pull requests |
| Your project folder | Keep the local files that belong to one project |

**One rule:** Write, save and commit in VS Code. Run SQL in Snowflake.

1. Find your student number.
2. Locate the class repository link.
3. Keep individual practice inside `students/DEx/weekN/`.
4. Keep team-project work in a separate team repository.

Downloaded data belongs in `data/raw/...` at the repository root.
That local folder is excluded by `.gitignore`.
Never commit downloaded data.

> **Check it worked**
>
> You can explain where files are saved and where SQL runs.
> You know the difference between individual practice and team work.

**If it fails:** Read the "Where you work" box in section 4.

### 5.2 Install Git and create your GitHub account

Git records changes on your computer.
GitHub hosts repositories for sharing and review.

1. Open [git-scm.com](https://git-scm.com).
2. Follow the installation instructions for your operating system.
3. Open Git Bash on Windows.
4. Open Terminal on macOS or Linux.
5. Check Git:

   ```bash
   git --version
   ```

6. Follow [GitHub docs](https://docs.github.com) to create an account.
7. Set your name:

   ```bash
   git config --global user.name "YOUR NAME"
   ```

8. Set your chosen commit email:

   ```bash
   git config --global user.email "YOUR_EMAIL"
   ```

9. Check both values:

   ```bash
   git config --global --get user.name
   git config --global --get user.email
   ```

Replace the quoted example values before running the commands.
Use the email you intend to associate with your GitHub commits.

> **Check it worked**
>
> Git prints a version.
> Your name and email are correct.
> You can sign in to GitHub.

**If it fails:** Follow the operating-system instructions on the Git site.

### 5.3 Fork, clone and create your week branch

A fork is your GitHub copy of the class repository.
A clone is a copy of a repository on your computer.
A branch is a separate line of changes in a repository.

The class repository name and URL come from the program lead.
The commands below use variables so you do not guess either value.

1. Open the class repository on GitHub.
2. Follow GitHub's fork instructions.
3. Find your fork's clone URL.
4. Replace `YOUR_USERNAME` with your GitHub username in that URL.
5. Open Git Bash or your macOS/Linux terminal.
6. Move to your chosen parent folder:

   ```bash
   cd "PATH_TO_YOUR_PARENT_FOLDER"
   ```

7. Store your fork's actual clone URL:

   ```bash
   FORK_URL='PASTE_YOUR_FORK_CLONE_URL_HERE'
   ```

8. Clone it into a local folder:

   ```bash
   git clone "$FORK_URL" class-repo
   ```

9. Enter the cloned folder:

   ```bash
   cd class-repo
   ```

10. Store the original class repository URL:

    ```bash
    CLASS_REPO_URL='PASTE_CLASS_REPOSITORY_CLONE_URL_HERE'
    ```

11. Add the original repository as `upstream`:

    ```bash
    git remote add upstream "$CLASS_REPO_URL"
    ```

12. Check the remote addresses:

    ```bash
    git remote -v
    ```

13. Create your branch:

    ```bash
    git switch -c DEx_week1
    ```

Replace `DEx` before running the branch command.
`origin` points to your fork.
`upstream` points to the original class repository.

> **Check it worked**
>
> `git remote -v` shows your fork and the class repository.
> `git branch --show-current` shows your student week branch.

**If it fails:** Check both URLs against GitHub and read the exact Git error.

### 5.4 Install VS Code and open the project

1. Install VS Code from [code.visualstudio.com](https://code.visualstudio.com).
2. Watch Corey video #110 for Mac or #111 for Windows.
3. Choose **File > Open Folder**.
4. Open the cloned `class-repo` folder.
5. Open the integrated terminal.
6. Confirm the terminal is inside the cloned folder.
7. Create `students/DEx/week1/setup/` in the file panel.
8. Create `students/DEx/week1/setup/hello.md`.
9. Save the file.
10. Open the Extensions panel.
11. Install Python.
12. Install the official Snowflake extension published by Snowflake.
13. Check that it has the Snowflake badge.
14. Install SQLFluff.
15. Install Markdown All in One.

Python supports Python work.
Snowflake connects the editor to Snowflake.
SQLFluff helps check SQL style.
Markdown All in One supports Markdown editing.

> **Check it worked**
>
> The file panel shows the cloned repository.
> `hello.md` appears inside your own week folder.
> `git status` works in the VS Code terminal.

**If it fails:** Reopen the cloned folder using **File > Open Folder**.

### 5.5 Install Python and create `.venv`

This prepares your computer for Python work starting in week 2.
A package adds features to Python.

1. Install Python 3.11 or later from [python.org](https://www.python.org/downloads/).
2. Open the VS Code terminal.
3. Check the installed version:

   ```bash
   python --version
   ```

4. Try `python3 --version` if `python` is unavailable.
5. Watch Corey video #105 or #106 for your operating system.
6. Create the environment from the repository root:

   ```bash
   python -m venv .venv
   ```

7. Activate it with the command for your terminal.
8. Select the `.venv` interpreter through VS Code's Python interpreter selector.
9. Install the requested packages:

   ```bash
   python -m pip install pandas pydantic requests pdfplumber beautifulsoup4 lxml pytest python-dotenv snowflake-connector-python
   ```

Use `python3 -m venv .venv` if that is your installed Python command.

| Terminal | Activation command |
|---|---|
| macOS/Linux | `source .venv/bin/activate` |
| Windows Command Prompt | `.venv\Scripts\activate` |
| Windows PowerShell | `.\.venv\Scripts\Activate.ps1` |
| Windows Git Bash | `source .venv/Scripts/activate` |

These packages prepare for Python topics in weeks 2-11.
The Snowflake connector is also installed now for use in week 11.
Corey video #13 on pip is optional.

> **Check it worked**
>
> `python --version` reports 3.11 or later.
> `python -m pip --version` points inside `.venv`.

**If it fails:** Check the [Python tutorial](https://docs.python.org/3/tutorial/) and the matching venv video.

### 5.6 Log in to Snowflake

Use only the account details from the program lead.
MFA is required for human password sign-ins, with final enforcement in October 2026.

1. Open your supplied account URL.
2. Sign in with your supplied username and first password.
3. Change the first password.
4. Set up MFA with an authenticator app.
5. Select `STUDENT_ROLE`, unless given another role.
6. Select `STUDENT_WH`, unless given another warehouse.
7. Open **Projects > Workspaces** in Snowsight.
8. Create a SQL file in the Workspace.
9. Run this setup check:

   ```sql
   SELECT CURRENT_USER(), CURRENT_ROLE(), CURRENT_WAREHOUSE(), CURRENT_DATABASE();
   ```

10. Check the result against your assigned details.
11. Open VS Code and save the same query at `students/DEx/week1/setup/snowflake_login_check.sql`.
12. Add your result below the query as a comment that starts with `--`.
13. Save the file. This file is your `G0.2` commit.

Workspaces are the current SQL editor.
Legacy Worksheets are being removed.
Use Workspaces for this course.

A `NULL` database means no current database is selected.
Select the database supplied by the program lead.
Select your assigned warehouse if the warehouse value is `NULL`.
Ask the program lead if the assigned role, warehouse or database is missing.

Shared account rules:

- Use your assigned user and your own schema.
- Never share passwords or MFA codes.
- Use `LIMIT` when exploring large tables.
- Stop long queries when you no longer need them.
- Follow the program lead's warehouse instructions.
- Never change another student's objects.

> **Check it worked**
>
> The query returns your user, assigned role, warehouse and database.

**If it fails:** Ask the program lead and search [Snowflake docs](https://docs.snowflake.com) for "Planning for the deprecation of single-factor password sign-ins".

### 5.7 Connect VS Code to Snowflake

The extension runs SQL in Snowflake while your file stays on your computer.
Menu names can change.
Follow the [extension documentation](https://docs.snowflake.com/en/user-guide/vscode-ext) if they differ.

1. Confirm the official Snowflake extension is installed.
2. Look for your account identifier in the account details menu in Snowsight.
3. Ask the program lead if you cannot find it.
4. Start the extension's sign-in process.
5. Enter your account identifier.
6. Enter your username.
7. Enter your password when prompted.
8. Complete the MFA step.
9. Use the program lead's method if it differs.
10. Create a Snowflake SQL file at `students/DEx/week1/setup/connection_check.sql`.
11. Save the query from section 5.6 in that file.
12. Select your assigned connection, role and warehouse.
13. Open the Command Palette.
14. Run **Snowflake: Execute All Statements**.
15. Save the final working file.

The account identifier tells the extension which Snowflake account to contact.
Do not guess it from your username.

The extension also supports SSO or key-pair sign-in.
Connections may be stored in `connections.toml`.
Follow the program lead before creating connection files.
Keep credentials out of committed files.

> **Check it worked**
>
> The saved SQL runs through the extension and returns your assigned context.

**If it fails:** Check the account identifier, MFA and current extension docs.

### 5.8 Keep secrets safe

A secret is private login information.
A `.env` file stores local environment-variable values for later Python work.

1. Watch Corey video #146 on DotEnv.
2. Watch #54 or #55 for your operating system.
3. Check the repository's `.gitignore`.
4. Confirm it excludes `.env`, `.venv/` and local data.
5. Create an empty `.env` at the repository root.
6. Add only comments until the program lead gives connection settings.
7. Check that Git ignores it:

   ```bash
   git check-ignore .env
   ```

8. Review `git status` before every commit.

Later, `.env` can hold connection settings and approved secret values, such as a PAT.
Never put passwords in code, SQL, notes or committed files.
Do not put a Snowflake password in `.env` for this setup.

`.gitignore` tells Git which untracked files to leave out.
It does not remove a secret that was already committed.
Tell the program lead immediately if a secret enters Git history.

In week 11, Python needs an approved login method.
A key pair has a public key and a private key.
The private key stays on your laptop; an admin attaches the public key to your Snowflake user.
A PAT is a secret token that a program can use to authenticate.

You do not set up key-pair authentication or a PAT now.
The program lead chooses the method for Python and dbt.
Search Snowflake docs for "Key-pair authentication and key-pair rotation" and "Programmatic access tokens".
Check the docs page for the exact parameter spelling before later configuration.

> **Check it worked**
>
> `git check-ignore .env` prints `.env`.
> `git status` does not list secrets, `.venv` or downloaded data.

**If it fails:** Fix the ignore rules before staging files and ask the program lead for help.

### 5.9 Your daily loop

Replace `CLASS_DEFAULT_BRANCH` with the branch name confirmed by the program lead.
Keep your current student branch when pulling upstream changes.

1. Open the cloned repository in VS Code.
2. Activate `.venv` when doing Python work.
3. Pull upstream with `git pull upstream CLASS_DEFAULT_BRANCH`.
4. Work on one task in your own week folder.
5. Run SQL through VS Code or Snowsight Workspaces.
6. Review the file and commit it with its task ID.
7. Push with `git push -u origin DEx_week1`.
8. Open or update your pull request to the class repository.

Before pulling, finish your current commit or follow the student guide for unfinished changes.
If you run SQL in Snowsight, copy the final working SQL back to the saved VS Code file.
Never squash-merge course task commits.

> **Check it worked**
>
> Your pushed commit appears in the pull request.
> The saved file contains the final working version.

**If it fails:** Read [docs/03-student-guide.md](03-student-guide.md) before retrying Git commands.

### 5.10 Add new tools later

Your team repository is separate from the class repository.
Individual practice stays in `students/DEx/weekN/` in the class repository.

| Tool and start week | Where it runs | Where files go in the team project | Setup resource |
|---|---|---|---|
| Python, week 2 | VS Code terminal with `.venv` active | `python/`; individual exercises in your week folder | Section 5.5; `docs/02-tools-setup.md` |
| Snowflake connector, week 11 | Python on your laptop | Python code in `python/`; secrets stay local | `docs/11-tools-and-technology.md`; Snowflake docs |
| dbt Core, week 12 | VS Code terminal | `dbt/`; local profile in `~/.dbt/profiles.yml` | [dbt docs](https://docs.getdbt.com) |
| Great Expectations, week 14 | Follow the cohort setup instructions | `quality/` | [Great Expectations docs](https://docs.greatexpectations.io) |
| Airflow, week 14 | Follow the cohort setup instructions | `airflow/` | [Airflow docs](https://airflow.apache.org/docs/) |
| Metabase, week 15 | Follow the cohort setup instructions | `dashboard/` for project artifacts | [Metabase docs](https://www.metabase.com/docs/latest/) |

1. Create a separate project folder for `G0.4`.
2. Open that folder in a separate VS Code window.
3. Run `git init` inside that folder.
4. Create a `README.md`.
5. Create the initial `python/`, `sql/` and `docs/` folders.
6. Describe each folder in the README.
7. Add `dbt/` in week 12, with a README line for it.
8. Add `quality/` and `airflow/` in week 14, with a README line for each.
9. Add `dashboard/` in week 15, with a README line for it.
10. Follow that week's setup instructions.

Git tracks files, so an empty folder will not appear in a commit.
For week 12 dbt Core, install with `pip install dbt-snowflake`.
Keep `profiles.yml` in `~/.dbt`, never committed.
Run `dbt debug` after the approved connection setup.
Later, run `dbt run` and `dbt test` in the VS Code terminal.

dbt Projects on Snowflake in Workspaces is an optional extra.
The cohort default is dbt Core on your laptop.

Only the team leader copies reviewed team files into the class repository's `platform/` and `delivery/` folders.
The class dbt layout is `platform/dbt/`.
Other platform folders are `raw/`, `staging/`, `marts/`, `quality/` and `orchestration/`.
Delivery folders are `discovery/`, `design/`, `dashboard/` and `presentation/`.

> **Check it worked**
>
> Your separate project has a README and Git repository.
> You know which week's instructions introduce each later tool.

**If it fails:** Check `docs/11-tools-and-technology.md` and ask the program lead about the cohort setup.

### 5.11 Common problems

1. Read the full error message.
2. Find the matching problem below.
3. Try the suggested check.
4. Ask for help with the error text and no secrets.

| Problem | What to check |
|---|---|
| Python not found | Try `python3`; check the installation instructions |
| Venv not active | Run the activation command for your terminal |
| Git push rejected | Check the branch and remote; do not force-push |
| Wrong email | Correct `git config --global user.email`; ask about earlier commits |
| Role has no access | Confirm your assigned role and schema with the lead |
| Warehouse suspended | Try the query; ask the lead if it cannot resume |
| MFA code fails | Check the account entry and your device's time |
| Extension cannot sign in | Check account identifier, login method and MFA |
| File saved in wrong folder | Reopen the clone and move it into your week folder |
| Pip install fails | Confirm `.venv` is active and read the package error |
| Current database is `NULL` | Select your assigned database |
| `.env` appears in Git | Check `.gitignore` before staging anything |
| Terminal is in another project | Open the terminal in the intended VS Code folder |

> **Check it worked**
>
> You can repeat the failed action successfully.

**If it fails:** Use [docs/10-troubleshooting.md](10-troubleshooting.md) and share the error with the program lead.

## 6. What to Commit

Complete this setup checklist:

- [ ] Git is installed.
- [ ] Git has your correct name and email.
- [ ] Your GitHub account works.
- [ ] Your fork is cloned.
- [ ] `origin` and `upstream` point to the correct repositories.
- [ ] Your branch is `DEx_week1`, with `DEx` replaced.
- [ ] VS Code opens the cloned folder.
- [ ] The four required extensions are installed.
- [ ] Python 3.11 or later and `.venv` work.
- [ ] The requested Python packages are installed.
- [ ] Snowflake MFA works.
- [ ] Your assigned Snowflake context is selected.
- [ ] Your saved SQL runs through the Snowflake extension.
- [ ] `.env`, `.venv` and downloaded data are excluded from Git.
- [ ] Your separate `G0.4` project repository exists.

For the exact first `G0.1` commit:

1. Open `students/DEx/week1/setup/hello.md`.
2. Write your name.
3. Write your operating system.
4. Record your installed Git, VS Code and Python versions.
5. Save the file.
6. Review `git status`.
7. Stage only that file:

   ```bash
   git add students/DEx/week1/setup/hello.md
   ```

8. Make the first commit:

   ```bash
   git commit -m "G0.1 first commit and setup check"
   ```

9. Push your branch:

   ```bash
   git push -u origin DEx_week1
   ```

Replace `DEx` in both commands.
Open the class pull request using [docs/03-student-guide.md](03-student-guide.md).
Include `G0.1` in its title.

For the exact `G0.2` commit:

1. Open `students/DEx/week1/setup/snowflake_login_check.sql`.
2. Check it holds the query from section 5.6 and your result as a `--` comment.
3. Save the file.
4. Review `git status`.
5. Stage only that file:

   ```bash
   git add students/DEx/week1/setup/snowflake_login_check.sql
   ```

6. Make the `G0.2` commit:

   ```bash
   git commit -m "G0.2 Snowflake login and first query"
   ```

7. Push your branch:

   ```bash
   git push -u origin DEx_week1
   ```

Replace `DEx` in both commands.
Put `G0.2` in the pull request title or in your pull request comment.

For the exact `G0.3` commit:

1. Open `students/DEx/week1/setup/connection_check.sql`.
2. Check it holds the final working SQL that ran through the Snowflake extension.
3. Save the file.
4. Review `git status`.
5. Stage only that file:

   ```bash
   git add students/DEx/week1/setup/connection_check.sql
   ```

6. Make the `G0.3` commit:

   ```bash
   git commit -m "G0.3 connect VS Code to Snowflake and run SQL file"
   ```

7. Push your branch:

   ```bash
   git push -u origin DEx_week1
   ```

Replace `DEx` in both commands.

For the exact `G0.4` commit:

`G0.4` is a separate local repository on your computer.
Do not copy that repository into the class repository.
Submit evidence of it like this:

1. In your separate project folder, run these commands in its own VS Code terminal:

   ```bash
   git status
   git log --oneline -1
   ```

2. In the class repository, create `students/DEx/week1/setup/team_project_repo.md`.
3. Write in it:

   - The local folder path of the separate project.
   - The folder list: `python/`, `sql/` and `docs/`.
   - The first line of that project's `README.md`.
   - The output of `git log --oneline -1` from the separate project.

4. Add no secrets and no data to that file.
5. Save the file.
6. Review `git status`.
7. Stage only that file:

   ```bash
   git add students/DEx/week1/setup/team_project_repo.md
   ```

8. Make the `G0.4` commit:

   ```bash
   git commit -m "G0.4 create team project repository"
   ```

9. Push your branch:

   ```bash
   git push -u origin DEx_week1
   ```

Replace `DEx` in both commands.
The separate repository stays on your computer.
The class commit holds only the evidence file.

- [ ] `students/DEx/week1/setup/hello.md` is in the first commit.
- [ ] The first commit message is `G0.1 first commit and setup check`.
- [ ] `students/DEx/week1/setup/snowflake_login_check.sql` is in the `G0.2` commit.
- [ ] The `G0.2` commit message is `G0.2 Snowflake login and first query`.
- [ ] `students/DEx/week1/setup/connection_check.sql` is in the `G0.3` commit.
- [ ] The `G0.3` commit message is `G0.3 connect VS Code to Snowflake and run SQL file`.
- [ ] `students/DEx/week1/setup/team_project_repo.md` is in the `G0.4` commit.
- [ ] The `G0.4` commit message is `G0.4 create team project repository`.
- [ ] Your pull request contains no secrets or data files.
- [ ] Each completed task has its own commit.

Next: Return to your week 1 problem statement and continue with the next task.
