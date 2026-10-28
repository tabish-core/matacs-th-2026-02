# AI Proposal Triage

**Name:** Tabish Khalil  
**Reference:** MATACS-TH-2026-02  
**Time spent:** Approximately 2 hours, including prototype development.

## Project Files

- [WRITEUP.md](WRITEUP.md): System design, scoring, human review, and risks.
- [PROMPTS.md](PROMPTS.md): AI extraction prompt and JSON structure.
- [ranking.json](ranking.json): Results for proposals A–E.
- [Process diagram](diagram/flow-diagram.png): Proposal review flow.
- [Dashboard prototype](protoype/index.html): Interactive HTML dashboard.
- [Favicon](protoype/favicon.svg): Dashboard icon.

## AI Tools Used

- **ChatGPT:** Discussed the approach and scoring rules. I questioned the suggestion to send PDFs directly to an LLM and chose to separate AI extraction from code-based scoring.
- **Gemini:** Helped improve the wording in the write-up and extraction prompt.
- **Mermaid.js:** Used to create the process diagram.
- **AI assistant:** Helped scaffold the HTML dashboard and favicon.