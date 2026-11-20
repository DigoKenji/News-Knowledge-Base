---
type: Article Summary
title: Anthropic Just Revealed 3 NEW Rules for Claude Skills
description: Claude 5.5 skills need critical instructions up front, freedom matched to task risk, and checklists with validation
  loops.
resource: https://medium.com/ux-planet/anthropic-just-revealed-3-new-rules-for-claude-skills-bf6d5ff66930
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/ux-planet/anthropic-just-revealed-3-new-rules-for-claude-skills-bf6d5ff66930
  title: Anthropic Just Revealed 3 NEW Rules for Claude Skills
  author: UX Planet
status: stable
source_url: https://medium.com/ux-planet/anthropic-just-revealed-3-new-rules-for-claude-skills-bf6d5ff66930
source: Medium (UX Planet)
source_type: medium
published: '2026-11-01'
domain: ai-ml
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d8144acc7c287c750aae3
---

## 🎯 Core thesis
Skills built for Claude 5.5 models (Haiku 5.5, Sonnet 5.5, Opus 5.5) should follow three updated rules: put critical instructions in the first 100 lines of SKILL.md, match instruction specificity to each task's risk and variability, and add checklists with validation feedback loops.

## 🧭 Why the author wrote it
Anthropic updated its skill best practices alongside the 5.5 models. The author argues that habits from earlier models can now cost tokens and produce generic output, so existing skills need a review against the new rules.

## Introduction
The article states the premise: rules that worked for earlier Claude models may now hurt performance, through unnecessary token consumption or generic outputs. It promises three rules and a ready-to-use prompt for auditing existing skills.

## 1. Put the most important information and instructions in the first 100 lines of SKILL.md

### SKILL.md length guideline
Anthropic recommends keeping a skill's SKILL.md under 500 lines of text. The author points out that those lines are not equally important to Claude. The rest of the rule depends on where Claude looks first.

### The first-100-line preview
When running a skill, Claude typically previews only the first 100 lines of SKILL.md, for example with `head -100 SKILL.md`, rather than reading the whole file. The author stresses that 100 lines is a recommended rule, not a technical cutoff. Critical context (what the skill is for and how to start) belongs in those first lines, and secondary detail can fill the remaining 400 lines.

```javascript
head -100 SKILL.md
```

**What this does:**
- Shows the command Claude typically runs to preview a skill file.
- Limits the read to 100 lines, so only the top of the file is seen at first.
- Explains why placement matters: what sits below line 100 may never be read in the preview pass.

> ❌ **Anti-pattern — Burying critical context (implied by the article):** Putting the core instructions after the first 100 lines, for example after a long preamble. **Why people do it:** They write the skill top-down as a document, with background first. **Consequence:** The preview may miss the instructions that drive the task. **Fix:** Lead with purpose, the trigger context and the core steps, then move detail below.

### Progressive disclosure
When more information is needed, the author recommends progressive disclosure. SKILL.md references other Markdown files, and Claude loads them on demand, only when it needs them. Keeping the main file short and the detail in linked files lets the skill carry more total knowledge without paying for it on every run.

> 🔎 Added context: SKILL.md is the main instruction file of a Claude Skill, and the rest of a skill's folder can hold supporting files. The article's pattern relies on Claude opening those files only when a step needs them.

Example from the article: a UX review skill (design-review) whose Step 1 points to a separate PRD.md file instead of pasting its contents.

```javascript
---
name: design-review
description: conducts design review
---

# Design review

## Review process step by step
Step 1: Check product requiremenet documentation listed in PRD.md
Step 2: Conduct review process
Step 3: Generate a report
```

**What this does:**
- The YAML frontmatter gives the skill a `name` and a one-line `description` that tell Claude when to use it.
- The top-level heading and numbered steps form the short, scannable core that the preview reads.
- Step 1 refers to PRD.md rather than embedding the product requirements, which keeps SKILL.md small.
- The step text is kept in the file exactly as the author wrote it, including the spelling "requiremenet".

### Table of contents in referenced files
For every Markdown file a skill references, the author recommends a table of contents in its first 100 lines. Claude can scan the table to learn what the file contains without reading all of it.

```javascript
...

# What this file contains
1. Product Overview — What we're building and why
2. Problem Statement — What problem we're solving
3. Goals & Success Metrics — What success looks like
4. Target Users — Who we're building for
5. User Stories & Use Cases — How users will interact with the product

...
```

**What this does:**
- The `...` lines mark where the rest of the file continues; they are placeholders, not syntax.
- The heading "What this file contains" signals the purpose of the table to Claude.
- Each numbered entry pairs a section name with a one-line purpose, so Claude can decide which section to open.
- Putting the table within the first 100 lines lets the same preview logic work for referenced files.

### Practical step: audit your skill library
The author suggests checking every frequently used skill for two things: whether SKILL.md exceeds 500 lines, and which Markdown files it links to. The article gives a prompt for this audit that forbids changes.

```javascript
Check my most-used skills.

For each skill tell me
1. If SKILL.md is over 500 lines
2. If SKILL.md links to any markdown files, list every file it links to

Provide the output as a list

Do not change anything.
```

**What this does:**
- "Check my most-used skills" limits the audit to the skills that matter most.
- Item 1 tests the 500-line guideline for each SKILL.md.
- Item 2 lists linked files, which shows whether progressive disclosure is in use.
- "Do not change anything" makes the run read-only, so the audit can be reviewed before any edits.

## 2. Set the appropriate degree of freedom for Claude AI models
The article defines three levels of prescription for a skill's instructions. Claude 5.5 models are more capable and creative, so a level that suited older models may now be too rigid.

### High freedom
High freedom gives Claude general instructions about the goal, without specifying how to reach it. It suits tasks where Claude's own judgment is the main asset.

### Medium freedom
Medium freedom sets a goal plus boundaries, such as a required output format. The article's example is "Always use this template when creating a user research report." Claude still chooses how to do the work within those limits.

### Low freedom
Low freedom gives a very specific sequence that Claude must follow. The article's example is an interview script. It offers the most control and the least flexibility.

### Mixing levels in one skill
The author notes that one skill can combine all three levels. In a user research skill, research goals might use Medium freedom, study planning Low freedom and synthesis High freedom. The rule is to match specificity to the task's importance and variability.

> ❌ **Anti-pattern — Over-prescriptive instructions carried over:** Keeping strict, step-by-step rules written for earlier models. **Why people do it:** Strict rules felt safer and gave more control when models were less capable. **Consequence:** Newer models can be held back by unnecessary instructions. **Fix:** Review each instruction, remove the ones that feel unnecessarily prescriptive, and re-check the freedom level of each step.

## 3. Provide checklists for Claude
For complex, multi-step skills, the author recommends laying out the steps clearly and adding checklists of what Claude must verify before moving on. The example is a user research synthesis workflow.

> 📋 **Checklist — User research synthesis workflow**
> - [ ] Step 1: Review interview notes and research data
> - [ ] Step 2: Identify recurring patterns and user pain points
> - [ ] Step 3: Group findings into key themes
> - [ ] Step 4: Translate insights into design opportunities
> - [ ] Step 5: Validate findings against research evidence

The example is introduced as "Use this checklist to track your progress" under a "Research Progress" heading. Each step is a verifiable gate that keeps Claude from skipping work.

```javascript
....

## User research synthesis workflow

Use this checklist to track your progress:

Research Progress:
- [ ] Step 1: Review interview notes and research data
- [ ] Step 2: Identify recurring patterns and user pain points
- [ ] Step 3: Group findings into key themes
- [ ] Step 4: Translate insights into design opportunities
- [ ] Step 5: Validate findings against research evidence
```

**What this does:**
- The `....` line marks omitted earlier content of the skill file.
- The heading names the workflow, so Claude knows which checklist applies.
- The instruction "Use this checklist to track your progress" tells Claude to tick items as it goes.
- The `Research Progress:` label and the five `- [ ]` items form a checkable, ordered list.
- The step text covers the full path from raw notes to validated findings, in that order.

### Feedback loops
For the validation phase, the author recommends a feedback loop: run validation, fix the issues the validator found, then run validation again. The validator can live inside the skill's Markdown files, so the loop is part of the skill itself.

> 🔎 Added context: A feedback loop repeats a check until it passes, rather than trusting one pass. The article describes it as a pattern, and it does not give a specific validator implementation.

## 🧠 Mental models

### The first-100-lines rule
**The model:** Claude reads the top of SKILL.md first, so the top is the most-used part of the file.
**How to apply it:** Put purpose, trigger context and core steps in the first 100 lines, and move detail to linked files.

### Degree of freedom dial
**The model:** Each instruction sits on a scale from general goal (high freedom) to exact script (low freedom).
**How to apply it:** Set the level by the task's importance and variability. Use low freedom for risky, fixed steps, and high freedom for judgment-heavy ones.

### Checklist plus validator loop
**The model:** Verifiable gates and a repeat-until-pass check catch errors that a single pass misses.
**How to apply it:** Add a checklist for each multi-step workflow, and pair the validation step with a run, fix, re-run cycle.

## 🔑 Key takeaways
- Anthropic recommends keeping SKILL.md under 500 lines, and the author says only the first 100 are reliably previewed.
- Claude previews the top 100 lines of SKILL.md, so critical instructions belong there, with secondary detail in the remaining 400 lines or in linked files.
- Progressive disclosure lets Claude load referenced Markdown files on demand, so detail costs nothing until it is needed.
- Each referenced file should open with a table of contents inside its first 100 lines so Claude can scan it.
- Freedom level (high, medium, low) should match each task's importance and variability, and one skill can mix all three.
- Older skills can be too prescriptive for Claude 5.5 models, so review and remove unnecessary instructions.
- Multi-step workflows need checklists and a validate, fix, re-validate feedback loop.

## 🏛️ For Kenji
A skill's SKILL.md works like the entry point of an interface that an AI agent reads. Its structure (what is up front, what is linked, how strictly each step is prescribed) is an information-architecture decision, and an audit of the skill library resembles an architecture governance review. This is your interpretation, not the author's. Question it raises: how would you measure whether a more prescriptive skill produces better outcomes than a looser one?

## 📚 Reading plan
### From the article
- No external resources are cited. The only links in the article point to SKILL.md, a filename rather than a resource.

### To go further
- Anthropic's documentation on Claude Skills and their best practices — the source the article builds its rules on; the article gives no URL, so search for the official page.
- Progressive disclosure as a design pattern in documentation and interface design — a general pattern that the article applies to skill files.
