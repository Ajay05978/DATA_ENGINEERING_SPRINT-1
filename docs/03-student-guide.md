# Student Guide

## 1. What We Are Doing
This guide helps you set up Git and GitHub once, find your own student folder, find the weekly tasks, and follow the weekly working loop. You will learn how to make one branch per week, commit one task at a time, push your work, and open a pull request. It also shows where each kind of work happens and what to do when you get stuck.

## 2. Why We Are Doing It
We work on a client project for a fintech due-diligence company. Analysts check Indian companies: is the company real and active (MCA), was it ever in insolvency (IBBI), how does it compare to its state (MCA CDM), and what was the interest-rate backdrop (RBI). The final product is a Snowflake data platform with bronze, silver and gold layers (Medallion), a Kimball star schema, dbt, Airflow, Great Expectations and a Metabase dashboard. These are explained in plain language in `docs/01-project-brief.md`, section 4.

The repository has two zones on purpose. Your individual folder is for practice. You try every skill yourself, and a mistake there costs nothing. The `platform` folder is the one real version of the pipeline. The rotating team builds it each week, and everyone depends on it working. So every person practises every skill, and the cohort still ships one shared product that is good enough to show an employer. Both zones matter. They just have different jobs.

Your commits turn squares green on your GitHub contribution graph only after they land in the class repository's default branch. Pushing to your own fork does not do this on its own. So do the setup in section 5.1 correctly, especially the email, and always finish a week with a pull request. Once a core admin merges it, the contributions appear on your profile.

## 3. What to Learn First (Prerequisites)
Before you start, you need:
- A GitHub account.
- VS Code.
- Git installed on your computer.
- Your DE number from the program lead, for example DE12.

For installs and the Snowflake connection from VS Code, follow `docs/14-vscode-snowflake-setup-guide.md`.

You do not need to know Git or GitHub before you start. Follow the steps in order. If a command does not do what this guide says it does, ask in Discord before you try random fixes.

Core words you will see again and again:
- **Fork:** your own copy of the class repository, under your GitHub account.
- **Clone:** a copy of a repository on your own laptop.
- **Remote:** a nickname for a repository address, such as `origin` or `upstream`.
- **Branch:** a named line of work inside a repository, such as `DE12_week1`.
- **Commit:** a saved checkpoint with a short message.
- **Pull request (PR):** a request to add your branch into the class repository.

## 4. Where to Find Resources
Where you work:

| Kind of work | Where | What exactly |
|---|---|---|
| Write SQL | VS Code | `.sql` files inside your week folder, so they can be committed |
| Run SQL | Snowflake (Snowsight, in the browser) | Projects > Workspaces. Paste the SQL, pick your role and warehouse, run it |
| Write Python | VS Code | `.py` files inside your week folder |
| Run Python | VS Code terminal | With the project virtual environment active: `source .venv/bin/activate` on macOS/Linux, `.venv\Scripts\activate` on Windows |
| Download data | Browser | data.gov.in, ibbi.gov.in, data.rbi.org.in. Save into `data/raw/...` at the repo root |
| Commit and push | VS Code terminal | Git commands, from the repo folder |
| Review a pull request | Browser (GitHub) | Read the changed files, leave comments, or approve |

Snowflake is where the data lives. VS Code is where your work is saved. When SQL finally works in Snowsight, make sure the `.sql` file in VS Code holds that final working version, then commit it.

Docs and links:

| Resource | What it helps with |
|---|---|
| `docs/01-project-brief.md` | The client story. Section 4 explains Medallion and Kimball in plain language. |
| `docs/04-week-map.md` | The week by week view of the project. |
| `docs/05-task-list.md` | The full list of project tasks. |
| `docs/07-platform-and-cicd-guide.md` | Read this before you work in the `platform` and `delivery` folders. |
| `docs/platform-rotation-log.md` | The record of platform rotation turns. |
| `docs/10-troubleshooting.md` | Check this first when a command fails. |
| `docs/14-vscode-snowflake-setup-guide.md` | Install help and the Snowflake connection from VS Code. |
| `docs/15-team-project-repo-and-delivery.md` | Your team project repo and the delivery folder rules. |
| https://cbea.ms/git-commit/ | Why a good commit message matters and how to write one. |

Search keywords if you get stuck: `git fork clone upstream`, `git branch commit push`, `pull request base branch`, `git conflict markers`, `git rm cached data file`.

## 5. Step-by-Step Guide
Work in this order. Do one task, put it in one file, make one commit, then push. Follow the "Save as" path in your `problem_statement.md` exactly. If you get stuck, read the error message, check `docs/10-troubleshooting.md`, then ask in Discord with the full error text.

### 5.1 One time setup: fork, clone, configure
You do this once, at the very start of the project. For installs and the Snowflake connection from VS Code, follow `docs/14-vscode-snowflake-setup-guide.md`.

**Step 1. Fork the repository.**
Open the class repository page on GitHub. Click "Fork" in the top right. Your fork is your own copy. You can change it freely and it does not touch anyone else's work.

**Step 2. Clone your fork to your computer.**
Open the VS Code terminal and run this, replacing `YOUR_USERNAME` with your GitHub username:

```
git clone https://github.com/YOUR_USERNAME/Data-Sprint-1.git
cd Data-Sprint-1
```

**Step 3. Tell Git who you are.**
Your commits turn squares green on your GitHub profile only if the email in the commit matches your GitHub email. Run these inside the cloned folder:

```
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

Use the exact email your GitHub account uses. To check it, open GitHub Settings, then Emails.

**Step 4. Connect to the main class repository.**
Your fork will get out of date as the class repo gets new work. Add a second remote called `upstream` so you can pull those updates:

```
git remote add upstream https://github.com/StartwithDot/Data-Sprint-1.git
```

Check that both remotes exist:

```
git remote -v
```

You should see `origin` pointing at your fork and `upstream` pointing at the class repository.

**Step 5. Open the folder in VS Code.**
In VS Code choose File > Open Folder and pick the `Data-Sprint-1` folder. From now on, every file you write is saved here and every Git command runs in the VS Code terminal.

**Check it worked:** `git remote -v` shows `origin` and `upstream`. `git config user.email` shows the email your GitHub account uses.

### 5.2 Finding your folder
The program lead gives you a DE number, for example DE12.

Everything you write goes inside `students/DEx/`, where `DEx` is your own number. If you are DE12, your folder is `students/DE12`. Inside it there is one folder per week: `week1`, `week2`, and so on, up to `week15`.

Do not put your practice files anywhere else in the repository. Review is much slower when work is scattered.

#### Your week folder
This is what a week 1 folder looks like once you have done a few tasks. File names are examples, your tasks tell you the exact names:

```
students/DE12/week1/
├── problem_statement.md
├── discovery/
│   └── b1_client_questions.md
├── sql/
│   └── s1/
│       └── 01_create_database.sql
├── python/
│   └── p1/
│       └── read_mca_csv.py
└── design/
    └── a1_architecture.mmd
```

- `discovery/` holds your written answers for business and delivery tasks (IDs starting with B).
- `sql/` holds SQL tasks (IDs starting with S), in subfolders like `s1/`, `s2/`.
- `python/` holds Python tasks (IDs starting with P), in subfolders like `p1/`, `p2/`.
- `design/` holds architecture diagrams (IDs starting with A).

Downloaded data never goes in your week folder. Keep it at the repo root:

```
data/
└── raw/
    ├── mca/
    ├── ibbi/
    └── rbi/
```

The `.gitignore` file already excludes `data/raw/`, so these files never go into a commit. Do not force them in.

### 5.3 Finding your weekly tasks
The project runs for 15 weeks and gets harder each week.

Open your folder, then the current week folder, for example `students/DE12/week1`. Inside it you will find `problem_statement.md`. That file has the tasks for the week, numbered in working order.

In week 1, task group G0 covers your first commit, Snowflake login, VS Code connection and project folder.

Every task in that file uses the same layout, so you always know what to look for:

- **Task N - Title** with an ID such as `S1.2`. The ID never changes. It goes in your commit message and your PR title.
- **Where you work:** the exact place, for example VS Code or Snowflake (Snowsight).
- **Why we do this:** how the task helps the client project.
- **Learn first:** the videos or pages to watch before you start, with a time estimate.
- **Steps:** small numbered steps, with commands or SQL starters you can copy.
- **Save as:** the full path of the file to create.
- **Done when:** the checks that tell you the task is finished.
- **Commit message:** the exact message to use for this task.

Work through the tasks in order. The full list of the project is in `docs/05-task-list.md`, and the week by week view is in `docs/04-week-map.md`, but your weekly `problem_statement.md` is the file you actually work from.

### 5.4 The working loop
This is how every week works. You use one branch per week and one commit per task.

**Step 1. Update your main branch.**
Do this first, every time you start work.

```
git checkout main
git pull upstream main
git push origin main
```

The first command moves you to your main branch. The second brings in new work from the class repo. The third copies it to your fork, so your fork main is not behind.

**Step 2. Start your week branch.**
Once per week, create a branch named `DEx_weekN`, where `DEx` is your number and `N` is the week number from 1 to 15:

```
git checkout -b DE12_week1
```

You now work on this branch for the whole week. All your week 1 commits go here.

**Step 3. Do one task.**
Open your week folder in VS Code and do one task from `problem_statement.md`. Follow the "Save as" path exactly.

**Step 4. Commit that one task.**

```
git status
git add students/DE12/week1/sql/s1/01_create_database.sql
git commit -m "S1.1 create bronze, silver and gold schemas"
```

`git status` shows what changed. `git add` chooses the file for the next commit. `git commit` saves the checkpoint. Add one task, commit one task.

**Step 5. Push the branch to your fork.**
The first push of the week needs `-u`, so Git remembers where to send the branch:

```
git push -u origin DE12_week1
```

After that, `git push` alone is enough.

**Step 6. Open the pull request.**
You open one PR per week, not one per task.

1. Open your fork on GitHub in the browser.
2. Use the branch dropdown to switch to `DE12_week1`.
3. Click "Contribute", then "Open pull request".
4. Check the base repository is the class repo and the base branch is `main`. The base repository is where your work will land. The compare branch is your `DE12_week1`.
5. Write a title with the task IDs, for example `S1.1, S1.2 and P1.1 week 1 tasks`. Add one or two lines about what the PR covers.
6. Click "Create pull request".

A reviewer reads it and either merges it or leaves comments. The class repo never squash-merges, so your commits stay as separate commits in history.

**Step 7. Add the next task to the same branch.**
Stay on `DE12_week1`. Write the next file, then:

```
git add students/DE12/week1/python/p1/read_mca_csv.py
git commit -m "P1.1 read the MCA CSV and print the first rows"
git push
```

The open PR picks up the new commit by itself. Do not open a second PR for the same week.

**Step 8. After review comments.**
Fix the file in VS Code, then commit and push again, on the same branch:

```
git add students/DE12/week1/sql/s1/01_create_database.sql
git commit -m "S1.1 fix schema name to match the brief"
git push
```

The PR updates on its own. You do not open a new one and you do not close the old one.

If your branch has fallen behind and the reviewer asks you to update it, first run Step 1 on `main`, then:

```
git checkout DE12_week1
git merge main
git push
```

**What a good commit message looks like:** start with the task ID, then a short summary that starts with a verb, for example `S1.2 create raw MCA table`. "update" and "fix" tell a reviewer nothing. Read https://cbea.ms/git-commit/ for the reasons behind this.

### 5.5 One task, one file, one commit
This is a strict rule.

Do not build one giant file for the whole week. When you finish a task, put it in its own file, make one commit for just that task, and push.

Small commits are easier to review and easier to fix. They also show a clear record of your work. Your `problem_statement.md` already gives the file path for each task, so follow those paths exactly.

#### Common Git mistakes
Four things happen to almost everyone. Here is the fix for each.

- **You committed a data file.** Run `git rm --cached data/raw/mca/yourfile.csv`, then `git add .gitignore` if you changed it, then commit with a message such as `remove data file from git`. The file stays on your laptop. It just leaves the commit.
- **You committed on `main` by mistake.** Run `git checkout -b DE12_week1`. Your commits move to the new branch with you. Then run `git branch -f main origin/main` to put main back where it was. Do not use `reset --hard` unless the program lead tells you to.
- **You see a message with `CONFLICT` and lines of `<<<<<<<`, `=======`, `>>>>>>>`.** Two versions of the same lines met. Open the file, keep the version you want, delete those marker lines, save, then `git add` the file and `git commit`.
- **You pushed to the wrong remote.** `origin` is your fork, `upstream` is the class repo. If you typed `git push upstream ...` and saw "Permission denied", nothing broke, the push simply failed. Check with `git remote -v` and push again to `origin`. If the push did go through, tell the program lead. Do not use a force push (a push that overwrites history) to fix it.

### 5.6 Your team project repo and the delivery folder
Your team keeps its own project repo on a laptop in VS Code. Read `docs/15-team-project-repo-and-delivery.md` for the full rule. Only the team leader updates the class repo's `delivery/` and `platform/` folders. Everyone else keeps daily practice commits in `students/DEx/weekN/`.

### 5.7 Platform rotation
Each week the program lead picks a small group of students to work in the `platform` and `delivery` folders instead of their own practice folder.

The 15-week plan also has tool weeks: dbt in week 12, Great Expectations and Airflow in week 14, and Metabase in week 15.

If you are picked, you build the shared version of what the cohort just practised. The rules are stricter there and automated checks run on your work. Read `docs/07-platform-and-cicd-guide.md` before you start. The turns are recorded in `docs/platform-rotation-log.md`, and everyone gets a turn.

### 5.8 When you get stuck
You will get stuck. Everyone does. It is part of the work, not a sign that you are behind.

Post in the Discord channel. Say three things: what you were trying to do, what you tried, and what happened instead. Paste the full error message. Ask for a hint that points you in the right direction, not for the finished answer. If a command fails, the answer is very often in `docs/10-troubleshooting.md`.

## 6. What to Commit
When you finish the setup, these must be true:

- [ ] You forked the class repository to your GitHub account.
- [ ] You cloned your fork to your laptop and opened the folder in VS Code.
- [ ] `git config user.name` and `git config user.email` use your real name and your GitHub email.
- [ ] `git remote -v` shows `origin` (your fork) and `upstream` (the class repo).
- [ ] You can find your folder at `students/DEx/`, for example `students/DE12/`.
- [ ] You can find the week folder and open `problem_statement.md`.
- [ ] You created your week branch, for example `DE12_week1`.
- [ ] Each task has its own file, using the exact "Save as" path from `problem_statement.md`.
- [ ] Each commit message starts with the task ID, for example `S1.2 create raw MCA table`.
- [ ] You pushed the branch to your fork with `git push -u origin DE12_week1`.
- [ ] You opened one pull request for the week, with the task IDs in the title.
- [ ] No data files from `data/raw/` are in your commits.
- [ ] No passwords or secrets are in your files.

If you are short on time, cut in this order: extra reading, then extra polish, but never skip the commit and the pull request.

Now check your work one last time. Read the changed files in your pull request on GitHub. Make sure the files are in the right folder and the commit messages have the task IDs. If something is wrong, fix it on the same branch, commit, and push. The pull request updates by itself.
