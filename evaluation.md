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

#### Dataset limitations

The 5.1% accuracy regression is due to dataset size. With only 182 training examples across four labels, with 29 being Analytical/Predictive training examples, the model didn't have enough examples to learn the subtle distinctions that was intended. A larger dataset of  at least 500 examples per label would likely produce more reliable fine-tuning results. Furthermore, the inherent ambiguity in the Reaction/Opinion boundary, as seen in several edge cases examples illustrates a labeling challenge that can cause model uncertainty. 

---

#### Key takeaway

TakeMeter successfully demonstrates both the potential and the limitations of 
fine-tuning on small datasets. The model learned what it could from the available labels in the dataset, but could not learn the detailed distinctions the label taxonomy was designed to capture. This gap between classification logic and learned patterns is a fundamental challenge in classification that larger datasets and more sophisticated modeling approaches would be able to address.