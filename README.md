# Website Summarizer

A small web app that **scrapes a webpage and summarizes it with an LLM**. Enter
a company name and its landing-page URL, and it fetches the page, strips out the
noise (scripts, styles, images), sends the text to a [Groq](https://groq.com)-hosted
Llama model, and returns a concise summary in a simple [Gradio](https://www.gradio.app/)
web interface.

## Features

- 🌐 Scrapes and cleans page text with `requests` + `BeautifulSoup`
- 🤖 Summarizes via the Groq API (`llama-3.3-70b-versatile`)
- 🖥️ One-click local web UI built with Gradio
- ⚡ Optional streaming "brochure" generation (see [Notes](#notes))

## Prerequisites

- **Python 3.9+**
- A **Groq API key** — create a free one at <https://console.groq.com/keys>

## Installation

### 1. Get the code

```bash
git clone https://github.com/VinayakMokashi/website-summarizer.git
cd website-summarizer
```

### 2. Create and activate a virtual environment

**Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

> On Windows, if activation is blocked by the execution policy, run
> `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` once in that
> terminal and try again.

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add your API key

Copy the example env file and put your key in it:

**Windows (PowerShell):**
```powershell
Copy-Item .env.example .env
```

**macOS / Linux:**
```bash
cp .env.example .env
```

Then edit `.env` and set:
```
GROQ_API_KEY=your_actual_key_here
```

The app loads this automatically — your key is never hard-coded, and `.env` is
git-ignored so it won't be committed.

## Run

```bash
python app_websummarizer.py
```

Open the local URL it prints (e.g. `http://127.0.0.1:7860`). Enter:

- **Company name** — any label, e.g. `Anthropic`
- **Landing page URL** — a full URL *including* `http://` or `https://`, e.g.
  `https://www.anthropic.com`

Click submit to get the summary. Press **Ctrl+C** in the terminal to stop.

## Notes

- Each summary makes a real Groq API call and uses your account's quota.
- The UI is wired to the non-streaming summary. To stream a generated company
  **brochure** live instead, point the Gradio interface at `stream_summary`
  (change `fn=self.get_summary` to `fn=self.stream_summary` in the `run` method).
- Some sites block automated scraping or render content with JavaScript, which a
  simple `requests` fetch can't see — results vary by site.

## Project structure

| File | Description |
|------|-------------|
| `app_websummarizer.py` | The `Website` class (scraping, LLM calls, Gradio UI) and entry point |
| `requirements.txt` | Python dependencies |
| `.env.example` | Template for your `.env` (holds `GROQ_API_KEY`) |
