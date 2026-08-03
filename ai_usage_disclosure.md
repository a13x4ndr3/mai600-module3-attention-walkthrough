# AI Tool Usage Disclosure

## Student Information
- **Student name:** Alexandre Contaldi Pasquini
- **Course:** MAI 600 Natural Language Processing
- **Module:** Module 4 — Prompting, Evaluation & Controlling LLMs
- **Assignment:** Prompt Library & Evaluation Set
- **Date:** July 2026

## AI Tools Used

- [x] Claude (claude.ai)
- [ ] ChatGPT
- [ ] Claude Code
- [ ] GitHub Copilot
- [ ] OpenAI Codex
- [ ] Gemini
- [ ] Other

## How I Used AI

- Used Claude to test all five prompts against the ten evaluation cases and record the model outputs
- Used Claude to help structure the prompt library format and the before/after improvement table
- Used Claude to generate varied failure mode scenarios to confirm the prompts were stress-tested across different input types

## What I Verified Myself

- Read all model outputs for each prompt/case combination and assigned all scores independently
- Identified failure modes based on my own analysis of what was wrong with each low-scoring output
- Verified that invented content (deadlines, owners, decisions) was correctly flagged as hallucination rather than format drift
- Confirmed that the final best prompt (P5) consistently outperformed P1 across all ten cases before selecting it

## What I Changed or Corrected After Using AI

- Noticed that P4 (safety/uncertainty prompt) was overcorrecting on explicit information — for example, writing "Not stated" for a Friday deadline that was clearly in the input. I added a revision note to relax the constraint for explicitly stated values and recorded this as a prompt improvement
- Revised the "invented resolution" failure mode description after observing that P1 converted an open question into an approved decision in T2 — this was a more specific failure than generic hallucination and I separated it as its own failure mode category
- Added the "urgent item" sentence requirement to P5 after testing showed that P2 and P3 did not surface priority ordering, which reduced helpfulness for executive audiences

## Work Ownership Statement

I confirm that AI was used as a learning and support tool, not as a replacement for my own work. I reviewed and verified all outputs, assigned all scores, and wrote the failure mode analysis and reflection in my own words.
