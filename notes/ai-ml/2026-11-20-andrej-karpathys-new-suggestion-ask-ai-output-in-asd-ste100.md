---
type: Article Summary
title: Andrej Karpathy's New suggestion- Ask AI output in ASD-STE100 format and not in Text format
description: Karpathy's view is that reviewing AI output needs constrained wording and disposable visual tools, not verbose
  prose.
resource: https://medium.com/neuralnotions/andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100-format-and-not-in-text-format-b7b753f52f32
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/neuralnotions/andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100-format-and-not-in-text-format-b7b753f52f32
  title: Andrej Karpathy's New suggestion- Ask AI output in ASD-STE100 format and not in Text format
  author: Neural Notions
status: stable
source_url: https://medium.com/neuralnotions/andrej-karpathys-new-suggestion-ask-ai-output-in-asd-ste100-format-and-not-in-text-format-b7b753f52f32
source: Medium (Neural Notions)
source_type: medium
published: '2026-10-03'
domain: ai-ml
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81dfad99ede5264acb83
---

## 🎯 Core thesis
As AI does more autonomous work, people's main job becomes reviewing, supervising and understanding its output, so that output should be constrained text or a quick visual tool instead of fluent prose.

## 🧭 Why the author wrote it
The author finds raw AI text tiring to read: it rambles, uses many synonyms and buries key takeaways under polite filler. If the thesis holds, the choice of output format becomes part of reviewing AI work, not a cosmetic detail.

## Opening: Karpathy's premise and the two fixes
The article starts from Andrej Karpathy's view that, as models take on more autonomous work, human work shifts to reviewing, supervising and comprehending what they produce. Karpathy proposed two fixes: a constrained writing standard, and moving away from text toward diagrams, interactive HTML pages and throwaway explanatory animations. The author adopted both over the past week and reports what they learned.

### ASD STE100
The article describes ASD STE100 as an aviation maintenance standard from the 1970s, and later as simplified technical English created by European aerospace manufacturers in the late 1970s. It restricts vocabulary and forces clear, single-action instructions. The article dates the standard inconsistently between these two passages, so treat the exact decade as the author's claim.

> 🔎 Added context: ASD STE100 is a controlled form of English for aerospace technical documentation. Its rules limit vocabulary and sentence structure so that instructions are read the same way every time.

## The FLUFF Problem
The author argues that when people call an AI bad at technical work, they usually mean its output feels murky. Models are trained to sound fluent and expansive, and that creates ambiguity. The author calls the core problem "FLUFF" and links it to the aerospace field, where ambiguity in technical instructions is dangerous.

### Synonym switching and ambiguity
Ambiguity appears when a model names one object with several words. The article's example: a component called an engine in paragraph one, a motor in paragraph two, and a propulsion unit in paragraph three. A reader can no longer tell whether these are one part or three.

> ❌ **Anti-pattern — Synonym switching:** the same object gets a new name in each paragraph ("engine", "motor", "propulsion unit"). **Why people do it:** fluent writing rewards variety, and models are trained to sound expansive. **Consequence:** the reader cannot tell whether the terms refer to the same part, so review gets slower and less reliable. **Fix:** use one term per object for the whole text. In the article's words, "If an object is a bolt, call it a bolt forever."

### ASD STE100 writing rules
The article gives three core guidelines from the standard. Keep procedure sentences under twenty words. Describe only one action per instruction. Never switch terms for the sake of variety. The author adds a directive to technical prompts that asks the model to follow the spirit of ASD STE100:

> Explain this using the spirit of ASD STE100. Keep sentence structures direct, limit procedural steps to single actions and avoid unnecessary synonyms.

The author reports that model explanations began to read like precise workshop manuals instead of a chatty intern padding a word count. The article claims this one constraint saves a lot of mental effort when reviewing codebases, research papers or architecture proposals.

## Ephemeral Artifacts
The author sees the second part of Karpathy's observation as the transformative one. Writing custom software used to be expensive, so code was worth writing only if many people would use it for months or years. Now the marginal cost of code generation is close to zero, which makes large, custom, discardable artifacts practical.

### Ephemeral artifacts
An ephemeral artifact is a generated tool whose only job is to move an insight from the model into the reader's head quickly. The author wanted to understand how learning rates affect gradient descent. Instead of asking for an explanation, which would have produced four paragraphs about overshooting and slow convergence, the author asked for a single self-contained HTML file with a slider. The file was ready in about thirty seconds. Once the author understood the mechanism, they closed the tab and deleted the file.

> 🔎 Added context: Marginal cost is the extra cost of producing one more unit. Here it means the extra cost of generating one more piece of code, which the article says is essentially zero.

> Generate a single, self-contained HTML file with Tailwind CSS and a canvas script. Include an interactive slider for the learning rate, so I can watch a ball roll down an error surface in real time.

Moving the slider to 0.001 showed the ball crawling. Moving it to 1.5 showed the ball oscillating violently off the screen. The author says the whole explanation took ten seconds of visual interaction. The author did not need reusable software. They needed a tool that existed only to transfer one insight.

> ❌ **Anti-pattern — Prose explanation by default:** asking the model to describe a dynamic system in paragraphs. **Why people do it:** it is the usual way to ask for an explanation, and the article implies it was the author's own habit. **Consequence:** the reader must build the mental picture from words alone, when the behavior is easier to see when varied. **Fix:** for numbers or systems, ask for a throwaway interactive page that lets you change the parameters.

## New Visual Hierarchy
The author's position is that plain text is the lowest-bandwidth format for understanding something fast. The article then gives the ranking the author now uses when prompting for answers. The ranking is ordered from the most to the least effective for transferring understanding.

### Procedural animations and video
The article says that asking models for scripts for procedural animations, in the style of 3Blue1Brown, makes abstract math and algorithms click immediately. Pairing generated visual code with voiceover synthesis turns dense research into a personal lecture. This is the top rung of the hierarchy.

### Interactive HTML micro apps
When numbers or systems are involved, the author asks for an interactive page with sliders, toggles or live inputs. The article claims that playing with parameters teaches ten times more than reading static descriptions. This is the rung the author used for the learning-rate example.

### Structured diagrams
If an interactive page is more than the task needs, the author asks for Mermaid diagrams, SVG layouts or architectural flows. The article says spatial organization reduces cognitive load quickly.

### Constrained text
When text is necessary, the author constrains it: short sentences, direct verbs and strict terminology. This is the last rung before plain text, which the article treats as the least effective choice.

## 🧠 Mental models
### Constrained procedural writing
**The model:** Keep procedure sentences under twenty words, give each instruction one action, and use one term per object, as ASD STE100 does.
**How to apply it:** Add the article's directive to technical prompts, then check that each object in the reply keeps the same name.

### Disposable tools
**The model:** When generating code costs almost nothing, a tool built to make one insight clear can be used once and discarded. It does not need to be a reusable application.
**How to apply it:** When you need to understand a parameter, ask for a single-file interactive page, change the inputs, then delete it.

### Format bandwidth ranking
**The model:** Output formats differ in how fast they transfer understanding. The article ranks procedural animation first, then interactive pages, then diagrams, then constrained text, with plain text the weakest.
**How to apply it:** Choose the highest format that fits the content. Fall back to constrained text when a visual would be overkill.

## 🔑 Key takeaways
- The human role in AI-assisted work shifts toward reviewing and understanding model output, so the format of that output matters.
- Raw model prose is hard to review because it rambles, varies vocabulary and buries the key points.
- ASD STE100 rules (procedure sentences under twenty words, one action per instruction, one term per object) reduce ambiguity in AI output.
- Because generating code is nearly free, a throwaway interactive page can replace a written explanation for a single purpose.
- In the article's test, moving a learning-rate slider between 0.001 and 1.5 made the behavior clear in about ten seconds of interaction.
- The article ranks formats by how fast they transfer understanding: procedural animation, interactive pages, diagrams, constrained text, then plain text.

## 🏛️ For Kenji
This is your interpretation, not the author's. In architecture work, the constrained-writing rules map onto ADRs and proposals that use one term per component, and the visual rungs map onto Mermaid diagrams for architecture reviews. The question this raises: if a generated visual is discarded after one use, how would you check its numbers and assumptions before a decision depends on it?

## 📚 Reading plan
### From the article
- The article cites no links.

### To go further
- ASD STE100 (simplified technical English) — the aerospace standard whose rules the author applies to prompts.
- Mermaid diagrams and SVG layouts — the article's "structured diagrams" rung, useful for architecture flows.
- 3Blue1Brown-style procedural animation — the article's top rung for explaining abstract math and algorithms.
- Andrej Karpathy's original remarks on constrained output and ephemeral artifacts — the article summarizes them without linking to the source.
