# ENGR 4170 — Lab 5: Foundation Models and Small Language Models

## Problem
This project investigates whether small pretrained foundation models can support practical mechanical-maintenance questions. It combines zero-shot inspection of steel-surface images with a local Retrieval-Augmented Generation (RAG) assistant for bearings, pumps, belts and rotating machinery. The application is a classroom demonstration, not a certified maintenance tool.

## Foundation models

| Model | Role |
|---|---|
| `openai/clip-vit-base-patch32` | Zero-shot image classification of steel-surface defects |
| `HuggingFaceTB/SmolLM2-360M-Instruct` | Local small-LLM text generation |
| `sentence-transformers/all-MiniLM-L6-v2` | 384-dimensional text embeddings and semantic search |

The models are used for inference. No task-specific model training is performed in Lab 5.

## Knowledge base
`knowledge/maintenance_manual.txt` contains the **same six maintenance passages used in the notebook**: excessive bearing vibration; overheating; pump cavitation; shaft misalignment; belt slippage; and energy isolation/lockout. The passages come from the course laboratory manual, not from a manufacturer-specific maintenance manual. The source-aware Gradio modification presents retrieved passages alongside each generated answer.

## Architecture
`User question → all-MiniLM-L6-v2 embedding → cosine-similarity top-2 retrieval → context prompt → SmolLM2-360M-Instruct → generated answer + retrieved passages`

The source-aware Gradio application is **implemented and demonstrated inside `lab05.ipynb` (Sections 37–38)**. As confirmed with the course TA, no separate `app.py` is necessary for this submission. The notebook implementation does not add automatic relevance thresholds or refusal logic.

## Running the notebook
1. Open `lab05.ipynb` in Google Colab and select a GPU runtime when available.
2. Upload your original `NEU.zip` and `sample_scratch.bmp` to the Colab session root (`/content/`). The notebook expects these exact filenames. The sample is the TA-reviewed file used in the lab; the experiment and its interpretation have been preserved without changes.
3. Run the existing notebook sequentially. The first cell unpacks the six-class NEU dataset, then later cells load the models, perform the experiments, and display the code-generated tables.
4. Model files are downloaded on first run; no paid API key is needed. The original saved notebook already contains executed outputs.
5. Section 39 writes a separate `results/section39_evaluation.csv` in Colab containing full generated answers. The package includes the four-column summary extracted from the saved notebook output; rerun the cell and download the runtime CSV if the full answer text is also needed.

**Data availability:** The larger NEU image archive and single sample BMP are **not bundled** in this GitHub-ready package. This follows the submitted notebook's existing upload-based workflow. They must be supplied separately to reproduce the image experiments.

## Launching the application
Run the notebook in Colab through Section 38. It launches the original and modified Gradio interfaces directly from the notebook. Gradio share links in saved outputs are temporary and should not be treated as permanent hosting.

## Evaluation
The notebook records five supported maintenance questions, plus two unsupported questions about **bearing-housing bolt torque** and **motor insulation resistance in megaohms**.

- All five supported questions retrieved the expected passage in Section 34 and Section 39; the simple keyword check marked their answers **"Likely - verify"**, which is not independent factual validation.
- Both unsupported questions failed the system's explicit missing-information check in Section 39.
- Section 36 compares a standalone LLM response about pump cavitation against the RAG response. The RAG response was more concise and aligned with the provided maintenance passage.
- `results/` contains the code-generated prompt comparison, supported retrieval, LLM/RAG comparison, and **four-column Section 39 summary** extracted from the saved notebook outputs. These are preserved experimental outputs, not fabricated or newly rerun model results. The full Section 39 generated-answer strings are available through Colab's own saved CSV (see above).

## Failure cases
1. **Missing specification / failure to abstain.** When asked for bearing-housing bolt torque, the original assistant responded with irrelevant bearing-vibration information instead of explicitly indicating insufficient knowledge. The additional insulation-resistance query also failed the missing-information check. **Primary causes:** incomplete knowledge base and generation behavior.
2. **Unsupported causal relationship.** In an earlier pump-vibration response, the model linked impeller damage to excessive bearing vibration, a causal relationship not established by the retrieved passages. **Primary cause:** generation error despite relevant retrieval.

These are observed limitations, not corrected behavior; the Section 38 modification improves traceability by displaying sources but does not guarantee accurate answers.

## Engineering conclusion
Do **not** deploy the system as an autonomous industrial maintenance authority. Industrial adoption would require verified manufacturer specifications, systematic retrieval evaluation, reliable abstention on unsupported questions, validation of technical accuracy, version-controlled references, access/security controls, and human approval of safety-relevant decisions.

## Repository contents

```text
lab05/
├── lab05.ipynb
├── requirements.txt
├── knowledge/
│   └── maintenance_manual.txt
├── figures/
│   └── .gitkeep
├── results/
│   ├── section13_prompt_comparison.csv
│   ├── section34_supported_retrieval.csv
│   ├── section36_llm_vs_rag_comparison.csv
│   └── section39_evaluation_summary.csv
└── README.md
```

The notebook contains the images/visualizations inline. The `figures/` folder is empty except for `.gitkeep` to preserve the manual's suggested repository structure. The separate `app.py` listed in the printed manual is omitted based on the TA's confirmation that the in-notebook Gradio application satisfies the requirement.
