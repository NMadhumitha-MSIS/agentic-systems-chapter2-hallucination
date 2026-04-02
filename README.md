# Chapter 2: Hallucination: When Plausibility Beats Truth
### Design of Agentic Systems with Case Studies
**Author:** Madhumitha Nandhikatti | Student ID: 002304994

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
├── chapter/
│   └── chapter_2_hallucination.md        # Full chapter prose (5 sections)
├── notebook/
│   └── Take_home_Mid_Term.ipynb          # Runnable Jupyter Notebook
├── authors_note/
│   └── authors_note.pdf                  # 3 page Author's Note
└── video/
    └── video_link.txt                    # YouTube or Vimeo link
```

---

## Deliverables

### 1. Chapter Prose
Located in `chapter/chapter_2_hallucination.md`

The chapter is structured in five sections following the Tetrahedron framework:

- **The Scenario** — A concrete failure case where hallucination causes observable harm
- **The Mechanism** — Why next-token prediction makes plausibility and truth orthogonal objectives
- **The Design Decision** — The Fact Check List Pattern as an architectural layer
- **The Failure Case** — What breaks when the verification layer is absent or bypassed
- **The Exercise** — One modification the reader can make to trigger the failure themselves

### 2. Jupyter Notebook
Located in `notebook/Take_home_Mid_Term.ipynb`

**No API key required. No credits needed. Runs on Google Colab CPU runtime with Python 3.**

The notebook contains 5 cells:

| Cell | Purpose |
|------|---------|
| Cell 1 | Setup: imports only, no installation required |
| Cell 2 | Triggers all three hallucination types using pre-recorded LLM outputs |
| Cell 3 | Implements the Fact Check List Pattern and demonstrates pipeline halt |
| Cell 4 | Mandatory Human Decision Node: documents architectural correction |
| Cell 5 | Deliberate failure case: bypasses verification layer, hallucination reaches user |

**To run:**
1. Open Google Colab at colab.research.google.com
2. Upload `Take_home_Mid_Term.ipynb`
3. Run cells in order from Cell 1 to Cell 5
4. At Cell 4, read the comment block and write your architectural decision before pressing ENTER

### 3. Author's Note
Located in `authors_note/authors_note.pdf`

Three pages covering:
- Page 1: Design choices and what was deliberately left out
- Page 2: Tool usage, AI outputs used, and the Human Decision Node correction
- Page 3: Honest self-assessment against the rubric before submission

### 4. Video
Link in `video/video_link.txt`

10 minute Show and Tell video following the Explain, Show, Try structure. The Human Decision Node moment at Cell 4 is demonstrated on camera.

---

## The Human Decision Node

The most important architectural correction in this project occurred during notebook development. The AI scaffold proposed using a second LLM call as the verifier on the grounds that a fresh context window would reduce error reproduction.

This was rejected for the following reason: a fresh context window is not a different knowledge base. The generator and verifier are the same model trained on the same corpus. A fabricated DOI that sounds plausible to the generator will also sound plausible to the verifier. The shared plausibility bias is a training objective problem, not a context problem.

The correction replaced the second LLM call with a deterministic rule-based verifier. For any claim it cannot resolve without external ground truth, the verifier returns UNCERTAIN rather than PASS. This forces a human or external resolver to make the final decision rather than accepting a second opinion from the same biased source.

---

## Three Hallucination Types Demonstrated

| Type | Description | Example in Notebook |
|------|-------------|-------------------|
| Factual | Model states a false statistic confidently | 34.7% NLP citation figure that does not exist |
| Attribution | Model fabricates a plausible looking citation | Three GPT-3 papers with real looking but invalid DOIs |
| Coherence | Model contradicts itself within one response | Answers yes then immediately proves the answer is no |

---

## Architectural Argument

The book's master claim is: architecture is the leverage point, not the model.

This chapter demonstrates that claim directly. The hallucinated outputs in Cell 2 and Cell 5 are produced by the same pre-recorded content. The only variable between Cell 3 (pipeline halts) and Cell 5 (hallucination surfaces) is the presence or absence of the Fact Check List layer. The model does not change. The prompt does not change. The architecture changes. That is the argument.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Bookie the Bookmaker | Generated all five prose sections of the chapter |
| Eddy the Editor | Audited prose for jargon before intuition and architecture without mechanism |
| Figure Architect | Flagged three high assertion zones for priority figures |
| Courses | Generated five Bloom's Taxonomy learning outcomes |

---

## Learning Outcomes

After reading this chapter, a student will be able to:

1. Define hallucination as an emergent property of next-token prediction and distinguish it from a software bug
2. Explain why fluency and factual accuracy are orthogonal objectives in language model training
3. Classify a given hallucination as factual, attribution, or coherence type and trace its causal origin
4. Assess whether a prompt experiment reliably elicits hallucination and identify what architectural condition it exposes
5. Design a Fact Check List verification layer into an agentic pipeline and demonstrate what breaks when it is bypassed
