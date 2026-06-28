# TakeMeter — planning.md

---

## Project Description

TakeMeter analyzes discourse quality in World Cup 2026 Reddit communities.
Comments were collected from r/worldcup across five thread 
types: discussion, pre-match, match, megathread, and post-match. Match and megathread threads were the primary source as they naturally contain the full spectrum of comment 
types within a single thread. Comments were classified into four labels, 
Observation, Reaction, Opinion, and Analytical/Predictive. Theses were chosen, for they capture the most meaningful and consistent distinctions in how fans engage with the sport online.

---

## Label Taxonomy

**1. Observation**
A comment that states a factual or descriptive claim without taking a 
position, providing reasoning, or referencing external knowledge. The 
speaker reports what they see or experience without editorializing.

*Example: "where some of the countries fans can't get a visa to support 
their teams"*

---

**2. Opinion**
A comment that takes a clear stance and provides reasoning or justification 
to support it. May use comparisons or references as supporting evidence, 
but the argument or conclusion is the primary substance.

*Example: "The US might be one of the best hosts. That's exactly why we 
should save it for special occasions, not turn the World Cup into a 
permanent residence."*

---

**3. Reaction**
A comment that expresses an emotional or visceral response to a match 
event, decision, or situation. Minimal reasoning is provided — the emotion 
itself is the primary substance.

*Example: "What atmosphere?"*

---

**4. Analytical/Predictive**
A comment that uses statistics, standings, tactical reasoning, or logical 
if/then thinking to analyze a current situation or predict an outcome. The 
analysis or prediction is the primary substance.

*Example: "Either would have 4 points with a win, which would most likely 
be good for at least second, but Iran and Belgium both have 2 points now 
so it's a very close group, hard to tell who will go through tbh"*

> **Note:** Numbers alone don't make a comment Analytical/Predictive. The 
> comment must use those numbers to reason toward a conclusion or 
> prediction. If the numbers are simply being reported, it is Observation.

---

## Decision Rules

- If a comment uses numbers or standings to reason toward a conclusion or 
  prediction → **Analytical/Predictive**
- If a comment uses a reference or comparison to argue a point → **Opinion**
- If a comment states something without argument or prediction → 
  **Observation**
- If the emotion is the point and reasoning is absent or minimal → 
  **Reaction**
- If a comment is structured as an argument with multiple supporting 
  reasons toward a conclusion → **Opinion**, even if it contains emotional 
  language or analytical observations
- Conditional language ("had", "would have", "if only") doesn't 
  automatically mean Analytical/Predictive — backward looking conditionals 
  are Opinion, forward looking conditionals are Analytical/Predictive
- Tone (aggressive, humorous, warm, casual) does not determine the label — 
  the underlying structure does
- When in doubt between Opinion and Analytical/Predictive, ask: *is the 
  comment reasoning toward a conclusion using data/tactics, or expressing 
  a stance based on values/preferences?*

---

## Data Collection Plan

Data was collected manually by reading through World Cup 2026 threads on r/worldcup. Threads were selected across four types: discussion, pre-match, match, megathread and post-match. Comments were copied into an Excel file with the following columns: id, text, label, source_subreddit, thread_type, thread_url; CSV file was created using the first three columns, if, text, label.

Comments were selected based on whether they clearly fit one of the four 
label categories. Crude, spam, or highly ambiguous comments were 
excluded. All comments are in English.

A targeted second collection pass was conducted specifically for 
Analytical/Predictive comments after an initial label distribution check 
revealed that label was underrepresented at 8.6% of the dataset. 
Pre-match and megathread threads and group standings discussion threads were the primary 
sources for this pass.
NB. megathread is a combination thread that contains commentary related to pre-match, match, and post-match comments.

---

## Label Distribution

| Label | Count | Percentage |
|-------|-------|------------|
| Observation | 65 | 25.0% |
| Reaction | 83 | 31.9% |
| Opinion | 71 | 27.3% |
| Analytical/Predictive | 41 | 15.8% |
| **Total** | **260** | **100%** |

### Train / Validation / Test Split (70/15/15, stratified)

| Label | Train | Validation | Test |
|-------|-------|------------|------|
| Observation | 45 | 10 | 10 |
| Reaction | 58 | 13 | 12 |
| Opinion | 50 | 10 | 11 |
| Analytical/Predictive | 29 | 6 | 6 |
| **Total** | **182** | **39** | **39** |

---

## Labeling Process

Comments were labeled manually by a single annotator in two passes:

1. **Collection pass** — comments were copied into a CSV with metadata 
   and no label assigned.
2. **Annotation pass** — each comment was assigned one of four labels 
   based on the definitions and decision rules above.

After an initial labeling pass, label definitions were refined based on 
patterns observed in the data. Most notably, the original "Contextual" 
label was redefined as "Analytical/Predictive" to more accurately describe
the forward-looking reasoning pattern it was intended to identify. All 220 
original comments were re-labeled from scratch after definitions were 
finalized to ensure consistency.

NB. Since Analytical/Predictive had the lowest about of comments, an additional 40 was collected, to meet the required 15% threshold; therefore, total comments is 260. 

---

## Edge Cases & Difficult Labeling Decisions

Since most World Cup commentary is inherently reactive, users are responding to live match events, referee decisions, or ongoing group standings, many comments carry signals from multiple labels simultaneously. Below are six difficult cases encountered during 
annotation, along with the decision made and the reasoning behind it.

---

**Edge Case 1: Reaction vs. Observation**
*"HOW WAS THAT NOT A PK??? Sorry for yelling"*

This comment could be labeled either Reaction or Observation. The user is 
responding to a specific play (Observation), but the all-caps phrasing and 
explicit acknowledgment of yelling signals that emotion is the primary 
substance. **Decision: Reaction.** The qualifying word "yelling" confirms 
the speaker is aware they are emoting rather than reporting. When emotional 
expression dominates the form of a comment, even if grounded in something 
observable, Reaction takes precedence.

---

**Edge Case 2: Opinion vs. Analytical/Predictive**
*"Either would have 4 points with a win, which would most likely be good 
for at least second, but Iran and Belgium both have 2 points now so it's 
a very close group, hard to tell who will go through tbh"*

This comment uses standings data, which initially suggested Contextual 
(later redefined as Analytical/Predictive). However, the reasoning is 
being used to reach a conclusion ("hard to tell who will go through") 
rather than simply presenting data. **Decision: Analytical/Predictive.** 
The decision rule applied: if a comment uses numbers or standings to reason 
toward a prediction or conclusion, it is Analytical/Predictive regardless 
of how uncertain or casual the tone is.

---

**Edge Case 3: Observation vs. Opinion**
*"Anywhere can host it. We don't need 48 teams and we don't need 80000 
seat stadiums charging a fortune. An affordable world cup for fans would 
be far preferable to this."*

This comment opens with what sounds like a neutral claim but quickly shifts 
into a normative argument about what the World Cup should look like. The 
presence of "we don't need" and "far preferable" signals value-based 
reasoning rather than neutral description. **Decision: Opinion.** When a 
comment moves from describing what is to arguing what should be, it crosses 
from Observation into Opinion.

---

**Edge Case 4: Analytical/Predictive vs. Observation**
*"Isn't it risky to aim for 3rd? Not every 3rd place is making it out of 
group. Second place is a death sentence for us so our only hope is to 
destroy Haiti while Brazil struggle against Scotland, and honestly I don't 
see us scoring many goals."*

This comment uses group standing logic and match predictions to arrive at 
a conclusion. The observation elements ("not every 3rd place makes it") 
serve as premises in the reasoning, not the primary substance. 
**Decision: Analytical/Predictive.** The comment is fundamentally reasoning 
toward a prediction about qualification using conditional match scenarios.

---

**Edge Case 5: Reaction vs. Analytical/Predictive**
*"As an Egyptian fan, I have to admit this is peak self-awareness 😂. If 
Egypt somehow makes it out of the group this time, this video is going to 
age horribly. If not... they called it perfectly before the tournament 
even started."*

This comment initially reads as Reaction due to the emoji, humor, and 
personal fan identification. However, the primary substance is explicit 
conditional reasoning, the speaker is evaluating two possible outcomes 
and their implications against a prior prediction. **Decision: 
Analytical/Predictive.** Casual tone, humor, and emojis do not determine 
the label, its underlying structure does. If the primary substance is 
if/then reasoning about outcomes, it is Analytical/Predictive regardless 
of how conversational or emotional the delivery feels.

---

**Edge Case 6: Reaction vs. Analytical/Predictive**
*"Really genius? So with 12 teams all vying for 8 knockout round slots, 
you don't think goal differential will matter? 12 teams will all have 2 
or 4 points. They always do. Then it comes down to goals. A national 
team's entire existence can be validated by advancing out of the group 
stage, and you don't think goal differential is going to decide any of 
it. I don't think you're too good at how math works. Try adding up the 
number of games versus the 12 third place teams. I'll wait…"*

This comment initially reads as Reaction due to its aggressive, sarcastic, 
and confrontational tone ("Really genius?", "I'll wait…"). However, 
removing the reactionary tone, the primary substance is a structured 
mathematical argument about goal differential and qualification scenarios. 
**Decision: Analytical/Predictive.** Aggressive or contemptuous tone does 
not determine the label any more than humor or warmth does. If the primary 
substance is explicit mathematical or logical reasoning, it is 
Analytical/Predictive regardless of how hostile the delivery feels.

> **Pattern these six cases establish:** Tone in any emotional direction — 
> warm, humorous, aggressive, sarcastic — does not override the underlying 
> structure of a comment. Always identify what the comment is *doing*, not 
> how it *sounds*.

---

## Model & Training

**Base model:** `distilbert-base-uncased`

DistilBERT was chosen for its efficiency on a T4 GPU and strong 
performance on text classification tasks with limited training data.

**Initial hyperparameters** (produced near-random accuracy of 35.9%):
- Learning rate: 2e-5
- Epochs: 3
- Batch size: 16

**Final hyperparameters** (peak validation accuracy 58.97% at epoch 4):
- Learning rate: 5e-5
- Epochs: 8
- Batch size: 8

The model peaked at epoch 4 and began overfitting thereafter. `load_best_model_at_end=True` automatically saved the epoch 4 checkpoint as the final model.

---

## Evaluation Metrics

Performance was measured on the test dataset (39 examples) for both the fine-tuned model and a zero-shot baseline using `llama-3.3-70b-versatile` via the Groq API.

| Model | Accuracy |
|-------|----------|
| Zero-shot baseline (Groq llama-3.3-70b-versatile) | 61.5% |
| Fine-tuned DistilBERT | 56.4% |
| Difference | -5.1% |

Fine-tuning significantly improved Analytical/Predictive classification 
(F1: 0.62 → 0.83) while regressing on Reaction (F1: 0.69 → 0.53). The 
baseline remained competitive due to Llama's broad pretraining on general 
language, which handles emotional vocabulary reliably without task-specific 
training.

---

## AI Tool Usage

**Label stress-testing:** Claude was used to refine label definitions 
throughout the planning phase. Comments and their proposed labels were 
discussed iteratively — Claude would explain when a label assignment 
didn't align with the stated definition and suggest tighter decision rules. 
This process led to the most significant taxonomy change: the original 
"Contextual" label was identified as too ambiguous and was redefined as 
"Analytical/Predictive", which better captured the forward-looking 
reasoning pattern the label was intended to identify.

**Annotation:** All data was hand-labeled without AI assistance. Claude was not used to assign labels to any comments in the dataset.

---

## Project Success Criteria

The project will be considered successful if the model is able to identify 
each label reliably and handle the nuance associated with World Cup 
discourse, which includes comments that use slang, abbreviations, sarcasm, 
and mixed emotional/analytical content. A secondary success criterion is 
honest, well-reasoned error analysis that identifies where and why the 
model falls short.

---

## A Complete Interaction (Step by Step)

1. Open the Colab notebook and set the runtime to T4 GPU
2. Run Section 1 — define the label map
3. Run Section 2 — upload `test_data.csv` when prompted; the notebook 
   automatically splits the data into train, validation, and test sets 
   and tokenizes all splits
4. Run Section 5 — runs the zero-shot Groq baseline on the same test set
5. Run Section 3 — fine-tunes DistilBERT on the training set; training 
   takes 5–15 minutes on a T4 GPU
6. Run Section 4 — evaluates the fine-tuned model on the test set and 
   generates `confusion_matrix.png`
7. Run Section 6 — prints the side-by-side baseline vs fine-tuned 
   comparison and writes `evaluation_results.json`

**Example output from Section 4:**
Input: *"If they win tonight they're through regardless of the last 
group game"*  
Predicted label: **Analytical/Predictive** (confidence: 0.87)