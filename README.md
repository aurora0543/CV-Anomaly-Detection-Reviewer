# Universal CV Anomaly Detection Paper Reviewer

[中文版 (Chinese Version)](README_zh.md)

[![Status](https://img.shields.io/badge/Status-Universal_Standard_Skill-blue.svg)](https://github.com/aurora0543/CV-Anomaly-Detection-Reviewer)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.8+-green.svg)](https://www.python.org/)

An agent-agnostic, platform-portable technical review tool designed for **Computer Vision (CV) Anomaly Detection** and **Defect Detection** research. This skill provides a standardized, multi-agent workflow to analyze, verify, and archive academic papers with high rigor.

---

## 🚀 Key Features

- **Universal Platform Compatibility**: Designed as a **Universal Standard Skill**, compatible with Gemini, Codex, Claude Code, and any AI agent with shell capabilities.
- **Unified 15-Field Taxonomy**: Covers everything from modern **Zero-shot (CLIP-based)** paradigms to traditional supervised segmentation.
- **Truth-First Peer Reviewer**: Built-in "Reviewer Agent" that audits technical claims, removes buzzwords, and ensures data grounding.
- **Aesthetic Persistence**: High-fidelity Excel backend using `openpyxl` that preserves existing styles and automatically formats new entries.
- **Dual-Language Policy**: Technical data is kept strictly in English for global standardization, while narrative fields (Innovations/Summary) are user-configurable (e.g., Chinese).
- **Multi-Agent Verification**: Automatically cross-checks model names, venues, and metrics via online search.

---

## 📂 Repository Structure

```text
.
├── SKILL.md              # High-level AI Agent logic and mandates
├── INTERFACE.md          # Multi-agent communication standard (JSON Protocol)
├── config.json           # User-specific preferences (Language, Focus)
├── references/
│   └── template.md       # Rigid taxonomy and editorial standards
├── scripts/
│   └── update_excel.py   # Headless persistence engine (Python)
└── README.md             # Standard GitHub documentation
```

---

## 🛠️ Setup & Installation

The persistence layer requires a Python virtual environment to ensure stability.

### 1. Initialize Environment
```bash
# Navigate to the repo
cd CV-Anomaly-Detection-Reviewer

# Create and activate virtual environment
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install pandas openpyxl
```

### 2. Configure Your Preference
Edit `config.json` to set your desired narrative language and research focus:
```json
{
    "narrative_language": "Chinese (Simplified)",
    "technical_focus": "Industrial AD/DD",
    "auto_logging": true
}
```

---

## 📖 Usage Standards

### For Gemini CLI Users
Just drop a PDF or a Paper Title in the chat. The skill will automatically trigger based on the logic in `SKILL.md`.

### For Developers (Other Agents)
If using Codex or Claude, follow the protocol in `INTERFACE.md`. Your agent should:
1. Parse the paper based on `references/template.md`.
2. Run the headless backend command:
   ```bash
   ./.venv/bin/python scripts/update_excel.py "$(pwd)/CV_Anomaly_Detection_Master_List.xlsx" '<JSON_DATA>'
   ```

---

## ⚠️ Known Issues & Strategy

### PDF Reading Tool Selection Issue
**Problem**: In multi-agent environments, host models have vastly different native PDF reading capabilities. Blindly attempting to use built-in tools for unsupported PDF formats leads to **Context Poisoning**, resulting in 400 errors that break subsequent turns.

**Current Strategy (Capability-Based Routing)**:
This skill implements a "pre-execution capability check" to branch logic before any read attempt:
1. **Model Detection**: Host model IDs are matched against a **Multimodal Support Allowlist** (e.g., `claude-*`, `gpt-4*`, `gemini-*`, `o1-*`).
2. **Dynamic Routing**:
   - **Allowlisted Models**: Call native Agent read tools. Silently fall back to local extraction if unexpected terminal errors occur.
   - **Unsupported/Unknown Models (e.g., `ark-*`, custom proxy IDs)**: Bypass API attempts entirely and use local system tools (e.g., `pdftotext`, `pypdf`, or OCR scripts) to extract text.
3. **Benefit**: This prevents invalid content types from entering the context, ensuring robust performance even with models behind proxies or those without native document support.

> *We are constantly looking for more elegant ways to perform dynamic tool discovery. If you have suggestions for better implementation, feel free to open an Issue or PR.*

---

## 📊 The 15 Standard Fields
The database `CV_Anomaly_Detection_Master_List.xlsx` uses the following rigid schema:
1. **Paper Title** | 2. **Year/Venue** | 3. **Supervision Mode** | 4. **Task Type** | 5. **Target Domain** | 6. **Model Architecture** | 7. **Knowledge Source** | 8. **Dataset Source** | 9. **Image Metrics** | 10. **Pixel Metrics** | 11. **Efficiency** | 12. **Data Strategy** | 13. **Generalization** | 14. **Key Contributions** | 15. **Limitations & Summary**

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contribution

Contributions are welcome! Please ensure that any changes to the taxonomy are reflected in both `template.md` and `INTERFACE.md` to maintain universal compatibility.
