# Week 9 - Extractors and the design sign-off

**Data Sprint 1 · Week 9 of 15 · Theme: get real data out of a PDF and a web page, then put what you learned back into the design**

Read this whole file before you start. Then work through the task groups in order. This is new for most people. Go slowly.

## 1. What We Are Doing

This week you write Python code that pulls real rows out of the IBBI insolvency PDF and out of the MCA CDM state statistics web page. Then you put all three extractors (RBI, IBBI, CDM) behind one base class, so every source works the same way. Last, you use what the extracted data showed you to update the design: one risk paragraph and one updated pipeline flow diagram.

The task groups this week:

- **Group 1 - Object Oriented Extractors:** Task 1 (`P7.1`), Task 2 (`P7.2`), Task 3 (`P7.3`)
- **Group 2 - Design Review from the Python Side [MILESTONE]:** Task 4 (`P8.1`)
- **Group 3 - Design Review Update:** Task 5 (`A3.1`)

## 2. Why We Are Doing It

Our client is a fintech due-diligence company. Its analysts check Indian companies: is the company real and active (MCA), was it ever in insolvency (IBBI), how does it compare with its state (MCA CDM), and what was the interest-rate backdrop (RBI). Two of those sources are not clean tables. IBBI publishes a PDF list, and MCA CDM publishes HTML tables. So this week you turn a PDF and a web page into rows the platform can load. You also check that the design agreed earlier works with the extracted data. That review is the milestone of the week.

**The milestone this week: P8 Star Schema Design, Python side.** The star schema design agreed in week 7 must work with the extracted data. If your extractor produces duplicate CINs or unparseable dates, the design has to handle it, and this is the week to say so. Your week 8 pipeline flow diagram is a separate input for Task 5.

## 3. What to Learn First (Prerequisites)

### Before this week: what you must already have

Check each line before Task 1. If something is missing, redo that short earlier task first, or ask in Discord.

| What this week uses | Came from | If you are missing it |
|---|---|---|
| Python installed, `.venv` created and activated from the repo root | Week 2, task `P0.4` | Redo the week 2 venv steps, then `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows) |
| The validator function you wrote for the P2 Python task (it checks one row and stops on bad values) | Week 4, tasks `P2.1` to `P2.3` | Open your week 4 file in `students/DEx/week4/` and copy the function into this week's script, or write a small version now |
| Your RBI script that reads the policy rate file and carries the last rate forward | Week 7, task `P4.2` | You need it for Task 3. Reopen the week 7 file. If it is lost, rebuild the smallest version that reads the saved CSV. The file ends in 2024, so the last rate is carried forward - that is fine |
| Your pipeline flow diagram `pipeline_flow.md` in your week 8 design folder | Week 8 | You need it for Task 5. Rebuild a simple version from `docs/04-week-map.md` notes if lost |
| `data/raw/` folder at the repo root, already ignored by `.gitignore` | Week 2 | Create `data/` at the repo root, then `raw/` inside it. Check that `.gitignore` excludes `data/raw/` |

**Packages you install this week** (inside the activated `.venv`, from the repo root):

```bash
pip install pdfplumber beautifulsoup4 requests
```

### By the end of this week you can

- Pull real data out of a PDF and out of an HTML table
- Refactor three extractors behind one base class
- Write a specific, named risk your Python work creates for the design
- Update the pipeline flow diagram with the issues the extracted data showed you

### Core concepts to review first

- **Class and instance:** a class is a template, an instance is one object made from it.
- **Class variable:** a value that belongs to the class, not to one instance.
- **Inheritance:** a subclass takes the shape of a parent class and fills in its own parts.
- **Abstract base class:** a template that lists the steps a class must have. You cannot run it directly.
- **PDF table extraction:** reading rows from a PDF page with pdfplumber.
- **HTML table extraction:** fetching a page with `requests` and finding the table with BeautifulSoup.
- **Indian number format:** `12,34,567` means `1234567`. You must remove the commas.
- **Your P2 validator:** you reuse it to check each extracted row.
- **Mermaid flowchart syntax:** used for the pipeline flow diagram.

## 4. Where to Find Resources

### Where you work (read once)

- **VS Code** is your home base. You write, save and commit every file here.
- **Snowflake (Snowsight, in the browser, under Projects > Workspaces)** is where SQL runs. This week has no SQL tasks, so you only open it if you want to check old tables.
- **VS Code terminal** is where you run Python and Git commands, from the repo root, with `.venv` activated.
- Full paths look like `students/DEx/week9/...`. Replace **DEx** with your own number, for example `DE12`.
- Git steps are the same every week: `docs/03-student-guide.md`.

> **Team project repo:** only the team leader copies reviewed files from the team repo into the class repo `delivery/` or `platform/` folder. Everyone keeps practice files in `students/DEx/week9/`.

### Before you open this file: where to look

| You need | Where to look |
|---|---|
| Git commands (add, commit, push, pull request) | `docs/03-student-guide.md` |
| Client story and data sources | `docs/01-project-brief.md` |
| Meaning of an unknown word | `docs/08-glossary.md` |
| A tool is broken | `docs/10-troubleshooting.md` |
| What a tool is for | `docs/11-tools-and-technology.md` |

**How your tasks are numbered.** Each task has an ID. The first letter tells you the type: B = business and delivery, S = SQL and data modeling, P = Python, D = data platform, A = architecture diagrams. The ID goes in your commit message and your pull request (PR) title. One task, one file, one commit, then a PR to the class repo. Never squash-merge.

### The week at a glance

**Your week in order.** The time is for the learning only, not the coding.

| Step | Learn first | Then do |
|---|---|---|
| **1** | Corey video #40: "Python OOP Tutorial 1: Classes and Instances" and Corey video #41: "Python OOP Tutorial 2: Class Variables"; the pdfplumber page at https://pypi.org/project/pdfplumber/ (about 1 h 30 min) | Task 1 - the IBBI PDF extractor |
| **2** | Corey video #46: "Python Tutorial: Web Scraping with BeautifulSoup and Requests"; the BeautifulSoup docs quick start (about 1 h 15 min) | Task 2 - the CDM extractor |
| **3** | Corey video #43: "Python OOP Tutorial 4: Inheritance - Creating Subclasses"; the Python docs page for the `abc` module (about 1 hour) | Task 3 - the base class refactor |
| **4** | Re-read your week 8 pipeline flow diagram (20 min) | Tasks 4 and 5 - the design review paragraph and the diagram update |
| **5** | Nothing new | Cohort review: extractor demos, design review sign-off |

Optional if you have time: Corey video #42 "Python OOP Tutorial 3: classmethods and staticmethods", #44 "Python OOP Tutorial 5: Special (Magic/Dunder) Methods", #45 "Python OOP Tutorial 6: Property Decorators", #30 "Python Tutorial: re Module", #47 "Python Tutorial: Working with JSON Data using the json Module".

### Search keywords if you get stuck

- `pdfplumber extract_table returns None`
- `BeautifulSoup find table rows`
- `Python abstract base class abstractmethod`
- `requests get timeout example`
- `Python parse Indian number format commas`
- `Mermaid flowchart syntax`

## 5. Step-by-Step Guide

**How to work:** one task, one file, one commit. Do not mix two tasks in one commit. Git steps are in `docs/03-student-guide.md`. When you finish all tasks in your week folder, open a pull request. Never squash-merge.

### Group 1 - Object Oriented Extractors

#### Task 1 - Write the IBBI PDF extractor (ID `P7.1`)

##### 1. What We Are Doing

You will write a Python script that opens the IBBI insolvency PDF, extracts the table rows, validates them, and saves a clean CSV. Where you work: browser first (find the PDF on ibbi.gov.in), then VS Code (write the script), then VS Code terminal (run it).

##### 2. Why We Are Doing It

IBBI publishes lists of companies in insolvency proceedings. An analyst must check "was this company ever in insolvency". The list is a PDF, so we need code that turns the PDF into rows we can load.

##### 3. What to Learn First

- Corey video #40: "Python OOP Tutorial 1: Classes and Instances" (about 20 min).
- The pdfplumber page at https://pypi.org/project/pdfplumber/ - read "Extracting text" and "Extracting tables" only (about 25 min). Skip the rest of the page for now. Total about 1 hour.
- Your P2 validator function from week 4. You reuse it here.
- Optional: if a number needs cleaning, Corey video #30: "Python Tutorial: re Module".

##### 4. Where to Find Resources

- Video: Corey video #40: "Python OOP Tutorial 1: Classes and Instances" (about 20 min).
- Page: https://pypi.org/project/pdfplumber/ - sections "Extracting text" and "Extracting tables" (about 25 min).
- Docs in this repo: `docs/03-student-guide.md` for Git steps, `docs/10-troubleshooting.md` if a tool is broken.
- Search keywords: `pdfplumber extract_table`, `pdfplumber multiple pages`, `Python csv.writer example`.

##### 5. Step-by-Step Guide

1. In VS Code at the repo root, create these folders if they do not exist: `data/raw/ibbi/`, `data/raw/cdm/`, `students/DEx/week9/python/p7/`, `students/DEx/week9/design/`. Check: the folders show in the VS Code Explorer.
2. In the browser, open https://ibbi.gov.in and find the current CIRP list PDF. Save it by hand into `data/raw/ibbi/` at the repo root. Put the pull date in the file name. This folder is never committed.
3. In VS Code, in `students/DEx/week9/python/p7/`, create `ibbi_extractor.py` (right-click the `p7` folder > New File). Start from this skeleton and fill the TODOs:
   ```python
   import csv
   import pdfplumber

   PDF_PATH = "data/raw/ibbi/"   # TODO: add the exact file name you saved
   OUT_PATH = "data/raw/ibbi/ibbi_cirp_<quarter>_<pulldate>.csv"  # TODO: fill in

   def pull(path):
       rows = []
       with pdfplumber.open(path) as pdf:
           for page in pdf.pages:
               table = page.extract_table()   # TODO: what does this return?
               # TODO: skip repeated header rows on later pages
               rows.extend(table or [])
       return rows

   def validate(rows):
       kept, rejected = [], []
       # TODO: map each row to named fields, then reuse the P2 validator on each row.
       # Catch each row's validation error so processing continues.
       return kept, rejected

   def save(rows, out_path):
       # TODO: write one CSV with csv.writer
       pass

   if __name__ == "__main__":
       rows = pull(PDF_PATH)
       print(len(rows), "raw rows")
       kept, rejected = validate(rows)
       print(len(kept), "kept,", len(rejected), "rejected")
       save(kept, OUT_PATH)
   ```
4. Step 1: open the PDF and print the text of page 1. Run the script. Check: the terminal prints text from page 1.
5. Step 2: extract the table from page 1 only. Run the script. Check: it prints rows from page 1.
6. Step 3: loop every page. Run the script. Check: the row count is bigger than page 1 alone.
7. Step 4: map the extracted columns to named fields (for example CIN, company name, bench, start date, admitted claims). Run the script. Check: the first row shows the named fields.
8. Step 5: add the validate step. Adapt your P2 validation rules to IBBI, and catch each row's validation error so processing continues. Run the script. Check: it prints a kept count and a rejected count.
9. Step 6: write the CSV. Run the script. Check: the CSV exists in `data/raw/ibbi/`.
10. Run it from the repo root with the venv active:
    ```bash
    python students/DEx/week9/python/p7/ibbi_extractor.py
    ```
11. Copy the first ten extracted rows into `notes.md`, and write down anything the PDF did that surprised you (merged cells, headers on every page, numbers with commas).
12. If the PDF layout differs from what you expected, write what you see in `notes.md` and ask in Discord. That is a normal result, not a failure.

**If you get stuck:** read the full error message first, it often names the exact line. Search `pdfplumber extract_table returns None` and try printing the text of page 1 instead of the table. Check `docs/10-troubleshooting.md`, then ask in Discord and paste the error text.

**How to think about it:** the PDF will not extract cleanly on the first attempt. Merged cells, headers repeating on every page, and numbers with commas are all normal. Fix the parsing and record what was wrong in your notes. Websites change too. If the IBBI PDF layout differs from this task, write what you see and ask in Discord. That is a valid finding for the design review.

##### 6. What to Commit

- [ ] `students/DEx/week9/python/p7/ibbi_extractor.py`
- [ ] `students/DEx/week9/python/p7/notes.md`
- [ ] The CSV stays in `data/raw/ibbi/` and is not committed

Done when:

- [ ] the script runs from the repo root without errors
- [ ] the CSV exists in `data/raw/ibbi/` with a name that has the quarter and pull date
- [ ] `notes.md` has the first ten rows and your notes
- [ ] `git status` shows no data file

Commit message: `P7.1 add IBBI PDF extractor and first extracted rows`

#### Task 2 - Write the CDM portal extractor (ID `P7.2`)

##### 1. What We Are Doing

You will write a Python script that reads the MCA CDM state statistics table from the web page and saves the numbers as a clean CSV. Where you work: browser first (https://mcacdm.nic.in, find the state statistics table), then VS Code, then VS Code terminal.

##### 2. Why We Are Doing It

The analyst compares one company with its state. MCA CDM publishes state statistics as HTML tables, so we read the table from the page and store plain numbers.

##### 3. What to Learn First

- Corey video #46: "Python Tutorial: Web Scraping with BeautifulSoup and Requests" (about 45 min).
- The BeautifulSoup quick start at https://www.crummy.com/software/BeautifulSoup/bs4/doc/ (about 20 min).
- How to convert Indian number format: `12,34,567` becomes `1234567`. Remove commas, and handle `-` or empty text.
- Optional: Corey video #47: "Python Tutorial: Working with JSON Data using the json Module" if the page loads its numbers from JSON. Total about 1 hour 15 min.

##### 4. Where to Find Resources

- Video: Corey video #46: "Python Tutorial: Web Scraping with BeautifulSoup and Requests" (about 45 min).
- Page: https://www.crummy.com/software/BeautifulSoup/bs4/doc/ - quick start (about 20 min).
- Docs in this repo: `docs/10-troubleshooting.md`.
- Search keywords: `BeautifulSoup find table rows`, `requests get timeout`, `Python parse Indian number format commas`.

##### 5. Step-by-Step Guide

1. In the browser, open https://mcacdm.nic.in and find the state statistics table. Note the page address and the pull date.
2. In `students/DEx/week9/python/p7/`, create `cdm_extractor.py`. Skeleton:
   ```python
   import csv
   import requests
   from bs4 import BeautifulSoup

   URL = "https://mcacdm.nic.in"   # TODO: the exact page address you found
   OUT_PATH = "data/raw/cdm/cdm_state_stats_<pulldate>.csv"  # TODO: fill in

   def indian_to_int(text):
       # "12,34,567" -> 1234567
       # TODO: remove commas, handle "-" or empty text
       pass

   def pull(url):
       page = requests.get(url, timeout=30)
       soup = BeautifulSoup(page.text, "html.parser")
       table = soup.find("table")   # TODO: is this the right table?
       return table

   def validate(rows):
       # TODO: drop rows where the state name is empty
       return rows

   def save(rows, out_path):
       # TODO: csv.writer
       pass

   if __name__ == "__main__":
       print(pull(URL))
   ```
3. Step 1: fetch the page and confirm you get HTML. Run the script. Check: `page.status_code` is 200 and you see a small piece of `page.text`.
4. Step 2: find the right table. Run the script. Check: the table you print is the state statistics table.
5. Step 3: loop the rows. Run the script. Check: it prints the rows you expect.
6. Step 4: convert Indian number format to plain numbers. Run the script. Check: at least one number went through the conversion.
7. Step 5: write the CSV. Run the script. Check: the CSV exists in `data/raw/cdm/`.
8. Run it from the repo root with the venv active:
   ```bash
   python students/DEx/week9/python/p7/cdm_extractor.py
   ```
9. Copy a sample of output rows into `notes.md`. If the live page differs from what you expected, write what you see and ask in Discord.

**If you get stuck:** print `page.status_code` and a small piece of `page.text` to check you really got HTML. Search `BeautifulSoup find table rows` and try `soup.find_all("table")` to see how many tables the page has. If the CDM portal does not open, note it and use the saved copy the program lead gives you.

**How to think about it:** Indian number format is the trap here. `12,34,567` is not twelve point three four. It is one million two hundred thirty four thousand five hundred sixty seven. Test your converter on a few values before you trust the CSV.

##### 6. What to Commit

- [ ] `students/DEx/week9/python/p7/cdm_extractor.py`
- [ ] `students/DEx/week9/python/p7/notes.md` with the sample rows
- [ ] The CSV stays in `data/raw/cdm/` and is not committed

Done when:

- [ ] the script runs and prints or writes rows from the real page
- [ ] at least one number went through the Indian-format conversion
- [ ] the CSV is in `data/raw/cdm/`
- [ ] `notes.md` has sample rows and the page address with the pull date

Commit message: `P7.2 add MCA CDM state statistics extractor`

#### Task 3 - Refactor behind one base class (ID `P7.3`)

##### 1. What We Are Doing

You will move your three extractors (RBI, IBBI, CDM) behind one base class, so each source has the same three steps: pull, validate, save. Where you work: VS Code (write and edit the files), then VS Code terminal (run each subclass once).

##### 2. Why We Are Doing It

You now have three extractors (RBI from week 7, IBBI, CDM). Before the scheduler arrives later in the sprint, they should share one shape, so a failure looks the same for every source.

##### 3. What to Learn First

- Corey video #41: "Python OOP Tutorial 2: Class Variables" (about 15 min).
- Corey video #43: "Python OOP Tutorial 4: Inheritance - Creating Subclasses" (about 20 min).
- The `abc` page at https://docs.python.org/3/library/abc.html (about 20 min).
- What an **abstract base class** is: a template that lists the steps every extractor must have. You cannot run the template itself. Each source writes its own class that fills in those steps. If a new source forgets one step, Python raises an error when you try to create an instance of a subclass that still has an abstract method.
- Optional: Corey videos #42, #44, #45. Total about 1 hour.

##### 4. Where to Find Resources

- Videos: Corey video #41: "Python OOP Tutorial 2: Class Variables" (about 15 min), Corey video #43: "Python OOP Tutorial 4: Inheritance - Creating Subclasses" (about 20 min).
- Page: https://docs.python.org/3/library/abc.html (about 20 min).
- Docs in this repo: `docs/10-troubleshooting.md`.
- Search keywords: `Python abstract base class abstractmethod`, `Python subclass override method`, `Python ABC cannot instantiate`.

##### 5. Step-by-Step Guide

1. Read this once. An **abstract base class** is a template. It lists the steps every extractor must have: pull, validate, save. You cannot run the template itself. Each source writes its own class that fills in those steps. If a new source forgets one step, Python raises an error when you try to create an instance of a subclass that still has an abstract method.
   ```python
   from abc import ABC, abstractmethod

   class BaseExtractor(ABC):
       name = "base"

       @abstractmethod
       def pull(self):
           """Get the raw data. Return a list of rows."""

       @abstractmethod
       def validate(self, rows):
           """Return kept rows and rejected rows."""

       @abstractmethod
       def save(self, rows):
           """Write one CSV under data/raw/... Return the path."""

       def run(self):
           rows = self.pull()
           kept, rejected = self.validate(rows)
           return self.save(kept), len(rejected)
   ```
2. In `students/DEx/week9/python/p7/`, create a folder `extractors/` (right-click `p7` > New Folder).
3. Inside it create `base.py` with the skeleton above and your notes explaining it in your own words.
4. Move your three extractors into `extractors/` as `rbi_extractor.py`, `ibbi_extractor.py`, `cdm_extractor.py`. Each one becomes a subclass: `class IbbiExtractor(BaseExtractor):` and so on. Put your existing code inside `pull`, `validate`, `save`.
5. Every subclass's `validate()` method must return two lists: `kept` and `rejected`. Update your CDM `validate()` from Task 2, which returned one list, so it now returns `kept, rejected`.
6. In each extractor file, add `from base import BaseExtractor` at the top. With the run command in this task, this direct import is the one that works.
7. Keep your old files working or delete them, but say which you did in `notes.md`.
8. Replace the old `if __name__ == "__main__":` block in each extractor with a block that creates one instance of the subclass and calls its `run()` method. Example for `ibbi_extractor.py`:
   ```python
   if __name__ == "__main__":
       IbbiExtractor().run()
   ```
   Do the same in `rbi_extractor.py` and `cdm_extractor.py`, using that file's class name.
9. Run each subclass once from the repo root:
   ```bash
   python students/DEx/week9/python/p7/extractors/ibbi_extractor.py
   ```
   Run the other two files the same way. Check: each file creates an instance and calls `run()` without errors.
10. Write two sentences in `notes.md` on what the refactor removed (repeated code, different function names, and so on).
11. Test it: if you remove `save` from one subclass, Python raises an error when you try to create an instance of a subclass that still has an abstract method. Run that file and write down what happened.
12. Restore `save`, then run the extractor again before committing.

**If you get stuck:** read the error, it often says "Can't instantiate abstract class" and names the missing method. Search `Python ABC cannot instantiate`. Check that each subclass uses `from base import BaseExtractor` when you run the file directly as shown.

**How to think about it:** this is the refactor where you stop copying code three times and use a shared base class. Keep each subclass small: only the code that is different for that source.

##### 6. What to Commit

- [ ] `students/DEx/week9/python/p7/extractors/base.py`
- [ ] `students/DEx/week9/python/p7/extractors/rbi_extractor.py`
- [ ] `students/DEx/week9/python/p7/extractors/ibbi_extractor.py`
- [ ] `students/DEx/week9/python/p7/extractors/cdm_extractor.py`
- [ ] `students/DEx/week9/python/p7/notes.md` with the two sentences and the test result

Done when:

- [ ] `base.py` has the abstract methods
- [ ] all three sources are subclasses in the `extractors/` folder
- [ ] each `validate()` returns kept and rejected lists
- [ ] each subclass has a main block that creates an instance and calls `run()`
- [ ] each subclass runs once without errors
- [ ] the two sentences and the test result are in `notes.md`

Commit message: `P7.3 move RBI, IBBI and CDM extractors behind one base class`

**On this group:** the PDF will not extract cleanly on the first attempt. Merged cells, headers repeating on every page, and numbers with commas are all normal. Fix the parsing, record what was wrong in notes, and keep the CSVs out of Git. Only code and sample rows are committed. Websites change. If the IBBI PDF layout or the CDM page differs from this task, write what you see in `notes.md` and ask in Discord - that is a valid finding for the design review. If the IBBI link or the CDM portal does not open, note it and use the saved copy the program lead gives you.

### Group 2 - Design Review from the Python Side [MILESTONE]

#### Task 4 - Write the pipeline risk paragraph (ID `P8.1`)

##### 1. What We Are Doing

You will write one short paragraph that names a real risk your Python extractors create for the pipeline design, and say what the design must change. Where you work: VS Code (write the paragraph), then the group design review meeting, then VS Code terminal (commit).

##### 2. Why We Are Doing It

The star schema design agreed in week 7 must handle real data. Your extractors are the first code to work with that data, so you are the one who must name the risk.

##### 3. What to Learn First

Nothing new. You use what you learned in Tasks 1 to 3. Re-read your week 8 design review notes and `notes.md` from Tasks 1 to 3. About 20 min.

##### 4. Where to Find Resources

- Your own week 8 design review notes.
- Your `students/DEx/week9/python/p7/notes.md` from Tasks 1 to 3.
- Docs in this repo: `docs/04-week-map.md`, `docs/08-glossary.md`.
- Search keywords: `data quality risk data pipeline`, `duplicate CIN check`, `load timestamp data warehouse`.

##### 5. Step-by-Step Guide

1. In `students/DEx/week9/design/`, create `pipeline_risk_notes.md` (right-click the `design` folder > New File).
2. Answer these questions in short notes first, then turn them into one paragraph:
   - Which table or file does my Python work feed, and which task or model reads it next?
   - What did the real data do that the design does not expect yet? (duplicate CIN, shifted column, missing page, unreadable number)
   - If that happens in week 11, who notices, and what should the pipeline do?
   - What small field would let the design spot this later? (load timestamp, source file name, quarter)
3. Write one paragraph, 5 to 8 sentences. Name the file, the column and the fix.
4. Bring it to the group design review and read it out.

**If you get stuck:** go back to your `notes.md` from Tasks 1 and 2 and pick one surprise you actually saw in the data. If you cannot name a file or a column, your risk is still too vague, so make it smaller. Ask in Discord with the sentence you have and say what feels weak.

**How to think about it:** Name an observed failure, the affected file and column, and the change you propose. Use evidence from your extraction notes, not a general worry. If you cannot point to a note, the risk is still too vague, so make it smaller.

##### 6. What to Commit

- [ ] `students/DEx/week9/design/pipeline_risk_notes.md`

Done when:

- [ ] the paragraph names a specific file or column and a specific failure
- [ ] it says what the design must change
- [ ] it uses evidence from your extraction notes
- [ ] you have read it in the design review

Commit message: `P8.1 add pipeline risk paragraph for design review`

### Group 3 - Design Review Update

#### Task 5 - Update the pipeline flow diagram (ID `A3.1`)

##### 1. What We Are Doing

You will copy your week 8 pipeline flow diagram into this week and update it with what the real extractors showed you, then write one line per change in a notes file. Where you work: VS Code (copy the week 8 file, edit the diagram and notes), then VS Code terminal (commit).

##### 2. Why We Are Doing It

The diagram is what new team members and the client read. If it does not match the real extractors, people trust a picture that is wrong.

##### 3. What to Learn First

- Re-read your week 8 `pipeline_flow.md`.
- If you need the diagram syntax, skim https://mermaid.js.org for the flowchart section only. About 20 min.
- **Mermaid** is a simple text syntax for diagrams. GitHub draws it when it sits inside a ```mermaid code block.

##### 4. Where to Find Resources

- Your week 8 file: `students/DEx/week8/design/pipeline_flow.md`.
- Page: https://mermaid.js.org - flowchart section only (about 20 min).
- Docs in this repo: `docs/04-week-map.md`.
- Search keywords: `Mermaid flowchart syntax`, `GitHub render mermaid markdown`.

##### 5. Step-by-Step Guide

1. Copy your week 8 diagram into this week: `students/DEx/week8/design/pipeline_flow.md` to `students/DEx/week9/design/pipeline_flow.md`. Keep the same file name.
2. Review each candidate issue: which source needs a browser download by hand, which PDF columns shift between quarters, which number formats needed converting, and where the bronze load timestamp sits. Change the diagram only where your extraction results support a change.
3. If your diagram is Mermaid, keep it inside a ```mermaid code block so GitHub draws it.
4. In `students/DEx/week9/design/pipeline_flow_notes.md`, list each change in one line: what changed and why, with the source of the evidence (your `notes.md` from Task 1 or 2).
5. Commit both files and open the pull request.

**If you get stuck:** check that the ```mermaid block opens and closes correctly, a broken block shows as plain text. Search `Mermaid flowchart syntax` and copy a small working example first. Look at your week 8 file and change one box at a time.

**How to think about it:** a diagram that is wrong is worse than no diagram, because nobody re-checks a picture. This is the first real check of whether the design works with the extracted data.

##### 6. What to Commit

- [ ] `students/DEx/week9/design/pipeline_flow.md`
- [ ] `students/DEx/week9/design/pipeline_flow_notes.md`

Done when:

- [ ] the week 9 diagram differs from the week 8 one only where your extraction results support a change
- [ ] every change has one line in the notes file saying why
- [ ] the diagram renders on GitHub

Commit message: `A3.1 update pipeline flow diagram after real extractors`

## 6. What to Commit

End of week checklist:

- [ ] Task 1 - the IBBI PDF extractor with the first ten rows in notes
- [ ] Tasks 2-3 - the CDM extractor and all three behind one base class, with what the refactor removed
- [ ] Task 4 - a specific, named risk your Python work creates for the design
- [ ] Task 5 - the pipeline flow diagram updated, with the change list in notes
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] No CSV, PDF, or ZIP anywhere in your commits

**If you are short on time, cut in this order:** Task 5 (A3.1), then Task 3 (P7.3). Never cut Task 1 (P7.1) or Task 4 (P8.1). The extractor is the hardest code in the sprint, and the design review is the milestone.

When your week folder is ready, open one pull request to the class repo with the task ID in the title (see `docs/03-student-guide.md`).

Next: `week10/problem_statement.md`.
