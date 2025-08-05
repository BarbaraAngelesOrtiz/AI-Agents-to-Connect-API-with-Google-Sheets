# 🤖 From Beginner to Automation: Using AI Agents to Connect Yahoo Finance with Google Sheets

<img width="1536" height="1024" alt="ChatGPT Image 6 ago 2025, 12_03_26 a m" src="https://github.com/user-attachments/assets/d09227b4-1b55-43f0-8dae-fbc9f0e9fa05" />

AI agents are becoming more and more common in data workflows, automation, and even code generation. In this article, I’ll show you how to get started using agents for a practical task: automating financial data collection and storing it in a Google Sheets spreadsheet.

At the end, I’ll also share how I took this idea further in a more advanced multi-agent project, available on GitHub.

---

## 🧠 What Is an Agent in Programming?
An agent is a small autonomous program that behaves like a specialized assistant: it receives instructions, makes decisions, and executes tasks.

In our case, an agent can:

- Fetch financial data from the internet  
- Run Python code  
- Collaborate with other agents to complete a goal  
- Automate all of the above without any human interaction  

📈 Our First Objective
Let’s begin with a simple task: ➡️ Get historical stock price data from Yahoo Finance and store it in a Google Sheets spreadsheet. using Google Cloud.

This is already useful for automating personal finance tracking, regular market reports, or even as a learning project.

🧰 Tools We’ll Use

- **yfinance**: to access stock data easily  
- **pandas**: to work with tabular data  
- **gspread**: to connect to Google Sheets  
- **gspread_dataframe**: to send a DataFrame directly into Sheets  
- A simple agent that receives a natural language task and executes it  

---

✅ A Basic Script (Before Using Agents)
Before introducing agents, let’s look at a basic Python script that connects Yahoo Finance to Google Sheets:

```
import yfinance as yfimport pandas as pd
import gspread
from gspread_dataframe import set_with_dataframe
from oauth2client.service_account import ServiceAccountCredentials

# Step 1: Download stock data
df = yf.download("AAPL", period="7d", interval="1d")

# Step 2: Connect to Google Sheets
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]
creds = ServiceAccountCredentials.from_json_keyfile_name("credentials.json", scope)
client = gspread.authorize(creds)

# Step 3: Open the spreadsheet
spreadsheet = client.open("FinanceTracker")
sheet = spreadsheet.worksheet("AAPL_Data")

# Step 4: Write data to the spreadsheet
set_with_dataframe(sheet, df)
print("Data successfully updated in Google Sheets.")
```

✅ And just like that, you have financial data pulled from the internet and stored in a cloud-based spreadsheet, no Excel file required.

---

🧠 Now Let’s Add Agents
Using libraries like AutoGen, you can create an AI agent that interprets a natural language instruction like:

"Get the last 7 days of MSFT stock prices and store them in my spreadsheet called 'MSFT_Data'."

Here’s a simplified example of how that might look:

```
from autogen import UserProxyAgent, AssistantAgent

llm_config = {
    "model": "gpt-4",
    "api_key": "your_openai_api_key",
}

excel_agent = AssistantAgent(name="ExcelAgent", llm_config=llm_config)

user_proxy = UserProxyAgent(
    name="UserProxy",
    human_input_mode="NEVER",
    code_execution_config={"work_dir": "data"}
)

user_proxy.initiate_chat(
    excel_agent,
    message="""
    Get the MSFT stock prices for the past 7 days and write them into my Google Sheet named 'MSFT_Data'.
    """
)
```
💡 The agent interprets your message, runs the necessary code (including connecting to Yahoo Finance and Google Sheets), and updates the spreadsheet automatically.

---

🔁 Automate It with GitHub Actions
Once your script is working, you can schedule it to run automatically every day using GitHub Actions.

Here’s a simple workflow file: .github/workflows/run.yml

```
name: Run Daily Agent

on:
  schedule:
    - cron: '0 7 * * *'  # Every day at 7:00 UTC
  workflow_dispatch:

jobs:
  run-agent:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - run: pip install -r requirements.txt
      - run: python main.py
```

📌 Tip: Store your credentials.json for Google Sheets as a GitHub Secret to keep it secure.

---

🧪 My Project: Multi-Agent Financial Insights
🔗 GitHub Repository: 👉 github.com/BarbaraAngelesOrtiz/Multi-agent-financial-insights

In this more advanced project, I went further by creating a multi-agent system where:

- Each agent has a specific role (fetching, transforming, saving)
- They talk to each other to coordinate actions
- The system connects to Yahoo Finance and Google Sheets
- It runs daily from GitHub using automation

This is perfect if you want to scale up and build intelligent assistants that handle different parts of a workflow, like a team of virtual analysts.

---

🧭 Final Thoughts
✅ If you’re new, start with a basic script using yfinance + Google Sheets 

✅ Add a simple AI agent that interprets instructions 

✅ Use GitHub Actions to run the process automatically 

✅ And if you're curious about advanced setups, check out my multi-agent project

💬 Would you be interested in a full step-by-step guide or video tutorial? I’d love to hear what kind of tasks you’d like to automate.

Thanks for reading! 🙌
