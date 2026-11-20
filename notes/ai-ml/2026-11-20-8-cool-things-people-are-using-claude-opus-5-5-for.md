---
type: Article Summary
title: 8 Cool Things People Are Using Claude Opus 5.5 For
description: Opus 5.5 can turn single prompts into working animations, apps, 3D scenes, decks and games with little supervision.
resource: https://medium.com/generative-ai/8-cool-things-people-are-using-claude-opus-5-5-for-0dec7df4da4e
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/generative-ai/8-cool-things-people-are-using-claude-opus-5-5-for-0dec7df4da4e
  title: 8 Cool Things People Are Using Claude Opus 5.5 For
  author: Generative AI
status: stable
source_url: https://medium.com/generative-ai/8-cool-things-people-are-using-claude-opus-5-5-for-0dec7df4da4e
source: Medium (Generative AI)
source_type: medium
published: '2026-09-27'
domain: ai-ml
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81e2af56f5f5fafcf5d6
---

## 🎯 Core thesis
Opus 5.5 can turn a single prompt into working, polished creative and technical output (animations, interactive apps, 3D scenes, websites, Blender scenes, decks, UIs and games) with little steering from the user.

## 🧭 Why the author wrote it
The author says Opus 5.5 is "finally back" and that it completes requests without constant babysitting. He collects eight examples from his social feed to show what people are building in the first days after the release, so readers can try similar projects.

## 1. Animating Stories With JavaScript

### Frame-by-frame animation generated in code
Kevin Ngo asked Opus 5.5 for a 28-second animated story in which every frame is produced by JavaScript. The result is hand-drawn in style: a starry night town, paper airplanes, a smiling star-like creature, a girl writing at her window, and responses shown as dogs and tea. The author describes it as blending cut-paper aesthetics with code-generated motion. The idea is that the animation exists only as program output, not as image or video files.

### Animating with brush strokes and no image or video model
A second creator asked Claude to animate its own life from day zero to the present, using JavaScript brush strokes. The author points out that the model cannot generate images or video, yet can produce this kind of motion. His explanation is that no image input is needed, so the work depends on how creative the prompt is.

## 2. Building Interactive Apps That Explain a Concept

### Interactive explainers as a teaching format
Ryan Sael asked for an interactive lens lab that explains camera focus. Turning the focus ring moves the glass elements, and the plane of sharp focus shifts through the scene. The pattern is an app whose controls map directly onto the physical mechanism being taught, so the learner sees cause and effect while adjusting it.

### Cost of building such an app
The whole app took 1 hour and 26 minutes in a single run and cost $25.66 through the API. The author estimates that building the same level of app with Fable 5.1 or GPT 6 Astra would have cost hundreds of dollars. This is his estimate, not a measured comparison. He would like more apps like this, such as a music theory app where dragging notes changes the chord, or a sorting visualizer that can be paused and reshuffled.

## 3. Generating and Animating 3D Objects

### Three.js and TSL in a generated 3D scene
Techartist built "Architect's Model," an interactive web experience that turns a rough 2D sketch into a finished 3D house. It is built entirely in Three.js and TSL, and Opus 5.5 wrote the code.

> 🔎 Added context: Three.js is a JavaScript library for 3D graphics in the browser. TSL is a node-based shading language used with Three.js.

### Staged reveal as a design-history view
The viewer can orbit, scroll, and scrub through four stages: initial linework, basic massing, added detail, and the finished building. The author notes that scrubbing backward is the most useful part, because it shows which decisions produced the final shape.

### Presentation layer, not CAD
The author sees this as a way for architects and engineers to present concepts to clients, who often struggle with plans but can use a slider. His warning: "Just don't mistake it for CAD." The model has no real dimensions, no structural data, and nothing that could be built from. It is a presentation layer on top of the real design work.

> ❌ **Anti-pattern — Mistaking a presentation model for a design model:** a visually convincing 3D house with no dimensions or structure. **Why people do it:** the visual result looks finished and persuasive. **Consequence:** stakeholders may read the visual as a buildable design. **Fix:** keep the presentation model separate from the real design artifacts (CAD, structural data) and say so when presenting it.

### Model comparison on a jelly slice
Another creator made a stretchy jelly slice and compared the Opus result with an Astra version. The creator preferred Opus for its color, glossy surface, and how the slice folds and jiggles after release.

### Pinning dependencies when the library is changing
The author's tip: TSL is new and still changing, so pin your Three.js version and paste the docs into the context. Without this, the model will reach for APIs that no longer exist.

> ❌ **Anti-pattern — Generating code against an unpinned, moving API:** letting the model write against whatever version it remembers. **Why people do it:** it works on the first try and the output looks current. **Consequence:** code that calls APIs which have been removed or renamed. **Fix:** pin the library version and give the model the current docs in context, as the author suggests.

## 4. Designing Websites With 3D Elements

### Building high-end 3D web experiences from a tutorial
Viktor Oddy's 12-minute video tutorial shows how to build high-end 3D interactive websites step by step. The visuals and motion effects are the kind the creator says usually cost around $50k to produce. The author treats this as a sign of pressure on platforms like Lovable, which build AI-generated web designs. His point: a user can ask Claude to build the app directly inside their IDE.

> 🔎 Added context: An IDE is an integrated development environment, the editor where engineers write and run code.

## 5. Working Inside Blender

### Agentic 3D animation in Blender
Alex Albert from Anthropic highlighted Opus 5.5's ability to generate professional-quality stop-motion claymation videos in Blender from one prompt on [claude.ai](http://claude.ai). The model plans the scenes, renders the frames, checks the output, and assembles a roughly 20-second animation. The author takes this as evidence of how agentic Claude has become: it runs a multi-step creative pipeline on its own.

> 🔎 Added context: Blender is an open-source 3D creation suite. It is scriptable through a Python API.

### Expect version drift
The author expects some errors when people try this themselves, because Blender's Python API changes between versions. He plans to try it anyway.

## 6. Creating Brand Presentations

### Full brand decks from a prompt
The author has recommended AI presentation tools such as Gamma, Plus AI and NotebookLM. He suggests Claude with Opus 5.5 may now be at the top of that list. The deck Yahyavision shared has a cohesive futuristic look: a custom interlocking logo, the tagline "Every AI Agent, One Index," website mockups, app icons, and a teal and cyan palette with Electric Lime and Cerulean Blue. He plans a comparison against Gemini Notebook in coming days.

## 7. Building Frontend Shells and Dashboards

### Replacing purchased UI kits with generated ones
The author is not a frontend designer, and design is where he spends the most tokens, because he makes hundreds of tweaks to get it right. He was impressed by generated dashboards, UI libraries, components and motion icons. He wonders whether he still needs to buy premium libraries and templates.

### Designer productivity
Tanjim shared a UI that a senior product designer built in one hour, plus another hour of polishing. He says the same work could otherwise take about a week. Tanjim, who founds an AI product design studio, argues that designers who use tools like Opus 5.5 will become "unstoppable."

## 8. Making Playable Games

### AI-built 3D driving game
The author calls this probably the main reason to be excited about Opus 5.5: it can build high-quality, complex games. Chetan Ankola's upgraded Three.js driving simulation lets the player drive along surreal, hand-drawn looping roads, past floating islands and scenes such as Lemon Spiral and Möbius Arcade. The video shows dynamic camera angles and real-time studio controls for ink effects, lighting, and soundscapes synced to tracks like "Very Slow Waves." The author highlights how the UI, animation, effects and gameplay work together.

## Final Thoughts

### Real product use
The author has used Opus 5.5 to upgrade parts of his own products, including Zeniteq's automation system. He reports it has worked flawlessly for him so far and uses far fewer tokens than other frontier models he has tried for that work. He is most curious about its integration with After Effects and Blender. He is not an animator, but the claymation example made him want to try creating something for his websites or articles. He invites readers to try a small version of one example and share it.

## 🧠 Mental models
### Prompt as the only input
**The model:** The author's point in section 1 is that a model with no image or video generation can still produce convincing motion, because the output is code and the quality depends on how the task is described.
**How to apply it:** When a target is visual but the model cannot render it directly, ask for the artifact as code (JavaScript, Three.js, Blender Python) and judge the result by running it.

### Presentation layer versus source of truth
**The model:** A visually persuasive 3D or slide artifact is a communication tool. It does not carry the structured data that the real design needs.
**How to apply it:** Label generated visuals as presentations, and keep the source-of-truth artifacts (CAD, specifications, data) separate.

### Pin the moving parts
**The model:** When a library or API is new and still changing, generated code will drift toward whatever the model remembers.
**How to apply it:** Pin the library version and supply current docs in the prompt before asking for code.

## 🔑 Key takeaways
- Opus 5.5 produced complete, runnable creative projects from prompts alone, including animation built as frame-by-frame JavaScript output.
- One interactive explainer took 1 hour 26 minutes in a single run and cost $25.66 through the API, per the article.
- Generated 3D scenes are presentation layers: no real dimensions or structure, so they are not CAD.
- With TSL still changing, pin the Three.js version and paste current docs into context, or the model uses APIs that no longer exist.
- The model can plan, render, check and assemble a roughly 20-second Blender claymation animation from one prompt, per the author's reading of the Anthropic post.
- The author reports using Opus 5.5 on his own product automation and finding it uses fewer tokens than other frontier models for that work.

## 🏛️ For Kenji
Your interpretation, not the author's: for an architect, the useful part is the split between a convincing presentation artifact and the real design record, since the author himself warns that the 3D house has no dimensions or structural data. The question it raises is which parts of your own design workflow could safely use generated visuals or prototypes for client communication, and which must stay tied to governed design artifacts with pinned tool versions.

## 📚 Reading plan
### From the article
- [claude.ai](http://claude.ai) — the platform where the Blender claymation example was generated from one prompt, cited in section 5.

### To go further
- Anthropic's announcement and model documentation for Opus 5.5 — to confirm the capabilities and pricing the author describes.
- Three.js and TSL documentation — needed if you pin versions as the author advises for section 3.
- Blender Python API release notes — to check the version drift the author expects in section 5.
