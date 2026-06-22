# TakeMeter — planning.md

---

## Project Description
- Analyzed comments on the various topics related to the current world cup, World Cup 2026. Obtained comments from the threads involving discussions as well as pre-match, match, and post-match comments regarding select games.
---

## Label Definition
- The definition of each label is as follows:
    - Observation: re-states obvious facts 
    - Opinion: provides justification based on an observable question and/or reaction
    - Reaction: response to comments that involve varied emotion
    - Contextual: explanation with a reference

---

## Label and Example
- Observation:
    - ' where some of the countries fans can't get visa to support their teams kinda unfair for them '
    - ' Could be most of the stadiums are huge bowls. Not even a hint of roof or cover, maybe the sound is just getting lost.'

- Reaction:
    - ' No thank you '
    - ' With those ticket prices? Great way to completely ruin it '

- Opinion
    - 'Anywhere can host it. We don’t need 48 teams and we don’t need 80000 seat stadiums charging a fortune. An affordable world cup dor fans would be far preferable to this, enough money is made off tv deals that we don’t need big expensive venues. '
    - ' The ambience has been meeeh at most, we have been in 3 games and only yesterday’s match between Netherlands and Sweden had an OK vibe; have you seen the ambience in Mexico? That is where the whole cup should’ve go to '

- Contextual
    - ' I was at a game and can confirm when I watched the replay on TV it sounded like the crowd was dead compared to what it was like in stadium '
    - ' Either would have 4 points with a win, which would most likely be good for at least second, but Iran and Belgium both have 2 points now so it's a very close group, hard to tell who will go through tbh '

---

## Edge Cases
- Since most commentary are a reaction to a game, it is important to note that comments can fall between two distinct labels. For instance, 'HOW WAS THAT NOT A PK??? Sorry for yelling' can be considered as both a reaction and an observation. Reason being that the user is reacting to a play on the TV screen, based on an observation. In this case, I decided to label as a 'reaction' due to the qualifying word 'yelling'. Consequently, I used examples that are less ambiguous when selecting comments that are used in the project. 

---

## AI Tool Plan



**Milestone 3 — Individual tool implementations:**
- Completed in the planning phase, since stretch features was included as part of the planning step. The project contains the following tools:
     - search_listings: searches database based on user input
     - suggest_outfit: suggests outfit pairings based on result from search
     - create_fit_card: creates outfit pairings 
     - retry_with_fallback: error handling 
     - compare_prices: compare prices based on search_listings
     - trend_awareness: suggest trends based on result from search_listing
     - style_profile_memory:saves results for future reference or creates a new list for a new user
- created a test file to test for each tool. All test, 41 in total, passed for each tool
- updated LLM model from `llama3-8b-8192` to `llama-3.1-8b-instant`, as llama3-8b-8192 has been decommissioned. This was flagged when running tools.py 

**Milestone 4 — Planning loop and state management:**
- Loop is initialized at the start of each `run_agent` and the user does not re-enter the same information as follows:
session = {
    "query": str,               # original user query
    "parsed": dict,             # description, size, max_price
    "search_results": list,     # all matching listings
    "selected_item": dict,      # top result → passed into suggest_outfit
    "wardrobe": dict,           # user's wardrobe → passed into suggest_outfit
    "similar_items": list,      # output of compare_prices
    "outfit_suggestion": str,   # output of suggest_outfit → passed into create_fit_card
    "trend_info": str,          # output of trend_awareness
    "fit_card": str,            # output of create_fit_card
    "fallback_message": str,    # set if retry_with_fallback loosened any filters
    "error": str | None,        # set on early exit; None on success
}

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
