# Module 3 Attention Walkthrough: AI CRM Automation Example

**Course:** MAI 600 — Natural Language Processing  
**Student:** Alexandre Contaldi Pasquini  
**Atlantis University | MSc in Artificial Intelligence**

---

## Problem Description

This project explains how a Transformer model uses self-attention to process a fictional operational paragraph from an AI-driven CRM and automation workflow (Axelis AI). The goal is to identify token relationships, document concrete attention behaviors, and explain how the Transformer architecture — from tokenization through output probabilities — handles context, coreference, and long-range dependencies in natural language.

---

## Dataset / Text Description

**Text source:** Original fictional paragraph written for this assignment  
**Domain:** AI-driven CRM and voice agent automation (Axelis AI)  
**Why selected:** The paragraph contains multiple pronouns with different referents, named entity coreference, a cause-and-effect chain spanning a sentence boundary, and a long-range entity reference across the full paragraph — all of which are strong test cases for attention analysis  
**Sensitive data:** None — all names, events, and entities are fictional

### Text Sample

> The AI voice agent completed the outbound call because it detected that the lead had previously submitted a contact form. The system flagged the contact as high priority after scoring it above the qualification threshold. Sara, the voice agent, confirmed the appointment with the prospect and logged the outcome in the CRM. When the webhook failed to trigger, the automation team reviewed the pipeline logs and discovered that the stage name had been changed, which broke the trigger condition. They corrected the configuration and reprocessed the affected leads.

---

## Attention Behaviors Found

**1. Pronoun resolution — "it" appears twice with different referents**  
In sentence 1, "it" refers to "lead." In sentence 2, "it" refers to "contact." Self-attention must resolve each instance using the surrounding token context, which differs in each case.

**2. Entity tracking across sentences — Sara / voice agent**  
"Sara" is introduced in sentence 3 as a named alias for "the AI voice agent" from sentence 1. The model must link these two references and then correctly attribute the verbs "confirmed" and "logged" to Sara across the sentence.

**3. Cause and effect across a sentence boundary — stage rename → webhook failure**  
The webhook failure in sentence 4 is caused by the stage name change. The model must connect the effect ("webhook failed") to the mechanism ("stage name had been changed") and the result ("broke the trigger condition") — a long-range dependency within a single complex sentence.

**4. Full-paragraph coreference — "lead" → "prospect" → "affected leads"**  
The entity "lead" in sentence 1 is referred to as "the prospect" in sentence 3 and "affected leads" in sentence 5. Maintaining this entity identity across the full paragraph requires persistent context tracking across multiple attention layers.

---

## Transformer Diagram

See `transformer_diagram.png` in this repository.

The diagram covers all required components:
1. Raw text input
2. Tokenization
3. Token IDs
4. Embeddings + Positional encoding
5. Self-attention
6. Queries, Keys, Values
7. Attention weights
8. Multi-head attention
9. Feed-forward network
10. Add + Normalize (residual connections + layer normalization)
11. Output probabilities
12. Generated output

---

## Results / Observations

The most revealing observation was that "it" appears twice in the paragraph but refers to a different entity each time. A model resolving this correctly must use not just the nearest preceding noun, but the semantic role of each noun in context — "it" cannot refer to an action or to a system component in these cases, only to the human or contact entity. Self-attention enables this by allowing the "it" token to attend across the full sequence rather than only looking backward one or two positions.

The cause-and-effect chain (webhook failure → stage rename → trigger break) demonstrated why long-range attention is valuable over recurrent approaches. The causal explanation appears after the effect in the sentence, requiring the model to revise its representation of the webhook failure once it processes the subordinate clause.

In a real AI system like Axelis AI, these attention behaviors matter because models interpreting CRM logs, agent call summaries, or support tickets must correctly resolve entity references and causal chains to generate accurate summaries or trigger the right automation actions.

---

## Files

| File | Description |
|---|---|
| `README.md` | This file |
| `attention_walkthrough.ipynb` | Notebook with tokenization, token tables, attention behavior tables, heatmap, and reflection |
| `transformer_diagram.png` | Annotated Transformer architecture diagram |
| `ai_usage_disclosure.md` | AI tool usage disclosure |

---

## AI Tool Usage

See `ai_usage_disclosure.md` for full details.

Claude (claude.ai) was used to help generate the fictional text sample and suggest the matrix structure. All token relationship scores, attention behavior explanations, Q/K/V analysis, and reflection were written independently.
