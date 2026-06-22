# TakeMeter
TakeMeter is  a fine-tuned text classifier that evaluates discourse quality in an online community. 

---

## Data Source
- Data used in the project was obtained from World Cup 2026 subreddit. Comments were obtained from threads that involved discussion, pre-match, match, and post-match. Most of the comments relied on 'match' threads, since it contained a mixture of commentary involving all stages of the game. Comments were labeled as either an observation, reaction, opinion, or contextual. The labels were chosen as most comments seems to fall into one of the four labeled categories above. Since I read each comment, I made the decision to include or exclude comments based on the label criteria I had chosen.  
---

## Label Count Distribution 
- Observation:
- Reaction: 
- Opinion:
- Contextual: 

---

## Planning loop
- Plan involved 

---

## Example Labels
- Observation:
    - 


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
pytest tests/ -v
```

41 tests covering every tool's failure modes — no API key required, all LLM calls are mocked.

---

## Example Labels


---
## Video Demo
[Demo Video](https://youtu.be/EDhRE7JD9E8)
