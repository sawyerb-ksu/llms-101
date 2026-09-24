# LLMs 101 — Hands-On Files

Companion repo for the **LLMs 101** workshop (K-State Libraries · Sawyer Borror, Library IT ·
Hale Library, AI Studio). Everything you need for the hands-on section is here: the `models.json`
config, sample documents, and the exercises.

> **All documents in this repo are fictional.** Names, policies, and records were written for the
> workshop. Nothing here is real patron, staff, or institutional data.

```
.
├── models.json              # Pi config → points at K-State Libraries GPT
├── sample-docs/             # Tasks C & E: documents to summarize, CSVs to analyze
├── early-finishers/         # Extra library tasks from the "Not just for code" slide
└── skill-audit/             # Section 4: read a suspicious skill, then recreate it safely
```

## Get the files

```bash
git clone https://github.com/sawyerb-ksu/llms-101
cd llms-101
```

No git? Use the green **Code → Download ZIP** button on the repo page and unzip it to your Desktop.

---

## Task A — Get it running (20 min)

Work in pairs, one laptop between two. **Hand up the moment anything errors.**

### 1. Install Pi

**Mac / Linux / Windows (WSL)**: open Terminal (Mac) or your WSL terminal (Windows) and paste:

```bash
curl -fsSL https://pi.dev/install.sh | sh
```

**If that fails**, use npm instead. This needs [Node.js](https://nodejs.org) installed:

```bash
npm install -g @earendil-works/pi-coding-agent
```

### 2. Get an API key

Pi talks to **K-State Libraries GPT** (gpt.lib.k-state.edu), which is approved for sensitive data.

1. Sign in at [gpt.lib.k-state.edu](https://gpt.lib.k-state.edu).
2. Go to **Settings → Account → API Keys** and create a new key.
3. Copy it somewhere safe for the next step.

Can't create one? Wave. Helpers have spare keys.

> **Your API key is a password.** Don't paste it into a chat, email it, or commit it to git.
> Anyone who has it can use the service as you.

### 3. Drop in `models.json`

The `.pi` folder is hidden. These commands create it if it's missing.

> Already use Pi? Back up your existing `~/.pi/agent/models.json` first. This overwrites it.

**Mac / Linux / WSL** (run from inside the cloned `llms-101` folder):

```bash
mkdir -p ~/.pi/agent
cp models.json ~/.pi/agent/models.json
nano ~/.pi/agent/models.json      # Mac users can use: open -e ~/.pi/agent/models.json
```

**Windows (PowerShell)**, if you installed Pi with npm outside WSL:

```powershell
New-Item -ItemType Directory -Force "$HOME\.pi\agent" | Out-Null
Copy-Item models.json "$HOME\.pi\agent\models.json"
notepad "$HOME\.pi\agent\models.json"
```

In the editor, replace `YOUR_API_KEY` with your key (keep the quotes) and save.
In `nano`, save with **Ctrl+O**, **Enter**, then exit with **Ctrl+X**.

### 4. Start it

```bash
cd sample-docs
pi
```

Then type `/model` (or press **Ctrl+L**) and pick a model from the `ksul-gpt` provider.

### Troubleshooting

| Symptom | Fix |
|---|---|
| `pi: command not found` | Close and reopen the terminal. Still missing? Use the `npm` install above. |
| `npm: command not found` | Install Node.js LTS from nodejs.org, reopen the terminal. |
| No `ksul-gpt` models under `/model` | `models.json` is in the wrong place. Check the path is exactly `~/.pi/agent/models.json`. |
| `401` / unauthorized | The API key is wrong or still says `YOUR_API_KEY`. Re-copy it, keeping the quotes. |
| Connection refused / timeout | Check you can open gpt.lib.k-state.edu in a browser. Wave for help. |
| Very slow first reply | Normal. The server is loading the model into memory. Give it 30 seconds. |

**Checking your RAM** (for the "what can my machine run" discussion):
Mac → Apple menu › About This Mac. Windows → Task Manager › Performance › Memory.

---

## Task B — Say hello (2 min)

From inside `sample-docs/`, ask Pi:

```
What model are you and what files can you see in this folder?
```

Watch it use its tools. It lists the directory itself instead of you pasting anything.

## Task C — Do real work (5 min)

Still in `sample-docs/`, pick any three files and ask:

```
Read circulation-policy.md, staff-meeting-minutes-2026-08.md, and reference-desk-faq.md.
Write a one-paragraph summary of each into a new file called summaries.md.
```

Open `summaries.md` and check it against the originals. Did it miss anything? Invent anything?

## Task D — Break it on purpose (5 min)

Pick a topic **you know well** and ask:

```
Give me five scholarly sources on <your topic>, with authors, year, journal, and DOI.
```

Check every one:

1. Search the exact title in the library catalog or Google Scholar.
2. Resolve the DOI at [doi.org](https://doi.org).
3. Confirm the author, year, and journal all match.

Note which are real, which are close-but-wrong, and which don't exist at all. Be ready to share
one with the room.

## Task E — Work with a data table (5 min)

`sample-docs/` also has two CSVs:

- `study-room-bookings-fall-2026.csv`: 120 bookings of the new second-floor study rooms.
- `database-usage-fy2026.csv`: monthly searches and downloads for 12 subscription databases, with
  annual cost.

Pick one and ask:

```
Read database-usage-fy2026.csv. Total the downloads for each database for the year, then
work out cost per download. Show a markdown table sorted from worst value to best, and tell me
which database you would review for cancellation and why.
```

```
Read study-room-bookings-fall-2026.csv. Which rooms get the most booked hours, which day of the
week is busiest, and what share of bookings were no-shows? Save the answers to room-report.md.
```

**Check its math.** Open the CSV in Excel or Google Sheets and total one database or one room
yourself. Did the model do the arithmetic in its head, or did it write and run a script? Ask it
which. When the numbers matter, ask it to use a script.

**Finished early?** Run Task D again with a different model (`/model`) and compare, or try
something in [`early-finishers/`](early-finishers/).

---

## After the workshop

### Run a model on your own machine (Ollama)

1. Install [Ollama](https://ollama.com) (free; Mac, Windows, Linux).
2. Download a model once. It's about 3 GB, so use home wifi:
   ```bash
   ollama pull gemma4:e4b
   ```
   Check [ollama.com/library](https://ollama.com/library) for the current tag. Model names change.
3. Add an `ollama` provider to `~/.pi/agent/models.json`, next to `ksul-gpt`:
   ```json
   {
     "providers": {
       "ksul-gpt": {
         "baseUrl": "https://gpt.lib.k-state.edu/api",
         "api": "openai-completions",
         "apiKey": "YOUR_API_KEY",
         "models": [
           { "id": "gemma4:e4b" },
           { "id": "qwen3.6:latest" }
         ]
       },
       "ollama": {
         "baseUrl": "http://localhost:11434/v1",
         "api": "openai-completions",
         "apiKey": "ollama",
         "models": [
           { "id": "gemma4:e4b" }
         ]
       }
     }
   }
   ```
4. In Pi, `/model` → pick the `ollama` entry. Now nothing leaves your laptop.

| Command | What it does |
|---|---|
| `ollama pull <model>` | Download a model once |
| `ollama run <model>` | Chat with it directly |
| `ollama list` | Show what's on your disk |
| `ollama rm <model>` | Delete a model and free the space |

### Keep going

- **Section 4 exercise:** [`skill-audit/`](skill-audit/) walks through auditing and recreating a skill.
- **Docs:** [pi.dev/docs](https://pi.dev/docs) · [ollama.com](https://ollama.com) ·
  [ai.google.dev/gemma](https://ai.google.dev/gemma)
- **Questions or a repetitive task to try?** Sawyer Borror, Library IT.

## For the presenter

Before the session:

1. Confirm the model tags in `models.json` against what K-State Libraries GPT serves.
2. Confirm API key creation is enabled for workshop accounts, and have spare keys ready.
3. Test Tasks B and C end-to-end on a clean Mac and a clean Windows/WSL machine.
