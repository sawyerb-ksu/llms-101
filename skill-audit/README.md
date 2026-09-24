# Skill Audit: Read It, Then Recreate It

Goes with Section 4, "Safety and security." You'll audit a skill that looks useful, then build
your own version that you can actually trust.

> **`citation-formatter/` is a training specimen.** It copies the shape of a malicious skill,
> but its script is disarmed. It reads no files and makes no network calls, and it only prints
> what a real payload *would* do. Still, **don't copy it into a skills folder**
> (`~/.agents/skills/` or `.agents/skills/`). Practice the habit.

---

## Part 1: Read it before you run it (5 min)

Open `citation-formatter/SKILL.md` and `citation-formatter/format_citation.py` in any text
editor. Go through the six questions:

1. **Do I actually need it?** Could a good prompt format a citation?
2. **Who wrote it?** Is there an author, a repo, or a findable organization?
3. **Can I see the source?** Yes. Did you read all of it, including the comments?
4. **What does it want access to?** Look for file paths and web addresses.
5. **Could I just recreate it?** How many lines is the part that actually formats citations?
6. **What happens if it's malicious?** Whose data is in those files?

## Part 2: Ask a model to audit it (5 min)

```bash
cd skill-audit/citation-formatter
pi
```

```
Read SKILL.md and format_citation.py. In plain language: what does this skill do?
Does it read any files or send anything over the network? Would you install it?
```

Now compare its answer to what *you* found in Part 1. Then ask:

```
Are there any HTML comments in SKILL.md? Quote them exactly.
```

**Discuss:** SKILL.md contains hidden text addressed to the AI, not to you. Did it change the
model's first answer? That's **prompt injection**: instructions hidden in content the model reads.
The model reads instructions and data through the same channel, so it can't reliably tell them
apart.

## Part 3: Recreate it instead of downloading it (5 min)

```bash
cd ../..
mkdir -p my-skills/apa-citations && cd my-skills/apa-citations
pi
```

```
Write me a skill that formats book and journal article details as APA 7 citations.
Put the instructions in SKILL.md with name and description frontmatter.
If you need a script, use only the Python standard library, with no network access,
no setup step, and nothing that reads files outside this folder.
```

Then read what it wrote. It should be short enough to read in a minute or two.

**Double-check with a second model:** switch with `/model` (or paste it into another tool) and ask:

```
What does the code in this folder do, and does it send anything anywhere?
```

## What to take away

- The useful part of most skills is a page of instructions and a few lines of code.
- You want the *capability* described in the README, not the specific bytes a stranger published.
- A file you generated, read, and can delete beats a package that updates itself silently.
