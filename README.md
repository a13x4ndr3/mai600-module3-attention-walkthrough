# mai600-module4-prompt-library
## Prompt Library & Evaluation Set — Business Operations / Project Management

**Course:** MAI 600 — Natural Language Processing  
**Student:** Alexandre Contaldi Pasquini | Atlantis University | MSc in Artificial Intelligence  
**Module:** Module 4 — Prompting, Evaluation & Controlling LLMs

---

## Problem Description

Companies using LLMs for recurring operational tasks often encounter inconsistent outputs: some responses are well-structured and accurate, while others hallucinate deadlines, invent decisions, drift from the requested format, or miss critical details. This project builds a reusable prompt library and evaluation set to address that inconsistency for one specific task.

---

## Task Selected

**Task:** Convert meeting notes and operational updates into structured action items and risk summaries  
**Domain:** Business Operations / Project Management (Apex Process Solutions — fictional company)  
**Audience:** Project managers, department leads, and executive sponsors  
**Why this task:** Meeting note summarization is one of the highest-volume recurring AI tasks in business operations. It is also prone to hallucination (invented deadlines/owners), format drift (prose instead of tables), and overconfident resolution of open questions — making it an ideal stress test for prompt design.

---

## Dataset / Input Set

**Source:** Instructor-provided fictional sample cases — Apex Process Solutions domain  
**Input count:** 10 cases (OPS-01 through OPS-10)  
**Input length:** 80–150 words each  
**Sensitive data:** None — all cases are fictional and were written for academic use  
**Topics covered:** Dashboard launch blockers, vendor onboarding delays, escalation tracking, training gaps, vendor tool evaluation, KPI misinterpretation, intake form standardization, pilot program results, support hour decisions, approval ownership gaps

---

## Prompting Approach

Five prompts were designed for the same task, each serving a different purpose:

| Prompt | Type | Purpose |
|---|---|---|
| P1 | Zero-shot | Baseline — no constraints or format |
| P2 | Structured | Role, rules, exact table schema |
| P3 | Few-shot | One worked example to anchor format |
| P4 | Safety/uncertainty | Anti-hallucination rules, explicit grounding |
| P5 | Final best | Combines structure, grounding, few-shot, urgency flag |

See `prompt_library.md` for full prompt text.

---

## Evaluation Criteria

Each output was scored 1–5 on:

| Criterion | Meaning |
|---|---|
| Accuracy | Is the answer correct based on the input? |
| Helpfulness | Does it solve the task for the target audience? |
| Format adherence | Did the model follow the requested table structure? |
| Completeness | Were all required parts included? |
| Grounding | Did it stay grounded in the input — no invented details? |

---

## Results Summary

| Prompt | Avg Accuracy | Avg Helpfulness | Avg Format | Avg Completeness | Avg Grounding |
|---|---|---|---|---|---|
| P1 (Zero-shot) | 2.2 | 2.1 | 1.0 | 2.3 | 1.8 |
| P2 (Structured) | 5.0 | 5.0 | 5.0 | 5.0 | 5.0 |
| P3 (Few-shot) | 5.0 | 5.0 | 5.0 | 5.0 | 5.0 |
| P4 (Safety) | 3.8 | 3.7 | 4.8 | 3.8 | 5.0 |
| P5 (Final best) | 5.0 | 5.0 | 5.0 | 5.0 | 5.0 |

P1 consistently produced the worst results — format drift, hallucination, and missing risk items. P4 was well-grounded but overcautious, writing "Not stated" for information that was clearly present in the input. P2, P3, and P5 all performed at the highest level, with P5 adding the urgent-item summary that made it the most useful for executive audiences.

---

## Failure Modes Found

| Failure Mode | Frequency | Example |
|---|---|---|
| Format drift | 10/10 for P1 | P1 returned prose instead of a table in every case |
| Hallucination (invented detail) | 4/10 for P1 | T9/P1: invented a specific staffing model not in input |
| Invented decision | 2/10 for P1 | T2/P1: converted open director question into an approved decision |
| Overcaution | 5/10 for P4 | T1/P4: wrote "Not stated" for a Friday deadline explicitly in the input |
| Missing risk / dependency | 6/10 for P1 | T8/P1: reported 30% reduction without noting template compliance dependency |

---

## Prompt Improvements Made

| Test | Original Prompt | Problem | Revised Prompt | Result |
|---|---|---|---|---|
| T1 | P1 | Format drift; deadline invented | P5 | Table correct; Friday noted accurately |
| T2 | P1 | Open question converted to decision | P5 | Open question row preserved |
| T9 | P1 | Specific staffing model invented | P2 | Budget/volume tension captured; no invented model |
| T1 | P4 | "Not stated" for explicit Friday date | P5 | Relaxed constraint applied correctly |

---

## Final Best Prompt

**Prompt P5** performed best across all ten evaluation cases.

**Why it outperformed:**
- Combines role framing, explicit rules, a worked example, and an urgency-summary requirement
- The grounding rules (extract only what is stated; use "Not specified" only for genuinely missing details) eliminated hallucination without triggering overcaution
- The "most urgent item" sentence at the end made outputs immediately actionable for executive audiences — a feature missing from P2–P4
- Open questions are captured as separate rows, preventing the model from resolving ambiguity incorrectly

**Remaining limitations:**
- Longer token cost due to the worked example in the prompt
- Very complex inputs with 8+ action items may cause the table to truncate or compress rows
- The urgency classification still depends on the model's interpretation of "risk" from limited context

**How it would be used in a real workflow:**
Replace `{input_text}` with the raw meeting transcript or operational update, pipe the output to a project management system (e.g., Jira, Notion, or a CRM), and flag any row where Risk = High or "Open question" for human review before distribution.

---

## Reflection

The most important thing I learned from this assignment is that the difference between a zero-shot prompt and a structured prompt is not just cosmetic — it is the difference between an output that looks like a response and one that is actually usable. P1 produced responses that appeared helpful at a glance but consistently failed on grounding: invented deadlines, invented resolutions to open questions, and missing risk items. None of those failures were obvious without reading the input carefully and comparing it to the output line by line.

The P4 (safety) prompt revealed a different problem: a prompt that is too conservative produces outputs that are technically grounded but practically useless. Writing "Not stated in input" for a Friday deadline that was explicitly stated in the text is not safe behavior — it is an error in the opposite direction. The best prompt (P5) resolved this by making the grounding rule precise: do not invent what is not stated, but do extract what is clearly there.

For anyone building AI workflows for business operations, this is the central design challenge: prompts that are too vague hallucinate; prompts that are too restrictive refuse. The structured, few-shot approach with a worked example and explicit rules is the most reliable starting point.

---

## Files

| File | Description |
|---|---|
| `README.md` | This file |
| `prompt_library.md` | Five prompts with type, purpose, and known risks |
| `evaluation_set.csv` | 10 test cases with expected behavior and risk to watch |
| `results_table.csv` | Scored outputs for all 5 prompts × 10 cases |
| `ai_usage_disclosure.md` | AI tool usage disclosure |

---

## AI Tool Usage

See `ai_usage_disclosure.md` for full details. Claude (claude.ai) was used to test prompts and support document formatting. All scores, failure mode analysis, and reflection were produced independently.
