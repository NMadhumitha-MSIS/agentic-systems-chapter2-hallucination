# Chapter 2: Hallucination: When Plausibility Beats Truth
### Design of Agentic Systems with Case Studies
**Author:** Madhumitha Nandhikatti

---

## Overview

This repository contains all deliverables for the Chapter 2 midterm submission. The chapter argues that hallucination is not a bug to be patched but an emergent structural property of next-token prediction, where fluency and factual grounding are orthogonal objectives. The architectural solution demonstrated is the Fact Check List Pattern: a verification layer inserted between generation and surfacing that halts the pipeline when output cannot be confirmed as safe.

**Core Claim:**
> After reading this chapter, a student will understand why hallucination is an unavoidable emergent property of next-token prediction well enough to design a Fact Check List verification layer into an agentic pipeline, without making the mistake of treating hallucination as a bug that better prompting alone can fix.

---

## Repository Structure

```
.
├── README.md
├── Author's Note/
│   └── Author's Note.pdf                 
├── Chapter/
│   └── Chapter 2-Hallucination-When Plausibility Beats Truth.pdf
├── Notebook/
│   └── CHAPTER_2_Hallucination_Whe...ipynb
└── Video/
    └── video_link.txt                    
```

---

## Deliverables

### 1. Chapter Prose
Located in `Chapter/Chapter 2-Hallucination-When Plausibility Beats Truth.pdf`

The chapter is structured in five sections following the Tetrahedron framework:

- **The Scenario** — A pharmaceutical company's research agent fabricates six out of seven citations in a drug interaction report. The failure reaches the regulatory team before a skeptical scientist catches it on a weekend.
- **The Mechanism** — Why next-token prediction makes plausibility and truth orthogonal objectives. The loss function measures token prediction accuracy, not factual correspondence. This distinction is invisible to the training process.
- **The Design Decision** — The Fact Check List Pattern as an architectural layer with three components: claim extractor, verifier, and circuit breaker. Includes extraction method tradeoffs and why generator-as-extractor is architecturally forbidden.
- **The Failure Case** — The 2023 legal brief case where attorneys submitted fabricated citations to a federal court. Documents what happens when the pattern is absent and when it is bypassed.
- **The Exercise** — A reproducible prompt experiment that triggers hallucination, shows why stronger instructions make it worse, and implements the simplest possible verification layer.

The chapter includes six embedded figure descriptions:

| Figure | Section | Priority |
|--------|---------|----------|
| Figure 1: Hallucination Propagation Chain | The Scenario | Critical |
| Figure 2: Plausibility vs Truth as Orthogonal Axes | The Mechanism | Critical |
| Figure 3: Training Loop vs Inference Loop | The Mechanism | High |
| Figure 4: Fact Check List Pattern Architecture | The Design Decision | Critical |
| Figure 5: Extraction Method Tradeoffs | The Design Decision | High |
| Figure 6: Absent vs Bypassed Pattern | The Failure Case | High |

### 2. Jupyter Notebook
Located in `Notebook/CHAPTER_2_Hallucination_When_Plausibility_Beats_Truth.ipynb`

**Uses Groq free API. Requires a free Groq API key from https://console.groq.com. No credit card needed.**

The notebook makes live API calls using `llama-3.1-8b-instant` to demonstrate hallucination as an architectural failure mode in real time.

| Cell | Purpose |
|------|---------|
| Cell 1 | Setup: installs Groq SDK, connects to API |
| Cell 2 | Triggers all three hallucination types using live Groq API calls |
| Cell 3 | Implements the Fact Check List Pattern with deterministic rule-based verifier |
| Cell 4 | Mandatory Human Decision Node: documents architectural correction on camera |
| Cell 5 | Deliberate failure case: bypasses verification layer, hallucination reaches user |

**To run:**
1. Get a free Groq API key at https://console.groq.com
2. Open Google Colab at colab.research.google.com
3. Upload `CHAPTER_2_Hallucination_When_Plausibility_Beats_Truth.ipynb`
4. Paste your Groq API key into Cell 1 where it says `YOUR_GROQ_API_KEY_HERE`
5. Run cells in order from Cell 1 to Cell 5
6. At Cell 4, read the comment block and write your architectural decision before pressing ENTER

### 3. Author's Note
Located in `Author's Note/Author's Note.pdf`

Three pages covering:
- Page 1: Design choices, scenario selection, and what was deliberately left out
- Page 2: Full tool usage documentation including Bookie corrections, Eddy's eight audit findings, Figure Architect's six figures, and the Human Decision Node correction with exact AI quote
- Page 3: Honest self-assessment against all three rubric criteria before submission

### 4. Video
Link in `Video/video_link.txt`

10 minute Show and Tell video following the Explain, Show, Try structure. The Human Decision Node moment at Cell 4 is demonstrated on camera including the exact AI output rejected and the architectural reasoning for the rejection.

---

## The Human Decision Node

The most important architectural correction in this project occurred during notebook development. The AI scaffold proposed using a second Groq API call as the verifier, reasoning that a fresh context window would reduce error reproduction.

**Exact AI output rejected:**
> "A second LLM call can serve as the verifier because it approaches the output from a fresh context window, reducing the likelihood that it will reproduce the same error."

**Reason for rejection:** A fresh context window is not a different knowledge base. The generator and verifier are the same model trained on the same corpus. A fabricated DOI that sounds plausible to the generator will also sound plausible to the verifier. The shared plausibility bias is a training objective problem, not a context problem.

**Architectural correction:** The verifier uses deterministic rule-based checks for factual claims rather than a second LLM call. For any claim it cannot resolve without external ground truth, it returns UNCERTAIN rather than PASS. UNCERTAIN is explicitly not PASS in the output schema. The architecture is honest about what it cannot verify.

---

## Three Hallucination Types Demonstrated

| Type | Mechanism | Example in Notebook |
|------|-----------|-------------------|
| Factual | Model fills gap with statistically likely content | Fabricated ACL Anthology citation percentage |
| Attribution | Model produces structurally valid citation format with invented content | GPT-3 papers with plausible but unresolvable DOIs |
| Coherence | Model loses track of a constraint it set earlier in the same response | Yes/No answer contradicts the explanation that follows |

---

## Architectural Argument

The book's master claim is: **architecture is the leverage point, not the model.**

This chapter demonstrates that claim directly. The same live model produces unverified output in Cell 5 that was halted in Cell 3. The only variable between the two cells is the presence or absence of the Fact Check List layer. The model does not change. The prompt does not change. The architecture changes. That is the argument.

---

## Tools Used

| Tool | Purpose | What It Produced |
|------|---------|-----------------|
| Bookie the Bookmaker | Chapter prose generation | All five sections. Two errors corrected by human decision. |
| Eddy the Editor | Pedagogical audit | Eight issues identified across four categories. All applied. |
| Figure Architect | Visualization planning | Six figure descriptions, three critical priority. |
| Groq API (llama-3.1-8b-instant) | Live hallucination demonstration | Real-time model outputs in Cells 2, 3, and 5 |

---

## Learning Outcomes

After reading this chapter, a student will be able to:

1. Define hallucination as an emergent property of next-token prediction and distinguish it from a software bug or model error
2. Explain why the loss function makes fluency and factual accuracy orthogonal objectives in language model training
3. Classify a given hallucination as factual, attribution, or coherence type and trace its causal origin to the training mechanism
4. Assess whether a prompt intervention reduces hallucination rate or only changes the style of hallucination
5. Design a Fact Check List verification layer into an agentic pipeline and demonstrate what breaks when it is bypassed
