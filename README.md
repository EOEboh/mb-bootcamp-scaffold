# Bootcamp Scaffold

The base template for every project in the **Build 10 AI Projects in 30 Days** bootcamp.

All 10 projects share this identical folder structure and the same `ai/ollama.go` client.
This is intentional, you build muscle memory, not boilerplate.

---

## Folder Structure

```
.
├── main.go              # HTTP server + route registration
├── handlers/
│   └── handlers.go      # Request handlers (one file per project feature)
├── ai/
│   └── ollama.go        # ALL Ollama communication lives here — nowhere else
├── templates/
│   └── index.html       # Server-rendered HTML (HTMX-powered)
├── static/
│   └── style.css        # Shared design tokens + project-specific styles
├── go.mod
├── .env.example
└── Makefile
```

**The rule:** AI communication only lives in `ai/`. Handlers never call `http.Post` to Ollama directly. This makes it trivially easy to swap providers.

---

## Prerequisites

- [Go 1.22+](https://go.dev/dl/)
- [Ollama](https://ollama.com) installed and running (`ollama serve`)

## Quick Start

```bash
# 1. Clone or use this as a GitHub template
git clone https://github.com/YOUR_GITHUB_USERNAME/bootcamp-scaffold my-project
cd my-project

# 2. Replace module name in go.mod
# Change: github.com/YOUR_GITHUB_USERNAME/bootcamp-scaffold
# To:     github.com/YOUR_GITHUB_USERNAME/my-project

# 3. Pull the required models (first time only)
make setup

# 4. Run
make run
# → http://localhost:8080
```

---

## The ai/ Package API

Every project uses exactly two functions:

```go
// Non-streaming — returns the full response as a string
response, err := ai.Chat(ai.DefaultModel, []ai.Message{
    {Role: "system", Content: "You are a helpful assistant."},
    {Role: "user",   Content: "Hello!"},
})

// Streaming — calls onChunk for each token as it arrives
err := ai.ChatStream(ai.DefaultModel, messages, func(chunk string) error {
    fmt.Println(chunk) // do something with each token
    return nil         // return error to abort stream early
})
```

---

## Stack

| Layer      | Technology               |
|------------|--------------------------|
| Backend    | Go 1.22+                 |
| LLM        | Ollama (local)           |
| Frontend   | HTMX + Vanilla JS        |
| Database   | SQLite (projects 4, 5, 10)|
| Streaming  | SSE / WebSockets         |
| Deploy     | Docker (project 10)      |

---

## Projects Built on This Scaffold

| # | Project                  | Key Addition                        |
|---|--------------------------|-------------------------------------|
| 1 | AI Chat Interface        | SSE streaming                       |
| 2 | Code Snippet Explainer   | System prompts + code models        |
| 3 | Smart Text Summarizer    | HTMX + prompt engineering           |
| 4 | AI Resume Analyzer       | File uploads + PDF extraction       |
| 5 | AI Writing Assistant     | SQLite + contextual AI commands     |
| 6 | Image Caption Generator  | Multimodal (LLaVA vision model)     |
| 7 | API Doc Generator        | Multi-pass prompting + export       |
| 8 | Meeting Notes Summarizer | Whisper audio pipeline              |
| 9 | AI Agent with Tool Use   | ReAct pattern + tool dispatch       |
|10 | Full-Stack AI SaaS       | JWT + multi-tenancy + Docker        |