# OBI-WAN: Occupation-Based Index for Workforce AI Navigator

## Overview
OBI-WAN is a prototype AI-driven career and education navigator built to support education-to-workforce transitions in higher education. By integrating O*NET, IPEDS, semantic retrieval, and graph-based querying, it delivers explainable career and program guidance grounded in reliable structured data. Unlike conventional conversational agents that can hallucinate recommendations, OBI-WAN uses a large language model strictly as a reasoning and routing engine within a deterministic graph architecture over real-world labor and education data.

## Motivation
Education-to-career decisions are increasingly difficult to navigate. Learners often face fragmented information about occupations, skills, degree pathways, and institutions, while generic LLM systems can produce plausible but ungrounded advice. OBI-WAN is designed to support transparent, explainable pathway exploration through human-AI collaboration and structured data grounding. 

## What OBI-WAN does
- Resolves user goals into standardized occupation and program entities
- Computes a context-adjusted AI applicability score for occupations
- Retrieves aligned competencies (skills/abilities/knowledge), programs, and institutions through relational data joins
- Supports multi-turn preference elicitation for missing parameters
- Produces explanation-driven guidance grounded in structured data rather than free-form model memory

## Why it is different
OBI-WAN combines:
- **Grounded retrieval:** semantic search + BigQuery graph traversal
- **AI Applicability Score:** an occupation-level signal that helps users gauge how strongly a role may be complemented by AI.
- **Human-AI collaboration:** multi-turn disambiguation and preference elicitation
- **Trust-oriented evaluation framework:** synthetic persona stress-testing for ambiguity, adversarial prompts, and impossible requests 

## System overview
OBI-WAN is a prototype AI-driven career and education navigator built to support education-to-workforce transitions in higher education. It integrates O*NET, IPEDS, semantic retrieval, and a node-and-edge schema implemented in BigQuery to generate explainable career and program guidance grounded in reliable labor and education data. Unlike conventional conversational agents that can hallucinate recommendations, OBI-WAN uses a large language model strictly as a reasoning and routing layer over structured relationships among jobs, competencies, programs, and higher educatation institutions.

## Evaluation framework
The goal is to stress-test the agent against ambiguous, adversarial, and non-standard inputs while preserving grounding and deterministic tool execution, by creating synthetic personas and inputting edge-case prompts.







## Technical Architecture & Stack
* **LLM & Orchestration:** Gemini 2.5 Flash via the Google Agent Development Kit (ADK).
* **Vector Embedding:** Vertex AI (`text-embedding-004`) for semantic matching of raw user queries to official SOC (occupation) and CIP (program) taxonomies.
* **Database & Graph Traversal:** Google Cloud BigQuery. The data pipeline traverses interconnected nodes (Competencies ↔ Occupations ↔ Academic Programs ↔ Institutions) to build personalized, data-backed career roadmaps.

## Repository Structure
* 📄 **`ed_workforce_navigator.ipynb`**: The core executable Colab notebook containing the data pipeline, tool execution logic, agent architecture, and the local ADK web UI deployment.
* 📁 **`evaluation/evaluation_synthetic_persona.md`**: The adversarial testing framework. Details the Stress-Test Persona Matrix and hallucination/determinism metrics used to validate the agent's logical safety and UX guardrails prior to deployment using synthetic personas.

## How to Run the Prototype
1. Open the `.ipynb` notebook in Google Colab.
2. Run **Section 1** to authenticate your Google Cloud environment (requires a valid `GOOGLE_API_KEY` stored in Colab Secrets and a GCP project with BigQuery access).
3. Execute the remaining cells sequentially to initialize the BigQuery client, load the vector registry, and compile the application engine.
4. Run the final cell to launch the local ADK Web UI proxy and interact with OBI-WAN.

## Research & Methodology (AIED 2026)
The pedagogical design, system architecture, and applied statistical methodologies powering OBI-WAN are documented in my paper for the **International Conference on Artificial Intelligence in Education (AIED) 2026**. 

The research focuses on bridging learning analytics with workforce data, ensuring that AI tools in higher education prioritize real post-secondary education and labor market data based to ensure users' trust towards my model and prevent any potential hallucination. 

*(Read the full conference paper abstract here - [INSERT LINK TO PDF OR DOCUMENT](https://drive.google.com/file/d/145SXYUiA1pEh_jDW2lt8lfMHa76u5j-m/view?usp=sharing])*
