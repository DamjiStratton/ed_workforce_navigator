# OBI-WAN KPI Framework

## Purpose
This document defines the core product KPIs for OBI-WAN. Unlike the agent evaluation framework, which focuses on system behavior and reliability under edge-case conditions, this KPI framework focuses on decision-support value, user usefulness, efficiency, and trust.

## Core KPIs

| KPI | Why It Matters | Definition / Calculation | Unit of Analysis | Audience |
| :--- | :--- | :--- | :--- | :--- |
| **Pathway Clarification Rate** | Measures whether OBI-WAN helps users move from vague intent to a more concrete direction | **# vague-intent sessions ending with a narrowed occupation, program, or degree direction / # vague-intent sessions** | Session | CEO, Product |
| **Actionable Recommendation Rate** | Measures whether the system produces usable next steps rather than only conversation | **# in-scope sessions ending with at least one grounded recommendation or clear next step / # in-scope sessions** | Session | CEO, CTO |
| **Time to First Actionable Recommendation** | Measures interaction efficiency and how much friction users face before getting something useful | **Median assistant turns until the first grounded, actionable recommendation across successful sessions** | Session | CEO, CTO |
| **Helpfulness Score** | Measures perceived user value of OBI-WAN’s recommendations | **Average user helpfulness rating across rated sessions**; optionally also track **% of ratings that are 4–5** | Session | CEO, Product |
| **Hallucination-Free Recommendation Rate** | Measures trustworthiness and whether final recommendations remain grounded in structured data | **# recommendation sessions with zero unsupported occupations, programs, or institutions / # recommendation sessions** | Session | CEO, CTO |

## KPI Definitions

### 1. Pathway Clarification Rate
A session is counted as a **vague-intent session** if the user begins without a clearly specified occupation, academic program, or degree direction.

A session is counted as **clarified** if it ends with at least one of the following:
- a narrowed occupation target
- a narrowed academic program or major
- a narrowed degree direction
- a grounded next-step pathway that meaningfully reduces ambiguity

**Formula**  
`Pathway Clarification Rate = clarified vague-intent sessions / vague-intent sessions`

### 2. Actionable Recommendation Rate
A session is counted as **actionable** if it ends with at least one grounded, usable next step, such as:
- a recommended occupation
- a grounded competency list tied to a target role
- a recommended academic program
- a recommended institution
- a clear next-step clarification that enables grounded retrieval

Only **in-scope sessions** should be included in the denominator.

**Formula**  
`Actionable Recommendation Rate = in-scope sessions with grounded recommendation or clear next step / in-scope sessions`

### 3. Time to First Actionable Recommendation
This measures how many assistant turns it takes before the user receives the first grounded, useful recommendation.

A recommendation counts only if it is:
- grounded in retrieved data
- relevant to the user’s request
- more than a generic clarification or vague response

Because this metric can be skewed by outliers, **median turns** is recommended instead of average.

**Formula**  
`Time to First Actionable Recommendation = median assistant turns until first actionable grounded recommendation across successful sessions`

### 4. Helpfulness Score
This measures how useful users perceive OBI-WAN’s responses to be.

Recommended collection method:
- prompt the user to rate the response on a **1–5 scale**
- optionally collect a short free-text explanation

**Formula**  
`Helpfulness Score = average helpfulness rating across rated sessions`

Optional supporting metric:  
`Top-2-Box Helpfulness = # ratings of 4 or 5 / # rated sessions`

### 5. Hallucination-Free Recommendation Rate
This measures whether OBI-WAN’s final recommendations remain fully grounded in retrieved O*NET and IPEDS-backed data.

A session counts as **hallucination-free** only if:
- every returned occupation is traceable to retrieved structured data
- every returned program is traceable to retrieved structured data
- every returned institution is traceable to retrieved structured data
- no unsupported pathway claims are introduced in the final answer

Only sessions containing recommendations should be included in the denominator.

**Formula**  
`Hallucination-Free Recommendation Rate = recommendation sessions with zero unsupported entities / recommendation sessions`

## Logging Requirements
To calculate these KPIs, OBI-WAN should log the following fields for each session:

- `session_id`
- `turn_id`
- `timestamp`
- `user_prompt`
- `initial_intent_type` (vague, specific, out_of_scope)
- `tools_invoked`
- `tool_parameters`
- `grounded_entities_returned`
- `unsupported_entities_flag`
- `first_actionable_turn_number`
- `session_outcome` (clarified_path, recommendation, redirected, no_result)
- `helpfulness_rating`

Optional but helpful:
- `trust_rating`
- `free_text_feedback`

## Notes
These KPIs are intended to measure **product value and user impact**, not only technical system performance. Technical correctness metrics such as tool-routing reliability, scope compliance, and parameter-gating compliance should remain in the separate agent evaluation framework.
