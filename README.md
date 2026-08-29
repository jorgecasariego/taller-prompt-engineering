# Taller Prompt Engineering

Hands-on exercises and experiments completed as part of the **Taller Prompt Engineering** course.

This repository documents my practical exploration of prompt design, including prompt specificity, controlled experiments, few-shot prompting, role and system instructions, decomposition, token efficiency, and output reliability.

Rather than only recording final prompts, the labs include the prompts, model outputs, comparisons, failures, and observations used to evaluate each technique.

---

## Course Progress

### Day 1 — Foundations

Topics covered:

- Prompt engineering fundamentals
- The four building blocks of a prompt
- Assumption auditing
- Prompt specificity
- Token budgeting
- Output variability
- Prompt ablation

**Lab:** [Day 1 — Diagnose and Rebuild a Prompt](day-01/day-1-lab.md)

#### Key takeaway

Prompt quality depends less on prompt length and more on providing the model with the right context, explicit constraints, and a clearly defined expected output.

---

### Day 2 — Core Techniques

Topics covered:

- Clear instructions and output specifications
- Zero-shot prompting
- Few-shot prompting
- Example selection and example bias
- Role prompting
- System and standing instructions
- Durable constraints
- Prompt decomposition
- Multi-step prompting
- Output verification and QA

**Lab:** [Day 2 — Core Techniques](day-02/day-2-lab.md)

#### Experiments

The Day 2 lab compares several prompting strategies through controlled experiments:

- Explicit output format vs. format ablation
- Zero-shot vs. balanced few-shot classification
- Deliberately biased few-shot examples
- Persistent role and behavioral constraints
- Role/tone changes with identical factual extraction
- Single-prompt vs. multi-prompt decomposition
- Dedicated QA as a final decomposition stage

#### Key takeaway

Start with a precise zero-shot prompt and add complexity only when it solves an observable problem.

Few-shot examples are useful when examples clarify ambiguity or edge cases, but they are not automatically necessary. For larger tasks, decomposition can improve verifiability and make errors easier to isolate, even when it does not automatically produce a better first draft.

---

## Repository Structure

```text
taller-prompt-engineering/
├── README.md
├── day-01/
│   ├── day-1-lab.md
│   ├── estimate_tokens.py
│   ├── long-input.txt
│   └── trimmed-input.txt
├── day-02/
│   └── day-2-lab.md
└── day-04/
```

---

## Approach

The exercises in this repository follow an experimental approach:

1. Define a prompting hypothesis or technique.
2. Run the prompt and preserve the actual model output.
3. Change one relevant variable when possible.
4. Compare the results.
5. Document both successful and unsuccessful outcomes.
6. Prefer conclusions supported by observed results rather than expected behavior.

This means that an experiment is still useful when a technique produces no measurable improvement. For example, a few-shot prompt that performs identically to a zero-shot prompt is evidence that the additional examples may not be necessary for that particular task.

---

## Tools and Topics

The repository currently explores:

`Prompt Engineering` · `LLMs` · `Zero-shot` · `Few-shot` · `Prompt Ablation` · `Role Prompting` · `System Prompts` · `Prompt Decomposition` · `Structured Output` · `Token Efficiency` · `LLM Evaluation`

---

## About

Created as part of my ongoing exploration of **AI-assisted software engineering** and practical techniques for working effectively with large language models.