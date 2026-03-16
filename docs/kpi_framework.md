# OBI-WAN KPI Framework

## Purpose
This document defines the current product KPIs for OBI-WAN that can be measured through synthetic persona evaluation. At this stage, these KPIs should be interpreted as **offline proxy metrics** rather than live user metrics, because they are based on controlled synthetic test cases rather than real user interaction data.

Unlike the agent evaluation framework, which focuses on system behavior under edge-case conditions, this KPI framework focuses on whether OBI-WAN produces useful, grounded, and decision-supportive outcomes.

## Scope
The current KPI framework is limited to metrics that are feasible to measure using synthetic persona prompts and structured evaluation logs.

## Core KPIs

## Core KPIs

| KPI | Why It Matters | Definition / Calculation | Unit of Analysis | Audience |
| :--- | :--- | :--- | :--- | :--- |
| **Proxy Pathway Clarification Rate** | Measures whether OBI-WAN helps move a vague starting query into a more concrete career or academic direction | **# vague-intent synthetic sessions ending with a narrowed occupation, program, or degree direction / # vague-intent synthetic sessions** | Session | CEO, Product Lead, Research Lead |
| **Actionable Recommendation Rate** | Measures whether the system produces usable next steps rather than only conversation | **# in-scope synthetic sessions ending with at least one grounded recommendation or clear next step / # in-scope synthetic sessions** | Session | CEO, CTO, Product Lead |
| **Time to First Actionable Recommendation** | Measures interaction efficiency and how much friction occurs before the system produces something useful | **Median assistant turns until the first grounded, actionable recommendation across successful synthetic sessions** | Session | CEO, CTO, Product Lead |
| **Hallucination-Free Recommendation Rate** | Measures trustworthiness and whether final recommendations remain grounded in structured data | **# synthetic recommendation sessions with zero unsupported occupations, programs, or institutions / # synthetic recommendation sessions** | Session | CEO, CTO, Product Lead, Research Lead |

## KPI Definitions

### 1. Proxy Pathway Clarification Rate
A session is counted as a **vague-intent synthetic session** if the initial prompt does not specify a clearly defined occupation, academic program, or degree direction.

A session is counted as **clarified** if, by the end of the session, OBI-WAN narrows the pathway into at least one of the following:
- a specific occupation target
- a specific academic program or major
- a specific degree direction
- a grounded next-step pathway that meaningfully reduces ambiguity

**Formula**  
`Proxy Pathway Clarification Rate = clarified vague-intent synthetic sessions / vague-intent synthetic sessions`

### 2. Actionable Recommendation Rate
A session is counted as **actionable** if it ends with at least one grounded, usable next step, such as:
- a recommended occupation
- a grounded competency list tied to a target role
- a recommended academic program
- a recommended institution
- a clear next-step clarification that enables grounded retrieval

Only **in-scope synthetic sessions** should be included in the denominator.

**Formula**  
`Actionable Recommendation Rate = in-scope synthetic sessions with grounded recommendation or clear next step / in-scope synthetic sessions`

### 3. Time to First Actionable Recommendation
This measures how many assistant turns it takes before OBI-WAN produces the first grounded, useful recommendation.

A recommendation counts only if it is:
- grounded in retrieved data
- relevant to the prompt
- more than a generic clarification or vague response

Because this metric can be skewed by outliers, **median turns** is recommended instead of average.

**Formula**  
`Time to First Actionable Recommendation = median assistant turns until first actionable grounded recommendation across successful synthetic sessions`

### 4. Hallucination-Free Recommendation Rate
A session counts as **hallucination-free** only if:
- every returned occupation is traceable to retrieved structured data
- every returned program is traceable to retrieved structured data
- every returned institution is traceable to retrieved structured data
- no unsupported pathway claims are introduced in the final answer

Only synthetic sessions containing recommendations should be included in the denominator.

**Formula**  
`Hallucination-Free Recommendation Rate = synthetic recommendation sessions with zero unsupported entities / synthetic recommendation sessions`

## Logging Requirements
To calculate these KPIs from synthetic persona evaluation, OBI-WAN should log the following fields for each session:

- `session_id`
- `turn_id`
- `persona_type`
- `prompt_variant`
- `initial_intent_type` (`vague`, `specific`, `out_of_scope`)
- `tools_invoked`
- `tool_parameters`
- `grounded_entities_returned`
- `unsupported_entities_flag`
- `first_actionable_turn_number`
- `session_outcome` (`clarified_path`, `recommendation`, `redirected`, `no_result`)
- `notes`

## Scoring Method
Current KPI values are calculated from manually reviewed synthetic persona sessions. Each session is scored using structured evaluation logs that capture:

- whether the initial ambiguity was reduced
- whether at least one grounded next step was provided
- the assistant turn at which the first actionable recommendation appeared
- whether any unsupported occupations, programs, or institutions appeared in the final answer

These scores are currently treated as offline proxy measures rather than live product metrics.

## Evaluation Sample
The current KPI snapshot is based on:

- **Total synthetic sessions evaluated:** `6`
- **Vague-intent sessions:** `2`
- **In-scope sessions:** `4`
- **Recommendation-containing sessions:** `1`
- **Successful actionable sessions used for median-turn calculation:** `3`
- **Evaluation mode:** Offline synthetic persona testing

## Latest Offline KPI Results

| KPI | Current Result | Supporting Count | Denominator | Interpretation |
| :--- | :--- | :--- | :--- | :--- |
| **Proxy Pathway Clarification Rate** | **100%** | **2 of 2** | 2 vague-intent sessions | Indicates how often OBI-WAN helps narrow an initially vague request into a more concrete pathway |
| **Actionable Recommendation Rate** | **75%** | **3 of 4** | 4 in-scope sessions | Indicates how often OBI-WAN ends with at least one grounded recommendation or clear next step |
| **Time to First Actionable Recommendation** | **1.0 turns (median)** | N/A | 3 successful actionable sessions | Indicates how quickly OBI-WAN produces a grounded, useful next step |
| **Hallucination-Free Recommendation Rate** | **100%** | **1 of 1** | 1 recommendation session | Indicates how often final recommendations remain fully traceable to structured data |

## Interpretation
These results suggest that, within the current synthetic evaluation sample, OBI-WAN is already demonstrating:

- strong pathway clarification performance on vague prompts
- a relatively high rate of grounded, actionable next-step recommendations
- low interaction friction before producing useful output
- no unsupported entities in the recommendation-containing sessions reviewed so far

Because the current denominators are still small, these results should be interpreted as **preliminary offline proxy KPIs**, not stable product-level estimates. They are useful for demonstrating early system behavior under controlled synthetic conditions, but they do not yet establish real-world user satisfaction, trust, or adoption.

## Interpretation Notes
These KPIs are currently designed as **offline proxy measures** based on synthetic persona testing. They are useful for assessing whether OBI-WAN can generate grounded, actionable, and efficient decision-support behavior under controlled conditions.

They should **not** yet be interpreted as direct measures of real user satisfaction, trust, or adoption. Those metrics require a future user-facing demo and real interaction data.

## Future KPI Expansion
Once OBI-WAN is deployed in a demo app with real users, the KPI framework can be expanded to include:

- user-rated helpfulness
- trust score
- repeat usage / retention
- clarification burden from real sessions
- completion of real-world next-step actions
