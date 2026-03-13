# OBI-WAN: Occupation-Based Index for Workforce AI Navigator

## Overview
OBI-WAN is a prototype AI-driven career and education navigator designed to solve the education-to-workforce transition in higher education settings. Unlike standard conversational agents that frequently hallucinate career advice, OBI-WAN operates on a deterministic graph architecture. It uses a Large Language Model strictly as a reasoning and routing engine to query real-world labor and education data (O*NET and IPEDS).

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

*(Read the full conference paper abstract here - [INSERT LINK TO PDF OR DOCUMENT])*
