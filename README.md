# 📈 Multi-Agent Financial Analysis

An AI-powered financial analysis system built with **CrewAI** that coordinates four specialized agents to research a stock, develop trading strategies, plan execution, and assess risk — producing a comprehensive day-trading risk report.

---

## 🧠 Overview

This project uses a **hierarchy of autonomous agents** that collaborate sequentially to analyze a given stock. Each agent owns a distinct role in the trading workflow, mirroring how a real trading desk operates.

```
Data Analyst → Trading Strategy Developer → Trade Advisor → Risk Advisor
```

The crew takes a stock ticker plus user-defined parameters (capital, risk tolerance, strategy preference) and returns a detailed risk-assessed trading report.

---

## 🤖 The Agents

| Agent | Role | Responsibility |
|---|---|---|
| **Data Analyst** | Market monitoring | Analyzes real-time market data to identify trends and predict movements |
| **Trading Strategy Developer** | Strategy design | Builds and refines trading strategies based on insights and risk tolerance |
| **Trade Advisor** | Execution planning | Determines optimal timing, pricing, and entry points for trades |
| **Risk Advisor** | Risk management | Evaluates risk exposure and recommends mitigation strategies |

Each agent is equipped with web search (`SerperDevTool`) and web scraping (`ScrapeWebsiteTool`) to gather live market context.

---

## 🛠️ Tech Stack

- **Framework:** CrewAI (multi-agent orchestration)
- **LLM:** DeepSeek (`deepseek-chat`) via OpenAI-compatible API — easily swappable for OpenAI or Gemini
- **Tools:** Serper (search), ScrapeWebsiteTool (web scraping)
- **Language:** Python
- **Environment:** Google Colab / Jupyter

---

## ⚙️ Setup

1. **Install dependencies**
   ```bash
   pip install crewai==0.28.8 crewai-tools==0.1.6 \
       langchain==0.1.20 langchain-core==0.1.53 \
       langchain-community==0.0.38 langchain-openai==0.1.7
   ```

2. **Set your API keys** (use environment variables — never hardcode keys)
   ```python
   import os
   os.environ["Deep_API_KEY"]   = "your-deepseek-key"
   os.environ["SERPER_API_KEY"] = "your-serper-key"
   ```

3. **Configure the LLM**
   ```python
   from langchain_openai import ChatOpenAI

   llm = ChatOpenAI(
       model="deepseek-chat",
       base_url="https://api.deepseek.com/v1",
       api_key=os.environ["Deep_API_KEY"],
       temperature=0.7,
   )
   ```

---

## ▶️ Usage

```python
financial_trading_inputs = {
    'stock_selection': 'AAPL',
    'initial_capital': '100000',
    'risk_tolerance': 'Medium',
    'trading_strategy_preference': 'Day Trading',
    'news_impact_consideration': True
}

result = financial_trading_crew.kickoff(inputs=financial_trading_inputs)
```

The crew runs sequentially and outputs a full report covering recommended strategies, execution plans, and a risk analysis with mitigation steps.

---

## 📊 Sample Output

For an `AAPL` day-trading run, the system produces:

- **Strategy 1 — Two-Day High/Low Breakout** (most recommended): breakout entries with volume confirmation and trailing stops
- **Strategy 2 — Range Trading with RSI** (second choice): oscillation trades with volume filters and time-based exits
- **Strategy 3 — Gap-Fill** (least recommended): gap-fill setups with strict trade-size and timing rules
- **Portfolio & execution risks**: overtrading, emotional trading, position sizing, liquidity, and technology risk — each with mitigation recommendations
- **Final risk rating & recommendation**

---

## ⚠️ Disclaimer

This project is for **educational and demonstration purposes only**. It is **not financial advice**. The agents generate analysis using LLMs and web data, which may be inaccurate or outdated. Do not use this system to make real investment or trading decisions.

---

## 📂 Notes

- API keys are read from environment variables and should be kept private — never commit them.
- The LLM is fully swappable; commented examples for OpenAI (`gpt-3.5-turbo`) and Gemini (`gemini-2.0-flash`) are included in the notebook.
- `max_rpm` is set to limit API request rate; adjust based on your provider's limits.
