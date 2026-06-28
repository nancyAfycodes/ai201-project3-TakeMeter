## Evaluation Report

### Overall Accuracy

| Model | Accuracy |
|-------|----------|
| Zero-shot baseline (Groq llama-3.3-70b-versatile) | 61.5% |
| Fine-tuned DistilBERT | 56.4% |
| Difference | -5.1% |

The zero-shot baseline outperformed the fine-tuned model by 5.1 percent(5.1 %). Since a small dataset of 260 was used, the result obtained isn't unusual. I think fine-tuned model adjusted for overall accuracy through label-specific improvement. For example, Analytical/Predictive comments performed significantly better on accuracy with the fine-tune model whilst Reaction label accuracy decreased.

---

### Per-Class Metrics

| Label | Model | Precision | Recall | F1 |
|-------|-------|-----------|--------|----|
| Observation | Baseline | 0.60 | 0.30 | 0.40 |
| Observation | Fine-tuned | 0.42 | 0.50 | 0.45 |
| Reaction | Baseline | 0.64 | 0.75 | 0.69 |
| Reaction | Fine-tuned | 0.71 | 0.42 | 0.53 |
| Opinion | Baseline | 0.70 | 0.64 | 0.67 |
| Opinion | Fine-tuned | 0.50 | 0.64 | 0.56 |
| Analytical/Predictive | Baseline | 0.50 | 0.83 | 0.62 |
| Analytical/Predictive | Fine-tuned | 0.83 | 0.83 | 0.83 |

The results show that F1-score for Analytical/Predictive improved F1 from 0.62 to 0.83, which suggests that the model successfully learned the distinctive linguistic patterns of that label (if/then structures, standings math, conditional reasoning). In contrast, 
Reaction F1-score dropped from 0.69 to 0.53, which indicates the model struggled to 
distinguish emotional expression from opinion-based reasoning when both 
share similar features.

---

### Confusion Matrix (Fine-tuned Model)

| True \ Predicted | Observation | Reaction | Opinion | Analytical/Predictive |
|-----------------|:-----------:|:--------:|:-------:|:---------------------:|
| Observation | 5 | 2 | 3 | 0 |
| Reaction | 3 | 5 | 3 | 1 |
| Opinion | 4 | 0 | 7 | 0 |
| Analytical/Predictive | 0 | 0 | 1 | 5 |

The confusion matrix reveals two systematic patterns:
1. Observation label acted as a catch-all for ambiguous predictions, meaning  that every misclassified statement was labeled an observation.
2. Analytical/Predictive is the cleanest label with only 1 misclassification 
   out of 6 examples.

![Confusion Matrix](confusion_matrix.png)

---

### Wrong Prediction Analysis

The fine-tuned model made 17 incorrect predictions out of 39 test examples. I have included three examples cases to illustrate systematic error patterns rather than isolated mistakes.

---

#### Wrong Prediction #1
**Text:** "Great game. I know it'll blow the minds of some people that a 0-0 
match was entertaining but i don't care. I love this sport."

**True label:** Reaction  
**Predicted label:** Opinion (confidence: 0.75)  

**Analysis:** The model used the counterargument structure, "I know 
it'll blow the minds of some people", which is a strong Opinion signal. However, the comment never develops that structure into a genuine argument. The speaker acknowledges a counterpoint only to dismiss it ("I don't care") and return to pure emotional expression ("I love this sport"). The model learned that counterargument framing signals Opinion, but 
missed the functional distinction: Opinion engages with counterarguments to strengthen a case, while Reaction dismisses them and returns to feeling. This was one of the statement I had difficulty labeling, and the model struggled too with this statement.

---

#### Wrong Prediction #2
**Text:** "Really genius? So with 12 teams all vying for 8 knockout round 
slots, you don't think goal differential will matter? 12 teams will all have 
2 or 4 points. They always do. Then it comes down to goals. A national 
team's entire existence can be validated by advancing out of the group stage, 
and you don't think goal differential is going to decide any of it. I don't 
think you're too good at how math works. Try adding up the number of games 
versus the 12 third place teams. I'll wait…"

**True label:** Analytical/Predictive  
**Predicted label:** Opinion (confidence: 0.52)  

**Analysis:** In this case, the model didn't comprehend the mathematical analysis of goal differential and qualification scenarios in the statement, with its sarcastic tone as Analytical, resulting in a low confidence score. This illustrates a core limitation: the model learned surface signals (sarcasm → Opinion) rather than the deeper functional distinction the taxonomy intended (what is the comment doing, not how does it sound). This same statement was documented as an edge case during annotation mainly because tone and substance point in opposite directions.

---

#### Wrong Prediction #3
**Text:** "The Uruguay fall off is crazy although not surprising after the 
era of Suarez Cavani Godin Forlan. Props to Cape Verde for the effort they 
put on. What Uruguay team will we get vs Spain"

**True label:** Reaction  
**Predicted label:** Analytical/Predictive (confidence: 0.46)  

**Analysis:** I think the closing question ("What Uruguay team will we get vs Spain") 
triggered an Analytical/Predictive prediction despite the comment being primarily emotional. The model interpreted a rhetorical question about an upcoming match as forward looking analytical reasoning, as can be seen from the very low confidence (0.46) score. This illustrates a systematic weakness: rhetorical questions in an emotional contexts look identical to predictive questions in an analytical contexts. Of note is that comments labeled as analytical/predictive was lower than all other labels.

---

## Error Pattern Analysis

To identify systematic patterns beyond individual wrong predictions, all 
15 misclassified examples were reviewed collectively. Four distinct error 
patterns emerged, each reflecting a specific boundary the model failed to 
learn reliably.

---

### Pattern 1 — Reaction misclassified as Observation (3 cases)
**Examples:** #3, #4, #5

Comments affected:
- "Agreed. For better or worse, England's my team. I'm Really excited 
  that Scotland's in it though and am rooting for them to go far 💪"
- "NO! Don't get excited! No expectations until the QF..."
- "definitely NO. unfairness about matches and political things"

**Pattern:** All three are short, low-information comments where the 
emotional signal is understated or indirect. The model appears to treat 
brevity and low information density as signals for Observation, the 
default label when no strong signal is present, regardless of emotional 
content.

**Why this boundary is hard:** Observation is defined by the absence of 
strong signals (no argument, no prediction, no emotion). Short Reaction 
comments also lack strong signals by definition, emotion is present 
but minimal. The model learned Observation as a residual category, which 
causes it to misclassify low-energy Reactions.

**What would fix it:** More training examples of short, understated 
Reaction comments would help the model learn that brevity alone does not 
signal Observation. A tighter Observation definition that explicitly 
excludes any evaluative language — however minimal — could also help.

---

### Pattern 2 — Reaction misclassified as Opinion (3 cases)
**Examples:** #6, #8, #9

Comments affected:
- "So? Why are people always looking for goals, this is soccer..."
- "I feel bad for whoever went for this match. This was arguably the 
  worst match of the cup!..."
- "Great game. I know it'll blow the minds of some people that a 0-0 
  match was entertaining but i don't care. I love this sport."

**Pattern:** All three comments contain counterargument structure, with the 
speaker acknowledging an opposing view before expressing their own 
reaction. The model learned that counterargument framing signals Opinion, 
but missed that these comments never develop that framing into a genuine 
argument; they collapse back into emotional expression.

**Why this boundary is hard:** The linguistic surface of "I know some 
people think X, but I feel Y" is identical whether the speaker is making 
an argument (Opinion) or dismissing a counterpoint to return to feeling 
(Reaction). The distinction is functional, not linguistic, for the model 
cannot reliably detect whether the counterargument is being engaged with 
or dismissed.

**What would fix it:** More training examples that show counterargument 
structure collapsing into emotional expression rather than developing into 
an argument. This is the hardest boundary to fix with data alone — it may 
require a more explicit decision rule encoded in the label definitions.

---

### Pattern 3 — Opinion misclassified as Observation (3 cases)
**Examples:** #10, #13, #14

Comments affected:
- "The ambience has been meeeh at most... have you seen the ambience 
  in Mexico? That is where the whole cup should've go to"
- "So far, the most exciting games, Croatia and England have performed 
  very well... England has shown a stronger ability to win the game"
- "Had Vozinha put just a little of what he did against Spain, Uruguay 
  would still be at 1 point"

**Pattern:** All three use neutral-sounding or descriptive language to 
make evaluative claims. The model missed the persuasive intent because 
the surface language resembles neutral reporting.

**Why this boundary is hard:** This is the most consistent annotation 
challenge documented throughout the labeling process, factual-sounding 
language that contains embedded normative claims. The model learned 
Observation as neutral declarative sentences, but many Opinion comments 
use declarative structure to make evaluative arguments.

**What would fix it:** More training examples that demonstrate Opinion 
comments using neutral-sounding language. Explicitly flagging evaluative 
adjectives ("better", "should've", "meeeh") as Opinion signals in 
training data would help the model learn the distinction.

---

### Pattern 4 — Analytical/Predictive misclassified as Opinion (1 case)
**Example:** #12

Comment affected:
- "Really genius? So with 12 teams all vying for 8 knockout round 
  slots... Try adding up the number of games versus the 12 third place 
  teams. I'll wait…"

**Pattern:** Aggressive, sarcastic tone overwhelmed mathematical reasoning 
underneath. The model classified based on delivery rather than substance.

**Why this boundary is hard:** The model correctly learned that sarcasm 
and confrontational language signal Opinion in most cases. 
This comment is the exception where hostile delivery wraps genuine 
analytical content. With only 29 Analytical/Predictive training examples, 
the model had insufficient exposure to this rare combination.

**What would fix it:** More Analytical/Predictive training examples with 
aggressive or sarcastic delivery. This is a data quantity problem as much 
as a definition problem — the pattern exists in the taxonomy but is 
underrepresented in training.

---

### Summary of Error Patterns

| Pattern | Cases | Root Cause |
|---------|-------|------------|
| Reaction → Observation | 3 | Model treats brevity as Observation signal |
| Reaction → Opinion | 3 | Counterargument framing overwhelms emotional substance |
| Opinion → Observation | 3 | Neutral-sounding language masks persuasive intent |
| Analytical/Predictive → Opinion | 1 | Aggressive tone overwhelms analytical substance |

**Dominant confused pair:** Reaction is the most misclassified label — 
6 of 17 errors (35%) involve true Reaction comments being assigned another 
label. This reflects the fundamental challenge of Reaction as a label: it 
is defined by the absence of reasoning rather than the presence of a 
distinctive signal, making it the hardest label for the model to identify 
reliably when emotional expression is understated or wrapped in other 
linguistic structures.

---

### Reflection: What the Model Learned vs What Was Intended

#### What the model learned well

The fine-tuned model successfully learned the distinctive surface patterns of Analytical/Predictive comments, achieving F1-score of 0.83, as compared to the baseline score of 0.62. Comments containing explicit if/then structures, points calculations, standings references, and conditional qualification reasoning were properly classified. This suggests DistilBERT can learn task-specific linguistic patterns effectively, when those patterns have consistent and distinctive features, such as numbers, conditional conjunctions ("if", "then", "would"), and standings vocabulary ("points", "group", "advance").

#### Where the model fell short

The intended taxonomy was built around a functional distinction — what is 
the comment doing — rather than surface features. The model did not perform properly wherever function and surface features diverged:

- **Tone masking function:** Aggressive or sarcastic delivery caused the 
  model to predict Opinion even when the underlying substance was Analytical/Predictive (Wrong Prediction #2). Warm or casual tone caused it to predict Reaction even when an argument was being made.

- **Observation as default:** The model treated Observation as a catch-all 
  for low-confidence predictions, misclassifying 7 out of 22 non-Observation examples into that label. This mirrors the difficulty experienced during labeling,which is Observation is the absence of strong signals rather than the presence of them, thereby making it the hardest label for both humans and models to apply consistently.

- **Reaction/Opinion boundary:** The model consistently confused emotional 
  comments that contain argument structure (Reaction) with genuine Opinion. 
  With only 182 training examples, it learned that counterargument framing 
  signals Opinion without learning when that framing collapses back into 
  emotional expression.

---

#### Why the baseline remained competitive

The zero-shot baseline (61.5%) outperformed the fine-tuned model (56.4%) 
overall. Llama-3.3-70b-versatile's broad pretraining gives it strong general 
language understanding that is difficult to surpass with a small labeled 
dataset. Results show that the baseline performed better with statement labeled Reaction (F1=0.69), since  emotional vocabulary is consistent and well represented in Llama's pretraining data. Fine-tuning with 182 examples was sufficient to improve 
Analytical/Predictive recognition but introduced overfitting on the 
Reaction/Opinion boundary.

---

### Sample Classifications

The following examples were run through the fine-tuned model. Each entry 
shows the input text, predicted label, and confidence score.

| # | Text | Predicted Label | Confidence | Correct? |
|---|------|-----------------|------------|----------|
| 1 | "They lost both matches, and even if they win their last game they can only reach 3 points. Australia and Paraguay already have 3 points and both beat Turkey H2H, so Turkey can't finish above them even if everything else goes their way." | Analytical/Predictive | 0.95 | ✓ |
| 2 | "As an Argentine fan, loving every bits of this Uruguay downfall." | Reaction | 0.99 | ✓ |
| 3 | "Cape Verde has single-handedly convinced me that going to 48 teams is not a mistake." | Opinion | 0.97 | ✓ |
| 4 | "Cape Verde would have qualified for a 32 team world cup. They had 6 wins, 1 draw and 1 loss in qualifying and topped the group ahead of Cameroon." | Observation | 0.97 | ✓ |
| 5 | "Really genius? So with 12 teams all vying for 8 knockout round slots, you don't think goal differential will matter? 12 teams will all have 2 or 4 points. They always do. Then it comes down to goals. Try adding up the number of games versus the 12 third place teams. I'll wait…" | Opinion | 0.46 | ✗ |

**Notes on selected predictions:**

**Example 1 (correct — Analytical/Predictive, confidence 0.95):**
The model correctly identified this as Analytical/Predictive with high 
confidence. The comment contains explicit elimination reasoning — it maps 
every possible outcome and proves mathematically that Turkey cannot qualify. 
The distinctive vocabulary (points, H2H, qualifying spot) combined with 
explicit if/then structure gave the model strong signal to classify 
correctly.

**Example 2 (correct — Reaction, confidence 0.99):**
The model's highest confidence prediction. Short, unambiguous emotional 
expression with clear partisan framing ("As an Argentine fan") and no 
reasoning or analytical content — exactly the clean Reaction signal the 
model learned to recognize reliably.

**Example 5 (incorrect — true label: Analytical/Predictive, predicted: 
Opinion, confidence 0.46):**
The model's lowest confidence prediction reflects genuine uncertainty — 
this is the aggressive tone trap documented in Wrong Prediction #2. The 
sarcastic opener ("Really genius?") and confrontational closing ("I'll 
wait…") overwhelmed the mathematical reasoning underneath. The very low 
confidence (0.46) suggests the model was essentially guessing, unable to 
reconcile the hostile delivery with the analytical substance.

---

#### Dataset limitations

The 5.1% accuracy regression is due to dataset size. With only 182 training examples across four labels, with 29 being Analytical/Predictive training examples, the model didn't have enough examples to learn the subtle distinctions that was intended. A larger dataset of  at least 500 examples per label would likely produce more reliable fine-tuning results. Furthermore, the inherent ambiguity in the Reaction/Opinion boundary, as seen in several edge cases examples illustrates a labeling challenge that can cause model uncertainty. 

---

#### Key takeaway

TakeMeter successfully demonstrates both the potential and the limitations of 
fine-tuning on small datasets. The model learned what it could from the available labels in the dataset, but could not learn the detailed distinctions the label taxonomy was designed to capture. This gap between classification logic and learned patterns is a fundamental challenge in classification that larger datasets and more sophisticated modeling approaches would be able to address.