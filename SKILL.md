---
name: cv-anomaly-detection-universal-reviewer
description: A platform-agnostic CV Anomaly Detection tool with interactive onboarding, rigid taxonomies, and truth-first auditing.
---

# Universal CV Anomaly Detection Paper Reviewer

## I. Interaction Mandates

### 1. Interactive Onboarding (First-Run Logic)
If this is the user's first time using the skill (or if `config.json` is missing), the agent MUST call `ask_user` to gather the following preferences:
- **Narrative Language**: Preference for Fields 14 & 15 (English, Chinese, etc.).
- **Technical Focus**: Focus on Industrial AD, Medical AD, or General CV.
- **Auto-Logging**: Enable/Disable automatic Excel updates.

### 2. Dual-Language Policy
- **Technical Fields (1-13)**: Strictly **English**.
- **Narrative Fields (14-15)**: Based on User Preference (from `config.json`).

### 3. Truth-First Audit
Before any output, act as a **Senior Scientific Peer Reviewer**:
- **Filtering**: Remove marketing fluff.
- **Grounding**: If a field is absent, mark as **"Not Reported"**. NEVER fabricate data.

## II. Operational Workflow

### Step 0: Initialize (Check Config)
- Check for `config.json` in the skill root.
- If missing: Use `ask_user` to set preferences and `write_file` to save `config.json`.

### Step 0.5: PDF Acquisition (Capability-Based Routing)
Before reading local PDFs, route by host model to prevent context-breaking errors.

- **Check Model**: Match host model ID against the **Document-Capable Allowlist**: `claude-*`, `gpt-*`, `o[1-4]-*`, `gemini-*`.
- **Route**:
  - **Allowlisted**: Use native `read_file` (or agent equivalent). Fall back to local extraction silently if it fails.
  - **Others (e.g., `ark-*`, proxies)**: Use local extraction (`pdftotext`, `pypdf`, or OCR) immediately.
- **Constraint**: Strictly use the allowlist. Do not probe unknown models with native reads.

### Step 1: Analyze & Verify
- Extract 15 fields defined in `references/template.md`.
- Use `google_web_search` to verify technical claims or venue rankings.

### Step 2: Dual-Mode Output
- **UI Output**: Display the detailed 15-field analysis in the chat.
- **Persistence**: Append data to `CV_Anomaly_Detection_Master_List.xlsx` in the **current working directory (CWD)** using:
  `./.venv/bin/python scripts/update_excel.py "$(pwd)/CV_Anomaly_Detection_Master_List.xlsx" '<JSON>'`

## III. Persistence Standard
- **Database Location**: Always create or update `CV_Anomaly_Detection_Master_List.xlsx` in the **user's current working directory** to ensure easy access.
- **Data Integrity**: Append only. Preserve styles/sheets. Fill blanks with "Not Reported".

## IV. The 15 Standard Fields
| # | Field Name | Language | Format |
|---| --- |---| --- |
| 1-13 | Technical Data | English | Rigorous Academic Standard |
| 14 | **Key Contributions** | **Configurable** | Narrative Summary |
| 15 | **Limitations & Summary** | **Configurable** | Narrative Summary |

## Resources
- [INTERFACE.md]: Multi-platform protocol.
- [template.md]: Rigid taxonomies and editorial rules.
- [README.md]: Portable environment setup.
