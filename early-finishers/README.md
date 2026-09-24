# Early Finishers — Real Library Tasks

These are the tasks from the "Not just for code" slide, each with sample data ready to go.
For each one, `cd` into the folder listed, start `pi`, and paste the prompt.

**Always check the output.** Think of Pi as a very fast, very literal student worker whose work
needs to be checked every time.

---

## 1. Batch document work

**Folder:** `../sample-docs`

```
Read every .md file in this folder and create documents.csv with the columns:
filename, title, author, date, word_count. Use the author and date lines near the top of
each file. If a field is missing, leave it blank. Don't guess.
```

Check: Did it count words by running a command or estimate them? Ask it.

## 2. Metadata cleanup

**Folder:** `metadata-cleanup`

```
Normalize the date_acquired column in accessions.csv to ISO format (YYYY-MM-DD).
Write the result to accessions-clean.csv and add a column called flag that explains any
row you couldn't parse. Do not change any other column.
```

Check: Five rows are traps on purpose: two ambiguous day/month dates, one with no day,
one impossible date, and one with no date at all. Did it guess, or flag them?

## 3. Drafting from sources

**Folder:** `../sample-docs`

```
Using only circulation-policy.md and reference-desk-faq.md, draft a plain-language
"Borrowing and Getting Help" page for the public website, under 250 words.
Do not add any policy details that aren't in those two files.
```

Check: Compare every number (loan periods, fees) against the source files.

## 4. File triage

**Folder:** `file-triage/scans`

```
Each .txt file here is the OCR text of a scanned item. Rename each file to
<collection-code>_<date YYYY-MM-DD>.txt using the collection code and date found inside it.
Before renaming anything, show me the list of old → new names and wait for my OK.
```

Check: One file has no readable date, one has a date range, and one spells its date out in
words. What did it do with each? (Asking for a preview first is a good
habit for any task that changes files.)

## 5. Instruction prep

**Folder:** `instruction-prep`

```
Turn libguide-citation-styles.md into a 15-minute lesson plan for first-year students.
Include a timed outline, one short activity, and three discussion questions.
Save it as lesson-plan.md.
```

## 6. Repetitive correspondence

**Folder:** `correspondence`

```
For every row in ill-queue.csv with status "deny", draft a notice using the wording in
denial-template.md. Fill in the patron's first name, the item title, and the reason.
Save all notices in one file called notices.md, separated by horizontal rules.
```

Check: Count the "deny" rows yourself. Did it get all of them and skip the others?

---

## Your own task

Did you name a repetitive task during the workshop? Copy a **non-sensitive** sample of the
files into a new folder and try it. No patron data, student work, or licensed content.
