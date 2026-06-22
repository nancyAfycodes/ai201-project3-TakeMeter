# TakeMeter — planning.md

---

## Project Description
- Analyzed comments on the various topics related to the current world cup, World Cup 2026. Obtained comments from the threads involving discussions as well as pre-match, match, and post-match comments regarding select games.
---

## Label Taxonomy
- The definition of each label is as follows:

**1. Observation**
A comment that states a factual or descriptive claim without taking a position, providing reasoning, and/or referencing external knowledge. The speaker reports what they see or experience without editorializing.

*Example: "where some of the countries fans can't get a visa to support their teams"*

---

**2. Opinion**
In this case, the comment takes a clear stance and provides reasoning or justification to support it. May make use of comparisons or references as supporting evidence, but the argument or conclusion is the primary substance.

*Example: "The US might be one of the best hosts. That's exactly why we should save it for special occasions, not turn the World Cup into a permanent residence."*

---

**3. Reaction**
A comment that expresses an emotional or visceral response to a match event, decision, or situation. Minimal reasoning is provided; the emotion itself is the primary substance.

*Example: "What atmosphere?"*

---

**4. Analytical/Predictive**
A comment that uses statistics, standings, tactical reasoning, or logical if/then thinking to analyze a current situation or predict an outcome. The analysis or prediction is the primary substance.

*Example: "Either would have 4 points with a win, which would most likely be good for at least second, but Iran and Belgium both have 4 points now so it's a very close group, hard to tell who will go through tbh"*

---

**Decision Rules**
- If a comment uses numbers or comparisons to reach a conclusion or prediction → Analytical/Predictive
- If a comment uses a reference or comparison to argue a point → Opinion
- If a comment states something without argument or prediction → Observation
- If the emotion is the point and reasoning is absent or minimal → Reaction
- When in doubt between Opinion and Analytical/Predictive, ask: *is the comment reasoning toward a conclusion using data/tactics, or expressing a stance based on values/preferences?*

---

## Counts
Data Collected  - 220
Label Counts:
    - Observation: 
    - Reaction:
    - Opinion:
    - Analytical / Predictive:
---

## Data Collection Plan
- Data was collected by reviewing several World Cup 2026 threads on current live games as well as completed matches. My main aim is to read as much comments as possible, which was then used to create labels for each comment. 
---

## Label Taxonomy Example
- Observation:
    - ' where some of the countries fans can't get visa to support their teams kinda unfair for them '
    - ' Could be most of the stadiums are huge bowls. Not even a hint of roof or cover, maybe the sound is just getting lost.'

- Reaction:
    - ' No thank you '
    - ' With those ticket prices? Great way to completely ruin it '

- Opinion
    - 'Anywhere can host it. We don’t need 48 teams and we don’t need 80000 seat stadiums charging a fortune. An affordable world cup dor fans would be far preferable to this, enough money is made off tv deals that we don’t need big expensive venues. '
    - ' The ambience has been meeeh at most, we have been in 3 games and only yesterday’s match between Netherlands and Sweden had an OK vibe; have you seen the ambience in Mexico? That is where the whole cup should’ve go to '

- Analytical/Predictive
    - 'Isn't it risky to aim for 3rd? Not every 3rd place is making it out of group. Second place is a death sentence for us so our only hope is to destroy haiti while brazil struggle against Scotland, and honestly i don't see us scoring many goals. '
    - ' Either would have 4 points with a win, which would most likely be good for at least second, but Iran and Belgium both have 2 points now so it's a very close group, hard to tell who will go through tbh '

---

## Edge Cases
- Since most commentary are a reaction to a game, it is important to note that comments can fall between two distinct labels. For instance, 'HOW WAS THAT NOT A PK??? Sorry for yelling' can be considered as both a reaction and an observation. Reason being that the user is reacting to a play on the TV screen, based on an observation. In this case, I decided to label as a 'reaction' due to the qualifying word 'yelling'. Consequently, I used examples that are less ambiguous when selecting comments that are used in the project. 
---

**Edge Cases & Difficult Labeling Decisions**

Since most World Cup commentary is inherently reactive, for users are responding to live match events, referee decisions, or ongoing group standings, several comments carry signals from multiple labels simultaneously. Below are three genuinely difficult cases encountered during annotation, along with the decision made and the reasoning behind it.

---

**Edge Case 1: Reaction vs. Observation**
*"HOW WAS THAT NOT A PK??? Sorry for yelling"*

This comment could be labeled either Reaction or Observation. The user is responding to a specific play (Observation), however, the all-caps phrasing and explicit acknowledgment of yelling signals that the emotion is the primary substance. **Decision: Reaction.** The qualifying word "yelling" confirms the speaker is aware they are emoting rather than reporting. When emotional expression dominates the form of a comment, even if grounded in something observable, Reaction takes precedence.

---

**Edge Case 2: Opinion vs. Analytical/Predictive**
*"Either would have 4 points with a win, which would most likely be good for at least second, but Iran and Belgium both have 2 points now so it's a very close group, hard to tell who will go through tbh"*

This comment uses group standings data, which initially suggests Contextual (later redefined as Analytical/Predictive). However, the reasoning is being used to reach a conclusion ("hard to tell who will go through") rather than simply presenting data. **Decision: Analytical/Predictive.** The decision rule applied: if a comment uses numbers or standings to reason toward a prediction or conclusion, it is Analytical/Predictive regardless of how uncertain or casual the tone is.

---

**Edge Case 3: Observation vs. Opinion**
*"Anywhere can host it. We don't need 48 teams and we don't need 80000 seat stadiums charging a fortune. An affordable world cup for fans would be far preferable to this."*

This comment opens with what sounds like a neutral claim but quickly shifts into a normative argument about what the World Cup should look like. The presence of "we don't need" and "far preferable" signals value-based reasoning rather than neutral description. **Decision: Opinion.** When a comment moves from describing what is to arguing what should be, it crosses from Observation into Opinion.

---
**Edge Case 4: Analytical/Predictive vs. Observation**
*"Isn't it risky to aim for 3rd? Not every 3rd place is making it out of group. Second place is a death sentence for us so our only hope is to destroy haiti while brazil struggle against Scotland, and honestly i don't see us scoring many goals."*

The above comment uses group standing logic and match predictions to arrive at a conclusion. The observation elements ('not every 3rd place makes it') helps in explaining the logic, but it is not the main aim of the comment.

---

## Evaluation Metrics 
- The overall project would be measured on how the LLM model `distilbert-base-uncased` is able to distinguish between labels especially nuance. Since some of the comments contain abbreviation and slang, it is important that the model is able to 'understand' the language the user uses in expressing thoughts and opinions in regards to the game being played as well as responses to comments made in each thread.
    
---
## Project Success
- Project will be considered successful, if model is able to identify each label as well as understand nuance associated with each comment

---

## AI Tool Plan
- Label stress-testing: Using Claude, I was able to refine my label definitions. For instance, I'll post a comment and its accompanying label, then state my reason. Based on the label definition, Claude will explain why my reasoning may not meet the label definition. For example, my original labels were, observation, reaction, opinion and contextual. However, the comments I had originally labeled as 'contextual' could easily have been categorizes as either ' observation or opinion'. Consequently, Claude identified 'contextual' label and suggest the use of 'analytical/predictive', for it was a better fit. 

- Annotation Assistance - Data was hand-labeled using label definitions, I didn't use Claude to label my data. 


**Milestone 3 — Individual tool implementations:**
- 

**Milestone 4 — Planning loop and state management:**
- 

---

## A Complete Interaction (Step by Step)

Write out what a full user interaction looks like from start to finish — tool call by tool call. Use a specific example query.

**Example user query:** "blue Levi jeans costing no more than $40"

**Step 1:**
<!-- What does the agent do first? Which tool is called? With what input? -->
- it searches the database matching the description, from search_listing
**Step 2:**
<!-- What happens next? What was returned from step 1? What tool is called now? -->
- based on results, it suggest top two styling pair options. It also reference trend_info to show how it compares to trend. 

**Step 3:**
<!-- Continue until the full interaction is complete -->
- If no result is generated, it automatically removes filters and tries again. The error messages generated stage is different from that generated if no item(s) is found.
- If no result after prompt modification, it notifies the user that the item is not found and ask the user to search for another item. 

**Final output to user:**
<!-- What does the user actually see at the end? -->
- Item based on the user's prompt. Also, it will display price comparison for the item as well as similar items that can be paired to create a final look. Furthermore, it'll display a message on recent trends based on the user's selection. Finally, it saves the complete look into profile memory or creates a new style memory for a new user for future reference.
