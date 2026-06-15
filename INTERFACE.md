# Universal AI Agent Interface: CV Anomaly Detection Skill

This document defines the universal standard for any AI agent (Gemini, Codex, Claude Code, etc.) to interact with this skill. By following this interface, the skill remains platform-agnostic.

## 1. Skill Capabilities
- **Domain**: Computer Vision (CV) Anomaly & Defect Detection.
- **Acquisition**: Local PDFs are routed by host-model allowlist *before* any read attempt (see `SKILL.md` Step 0.5) to avoid context errors. Document-capable models use native read; others use local extraction (`pdftotext` / `pypdf` / OCR).
- **Analysis**: 15-field technical extraction with "Truth-First" policy.
- **Persistence**: High-fidelity Excel logging (`CV_Anomaly_Detection_Master_List.xlsx` in current working directory).

## 2. Standard Interaction Protocol (For Agents)

### A. Information Extraction
Any agent MUST extract information according to the `references/template.md` standard.
- **Language**: Logic and persistence MUST be in English.
- **Aesthetics**: Avoid fluff/buzzwords. Use "Not Reported" for missing data.

### B. Backend Execution (The "Headless" Engine)
The persistence layer is handled by a standalone Python service. Any agent capable of running shell commands should execute:
```bash
./.venv/bin/python scripts/update_excel.py "$(pwd)/CV_Anomaly_Detection_Master_List.xlsx" '<JSON_Payload>'
```
- **Input**: A JSON string containing the 15 standard keys.
- **Exit Code 0**: Success.
- **Exit Code 2**: Header mismatch (Manual intervention or new file required).

## 3. Deployment & Portability
- **Platform Agnostic**: Does not rely on platform-specific APIs. Uses standard Python + Openpyxl.
- **Zero Hardcoding**: All paths must be relative to the skill's root directory.

## 4. The 15 Standard Fields (Persistence Layer)
1. Paper Title | 2. Year/Venue | 3. Supervision Mode | 4. Task Type | 5. Target Domain | 6. Model Architecture | 7. Knowledge Source | 8. Dataset | 9. Image-level Metrics | 10. Pixel-level Metrics | 11. Industrial Efficiency | 12. Data Strategy | 13. Generalization | 14. Key Contributions | 15. Limitations & Summary
