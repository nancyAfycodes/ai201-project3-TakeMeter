# TakeMeter
TakeMeter is  a fine-tuned text classifier that evaluates discourse quality in an online community. It uses labels to determine if a comment fits a particular definition. 

---

## Data Source
- Data used in the project was obtained from World Cup 2026 subreddit. Comments were obtained from threads that involved discussion, pre-match, match, and post-match. Most of the comments relied on 'match' threads, since it contained a mixture of commentary involving all stages of the game. Comments were labeled as either an observation, reaction, opinion, or contextual. The labels were chosen as most comments seems to fall into one of the four labeled categories above. Since I read each comment, I made the decision to include or exclude comments based on the label criteria I had chosen.  
---

## Label Count Distribution 
- Observation: 65
- Reaction: 83
- Opinion: 71
- Analytical/ Predictive: 41
- Total - 260 

---

## Planning loop
- Plan involved 

---

## Example Labels

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


---

## Project structure



---

## Setup

```bash
# 1. Clone the repo and install dependencies
pip install -r requirements.txt

# 2. Add your Groq API key
echo "GROQ_API_KEY=your_key_here" > .env

# 3. Run the app
python app.py 
py app.py (VS Code)
```

Then open the localhost URL shown in your terminal (usually `http://localhost:7860`).

---

## Running tests

```bash

```

---

## Example Labels
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
## Video Demo
[Demo Video]()
