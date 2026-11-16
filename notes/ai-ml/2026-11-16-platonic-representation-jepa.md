---
type: Post Summary
title: 'Two JEPAs, one world model: the Platonic Representation Hypothesis'
description: A post on LeCun and Balestriero's result that separately trained image and text JEPAs converge on the same representation, with 42x fewer samples than next-token prediction.
resource: https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB
tags:
- ai-ml
- deep-learning
- jepa
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB
  title: 'Two JEPAs, one world model: the Platonic Representation Hypothesis'
  author: alphaXiv
status: stable
source_url: https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB
source: LinkedIn (forwarded)
source_type: linkedin
published: '2026-11-01'
domain: ai-ml
subdomain: deep-learning
concepts:
- jepa
selected_by: forwarded
content: handoff
notion_url: https://app.notion.com/p/41abaf74172d81d6b296dc0cc88b2882
---

# LeCun and Balestriero on the Platonic Representation Hypothesis

**Source:** https://www.linkedin.com/posts/alphaxiv_yann-lecun-and-randall-balestriero-just-proved-activity-7522531199874027520-kLvB (LinkedIn post)
**Author:** alphaXiv (post; the post names Yann LeCun and Randall Balestriero as the researchers)
**Date:** 2026-11-01
**Tags:** #representation-learning #jepa #multimodal #format-linkedin-post

---

## What This Is About

The Platonic Representation Hypothesis says that models trained on different modalities, such as images and text, eventually learn similar representations of the world. The post's point is that how a model is trained matters more than simply scaling it up. It claims the researchers show mathematically that, under certain conditions, two [JEPA](/concepts/jepa.md)s trained completely separately learn essentially the same underlying representation. One learns from different images of the same scene, the other from different descriptions of that scene, and neither is trained on image-text pairs.

## Results Cited in the Post

- The same image-text alignment as next-token prediction, with 42x fewer training samples.
- A 109M-parameter text JEPA that outperforms LLMs up to 72B parameters on cross-modal alignment benchmarks.

These figures come from the post, not from the paper itself.

> The key idea: the training objective and data pairing matter more than scale. Two separately trained JEPAs can converge on the same representation without ever seeing image-text pairs.

## Patterns & Techniques Spotted

- **Joint-embedding prediction:** JEPAs predict representations of one view from another rather than predicting raw tokens or pixels.
- **Cross-modal alignment without paired data:** each model sees a different view of the same content (images of a scene, or descriptions of it), and alignment emerges without image-text pairs.

## Key Takeaways

- The post's central claim is that training method matters more than scale for learning shared representations.
- Alignment across modalities may come from matching different views of the same scene, not from paired image-text data.
- The 42x sample-efficiency gap over next-token prediction is the practical argument for JEPA-style training.

## Follow-up Reading

- 📄 Paper linked in the post — https://lnkd.in/gcenbTDQ → The full paper the post summarizes. Read it to check the conditions under which the result holds.
- 📚 Assran et al., 2023, *Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture* (I-JEPA, CVPR 2023), arXiv:2301.08243 → The image JEPA that this line of work builds on.
- 📚 Huh et al., 2024, *The Platonic Representation Hypothesis* (ICML 2024), arXiv:2405.07987 → The original hypothesis this post's result is tied to.

---
*Summary generated: 2026-11-16*
