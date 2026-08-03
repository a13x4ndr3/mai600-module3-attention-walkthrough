# Prompt Library — Apex Process Solutions
## Task: Convert Meeting Notes and Operational Updates into Action Items and Risk Summaries

**Student:** Alexandre Contaldi Pasquini | MAI 600 Natural Language Processing | Module 4  
**Audience:** Project managers, department leads, and executives  
**Output format:** Action table — Decision/Issue | Owner | Due Date (if stated) | Risk | Next Step

---

## Prompt 1 — Zero-Shot Prompt

**Purpose:** Basic task instruction without examples or constraints. Baseline to observe natural model behavior.

```
Summarize the following meeting notes or operational update into action items.
Extract any decisions made, owners assigned, and next steps.

Input:
{input_text}
```

**Expected behavior:** Model should extract key items but may miss structure, invent deadlines, or drift in format.  
**Known risks:** Format drift, invented due dates, missing risk identification.

---

## Prompt 2 — Structured Prompt

**Purpose:** Adds role, context, output format, and constraints to improve consistency.

```
You are a project coordinator summarizing meeting notes for a project manager and executive team.

Your task is to convert the following operational update into a structured action table.

Rules:
- Extract only decisions and actions that are explicitly stated in the input.
- Do not invent deadlines, owners, or outcomes that are not mentioned.
- If a due date is not provided, write "Not specified."
- If the owner is not named, write "Unassigned."
- Classify each item's risk as Low, Medium, or High based on the stated context.

Output format — use this exact table structure:
| Decision / Issue | Owner | Due Date | Risk | Next Step |
|---|---|---|---|---|

Input:
{input_text}
```

**Expected behavior:** Consistent table output, no invented details, clear risk classification.  
**Known risks:** Risk classification may be inconsistent without anchor criteria.

---

## Prompt 3 — Few-Shot Prompt

**Purpose:** Includes one worked example to anchor the model's output format and reasoning style.

```
You are a project coordinator summarizing meeting notes for a project manager.
Convert each input into a structured action table.

Rules:
- Only extract what is explicitly stated.
- Do not invent deadlines, owners, or outcomes.
- If a due date is not stated, write "Not specified."
- If an owner is not named, write "Unassigned."

---
EXAMPLE INPUT:
"The team agreed to update the vendor contract template. Legal will lead the revision. No deadline was set, but the procurement lead asked for a draft before the next board meeting. There is a risk that the current template will be used for renewals in the meantime."

EXAMPLE OUTPUT:
| Decision / Issue | Owner | Due Date | Risk | Next Step |
|---|---|---|---|---|
| Update vendor contract template | Legal | Not specified (draft before board meeting) | Medium — current template may be used in renewals | Legal to produce draft; procurement to pause renewals if possible |

---
Now summarize this input using the same format:

Input:
{input_text}
```

**Expected behavior:** Output closely mirrors the example structure with correct reasoning.  
**Known risks:** Model may overfit to example and force all items into one row.

---

## Prompt 4 — Safety / Uncertainty Prompt

**Purpose:** Reduces hallucination and overconfident claims, especially when input is ambiguous or incomplete.

```
You are a project coordinator summarizing meeting notes for an executive audience.

Your task is to convert the following operational update into a structured action table.

Important rules:
- Only include information that is explicitly stated in the input text.
- If a deadline, owner, or decision is unclear or missing, write exactly: "Not stated in input."
- Do not assume, infer, or invent any details that are not present.
- If the input is ambiguous, flag the ambiguity in the Next Step column.
- Classify risk as Low, Medium, or High. If you cannot determine the risk level from the input alone, write "Unclear — needs clarification."

Output format:
| Decision / Issue | Owner | Due Date | Risk | Next Step |
|---|---|---|---|---|

Input:
{input_text}
```

**Expected behavior:** Conservative, grounded output; ambiguities surfaced rather than resolved.  
**Known risks:** Model may be overly cautious and produce less actionable output.

---

## Prompt 5 — Final Improved Prompt

**Purpose:** Best-performing prompt after testing and revision. Combines structure, grounding, few-shot anchoring, and audience clarity.

```
You are an experienced project coordinator preparing a decision and action summary for a project manager and executive team at a business operations firm.

Your task is to convert the following meeting notes or operational update into a clear, structured action table.

Rules — follow all of these exactly:
1. Extract only decisions, risks, and actions that are explicitly mentioned in the input.
2. Do not invent deadlines, owners, risk levels, or outcomes that are not stated.
3. If a deadline is not stated, write "Not specified."
4. If an owner is not named, write "Unassigned — clarification needed."
5. Classify risk as Low, Medium, or High using only evidence from the input.
6. If the risk level cannot be determined, write "Unclear."
7. The Next Step must be specific and actionable — not generic phrases like "follow up."
8. If the input contains an open question that was not resolved, include it as a separate row with risk = "Open question."

Output format — use exactly this table:
| Decision / Issue | Owner | Due Date | Risk | Next Step |

End the table with one sentence summarizing the most urgent item from the input, or state "No urgent items identified" if none apply.

---
EXAMPLE:
Input: "The team agreed that the reporting dashboard cannot launch until the finance data mapping is confirmed. Maya will review the account categories, and Jordan will validate the sample export. No final date was set, but the sponsor asked for an update before Friday."

Output:
| Decision / Issue | Owner | Due Date | Risk | Next Step |
|---|---|---|---|---|
| Dashboard launch blocked by finance data mapping | Maya / Jordan | Update by Friday | High — launch delay if mapping not confirmed | Maya: review account categories; Jordan: validate sample export |
| Friday sponsor update | Project lead | Friday | Medium | Prepare status update confirming mapping progress |

Most urgent item: Dashboard launch is blocked — data mapping must be confirmed before Friday sponsor update.

---
Now summarize this input:

Input:
{input_text}
```

**Expected behavior:** Highest consistency, correct grounding, specific next steps, urgent item flagged.  
**Known risks:** Prompt is longer — token cost higher; very complex inputs may still overflow table structure.
