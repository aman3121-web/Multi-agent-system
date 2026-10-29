# Multi-Agent Research System

A Python research pipeline built with LangChain. It uses specialized agents to
search the web, read a relevant source, write a structured report, and review
the result.

## Features

- Web search through Tavily
- URL scraping with `requests` and BeautifulSoup
- OpenAI-powered search and reader agents
- Report generation with introduction, findings, conclusion, and sources
- Critic feedback with a score, strengths, and improvement areas

## Project structure

```text
.
├── agents.py       # Agents, prompts, and LangChain chains
├── pipeline.py     # End-to-end research workflow
├── tools.py        # Tavily search and URL scraping tools
├── requirements.txt
└── .env            # Local API keys; do not commit this file
```

## Requirements

- Python 3.10 or newer
- An OpenAI API key
- A Tavily API key

## Setup

Create and activate a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install dependencies:

```powershell
pip install -r requirements.txt
```

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your-openai-api-key
TAVILY_API_KEY=your-tavily-api-key
```

Never commit API keys or share them publicly. If a key has been exposed, revoke
it and create a replacement before running the project.

## Run

From the project directory:

```powershell
python pipeline.py
```

Enter a research topic when prompted. The pipeline will:

1. Search for recent and reliable information.
2. Select and scrape a relevant URL.
3. Generate a detailed research report.
4. Critique the report and print feedback.

The workflow can also be called from Python:

```python
from pipeline import run_research_pipeline

result = run_research_pipeline("The impact of renewable energy")
print(result["report"])
print(result["feedback"])
```

## Configuration

The language model is configured in `agents.py`:

```python
ChatOpenAI(model="gpt-4o-mini", temperature=0)
```

The search tool currently requests up to five Tavily results. The scraper
returns up to 3,000 characters of cleaned page text and uses an 8-second
request timeout.

## Troubleshooting

### Missing OpenAI credentials

Ensure the variable is named exactly `OPENAI_API_KEY`, not `OPEN_API_KEY`, and
that `.env` is located in the project root.

### Missing Tavily credentials

Ensure `TAVILY_API_KEY` is present in `.env` and that the key is active.

### Scraping fails

Some websites block automated requests or require JavaScript. Try another
topic or source; the scraper reports the failed URL in its returned message.