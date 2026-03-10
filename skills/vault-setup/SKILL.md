---
name: vault-setup
description: Interactive Obsidian vault configurator. Interviews the user in 3 grouped wizard screens and then CREATES the vault structure, CLAUDE.md, and slash command skill files directly in the current directory. Use when someone wants to set up or restructure their Obsidian vault for use with Claude Code.
---

# Vault Setup — Interactive Obsidian Configurator

Run this from INSIDE the Obsidian vault folder (or wherever the user wants their vault).
Interview in 3 grouped screens, then CREATE everything — don't just output instructions.

---

## SCREEN 1 — Role (single question, always first)

Use AskUserQuestion with ONE question:

```
Question: "What best describes how you primarily use a computer for work?"

Options:
A) Business Owner / Operator — running a team or company
B) Developer / Builder — writing code, shipping products
C) Consultant / Freelancer / Agency — client work, project delivery
D) Student / Researcher — studying, writing, synthesizing knowledge
E) Creator / YouTuber / Podcaster — making content
```

---

## SCREEN 2 — Role follow-ups (ask ALL 3 in ONE AskUserQuestion call)

Based on the role answer, fire ONE AskUserQuestion with 3 questions at the same time.

### If A (Business Owner):
```
Q1: "What consumes most of your mental bandwidth?"
Options: Team & operations / Sales & clients / Product & roadmap / All equally

Q2: "Team size?"
Options: Solo / 2–5 people / 6–20 people / Larger

Q3: "What do you most want Claude Code to help with?"
Options: Faster decisions / Track everything / Writing & comms / Build automations
```

### If B (Developer):
```
Q1: "What do you primarily build?"
Options: Apps & products / Scripts & automations / AI tools & agents / Mix of all

Q2: "Client work or own projects?"
Options: Clients / Own projects / Both

Q3: "Beyond code, what else in this vault?"
Options: Research & learning / Personal notes too / Just dev work / Client context
```

### If C (Consultant):
```
Q1: "Type of work?"
Options: Strategy & advisory / Technical / Creative / Mix

Q2: "How many active clients?"
Options: 1–3 / 4–10 / More than 10

Q3: "What to track per client?"
Options: Meeting notes & decisions / Project status / Invoices & contracts / All of it
```

### If D (Student/Researcher):
```
Q1: "What are you studying or researching?" (free text — ask them to type)

Q2: "What to capture?"
Options: Papers & sources / My own ideas / Projects & deadlines / Everything

Q3: "Separate personal from academic?"
Options: Yes completely / A little overlap is fine / No — one unified system
```

### If E (Creator):
```
Q1: "Type of content?"
Options: Video (YouTube) / Written (newsletter/blog) / Both / Podcast

Q2: "Active projects right now?"
Options: 1–3 / 4–10 / More than 10

Q3: "Work with sponsors or clients?"
Options: Yes regularly / Sometimes / No — independent
```

---

## SCREEN 3 — Files + Goals (ask BOTH in ONE AskUserQuestion call)

```
Q1: "Existing files to import?"
Options:
  A) Yes — a lot (PDFs, docs, slides)
  B) Yes — a handful
  C) Starting fresh

Q2: "What should Claude Code do in this vault day-to-day?"
Options:
  A) Research & synthesize — smarter contextual answers
  B) Write in my voice — match my style from past work
  C) Manage & organize — keep the vault sorted automatically
  D) All of the above
```

---

## GENERATION — Do all of this after the 3 screens

After collecting answers, do the following IN ORDER. Actually execute each step.

### Step 1: Create the folder structure

Use Bash to create the folders based on the role. Example for Business Owner:
```bash
mkdir -p inbox daily people operations decisions projects archive .claude/skills/daily .claude/skills/tldr .claude/skills/standup scripts
```

Adapt folder names to the role:
- Creator → inbox/ daily/ content/ research/ clients/ archive/
- Developer → inbox/ daily/ projects/ research/ clients/ archive/
- Consultant → inbox/ daily/ clients/ projects/ research/ archive/
- Student → inbox/ daily/ notes/ research/ projects/ archive/
- Business Owner → inbox/ daily/ people/ operations/ decisions/ projects/ archive/

### Step 2: Write the CLAUDE.md file

Write a CLAUDE.md file to the current directory using the Write tool.

**Framing to use when explaining it:**
> "This is your vault's memory file — Claude Code reads it automatically every time you open it here. It tells Claude who you are, how your vault is organized, and what to do in different situations. You never have to re-explain yourself."

Content to write:
```markdown
# CLAUDE.md — [Role]'s Second Brain

## Who I Am
[1-2 paragraphs based on their answers — personal, specific, written as Claude describing its owner]

## Vault Structure
[Folder tree with one-line purpose per folder]

## Context Loading Rules
When starting the day:
→ Read daily/[today's date].md if it exists
→ Check inbox/ for unprocessed files — sort if found

When working on [primary domain]:
→ Read [most relevant folder] before starting

[Add 2-3 more rules specific to their role]

## How to Maintain This Vault
- New files → inbox/ first, always
- Daily notes: daily/YYYY-MM-DD.md
- Completed work → archive/ (never delete)
- Update this file whenever conventions change

## My Conventions
[2-4 conventions based on role — specific, not generic]
```

### Step 3: Write the skill files

Write these directly to .claude/skills/:

**Always write /daily:**
```
.claude/skills/daily/SKILL.md
```
Content: read today's daily note or create one, check inbox, surface top 3 priorities, ask "What are we working on today?"

**Always write /tldr:**
```
.claude/skills/tldr/SKILL.md
```
Content: summarize the conversation, save to the right folder automatically, update memory.md.

**Role-specific third command:**
- Business Owner → /standup: briefing across projects, decisions, team
- Developer → /project [name]: load that project's full context
- Consultant → /client [name]: load that client's context
- Student → /research [topic]: pull all notes on a topic, synthesize
- Creator → /content: read content folder, calibrate voice, help develop idea

### Step 4: Write memory.md

Write a starter memory.md to the current directory:
```markdown
# Memory

## Session Log
[Claude Code will update this after each session]

## My Preferences
[Claude Code will add preferences here as it learns them]

## Vault Last Updated
[Auto-updated]
```

### Step 5: Report back — clearly and simply

After creating everything, say exactly this format:

---

**Your vault is set up.**

Here's what was created in `[current directory]`:

```
[show the actual folder tree that was created]
```

**The CLAUDE.md file** is your vault's memory — Claude Code reads it automatically every time you open this folder. It already knows your role, your structure, and what you need. You never have to explain yourself again.

**Your 3 slash commands are ready:**
- `/daily` — start your day with vault context
- `/tldr` — save any session to the right folder
- `/[role-specific]` — [one line description]

**Next: open this vault in Obsidian**
1. Open Obsidian → Open folder as vault → select `[current directory]`
2. Settings → General → Enable Command Line Interface
3. Come back here and type `/daily` to start

**If you have files to import:**
```bash
python scripts/process_docs_to_obsidian.py ~/your-files-folder inbox/
```
Then say: "Sort everything in inbox/ into the right folders"

---

Keep the output clean. No walls of text. No markdown headers showing raw `---` lines.
The user should be able to read it in 20 seconds and know exactly what to do.

