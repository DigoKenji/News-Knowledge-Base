---
type: Article Summary
title: 'AI on Excel Sheet: NVIDIA''s Kumo Runs 23x Faster Than Google''s TabFM'
description: NVIDIA's Kumo predicts from tables with no training run, and an independent benchmark confirms its speed, but
  its top rivals carry business-use restrictions.
resource: https://medium.com/towards-deep-learning/ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-google-s-tabfm-21c0d20518ee
tags:
- data
- data-drift
- concept-drift
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/towards-deep-learning/ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-google-s-tabfm-21c0d20518ee
  title: 'AI on Excel Sheet: NVIDIA''s Kumo Runs 23x Faster Than Google''s TabFM'
  author: Towards Deep Learning
status: stable
source_url: https://medium.com/towards-deep-learning/ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-google-s-tabfm-21c0d20518ee
source: Medium (Towards Deep Learning)
source_type: medium
published: '2026-11-08'
domain: data
concepts:
- data-drift
- concept-drift
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d813d8dfbdcb9a894ec05
---

## 🎯 Core thesis
NVIDIA's Kumo Tabular can predict from a spreadsheet with no training run, is far smaller and faster than the rival models in a 19-dataset benchmark, and is the most accurate model there that a business can use freely, though the LinkedIn post that promoted it got one key number wrong and left out the licensing limits of the two models that beat it.

## 🧭 Why the author wrote it
The author saw a wave of "RIP XGBoost" posts on LinkedIn claiming a new small model replaces the usual train-per-question workflow. He traced the post's numbers back to their sources to check whether they hold, and asks whether the model is truly better, what else exists, and whether a company can use it without a lawyer blocking the work.

## Opening: the claim and the three questions
The article opens with a familiar pain. For about twenty years, gradient-boosted trees (XGBoost is the best-known) have been the default for predicting outcomes from tables, such as customer churn, loan defaults or house prices. Their catch is that each new question needs a fresh model, trained from scratch, which can consume days of data cleaning, feature building and tuning.

The LinkedIn post made three claims about Kumo, NVIDIA's table model. It has 28 to 215 million parameters, against 1.65 billion for Google's TabFM. On a public test of 19 real datasets it ran about 23 times faster than TabFM and finished in the top three on 16 of them. The GPU time for the whole test cost \$1.19 for Kumo and \$27.40 for TabFM.

The author's three questions frame the rest of the note: is it really better, what else is available, and can it be used at work.

> 🔎 Added context: "Top three" means the model's average rank across datasets placed it among the three best models tested, and a model's rank on each dataset is its position on that dataset's accuracy score.

## So what is Kumo, in plain words?
Kumo Tabular works like teaching a new colleague by example. You show them fifty old customer records, marked as cancelled or stayed, then hand over a new record and ask for a judgment. Kumo receives a table in which some rows already have the answer and others are blank, and it fills in the blanks in one pass. There is no training run, no tuning and no feature engineering step.

The AI term for this is in-context learning. It is the same trick that lets a chat model pick up a task from examples written in the prompt. Here the rows with answers act as those examples. The contrast is between hiring and training a new specialist for each question (the traditional approach) and using one generalist that has already studied millions of tables and only needs to look at yours.

Kumo comes in three sizes. Small has about 28 million parameters, Medium about 62 million and Large about 215 million. The author points out that this is tiny by current standards and small enough to run on a single graphics card, which matters for cost. Even Kumo Large is about half the size of TabFM.

NVIDIA released Kumo Tabular on 29 September. The name comes from Kumo AI, a startup co-founded by Stanford professor Jure Leskovec. Its prediction models were used by companies such as Reddit and Sainsbury's, and NVIDIA bought the company in June for at least \$400 million, according to The Information. The author list includes Jingang Qu, lead author of TabICL, which explains some of the design.

The model never saw a real spreadsheet during training. NVIDIA wrote a generator that invents small cause-and-effect worlds. One example is a hidden "weather" variable that drives both "ice cream sales" and "sunburns". Each invented world becomes a table, and the table is made deliberately messy, with missing values, duplicate rows that disagree and heavily skewed numbers. The Large model learned from about 137 million of these synthetic tables, much as a pilot trains on simulated storms before flying a real one.

## How does something this small keep up?
Kumo processes a table in three steps. Each step has one job, and each is designed so that work grows slowly as the table gets bigger. The article says this is where most of the speed comes from.

**Step 1: turn every cell into numbers.** Kumo groups a row's cells three at a time into a single token, the same kind of unit that language models use for chunks of text. Each value is described with a set of wave patterns, called Fourier features (sines and cosines), so the model can tell small differences apart. A missing cell is replaced by the column's average plus a flag that says the value was missing. The user never has to fill the gaps. Rows with known answers carry the answer along.

**Step 2: summarize each row by reading the table in two directions.** Reading down a column teaches the model what is normal for that column. A value of 42 might be an ordinary age or an alarming number of complaints, depending on the column. Kumo does this by comparing each row against a fixed set of 128 or 256 reference points rather than against every other row. Doubling the rows therefore roughly doubles the work instead of quadrupling it. Reading across a row teaches the model how the columns relate. By the end of this step, every row, whether it had five columns or ninety, is compressed into four tokens.

**Step 3: make the prediction under one strict rule.** Rows with answers may attend to each other. Rows to be predicted may attend only to the answered rows, never to each other. The article compares this to exam students who may read the textbook but not each other's papers. The result is that the answered rows are processed only once. After that, new questions can be answered against the same examples very cheaply. This is the "fit once, predict many" mode in NVIDIA's library.

> 🔎 Added context: Attention is the mechanism that lets a model weigh which other items matter when it processes one item.

**Length-aware attention.** Attention spreads thinner as the number of rows grows, so a model trained on 1,024-row tables could lose focus on a 50,000-row table. Kumo compensates by scaling attention with the logarithm of the row count, using a dial learned separately for each attention head. The article notes this is the same basic idea TabICLv2 used under the name QASSMax.

**Training stages.** Training ran in three stages. It started with 1,024-row tables of up to 100 columns, moved to tables of 400 to 10,240 rows, and finished with tables of up to 60,000 rows. The 60,000-row figure marks the edge of what Kumo has ever seen.

**Ensembling in the library.** NVIDIA's library adds a technique on top of the model. If you ask for eight estimators, it runs the model on eight slightly different copies of your table, each with shuffled columns and rescaled numbers, and averages the results. This is the AI version of asking eight colleagues and going with the consensus. The library keeps at most 500 columns per copy, which matters for very wide tables.

## Checking the post: is it really 23 times faster?
NVIDIA's own tests place Kumo first on four public leaderboards. On TabArena, NVIDIA says it beats every rival while running 17 times faster than LimiX-2. The author treats launch material with caution, since every launch chart shows the new model winning. The more useful evidence is an independent benchmark by AIMultiple, the source of the post's numbers. It ran 13 models on the same 19 real datasets with the same type of GPU and repeated each model five times on different slices of the data.

The article reads the results on a chart where higher is more accurate and further left is faster. The top-left corner is the best place to be. Filled dots mark models a business may use freely, and empty circles mark models whose free licenses rule out business use.

**The headline holds up.** Kumo needed 7.6 seconds per test round against 173.6 seconds for TabFM. That is 166 seconds less, or almost exactly 23 times faster. Kumo finished in the top three on 16 of the 19 datasets. At AIMultiple's price of \$5.98 per GPU hour, the whole benchmark cost \$1.19 for Kumo and \$27.40 for TabFM. TabFM's average is partly driven by the largest tables, since its typical round took 16.9 seconds. On its first run it also spends more than 20 minutes on setup before it predicts anything.

**One number is badly wrong.** The post says TabFM has 1.65 billion parameters. Google's own paper lists 408.7 million. The author suggests the mix-up may come from 1.65 being close to the size in gigabytes of a 409-million-parameter file stored at full precision. Against the correct figure, Kumo Large is about half of TabFM and Kumo Small is roughly 15 times smaller.

**The two models that beat Kumo were left out.** LimiX-2, from Stable AI, is the most accurate model in the test. It has an average rank of 2.16 against Kumo's 2.63 and 11 outright wins. It took 111.9 seconds per round, which is 104 seconds more than Kumo, or about 15 times slower. It also used around 124 GB of GPU memory on the largest datasets, more than an 80 GB H100 can hold. Prior Labs' TabPFN-3.5 goes the other way. It finishes a round in 4.3 seconds, 3.3 seconds faster than Kumo, with an average rank of 3.26, about 0.6 places behind.

> ❌ **Anti-pattern — trusting the launch chart:** Accepting a benchmark because the vendor's own chart shows the new model on top. **Why people do it:** Launch posts are persuasive and easy to share. **Consequence:** Speed or accuracy claims go unchecked, and the comparison may leave out the strongest rivals. **Fix:** Check an independent benchmark that includes the strongest alternatives, and recheck the parameter counts and costs against the original papers.

## The fine print the post skipped
Almost every model in the chart can be downloaded, but most cannot be used freely in a business. Model makers often publish code under one license and the trained weights, which are what actually make the predictions, under a stricter one. For this family of models, the stricter license usually restricts use to research and testing.

TabPFN-3.5 is the strictest case. Its license forbids using even the model's outputs for internal business decisions, so a churn analysis for your own company counts as use and requires Prior Labs' enterprise license. TabFM has the most workable route among the restricted models, since Google runs it inside its BigQuery data warehouse. That suits teams whose data already lives there.

That leaves three models any company can use freely today: Kumo, TabICLv2 and Amazon's Mitra-v2. Kumo ranked about four places ahead of TabICLv2 and Mitra-v2 in the AIMultiple test. The article notes that Mitra-v2 ran there without the short fine-tuning step its makers use for their own published results, so it may be underrated. Among the models a business can freely use, the author's conclusion is that Kumo is the clear first pick.

> ❌ **Anti-pattern — equating open code with a free license:** Assuming that a model is free to use commercially because its code or weights are downloadable. **Why people do it:** Downloadable weights look the same as open-source software. **Consequence:** A project may be built on a model whose license forbids business use, and its outputs may be restricted too. **Fix:** Read the license of the weights, not just the code, and check whether it covers outputs used for internal decisions.

## What Kumo's license actually lets you do
Kumo's weights are released under OpenMDW-1.1, a Linux Foundation license whose name stands for Open Model, Data and Weights. NVIDIA uses it for 19 of its models, including the Nemotron family. The author says it reads much like the MIT license, one of the most permissive licenses in open-source software. In plain terms, it lets you use, modify, fine-tune, share and sell what you build with Kumo, provided you include a copy of the license and the original copyright notices whenever you pass the model on to someone else.

Two further points matter for businesses. The predictions you get from the model belong to you, with no conditions attached to the outputs. The only way to lose your license is to sue someone claiming the model infringes your patents or copyrights. The author notes that most companies will never come near that clause.

The article lists three caveats.
- NVIDIA's ready-made server package, the NIM container, comes under NVIDIA's own software license rather than OpenMDW, so read that license separately if you use it.
- The recipe and the data generator used to train Kumo have been promised but not published. You can use and fine-tune the model, but nobody outside NVIDIA can rebuild it from scratch.
- The Open Source Initiative is still reviewing OpenMDW-1.1. If your company approves only officially listed open-source licenses, expect a conversation with legal before you ship anything.

## Try it yourself in a dozen lines of Python
The setup needs Python 3.11 or newer and PyTorch 2.7 or newer. Installing NVIDIA's structured-data-models library takes one command, and the Kumo weights download from Hugging Face on first use. A GPU makes it fast, but the official example falls back to a normal CPU, which is fine for small tables.

```bash
pip install structured-data-models
```

> 🔎 Added context: The source tags this shell command as javascript. The code itself is unchanged.

**What this does:**
- Installs NVIDIA's library, which provides the sdm package used in the examples.
- Downloads the Kumo weights from Hugging Face automatically the first time a model is used.
- Requires no separate setup for training, because there is no training step.

The first full example uses scikit-learn's breast cancer dataset. The first 300 rows, whose answers are known, serve as examples, and Kumo predicts the remaining 269.

```py
import torch
from sklearn.datasets import load_breast_cancer

import sdm
df = load_breast_cancer(as_frame=True).frame  # 569 rows, 30 measurements, "target" is the answer
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
# Turn the pandas table into a format the library understands
table = sdm.TableTensor.from_pandas(
    df=df,
    stypes=sdm.infer_stypes(df, overrides={"target": "categorical"}),
    device=device,
)
# Pick a size: "small", "medium" or "large" (the default)
model = sdm.models.KumoTabular(task="classification", size="small", device=device)
with torch.amp.autocast(device.type, dtype=torch.float16, enabled=device.type == "cuda"):
    probs = model(
        x_context=table[:300].drop_columns("target"),  # the examples
        y_context=table[:300, "target"],               # their answers
        x_query=table[300:].drop_columns("target"),    # the rows to predict
        num_estimators=8,                              # 8 shuffled versions, averaged
    )
probs_df = probs.to_pandas()           # one column per possible answer
print(probs_df.head())                 # the probabilities
print(probs_df.idxmax(axis=1).head())  # the most likely answer for each row
```

**What this does:**
- Converts a pandas DataFrame into the library's tensor format and marks "target" as a categorical answer.
- Selects the small model size, the fastest of the three.
- Passes the first 300 rows as context (examples and their answers) and the other rows as the query, which is the whole "training" step.
- Uses float16 autocast on GPU to speed up the computation. The code falls back to full precision on CPU.
- Averages eight shuffled versions of the table, which makes the probabilities steadier.

The second example shows the fit-once, predict-many pattern for when you ask many questions against the same examples.

```py
model.fit(x=table[:300].drop_columns("target"), y=table[:300, "target"], num_estimators=8)
batch_a = model.predict(table[300:450].drop_columns("target"))
batch_b = model.predict(table[450:].drop_columns("target"))
model.clear()  # free the memory when you're done
```

**What this does:**
- Calls fit once to process the examples, so the expensive work of reading them happens only once.
- Runs predict on separate batches of rows, which reuse the processed examples.
- Calls clear at the end to release GPU or CPU memory held for the examples.

The third example handles a numeric target. It uses scikit-learn's diabetes dataset, and instead of one guess per row, the model returns 999 quantile columns that describe the likely range.

```py
from sklearn.datasets import load_diabetes

df_reg = load_diabetes(as_frame=True).frame  # 442 rows, 10 measurements, numeric "target"
reg_table = sdm.TableTensor.from_pandas(df=df_reg, stypes=sdm.infer_stypes(df_reg), device=device)
reg = sdm.models.KumoTabular(task="regression", size="medium", device=device)
quantiles = reg(
    x_context=reg_table[:350].drop_columns("target"),
    y_context=reg_table[:350, "target"],
    x_query=reg_table[350:].drop_columns("target"),
    num_estimators=8,
).to_pandas()
# q500 is the middle guess, and q050 to q950 brackets a 90% range
print(quantiles[["q050", "q500", "q950"]].head())
```

**What this does:**
- Switches the task to regression and uses the medium model size.
- Gives the model 350 example rows and asks about the remaining 92.
- Returns quantile columns from q001 to q999, so the output is a range of likely values rather than one number.
- Reads the 5th, 50th and 95th percentile columns to get a middle guess and a 90% range.

The article adds that the library's source handles some questions automatically. A question with more than 10 possible answers is split into smaller sub-questions behind the scenes. To compare with rivals, you change one line, such as `sdm.models.TabICLv2(device=device)` for TabICLv2 or `sdm.models.TabFM(task="classification", device=device)` for TabFM. The TabFM option first asks you to accept its non-commercial license. Free text and dates must be converted to numbers before Kumo can use them, and the library includes helpers for this. For data spread across several linked tables, `sdm.models.KumoRelational` and NVIDIA's churn prediction notebook are the recommended starting points. Those who prefer not to run code can try a hosted version on [build.nvidia.com](http://build.nvidia.com), or use the NIM container on their own servers.

## When I'd still pick XGBoost
Kumo has clear limits. It understands only numbers and categories, so text and dates need converting first. It has never seen a table with more than 60,000 rows or 100 columns, and NVIDIA warns that accuracy may drop well beyond those sizes.

The biggest risk involves time. Kumo assumes the rows you want predicted look like the rows you gave it as examples. Predicting next quarter's churn from last year's customers after prices or the market have shifted is exactly the situation where results can quietly degrade. The article says NVIDIA itself lists this as a limitation.

> ❌ **Anti-pattern — reusing examples across a shift in time:** Feeding a model examples from an earlier period and treating its answers as valid for a later period whose conditions have changed. **Why people do it:** The zero-training workflow makes it easy to reuse the same examples for every new question. **Consequence:** Predictions can get worse without any visible error, because the model still returns confident-looking probabilities. **Fix:** Check that the examples match the situation you are predicting, and monitor results against actual outcomes as conditions change.

The author's verdict has three parts. For a table under 60,000 rows and a question to answer, Kumo Tabular is the first thing to try before any tuning. It takes about a dozen lines, reports how sure it is, sits in the top group of an independent test while costing a fraction of the heavyweight models, and is the most accurate model on that list that a business can use without signing anything. If licensing is not a concern, LimiX-2 deserves a test too. If you are scoring millions of rows a day or chasing the last bit of accuracy on a very large table, gradient-boosted trees remain the right tool.

## References
- NVIDIA, Kumo Tabular announcement on the Hugging Face blog, 29 September 2026
- NVIDIA, Kumo-Tabular model card and NIM model card
- NVIDIA, NVIDIA Kumo developer page
- NVIDIA, structured-data-models 0.2.1 package and source
- AIMultiple, Tabular Models Benchmark: Performance Across 19 Datasets, updated 12 October 2026
- Let's Data Science, Nvidia acquires Kumo AI predictive model maker, June 2026
- The Linux Foundation, OpenMDW-1.1 submission to the OSI license review list, August 2026
- Google Research, Introducing TabFM, the TabFM paper and the TabFM model card

## 🔑 Key takeaways
- Kumo Tabular fills in unknown table values in a single pass by reading rows that already have answers, so it needs no per-question training, tuning or feature engineering.
- In AIMultiple's 19-dataset test, Kumo needed 7.6 seconds per round against 173.6 seconds for TabFM, which is about 23 times faster, and finished in the top three on 16 of 19 datasets.
- The launch post overstated TabFM's size. Google's paper lists 408.7 million parameters, not 1.65 billion, so Kumo Large is about half of TabFM.
- The most accurate model in the test (LimiX-2) and the fastest (TabPFN-3.5) both carry licenses that restrict business use, so Kumo is the most accurate option a business can use freely.
- Kumo's three-step design (cells to tokens, row summaries using 128 or 256 reference points, and an answer step where unknown rows never attend to each other) keeps the work growing close to linearly with table size.
- Kumo's weights are under OpenMDW-1.1, which allows commercial use with a license and copyright notice, but the training recipe is unpublished and the license is still under OSI review.
- The main risk is time drift: predictions can degrade when the rows to predict no longer resemble the examples, and Kumo does not warn about this.

## 🏛️ For Kenji
For an architect, Kumo changes the cost of an ML feature. A per-question training pipeline could become a fit-once, predict-many call behind a service boundary, which affects how you size and govern it. The question it raises for your governance work is who owns the drift monitoring when the examples stop matching the live data. This is my interpretation, not the author's.

## 📚 Reading plan
### From the article
- [build.nvidia.com](http://build.nvidia.com) — the hosted NVIDIA version to try Kumo without installing anything.

### To go further
- AIMultiple tabular models benchmark (19 datasets, updated 12 October 2026) — the independent test behind the speed and accuracy numbers.
- NVIDIA's Kumo Tabular release post on the Hugging Face blog (29 September 2026) — the design details and NVIDIA's own claims.
- Google Research's TabFM paper and model card — to check the parameter count and license terms.
- TabICL and TabICLv2 papers — the model family Kumo's design builds on, including the attention scaling idea.
- OpenMDW-1.1 license text — read before using the weights in a commercial product.
- Gradient-boosted trees (XGBoost) documentation — the baseline to compare against when the table is large or the speed budget is tight.
