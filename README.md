# EasyPDF MCP server

**Your AI says what to change. EasyPDF writes it into the PDF.**

EasyPDF is a remote [Model Context Protocol](https://modelcontextprotocol.io) server that lets ChatGPT, Claude, Cursor, VS Code and any MCP client edit real PDF files. Ask your assistant to fix a date, an amount, a name or a typo: EasyPDF rewrites the text inside the original PDF, keeps the fonts and the layout, and shows a before/after preview in the chat. It also compresses, merges, splits, converts and translates PDFs.

https://github.com/user-attachments/assets/7eae0cd7-aea4-478e-b06c-91234c613fa4

*21-second demo: one request, two edits written into the original invoice (fonts and layout kept), then the download. Also available as a [GIF preview](docs/demo.gif).*

- Website and setup guide: https://www.easypdf.fr/ai-assistants
- Claude directory listing: https://claude.ai/directory/easypdf
- No API key, nothing to install: it is a hosted server.

## Endpoints

| Client | URL | Authentication |
|---|---|---|
| Claude, Claude Code, Cursor, VS Code, other MCP clients | `https://www.easypdf.fr/mcp` | OAuth 2.1 (Google sign-in, dynamic client registration) |
| ChatGPT (apps / developer mode) | `https://www.easypdf.fr/chatgpt/mcp` | None |

Transport: Streamable HTTP.

## Install

### Claude (claude.ai, desktop and mobile apps)

Open the listing at https://claude.ai/directory/easypdf, click **Connect** and confirm with your Google account.

### Claude Code

The [EasyPDF plugin](https://github.com/Lorenzino69/easypdf-claude-plugin) adds the server plus skills that upload your local files and save the results next to them:

```bash
claude plugin marketplace add Lorenzino69/easypdf-claude-plugin
claude plugin install easypdf@easypdf
```

Or the server alone. Keep the trailing slash: without it, Claude Code rejects the OAuth resource.

```bash
claude mcp add --transport http easypdf https://www.easypdf.fr/mcp/
```

### Cursor

`~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "easypdf": { "url": "https://www.easypdf.fr/mcp" }
  }
}
```

### VS Code

`.vscode/mcp.json`

```json
{
  "servers": {
    "easypdf": { "type": "http", "url": "https://www.easypdf.fr/mcp" }
  }
}
```

### ChatGPT

The EasyPDF app is under review for the ChatGPT app directory. Until then, add it in developer mode on chatgpt.com:

1. In Settings, open Apps, then Advanced settings, and turn on developer mode.
2. Create an app named EasyPDF with the URL `https://www.easypdf.fr/chatgpt/mcp` and no authentication.
3. In a chat, attach a PDF and pick EasyPDF from the + menu.

## Tools

| Tool | What it does |
|---|---|
| `edit_pdf_text` | Replaces text in the original PDF (dates, amounts, names, typos) with the same font, size and position. Returns a before/after preview widget (MCP Apps) and reports each replacement, with the closest match when a text is not found. |
| `get_upload_link` | Gives the user a one-hour upload link when the client cannot pass a file to the server. |
| `compress_pdf` | Reduces the file size. |
| `merge_pdfs` | Merges several PDFs in order. |
| `split_pdf` | Splits a PDF into files of N pages. |
| `extract_pages` | Keeps only the listed pages. |
| `rotate_pdf` | Rotates all or some pages by 90, 180 or 270 degrees. |
| `add_page_numbers` | Adds page numbers. |
| `watermark_pdf` | Adds a text watermark. |
| `protect_pdf` | Adds a password. |
| `unlock_pdf` | Removes a known password. |
| `convert_pdf_to_word` | Converts to .docx. |
| `convert_pdf_to_excel` | Converts to .xlsx. |
| `convert_pdf_to_powerpoint` | Converts to .pptx. |
| `translate_pdf` | Translates the document and keeps the layout. |
| `generate_pdf` | Creates a PDF from a text prompt. |
| `chat_with_pdf` | Answers a question about a PDF, with source references. |

The ChatGPT endpoint exposes the tools that work on ChatGPT attachments: `edit_pdf_text`, `compress_pdf`, `merge_pdfs`, `split_pdf`, `extract_pages`, `rotate_pdf`, `add_page_numbers`, `protect_pdf` and `unlock_pdf`.

Every tool returns a download link valid for one hour.

## Example prompts

- "In this invoice, change the due date 17/10/2026 to 31/10/2026 and keep the layout."
- "Fix the spelling mistakes in my resume directly in the PDF."
- "Replace "Example Ltd" with "Example Consulting Inc." everywhere in this contract."
- "Compress this PDF under 2 MB so I can upload it to an online form."

## Privacy

- Uploaded PDFs are used during processing and deleted within one hour.
- Output files are available through a download link for one hour, then deleted.
- Text edits run without any AI model on the EasyPDF side: your assistant decides what to change, EasyPDF applies the replacements exactly.
- EasyPDF receives the file and the tool arguments, never the rest of your conversation.

Privacy policy: https://www.easypdf.fr/confidentialite

## Support

Questions or bugs: open an issue in this repository or write to easypdfbug@gmail.com.

This repository documents the hosted EasyPDF MCP server (the server itself is closed source). Documentation is released under the MIT license.
