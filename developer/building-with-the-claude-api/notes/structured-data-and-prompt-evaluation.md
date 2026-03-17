# Structured Data & Prompt Evaluation

**Course:** Building with the Claude API
**Section:** API Fundamentals
**Status:** ✅ Complete

---

## Objective

Learn how to extract clean structured data from Claude and how to systematically evaluate prompt performance.

---

## Structured Data Generation

### The Problem

Claude automatically adds markdown formatting, headers, and commentary when generating JSON or code. Users often want raw data for copy/paste or downstream processing.

### The Solution: Prefill + Stop Sequences

```
User message:    Request for structured data
Assistant prefill: Opening delimiter (e.g., ```json)
Stop sequence:   Closing delimiter (e.g., ```)
```

Claude sees the prefilled message, assumes it already started, generates only the requested content, and stops at the delimiter.

**Result:** Raw structured output with no extra formatting or commentary — works for JSON, Python, regex, lists, or any structured type.

---

## Prompt Evaluation

### Why It Matters

Engineers commonly under-test prompts. Two common traps:
1. Test once or twice, deploy to production
2. Test with a few custom inputs, make minor tweaks

The right path: **run through an evaluation pipeline for objective scoring before iterating and deploying.**

### The Typical Eval Workflow (6 Steps)

1. **Write initial prompt draft** — create a baseline to optimize
2. **Create evaluation dataset** — 3 examples or thousands; hand-written or LLM-generated
3. **Generate prompt variations** — interpolate each dataset input into the prompt template
4. **Get LLM responses** — feed each variation to Claude, collect outputs
5. **Grade responses** — use a grader system (e.g., 1–10 scale), average scores for overall prompt performance
6. **Iterate** — modify prompt based on scores, repeat, compare versions

### Generating Test Datasets

- Use Claude (Haiku for speed/cost) to generate test cases automatically
- Dataset structure: array of JSON objects with a `task` property
- Use prefill + stop sequences to get clean JSON output
- Save to `dataset.json` for reuse across eval runs

### Running the Eval

**Three core functions:**
- `run_prompt` — merges test case with prompt, sends to Claude, returns output
- `run_test_case` — calls `run_prompt`, grades result, returns summary
- `run_eval` — loops through dataset, calls `run_test_case` for each, assembles results

### Grader Types

| Type | How It Works | Best For |
|---|---|---|
| **Code graders** | Programmatic checks (length, syntax, regex validation) | Known output formats |
| **Model graders** | Additional API call to evaluate output — request strengths/weaknesses/score | Quality and instruction-following |
| **Human graders** | Person evaluates responses | Highest flexibility, most time-intensive |

**Model grader tip:** Always ask for reasoning + score, not just a score — avoids default middling scores.

**Code grader pattern:**
```python
validate_json()    # returns 10 if valid JSON, 0 if error
validate_python()  # returns 10 if valid AST, 0 if error
validate_regex()   # returns 10 if compiles, 0 if error

final_score = (model_score + syntax_score) / 2
```

---

## Key Takeaways

- Prefill + stop sequences = the cleanest path to raw structured output
- Eval pipelines are not optional for production prompts — they are the quality gate
- Dataset generation can be automated with Claude itself
- Combine model graders + code graders for semantic + syntactic validation

---

## Personal Notes

- The eval workflow maps directly to how I'd QA a data pipeline: test inputs, expected outputs, gap analysis, iterate
- Code-based grading (validate_json, validate_python) is fast, cheap, and objective — should be the first grader added to any eval pipeline
- Using Haiku to generate datasets and grade responses while Sonnet does the actual task is a smart cost-optimization pattern

---

## Follow-Up Questions

- What open-source eval frameworks does Anthropic recommend?
- How do you handle eval datasets that become stale as prompts improve?
- Can model-based graders be made more consistent with a lower temperature?
