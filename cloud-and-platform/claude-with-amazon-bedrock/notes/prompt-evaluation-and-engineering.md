# Prompt Evaluation & Prompt Engineering

**Course:** Claude with Amazon Bedrock
**Section:** Prompt Evaluations & Prompt Engineering
**Status:** ✅ Complete

---

## Objective

Build systematic prompt evaluation pipelines and apply prompt engineering techniques to improve Claude output quality — all within the AWS Bedrock context.

---

## Prompt Evaluation

**Three paths after writing a prompt:**
1. Test once or twice, deploy to production *(trap)*
2. Test with custom inputs, minor tweaks *(trap)*
3. Run through an evaluation pipeline for objective scoring *(recommended)*

**Core eval workflow (6 steps):**
1. Write initial prompt draft
2. Create evaluation dataset (hand-crafted or AI-generated)
3. Generate prompt variations by interpolating dataset inputs
4. Get LLM responses for each variation
5. Grade responses (typically 1–10 scale)
6. Iterate — modify prompt, repeat, compare versions

### Generating Test Datasets

Use Haiku (faster/cheaper) to generate test cases automatically. Use the JSON extraction technique (prefill + stop sequences) to get clean structured output. Save to `dataset.json` for reuse.

### Running the Eval

**Three core functions:**
- `run_prompt` — merges test case with prompt, sends to Claude, returns output
- `run_test_case` — calls `run_prompt`, grades result, returns summary
- `run_eval` — loops through dataset, assembles results

### Grader Types

| Type | Method | Best For |
|---|---|---|
| **Code graders** | `validate_json()`, `validate_python()`, `validate_regex()` | Known output formats |
| **Model graders** | Additional API call requesting reasoning + score | Quality and instruction-following |
| **Human graders** | Manual evaluation | Highest flexibility |

**Code grader scoring:** `final_score = (model_score + syntax_score) / 2`

**Model grader tip:** Always request reasoning + score — avoids default middling scores.

---

## Prompt Engineering Techniques

Applied iteratively, each measured against the eval pipeline.

### 1. Being Clear and Direct

Use action verbs in the **first line** of every prompt.

```
"Generate a one-day meal plan for an athlete that meets their dietary restrictions"
```

**Score improvement:** 2.32 → 3.92

---

### 2. Being Specific

Add guidelines to direct output.

- **Type A (Attributes):** List qualities/attributes for output (length, structure, format)
- **Type B (Steps):** Provide step-by-step reasoning process

**Score improvement:** 3.92 → 7.86

---

### 3. Structure with XML Tags

Wrap interpolated content in descriptive XML tags.

```xml
<athlete_information>
  height: 6'1", weight: 185lbs, goal: muscle gain
</athlete_information>
```

Use specific, descriptive tag names — `sales_records` beats `data`.

---

### 4. Providing Examples (One-Shot / Multi-Shot)

```xml
<example>
  <input>...</input>
  <ideal_output>...</ideal_output>
  <reasoning>Why this output is ideal</reasoning>
</example>
```

**Best for:** Corner case handling, complex output formatting, consistent style.

---

## Key Takeaways

- Eval pipelines are not optional for production prompts — they are the quality gate
- The four techniques are additive — combine them for compounding improvements
- Specificity (Type A + Type B guidelines) produces the largest single score jump
- XML tags are essential whenever injecting external content into prompts

---

## Personal Notes

- The score jump from 3.92 to 7.86 from specificity alone is the most compelling argument for structured prompting I can show a client
- Using Haiku to generate datasets while Sonnet does the actual task is the right cost-optimization pattern — carry this into any Bedrock project
- The eval-before-engineering workflow enforces measurement discipline — easy to skip but critical to maintain

---

## Follow-Up Questions

- Does AWS Bedrock have a batch inference API that could speed up eval runs?
- Can eval pipelines be run as scheduled Bedrock jobs rather than notebook-based scripts?
