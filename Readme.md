# Ask Your Data: AI Data Analyst Agent

![App screenshot](screenshot.png)

Upload a sales spreadsheet and ask questions in plain English. The agent writes and runs SQL on your data, so answers are exact, not guessed.

## The business problem
Managers often wait for someone to pull numbers from Excel. This app lets them ask directly.

## How it works
- **Streamlit** web interface
- **Google Gemini** (free tier) as the model
- **DuckDB** to run the SQL queries the agent writes
- **pandas** to clean the uploaded file

## What I changed from the original
- Switched the model from OpenAI GPT-4o to free Google Gemini
- Fixed a NumPy/pyarrow version conflict in requirements.txt
- Added a friendly message when the free-tier rate limit is hit, in place of raw error JSON

## Results
## Results
Tested on 10 manager questions using clean_sales.csv, checked against answers calculated with pandas (see accuracy_test.ipynb).

| # | Question | Correct (pandas) | App answer | Result |
|---|---|---|---|---|
| 1 | What is the total revenue? | 2,699,900 | 2,699,900 | ✅ |
| 2 | How many orders are there? | 180 | 180 | ✅ |
| 3 | Which region had the highest total revenue? | Delta | Delta | ✅ |
| 4 | Which product sold the most units? | Desk Lamp (459) | Desk Lamp (459) | ✅ |
| 5 | Which month had the highest revenue? | September | September | ✅ |
| 6 | Which sales rep brought in the most revenue? | Nour | Nour | ✅ |
| 7 | What is the average revenue per order? | 14,999.44 | 14,999.44 | ✅ |
| 8 | In Cairo, which product sold the most units? | Desk Lamp (106) | Desk Lamp (106) | ✅ |
| 9 | How many orders have no known sales rep? | 5 | 0 | ❌ |
| 10 | In September, which region had the lowest revenue? | Upper Egypt | Upper Egypt | ✅ |

**Score: 9/10**

**What failed and why:** Q9 returned 0 because missing sales reps are stored as the text "Unknown", and the agent searched for empty values. A possible fix is to describe this convention in the system prompt.

## How to run
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    streamlit run ai_data_analyst.py

Then enter a free Gemini API key from Google AI Studio in the sidebar.

## Credits
Based on ai_data_analysis_agent from awesome-llm-apps by Shubham Saboo.