# Industry Terms: ML Systems / AI Infrastructure

> **Usage note:** Use this file when the target direction is ML systems, AI infrastructure, LLM platform, model serving, or training/inference infrastructure. It captures the jargon shared by research and engineering work in MLSys-style domains.

---

## Training Infrastructure & Efficiency

| User's description | Industry term |
|---|---|
| "trained on multiple GPUs" | Distributed Training (Data / Model Parallelism) |
| "made training use less memory" | Mixed-precision Training / Gradient Checkpointing |
| "made model smaller for faster deployment" | Quantization / Model Compression |
| "made inference faster" | Inference Optimization / Model Acceleration |
| "served many requests with batching or cache reuse" | Continuous Batching / Inference Caching |
| "routed requests to different models based on cost or speed" | Model Routing / Cost-performance Optimization |
| "ran the model on constrained hardware or devices" | Edge Inference / Hardware-efficient Deployment |
| "benchmarked training or inference across different system setups" | ML Systems Benchmarking / Performance Characterization |

## Serving, Monitoring & LLMOps

| User's description | Industry term |
|---|---|
| "put model in production" | Model Serving / Model Deployment |
| "monitored model performance" | ML Monitoring / Observability |
| "model got worse over time" | Model Drift Detection (Data Drift, Concept Drift) |
| "auto-retrained the model" | Continuous Training / MLOps Pipeline |
| "served model via API" | Model-as-a-Service / Inference Endpoint |
| "rolled out model gradually" | Canary Deployment / Shadow Mode |
| "tracked model versions" | Model Registry / Model Versioning |
| "kept prompts and outputs versioned" | LLMOps / Prompt Versioning |
| "checked whether answers stayed grounded in sources" | Groundedness / Faithfulness Evaluation |
| "reduced hallucinations" | Hallucination Mitigation / Guardrailed Generation |
| "added safety filters before or after generation" | Safety Guardrails / Content Moderation Layer |
| "let the model use tools or APIs" | Tool Calling / Agentic Workflow Orchestration |

## Data, Adaptation & Platform

| User's description | Industry term |
|---|---|
| "built feature tables for training and serving" | Feature Engineering / Feature Store |
| "versioned datasets used by training jobs" | Data Versioning |
| "tracked where training data came from" | Data Lineage / Data Provenance |
| "connected the model to documents or search" | Retrieval-Augmented Generation (RAG) |
| "adapted a base model with lightweight updates" | Parameter-efficient Fine-tuning (PEFT, LoRA) |
| "tested, debugged, or monitored the full ML application" | End-to-end ML Application Debugging / ML Application Observability |
