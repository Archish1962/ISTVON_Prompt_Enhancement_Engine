# ISTVON Prompt Enhancement Engine

> Transform natural language prompts into structured, safe, and enhanced AI interactions

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.28+-red.svg)](https://streamlit.io/)
[![Gemini AI](https://img.shields.io/badge/Gemini-AI-orange.svg)](https://ai.google.dev/)

---

**Click to play the video ↓**

https://github.com/user-attachments/assets/6dcb21e8-b558-48a1-9d92-d426cf72a27e

## What is ISTVON?

ISTVON is a structured framework for organizing AI prompts into six key components:

|  Component  | What it means                                     | Example                                             |
| :---------: | ------------------------------------------------- | --------------------------------------------------- |
| **I** | **Instructions** — Clear, actionable steps | `["Write a professional email", "Include CTA"]`   |
| **S** | **Sources** — Reference materials          | `{"documents": ["guide.pdf"]}`                    |
| **T** | **Tools** — Resources to use               | `["Email templates", "Grammar checker"]`          |
| **V** | **Variables** — Constraints & parameters   | `{"tone": "professional", "length": "200 words"}` |
| **O** | **Outcome** — Expected output format       | `{"format": "Email", "success_criteria": [...]}`  |
| **N** | **Notifications** — Progress tracking      | `{"milestones": ["Draft complete"]}`              |

---

## How It Works

```
┌────────────────┐
│  User Prompt   │
└───────┬────────┘
        │
        ▼
┌────────────────┐     ┌─────────────┐
│  Safety Broker │────►│   BLOCK     │ (Dangerous content)
└───────┬────────┘     └─────────────┘
        │
        ▼
┌────────────────┐     ┌─────────────┐
│ COSTAR Analysis│────►│  NEEDS_FIX  │ (Missing elements / risky)
└───────┬────────┘     └─────────────┘
        │                     │
        │                     ▼
        │              ┌─────────────┐
        │              │  Sanitize   │
        │              │  + Enhance  │
        │              └──────┬──────┘
        │                     │
        ▼                     ▼
┌────────────────┐     ┌─────────────┐
│ Context Detect │     │   ALLOW     │
└───────┬────────┘     └─────────────┘
        │
        ▼
┌────────────────┐
│ ISTVON Mapping │
│ (Rules + LLM)  │
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ Schema Validate│
└───────┬────────┘
        │
        ▼
┌────────────────┐
│  ISTVON JSON   │
└────────────────┘
```

---

## Key Features

### Safety Broker System

The engine includes a comprehensive safety system that:

- **Analyzes prompts** for potentially harmful content
- **Blocks dangerous requests** (high-risk content)
- **Sanitizes risky content** (medium-risk content gets redacted)
- **Allows safe prompts** to proceed with enhancement

```
BLOCK     → Completely harmful content (blocked)
NEEDS_FIX → Contains risky elements (sanitized + enhanced)
ALLOW     → Safe to proceed (enhanced)
```

### AI-Powered Enhancement

Uses Google Gemini AI to:

- Complete missing ISTVON elements
- Improve instruction clarity
- Suggest appropriate tools and variables
- *Falls back to rule-based logic if API unavailable*

---

## Quick Start

### 1. Clone & Setup

```bash
git clone <repository-url>
cd ISTVON_Prompt_Enhancement_Engine

# Create virtual environment
python -m venv venv

# Activate it
# Windows:
venv\Scripts\activate
# Linux/macOS:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure (Optional)

Create a `.env` file for API keys:

```env
# Optional - app works without it using rule-based fallback
GEMINI_API_KEY=your-api-key-here

# Optional - for database logging
ORACLE_DSN=localhost:1521/ORCL
ORACLE_USERNAME=
ORACLE_PASSWORD=
```

> **No API key?** No problem! The engine works based on rule validation but no response is generated.

### 3. Run the App

```bash
streamlit run app.py
```

Open http://localhost:8501 in your browser.

---

## Example

**Input Prompt:**

```
Write a professional email about product launch
```

**Safety Verdict:** `ALLOW`

**Generated ISTVON:**

```json
{
  "I": [
    "Write a professional email announcement",
    "Use professional business language",
    "Focus on actionable insights and clarity"
  ],
  "S": {},
  "T": [
    "Business frameworks",
    "Professional templates",
    "Industry standards"
  ],
  "V": {
    "tone": "professional",
    "complexity": "medium"
  },
  "O": {
    "format": "Business document",
    "delivery": "Professional format",
    "success_criteria": [
      "Professionally formatted",
      "Actionable recommendations"
    ]
  },
  "N": {}
}
```

---

## Running Tests

```bash
# Run all tests
python -m pytest tests/ -v

# Run specific test
python -m pytest tests/test_broker.py -v
```

---

## Project Structure

```
ISTVON_Prompt_Enhancement_Engine/
├── app.py                 # Main Streamlit application
├── config.py              # Configuration (loads from .env)
├── database.py            # Oracle database integration
├── requirements.txt       # Python dependencies
│
├── engine/                # Core processing modules
│   ├── broker.py          # Safety broker (ALLOW/BLOCK/NEEDS_FIX)
│   ├── llm_mapper.py      # Gemini AI integration
│   ├── context_analyzers.py
│   ├── pattern_matchers.py
│   ├── completion_rules.py
│   └── istvon_schema.py
│
├── utils/                 # Utility modules
│   ├── json_logger.py     # Rule engine decision logging
│   ├── json_parser.py
│   ├── helpers.py
│   └── validators.py
│
├── tests/                 # Test suite
│   ├── test_broker.py
│   ├── test_llm_mapper.py
│   └── test_rules.py
│
└── exports/               # Generated JSON exports
```

---

## Configuration Options

| Environment Variable | Description                 | Required           |
| -------------------- | --------------------------- | ------------------ |
| `GEMINI_API_KEY`   | Google Gemini API key       | No (uses fallback) |
| `ORACLE_DSN`       | Oracle DB connection string | No                 |
| `ORACLE_USERNAME`  | Database username           | No                 |
| `ORACLE_PASSWORD`  | Database password           | No                 |

---

## Troubleshooting

| Issue                             | Solution                                        |
| --------------------------------- | ----------------------------------------------- |
| `ModuleNotFoundError: oracledb` | Run `pip install oracledb`                    |
| Port 8501 already in use          | Use `streamlit run app.py --server.port 8502` |
| Gemini API 403 error              | Check API key and quota                         |
| "No analytics data"               | Database not configured (this is fine!)         |

---
