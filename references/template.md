# CV Anomaly Detection Review Template (Rigid Standard)

## 1. Editorial Policy: "The Truth First"
- **Strict Objectivity**: Delete adjectives like "incredible", "revolutionary", "state-of-the-art". Use "Improvement of X% over Y".
- **Zero Fabrication**: If a specific value (e.g., FPS) is not in the paper, you MUST write **"Not Reported"**. NEVER guess based on model type.
- **Normalization**: Year/Venue MUST be **"Full Venue Name (YYYY)"**. No abbreviations.

## 2. Rigid Taxonomy & Authorized Values
Any AI agent MUST select from these authorized values. Do NOT create new categories unless absolutely necessary.

| Field | Authorized Values / Strict Formats |
| :--- | :--- |
| **Supervision Mode** | `Supervised`, `Unsupervised`, `Zero-shot`, `Semi-supervised`, `Weakly-supervised`, `Self-supervised`. |
| **Task Type** | `Anomaly Detection`, `Localization`, `Segmentation`, `Classification`, `Out-of-Distribution`. |
| **Knowledge Source** | `Vision-only`, `Vision-Language`, `CLIP-guided`, `Synthetic-driven`, `Foundation-Model`. |
| **Modality** | `RGB`, `Grayscale`, `Multimodal (Vision+X)`. |
| **Dataset Source** | Provide exact name (e.g., `MVTec AD`, `VisA`, `Private Dataset`). No fixed list required. |
| **Metrics** | Use professional notation. Separated by Image-level (e.g., `AUROC`) and Pixel-level (e.g., `AUPRO`). |
| **Industrial Efficiency**| Concrete numbers (e.g., `15.4 FPS`, `2.1M Params`) or `Not Reported`. |

## 3. Rigidity Categorization
- **Closed Fields (Must select from list)**: `Supervision Mode`, `Task Type`, `Modality`, `Knowledge Source`.
- **Open Fields (Must be descriptive/accurate)**: `Paper Title`, `Year/Venue`, `Target Domain`, `Model Architecture`, `Dataset Source`, `Metrics`, `Key Contributions`.

---

## 3. Example: Zero-shot AD (MoECLIP) - Rigid Output

**Paper Title**: MoECLIP: Patch-Specialized Experts for Zero-shot Anomaly Detection
**Year/Venue**: IEEE/CVF Conference on Computer Vision and Pattern Recognition (2026)
**Supervision Mode**: Zero-shot
**Task Type**: Anomaly Detection
**Target Domain**: General Industrial Objects (MVTec AD / VisA)
**Model Architecture**: CLIP ViT-L/14 Backbone + MoE LoRA Experts
**Knowledge Source**: CLIP-guided (Vision-Language alignment)
**Dataset**: MVTec AD
**Image Metrics**: Image AUROC: 94.5%
**Pixel Metrics**: Pixel AUROC: 97.2%; AUPRO: 92.1%
**Efficiency**: Not Reported
**Data Strategy**: Compositional Prompt Ensembles (CPE)
**Generalization**: High (Evaluated on 15+ unseen industrial categories)
**Key Contributions**: Patch-specialized routing to prevent uniform adaptation bias.
**Limitations**: Increased inference latency due to MoE routing mechanism.
