# Week 1 - Understand the ask

**Data Sprint 1 · Week 1 of 13 · Theme: understand the client, then make your first table, your first script, and your first diagram**

Read this whole file before you start anything. Then work through the task groups in order.

---

## Before you open this file

| You need | Where |
|---|---|
| The client story and the four data sources | `docs/01-project-brief.md` |
| Git, Python, and Snowflake working | `docs/02-tools-setup.md` |
| Fork, clone, commit, pull request commands | `docs/03-student-guide.md` |
| A word you do not know | `docs/08-glossary.md` |
| This week's reading and videos | `docs/09-resources.md` |
| A tool that will not work | `docs/10-troubleshooting.md` |
| What a tool is for and why we use it | `docs/11-tools-and-technology.md` |

**How your tasks are numbered:** each week numbers its tasks 1 to N in the order you should do them. Every task also keeps a stable ID like `S1.1` - that ID is what goes in your commit message and pull request title, so anyone can find the task behind any commit.

Every task below names the exact file path to commit to, and that path sits inside this same week folder.

---

## By the end of this week you can

- Describe the client's real problem in writing, in language the client would recognise
- Explain what each of the four data sources is for, without reading from the brief
- Draw the whole system on one page, so a non-technical person can see how it fits together
- Create a database, schemas, and a table in Snowflake, and say why every column is text
- Read a CSV in Python with functions that each do one job

## You do not clean any data this week

That is deliberate. Cleaning starts in week 3. This week is understanding plus first contact with the tools.

---

## The week at a glance

| Step | LEARN | DO |
|---|---|---|
| **1** | `docs/01-project-brief.md` sections 1 to 4 · the MCA data dictionary | Tasks 1–3 - the discovery brief, the open questions, the one-page system map |
| **2** | The four source websites, hands on · `docs/09-resources.md` data sources table | Tasks 4–5 - source summaries and the onboarding paragraph |
| **3** | Kudvenkat parts 1 to 2 · Snowflake "Key Concepts and Architecture" | Tasks 6–8 - database, schemas, raw table |
| **4** | Python docs tutorial sections 4 to 5 · Real Python CSV article | Tasks 9–10 - row counter, header check |
| **5** | Pro Git chapters 2 to 3 · Chris Beams on commit messages | Task 11 - state filter, first clean commits |
| **6** | - | Cohort review: discovery brief walkthrough, terminology check |

---

## 1 · Discovery Brief **[MILESTONE]**

- [ ] **Task 1 - Write the discovery brief** (ID `B1.1`): Read the client story in `docs/01-project-brief.md`, section 1. Write a one page discovery brief. It must cover: what we are building, who uses it, what questions it must answer, what is still unclear, and what "done" looks like. Use plain language, as if the client will read it.
  **Commit:** `discovery/discovery_brief.md`, open a pull request for peer review.
  
    The client is a fintech due diligence company ,major clients of that company are from banks, lenders . the company wants to make a product that can help the client understand whether the company is trustworthy or not , so that they can lend money . For that needed to check things such as whether the company is active , any financial crisis. Earlier a data analyst need to visit required sites such as MCA ,IBI to  collect the required information and store in a spreadsheet which is time consuming and also it needed to be updated regularly. The client wants a single platform that contain all the company and the details such as whether the company is active, registered or is their any financial crisis, the history to  be kept, finally a clean dashboard that shows all the above information.

    

- [ ] **Task 2 - List the open questions** (ID `B1.2`): Write down the five questions you would ask the client if you had thirty minutes with them. Focus on things the brief cannot decide alone, like how far back the history must go, and what "risk" means to them.
  **Commit:** append to `discovery/discovery_brief.md` in an "Open Questions" section, update the same pull request.
   1. how long do you want to keep the past record?
   2. which time interval do you want the dashboard to be updated ?
   3. which is the key factor you are considering whether it is the comapny overall debt or any other factor?
   4. what are the features you would needed to be in dashboard , like company details based on specific region and filter out risk companies?
   5. Do you need the state-level data broken down by districts and local towns, or is a state-wide summary enough for your analysis?

- [ ] **Task 3 - Draw the whole system on one page** (ID `A1.1`): Draw the end-to-end picture: the four data sources on the left, the pipeline in the middle (files, Snowflake, the dashboard), and the people who use it on the right. Label each source with its format (CSV, PDF, HTML table, CSV). A non-technical person must be able to read it without you explaining it. Use Mermaid in a Markdown file - it renders on GitHub and is easy to review in a pull request - or any diagram tool you already know.
  **Commit:** `design/context_diagram.md` (embed the diagram), add it to the discovery brief pull request.

**Why this is a milestone:** building the wrong thing perfectly is the most expensive mistake in data work. Nobody moves far past week 1 until the cohort agrees on what is being built.

**On Task 3:** keep it to one page. If a box needs its own smaller boxes inside it, you have added too much detail. This diagram is the whole project's table of contents; you will redraw it with more detail in weeks 4, 7, 9, and 13.

---

## 2 · Source Summaries and the Client Ask

- [ ] **Task 4 - Summarise the four sources** (ID `B2.1`): For each of the four data sources, write three sentences in your own words: what it is, which client question it answers, and how it arrives (file type and cadence). No copying from `docs/01-project-brief.md`.
  **Commit:** `discovery/source_summary.md`, open a pull request.

1.MCA : ministry of coperate affairs is resposible  for registering companies in india . it assign a unique CIN number to each companies it also coantains attributes such as company current status,registartion date . This dataset is used because it answers the question whether the company is actually regirsted, active ,total assets . The data set arrives in ZIP file each ZIP conatins the compaies registered in the corresponding ROC.

2.IBBI : it is responsible for pulishing the insolvency records of the companies that are going through financial crirs.this dataset is used because it give the whether the company is going through a financial crisis,total debts, total debts recovered ,we can map the data with MCA using CIN column. It arrives as a pdf file , we need to use python script to extract the data.

3.MCA Corporate Data Management Portal : It contains the statistical rerprestion of company status accros different states , also the statistics of how many active compnay across different sectors in th india.This data can be used suppose if the current active company in a state is high as compared to other sectors which shows high acceptance by the people ,which will guarantee the the growth of that company so that lenders,banks can give money. It comes with statistical infromation in the site so we need to do web scrapping to eract the data using pyhton tools such as butterfly soup.

4.RBI: It sets the repo rates to the banks,suppose if the economy is going through a inflation the RBI will set the repo rates high which will forces the banks to lend monwy with high interest also if the economy is going to crisis the RBI will reduce the rates so that banks can reduce the interest rate. Knowing the repo rate at the exact time a company was registered gives lenders a quick snapshot of the economic condition — whether money was expensive or cheap when that business first opened its doors.It comes as a CSV file but it needs some cleaning

- [ ] **Task 5 - Write the onboarding paragraph** (ID `B2.2`): Write the one paragraph business ask you would hand to a new teammate joining the project, so they understand the project without reading anything else.
  **Commit:** `discovery/teammate_onboarding.md`, open a pull request.

Our client is a financial due diligence company whose primary customers are organizations such as banks and other financial institutions that lend money to businesses and companies.
The main objective of our product is to build a single centralized platform/dashboard where the client can view and analyze all the important information about a company before making a lending decision.
dataset:
1.MCA
The Ministry of Corporate Affairs (MCA) provides key information about registered companies through their unique Corporate Identification Number (CIN).
The platform should collect information such as:
Company Name
CIN
Registration Date
current status
2. Insolvency Information — IBBI
We also need to identify whether the company is currently involved in or has previously gone through insolvency proceedings.
This information can be obtained from the Insolvency and Bankruptcy Board of India (IBBI).
3. Sector/Industry Analysis
Another important component is understanding the performance and activity of the sector in which the company operates.
4. RBI Repo Rate
Knowing the repo rate at the exact time a company was registered gives lenders a quick snapshot of the economic condition — whether money was expensive or cheap when that business first opened its doors.

Relevant historical changes
An important requirement is that the dashboard should not simply overwrite existing data when new information arrives.
The system should:
Regularly update company information
Maintain historical records
Track changes in company status
This will allow the client to understand not only the current position of a company but also how its position has changed over time.
End Goal
The ultimate goal is to create a centralized financial due-diligence platform that brings together company information, insolvency records, sector-level information, and economic indicators into one place.



---

## 3 · Databases and Tables

Foundation link: Kudvenkat parts 1 to 2 (databases, tables). Note that Kudvenkat teaches SQL Server; the differences that matter are listed in `docs/09-resources.md`.

- [ ] **Task 6 - Create the database and schemas** (ID `S1.1`): In Snowflake, create the project database and three schemas named BRONZE, SILVER, and GOLD. Write down in one sentence what each schema will hold.
  **Commit:** `sql/s1/01_create_database.sql` plus `sql/s1/notes.md`, open a pull request.

- [ ] **Task 7 - Create the raw MCA table** (ID `S1.2`): Create the raw MCA table in BRONZE with one VARCHAR column per field in the RoC CSVs. Explain in one sentence why every column is VARCHAR at this stage.
  **Commit:** `sql/s1/02_create_raw_mca.sql`, update the pull request.

- [ ] **Task 8 - List the non-empty columns** (ID `S1.3`): Which columns in the raw MCA table must never be empty for a row to be a real company record? List them and explain why in one sentence each.
  **Commit:** append to `sql/s1/notes.md`, update the pull request.

---

## 4 · Python Basics: Files and Functions

- [ ] **Task 9 - Count rows in a CSV** (ID `P1.1`): Write a script that opens one RoC CSV file, counts the rows, and prints the count with the file name. Run it on three different RoC files.
  **Commit:** `python/p1/row_counter.py` plus output pasted in `python/p1/notes.md`, open a pull request.

- [ ] **Task 10 - Check the headers** (ID `P1.2`): Write a function that takes a file path and returns the column names from the CSV header line. Use it to verify two RoC files have identical headers. Report whether they match.
  **Commit:** `python/p1/header_check.py` plus result in notes, update the pull request.

- [ ] **Task 11 - Filter rows by state** (ID `P1.3`): Write a script that reads one raw CSV and writes a new CSV containing only rows where the state is a given value passed as text. Test it with "Kerala" on two files.
  **Commit:** `python/p1/state_filter.py`, update the pull request.

---

## End of week checklist

- [ ] Tasks 1–2 - discovery brief with open questions, in a pull request
- [ ] Task 3 - the one-page system map in `design/context_diagram.md`
- [ ] Tasks 4–5 - source summaries and the onboarding paragraph
- [ ] Tasks 6–8 - database, schemas, raw table, and the notes explaining both choices
- [ ] Tasks 9–11 - three scripts that run, with output pasted in notes
- [ ] At least one teammate's pull request reviewed with a real comment
- [ ] No data file, no credential, and no `.venv` anywhere in your commits

**If you are short on time, cut in this order:** Task 11 (P1.3), then Task 5 (B2.2). Never cut Tasks 1–2 (B1) or the notes files. The discovery brief is a milestone, and the written reasoning cannot be added back later.

Next: `week2/problem_statement.md`.
