# 通用计算机视觉异常检测论文审稿工具 (Universal CV Anomaly Detection Paper Reviewer)

[English Version](README.md)

一个跨代理（Agent-agnostic）、跨平台可移植的技术审稿工具，专为 **计算机视觉 (CV) 异常检测 (Anomaly Detection)** 和 **缺陷检测 (Defect Detection)** 研究而设计。该 Skill 提供了一个标准化的多 Agent 工作流，用于以高度严谨的方式分析、验证和归档学术论文。

---

## 🚀 核心特性

- **通用平台兼容性**：作为 **通用标准 Skill** 设计，兼容 Gemini CLI、Codex、Claude Code 以及任何具备 Shell 能力的 AI Agent。
- **统一的 15 字段分类法**：涵盖从现代 **零样本 (基于 CLIP)** 范式到传统监督分割的所有内容。
- **真相优先的审稿人**：内置“审稿人 Agent”，负责审核技术声明，去除营销词汇，并确保数据的地面性（Grounding）。
- **美观的持久化层**：使用 `openpyxl` 的高保真 Excel 后端，保留现有样式并自动格式化新条目。
- **双语策略**：技术数据严格保持英文以实现全球标准化，而叙述性字段（创新点/总结）可根据用户偏好配置（如中文）。
- **多 Agent 验证**：通过在线搜索自动交叉检查模型名称、发表刊物和指标。

---

## 📂 仓库结构

```text
.
├── SKILL.md              # 高层 AI Agent 逻辑与指令
├── INTERFACE.md          # 多 Agent 通信标准 (JSON 协议)
├── config.json           # 用户特定偏好 (语言, 研究重心)
├── references/
│   └── template.md       # 严格的分类法与编辑标准
├── scripts/
│   └── update_excel.py   # 无头持久化引擎 (Python)
└── README.md             # 标准 GitHub 文档
```

---

## 🛠️ 安装与设置

持久化层需要 Python 虚拟环境以确保稳定性。

### 1. 初始化环境
```bash
# 进入仓库
cd CV-Anomaly-Detection-Reviewer

# 创建并激活虚拟环境
python3 -m venv .venv
source .venv/bin/activate  # Windows 用户: .venv\Scripts\activate

# 安装依赖
pip install pandas openpyxl
```

### 2. 配置偏好
编辑 `config.json` 设置您期望的叙述语言和研究重心：
```json
{
    "narrative_language": "Chinese (Simplified)",
    "technical_focus": "Industrial AD/DD",
    "auto_logging": true
}
```

---

## 📖 使用标准

### 对于 Gemini CLI 用户
只需在聊天中输入 PDF 路径或论文标题。Skill 将根据 `SKILL.md` 中的逻辑自动触发。

### 对于开发者 (其他 Agent)
如果使用 Codex 或 Claude，请遵循 `INTERFACE.md` 中的协议。您的 Agent 应当：
1. 根据 `references/template.md` 解析论文。
2. 运行无头后端命令：
   ```bash
   ./.venv/bin/python scripts/update_excel.py "$(pwd)/CV_Anomaly_Detection_Master_List.xlsx" '<JSON_DATA>'
   ```

---

## ⚠️ 已知问题与解决策略

### PDF 阅读工具选择问题
**问题描述**：在多 Agent 环境中，不同的 host 模型对 PDF 的原生读取能力差异巨大。如果 Agent 盲目尝试使用内置工具阅读不支持的 PDF 格式，会导致上下文污染（Context Poisoning），产生 400 错误并破坏后续对话。

**当前策略 (Capability-Based Routing)**：
本 Skill 采用“前置能力判定”策略。在模型执行阅读指令前，先通过以下逻辑进行分流：
1. **模型识别**：检查当前 host 模型是否在**多模态支持白名单**中（如 `claude-*`, `gpt-4*`, `gemini-*`, `o1-*` 等）。
2. **动态路由**：
   - **支持白名单**：调用 Agent 原生读取工具。若意外失败，则静默回退至本地提取。
   - **不支持/未知模型 (如 `ark-*`, 各种代理 ID)**：绕过 API 尝试，直接调用本地系统工具（如 `pdftotext`, `pypdf` 或 OCR 脚本）提取文本。
3. **优势**：该方法避免了非法 content type 进入上下文，确保了审稿流程在国产模型或 API 代理环境下的健壮性。

> *我们一直在探索更优雅的探测方法，如果您有更好的动态工具发现建议，欢迎通过 Issue 或 PR 提出。*

---

## 📊 15 个标准字段
数据库 `CV_Anomaly_Detection_Master_List.xlsx` 使用以下严格模式：
1. **Paper Title** | 2. **Year/Venue** | 3. **Supervision Mode** | 4. **Task Type** | 5. **Target Domain** | 6. **Model Architecture** | 7. **Knowledge Source** | 8. **Dataset Source** | 9. **Image Metrics** | 10. **Pixel Metrics** | 11. **Efficiency** | 12. **Data Strategy** | 13. **Generalization** | 14. **Key Contributions** | 15. **Limitations & Summary**

---

## 📜 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。
