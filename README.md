# 🤖 AI Meeting Notes Generator

![n8n](https://img.shields.io/badge/n8n-Automation-orange)
![Ollama](https://img.shields.io/badge/Ollama-Local%20LLM-blue)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Storage-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

> Automatically transform raw meeting transcripts into structured, actionable notes using a local LLM — privacy-first AI automation.

---

## 📖 Overview

**AI Meeting Notes Generator** is a privacy-focused automation pipeline that converts unstructured meeting transcripts into clean, structured notes using **Ollama** (local LLM) — no data ever leaves your machine.

It extracts summaries, key points, decisions, and action items with owners and deadlines, then stores everything in **Google Sheets** via an **n8n** workflow.

This project demonstrates real-world AI automation using workflow orchestration, REST APIs, JavaScript processing, and local LLM inference.

---

## 🏗️ Architecture

```
Postman / API Client
        │
        ▼
┌─────────────────────┐
│  Webhook Trigger    │  ← n8n HTTP endpoint
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Ollama LLM Node    │  ← Local model inference
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  JavaScript Node    │  ← Parse & format output
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│  Google Sheets      │  ← Persist structured notes
└─────────────────────┘
```

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔗 Webhook Trigger | Accepts meeting text via HTTP POST from any API client |
| 🧠 Local LLM Processing | Runs Ollama on-device — zero data sent to external AI services |
| 📋 Structured Extraction | Automatically pulls summaries, key points, decisions, and action items |
| 👤 Owner & Deadline Parsing | Identifies who owns each task and when it's due |
| ⚙️ JS Transformation Layer | Cleans and normalizes raw LLM output into consistent structured data |
| 📊 Google Sheets Storage | Persists every meeting's notes to a spreadsheet for easy access and sharing |
| 🔒 Privacy-First | No cloud AI dependency — fully self-hosted pipeline |

---

## ⚙️ Tech Stack

| Tool | Role |
|---|---|
| [n8n](https://n8n.io/) | Workflow automation & orchestration |
| [Ollama](https://ollama.com/) | Local LLM inference (Llama 3, Mistral, etc.) |
| JavaScript | Data transformation inside n8n |
| Google Sheets API | Cloud spreadsheet storage |
| Postman | API testing & trigger simulation |

---

## 📥 Input Format

Send a `POST` request to your n8n webhook with the following JSON body:

```json
{
  "meeting_text": "John will finish login page by Friday. Sarah confirmed API is ready. Mark will review designs tomorrow."
}
```

---

## 📤 Output Format

The pipeline extracts and stores the following structured fields:

```json
{
  "summary": "The team reviewed product progress. Key milestones were confirmed and next steps assigned.",
  "key_points": [
    "API integration confirmed ready",
    "Login page development in progress",
    "Design review scheduled for tomorrow"
  ],
  "decisions": [
    "Proceed with current API — no changes needed"
  ],
  "action_items": [
    { "task": "Finish login page", "owner": "John", "deadline": "Friday" },
    { "task": "Review designs",    "owner": "Mark", "deadline": "Tomorrow" }
  ]
}
```

---

## 🚀 Getting Started

### Prerequisites

- [n8n](https://docs.n8n.io/hosting/) running locally or self-hosted
- [Ollama](https://ollama.com/download) installed with at least one model pulled
- Google account with Sheets API credentials configured in n8n

### 1. Pull an Ollama Model

```bash
ollama pull llama3
# or
ollama pull mistral
```

### 2. Import the n8n Workflow

1. Open your n8n instance
2. Go to **Workflows → Import**
3. Upload `workflow.json` from this repo
4. Configure your **Google Sheets credentials** in the Sheets node

### 3. Activate the Workflow

Toggle the workflow to **Active** in n8n to enable the webhook endpoint.

### 4. Test with Postman

```
POST http://localhost:5678/webhook/meeting-notes
Content-Type: application/json

{
  "meeting_text": "Your raw meeting transcript here..."
}
```

---

## 🗂️ Project Structure

```
ai-meeting-notes/
├── workflow.json           # n8n workflow export
├── README.md               # This file
└── examples/
    ├── sample_input.json   # Example meeting transcript
    └── sample_output.json  # Example structured output
```

---

## 🛠️ Customization

- **Swap the model** — Change the Ollama model to any you have pulled (e.g. `phi3`, `gemma`, `mistral`)
- **Edit the prompt** — Modify the system prompt in the Ollama node to extract different fields or adjust tone
- **Change the destination** — Replace Google Sheets with Notion, Airtable, a database, or a Slack message

---

## 📌 Use Cases

- Engineering standups & sprint planning
- Client calls & discovery meetings
- Weekly team syncs
- Interview debriefs
- Board or investor meetings

---

## 🤝 Contributing

Contributions welcome! Open issues or PRs for:

- New output destinations (Notion, Slack, email)
- Improved prompts for better extraction accuracy
- Audio input support via Whisper transcription

---

## 📄 License

MIT License — free to use, modify, and distribute.
