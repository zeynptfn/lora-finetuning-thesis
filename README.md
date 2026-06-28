TurkMotifTesettür — SDXL LoRA for Turkish Motif Modest Fashion

A domain-specific LoRA (Low-Rank Adaptation) model trained on Stable Diffusion XL (SDXL)
to generate modern modest-fashion designs inspired by traditional Turkish motifs
(Ottoman tulip, Iznik tile, rumi, and saz-leaf patterns).

Overview

General text-to-image models do not recognize specific Turkish ornamental motifs and tend
to produce generic floral patterns. This project trains a custom LoRA adapter — activated
through the trigger token turkmotiftesettur — that enables the generation of culturally
specific modest-fashion designs.

The full pipeline (dataset preparation, training, inference, and quantitative evaluation)
is documented in a single notebook: TurkMotifTesettur_SDXL_LoRA.ipynb.

Notebook Contents

SectionDescription1. Environment SetupDrive mount, GPU check, dependencies, Kohya sd-scripts2. Base Model DownloadSDXL base 1.0 from Hugging Face3. Project Folder StructureDirectory layout4. Dataset PreparationKohya repeat format + caption enrichment5. TOML Dataset ConfigPer-subset repeat / class_tokens settings6. Sample PromptsTraining-time preview prompts7. Model TrainingFour experiments: Debug → v1 → Exp01 → Exp028. InferenceImage generation with the trained LoRA9. TroubleshootingDependency version fixes10. EvaluationCLIP, DINOv2, FID/KID, LPIPS11. ResultsAggregation and visualization

Training Experiments

Stagenetwork_dimresolutiontext encoderdurationresultDebug8768on1 epochdid not learn (capacity too low)v1161024on10 epochCUDA out of memory (T4)Exp0116768off4 epochweak motif identityExp02321024on1800 stepssuccess

Technical Details


Base Model: Stable Diffusion XL Base 1.0
Method: LoRA (network_dim=32, network_alpha=16)
Resolution: 1024×1024
Training: 1800 steps, UNet + text encoder, bf16
Framework: Kohya sd-scripts
Platform: Google Colab (A100 GPU)


Evaluation Metrics

MetricMeasuresBetterCLIP Scoretext–image alignmentHigherDINOv2 Similarityresemblance to real dataHigherDINOv2 / LPIPS Diversityoutput variety (no overfitting)HigherFID / KIDdistribution distanceLower

Two CLIP scores are computed per image: alignment with the prompt, and alignment with a
fixed "Turkish motif" reference text. The latter quantifies how strongly the motif
identity was learned. High diversity values provide evidence that the model generalizes
rather than memorizing training images.

How the Model Learns

The model is not given copies of the target outputs. It receives image–caption pairs
in three categories (motif garments, plain forms, motif references) and learns abstract
visual concepts (e.g. what an "Iznik motif" looks like). Generated images are novel
combinations of these learned concepts, not reproductions of dataset images — a property
confirmed quantitatively by the diversity metrics.

Note on Large Files

Trained model weights (.safetensors), datasets, and the base model are excluded from this
repository (see .gitignore). Only the source code is tracked.

Author

Graduation Project — Zeynep Tufan
