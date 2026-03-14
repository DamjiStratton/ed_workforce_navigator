# OBI-WAN: Occupation-Based Index for Workforce AI Navigator

## Overview
OBI-WAN is a prototype AI-driven career and education navigator built to support education-to-workforce transitions in higher education. By integrating O*NET, IPEDS, semantic retrieval, and a node-and-edge schema implemented in BigQuery, it delivers explainable career and program guidance grounded in reliable labor and education data. Unlike conventional conversational agents that can hallucinate recommendations, OBI-WAN uses a large language model as a reasoning, dialogue-management, and tool-routing layer over structured relationships among occupations, competencies, programs, and institutions.

## Motivation
Education-to-career decisions are increasingly difficult to navigate. Learners often face fragmented information about occupations, skills, degree pathways, and institutions, while generic LLM systems can produce plausible but ungrounded advice. OBI-WAN is designed to support transparent, explainable pathway exploration through human-AI collaboration and structured data grounding. 

## What OBI-WAN Does
- Resolves user goals into standardized occupation and program entities
- Computes a context-adjusted AI applicability score for occupations
- Retrieves aligned competencies (skills/abilities/knowledge), programs, and institutions through relational data joins
- Supports multi-turn preference elicitation for missing parameters
- Produces explanation-driven guidance grounded in structured data rather than free-form model memory

## Why It Is Different
OBI-WAN combines:
- **Grounded retrieval:** semantic search + SQL joins across node-and-edge tables in BigQuery
- **AI Applicability Score:** an occupation-level signal that helps users gauge how strongly a role may be complemented by AI.
- **Human-AI collaboration:** multi-turn disambiguation and preference elicitation
- **Trust-oriented evaluation framework:** synthetic persona stress-testing for ambiguity, adversarial prompts, and impossible requests 

## Evaluation Framework
OBI-WAN includes an in-progress evaluation framework designed to stress-test the agent against ambiguous, adversarial, and non-standard inputs. The current approach uses synthetic personas and edge-case prompts to assess grounding, tool-routing reliability, and resistance to hallucinated career or academic advice.

See [Evaluation/evaluation_synthetic_persona.md](Evaluation/evaluation_synthetic_persona.md) for the current version of framework.

## Repository Structure
- `README.md` — project overview
- `notebooks/obiwan_navigator.ipynb` — prototype notebook
- `Evaluation/evaluation_synthetic_persona.md` — synthetic persona evaluation framework
- `assets/obiwan-architecture.png` — architecture diagram

## Current Status
This repository contains the current prototype and an in-progress evaluation framework. Next steps include expanding the persona-based evaluation harness, analyzing failure patterns systematically, and conducting broader user-centered validation. For collaboration, please contact me at hdj514@gmail.com Thanks!

## Why This Matters for Organizations
OBI-WAN is a proof-of-concept for grounded AI systems that support scalable and explainable advising and pathway intelligence. The broader goal is to show how AI can function as a collaborative teammate in education-to-workforce decision support rather than an opaque recommendation engine. 

## Technical Architecture
- **LLM orchestration:** Gemini 2.5 Flash, via Google ADK, manages dialogue, clarification, and tool routing.
- **Semantic retrieval:** Vertex AI (`text-embedding-004`) maps user queries to standardized occupation (SOC) and academic program (CIP) entities using vector similarity.
- **Structured data layer:** BigQuery stores a node-and-edge schema connecting occupations, competencies, academic programs, and institutions.
- **Grounded recommendations:** OBI-WAN retrieves pathway-relevant information through SQL joins across node-and-edge tables, while the LLM formats and explains results rather than inventing domain facts.
- **Stateful interaction:** `ToolContext.state` preserves prior context across turns to support follow-up queries and missing-parameter clarification.

## Retrieval Flow
User query is embedded with Vertex AI.
Semantic matching maps the query to SOC/CIP entities.
BigQuery joins across node-and-edge tables retrieve occupations, competencies, programs, or institutions.
Gemini formats results, asks clarifying questions when needed, and preserves context across turns.

## Potential Users
universities and academic advising teams
workforce development organizations
career navigation platforms
edtech and upskilling providers

## Example Use Case
User question: “I’m interested in mental health counseling. What skills do I need, and which online master’s programs might fit?”
OBI-WAN response: maps the query to relevant occupations and programs, retrieves aligned competencies, asks for missing filters such as modality or degree level, and returns grounded institution recommendations from structured IPEDS and O*NET data.

## Related Research
A research draft describing the design and methodology of OBI-WAN is available upon request.
