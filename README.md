# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter classifies beauty-product posts from the Sephora Beauty Insider
Community. The dataset focuses on three distinctions: specific outcomes from
using a product (`results_based`), product characteristics such as texture or
scent (`product_details`), and general approval or disapproval without
supporting specifics (`general_opinion`). The goal is to test whether these
categories can be applied consistently and learned by a text classifier.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `results_based`

**Definition:** Reviews describing specific outcomes from using a beauty
product, including benefits or adverse effects.

**Example 1:** “Within just a few days of using this lip mask my dry lips from
the cold dry winter feel so much better.”

**Example 2:** “Both times it dried out my lips badly that it took a week to
get better.”

### `product_details`

**Definition:** Reviews focused on product characteristics such as texture,
scent, ingredients, packaging, or application.

**Example 1:** “not sticky, looks like gloss but feels like a lip balm or
butter. I hate sticky lip products and i love this. looks very pretty too.”

**Example 2:** “this product is a bit pricey but after using one scoop i
figured this will take a LONG time to use up the whole container. it smells so
good and the container is so cute.”

### `general_opinion`

**Definition:** Reviews expressing approval or disapproval without specific
supporting details about outcomes or product characteristics.

**Example 1:** “love her! as a sleeping mask, i don’t think you could ask for
anything better”

**Example 2:** “I heard amazing things about this so I got it and it was
amazing!”

### The hardest boundary

**Which two labels:** `results_based` and `product_details`

**The decision rule I used every time:**
If the main point is a specific effect the reviewer experienced after using
the product, label it `results_based`, even if the review also mentions scent
or texture. If the main point is how the product smells, feels, looks, is
packaged, or is applied, without a specific outcome being the focus, label it
`product_details`. Use `general_opinion` only when neither kind of supporting
detail is given.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Sephora Beauty Insider Community, as reported
by the student. Verify the collection source before submission.

**How I labelled them:** The student supplied labels for rows 1–20. Rows
21–200 were pre-labelled with AI and then reviewed and confirmed by the
student. The first 20 are not marked as cold because unaided labeling was not
confirmed.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `results_based` | 136 | 68% |
| `product_details` | 44 | 22% |
| `general_opinion` | 20 | 10% |
| **Total** |  | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**2.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

**3.**
> *The post:*
>
> *Could have been:*
>
> *I chose, because:*

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate `2e-5`, batch size 16, seed 42,
maximum sequence length 128.

**Anything I changed from the defaults, and why:** No hyperparameters changed.
Training ran on the Apple Silicon GPU (MPS) with PyTorch 2.14.1.

**Split sizes:** 139 train / 31 validation / 30 test. Test counts: 20
`results_based`, 7 `product_details`, and 3 `general_opinion`. The two smaller
test classes have fewer than 8 examples, so their scores are highly unstable.

**Seed 42 results:** Accuracy 0.667; macro F1 0.267. F1 by label:
`results_based` 0.800, `product_details` 0.000, `general_opinion` 0.000. The
model predicted every held-out post as `results_based`; its accuracy is close
to that class's 68% share in the full dataset and does not indicate useful
performance across all three labels. These are one-seed results, not the
three-seed evaluation required in unit 6. See [`results.json`](results.json)
and [`test_split.csv`](test_split.csv) for the saved run and held-out posts.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Help labeling the 200 collected beauty reviews using
  `results_based`, `product_details`, and `general_opinion`.
- *What came back:* AI-generated labels for all 200 posts.
- *What I changed:* I reviewed the labels and confirmed they looked good. The
  first 20 labels were supplied separately by me; their cold-label status is
  unverified.

**Moment 2**

- *What I asked for:* Help preparing the project taxonomy and acceptance
  criteria so I could move on to other coursework.
- *What came back:* AI-drafted taxonomy wording and measurable acceptance
  criteria with rationales based on the dataset's label distribution.
- *What I changed:* The taxonomy and criteria are recorded as AI-assisted.
  The criteria remain an AI-drafted proposal, not independently
  student-authored targets.

**Pre-labelling disclosure:**
Rows 21–200 in `labels.csv` were pre-labelled by AI and then reviewed and
confirmed by the student. Rows 1–20 have labels supplied by the student;
their cold-label status is unverified.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.667 | 0.667 | +0.000 |
| Macro F1 | 0.267 | 0.267 | +0.000 |
| F1 — `general_opinion` | 0.000 | 0.000 | +0.000 |
| F1 — `product_details` | 0.000 | 0.000 | +0.000 |
| F1 — `results_based` | 0.800 | 0.800 | +0.000 |

**What I predicted before I looked:**
Before seeing the baseline results, I expect the zero-shot model to do better
than the trained model on `product_details` and `general_opinion`, because the
trained model predicted every test post as `results_based`; I expect the
trained model may still have higher overall accuracy.

**What the gap actually means:**
The baseline and trained model produced the same scores on the same 30 held-out
posts: both predicted every post as `results_based`. Fine-tuning added no
measurable improvement on this split, and the prediction that the baseline
would do better on the two smaller labels was not borne out. Only three
`general_opinion` and seven `product_details` examples were in this test split,
so their zero F1 scores are based on very small supports. See
[`baseline_results.json`](baseline_results.json) for the baseline predictions
and scores.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
