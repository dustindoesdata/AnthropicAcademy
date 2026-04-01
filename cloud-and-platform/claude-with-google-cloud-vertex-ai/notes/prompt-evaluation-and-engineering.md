# Prompt Evaluation & Prompt Engineering

**Course:** Claude with Google Cloud's Vertex AI
**Section:** Prompt Evaluation & Prompt Engineering Techniques
**Status:** ✅ Complete

---

## Objective

Build systematic prompt evaluation pipelines and apply prompt engineering techniques — all within the Vertex AI context. Patterns are identical to the Anthropic and Bedrock courses with Vertex-specific SDK calls.

---

## Prompt Evaluation

**Three paths after writing a prompt:**
1. Test once or twice, deploy *(trap)*
2. Test with custom inputs, minor tweaks *(trap)*
3. Run through an evaluation pipeline for objective scoring *(recommended)*

**Core eval workflow:**
1. Write initial prompt draft
2. Create evaluation dataset (hand-crafted or AI-generated)
3. Execute prompts against dataset
4. Grade responses (1–10 scale)
5. Calculate average score
6. Iterate — modify prompt, repeat, compare versions

### Generating Test Datasets

Use Haiku (faster/cheaper) to generate test cases. Use pre-fill + stop sequences for clean JSON output. Save to file for reuse.

### Running the Eval

**Three core functions:**
- `run_prompt` — merges test case with prompt, sends to Claude
- `run_test_case` — calls `run_prompt`, grades result, returns summary
- `run_eval` — loops through dataset, assembles all results

### Grader Types

| Type | Method | Best For |
|---|---|---|
| **Code graders** | `validate_json()`, `validate_python()`, `validate_regex()` | Known output formats |
| **Model graders** | Additional API call requesting reasoning + score | Quality, instruction-following |
| **Human graders** | Manual evaluation | Highest flexibility |

**Final score:** `(model_score + syntax_score) / 2`

**Model grader tip:** Always request reasoning — avoids default middling scores.

---

## Prompt Engineering Techniques

### 1. Being Clear and Direct

Use action verbs in the **first line** of every prompt.

```
"Generate a one-day meal plan for an athlete that meets their dietary restrictions"
```
**Score improvement:** 2.32 → 3.92

### 2. Being Specific

- **Type A (Attributes):** List output qualities (length, structure, format)
- **Type B (Steps):** Provide reasoning steps for the model to follow

**Score improvement:** 3.92 → 7.86

### 3. Structure with XML Tags

Wrap interpolated content in descriptive XML tags.

```xml
<athlete_information>
  height: 6'1", weight: 185lbs, goal: muscle gain
</athlete_information>
```

### 4. Providing Examples (One-Shot / Multi-Shot)

```xml
<example>
  <input>...</input>
  <ideal_output>...</ideal_output>
  <reasoning>Why this output is ideal</reasoning>
</example>
```

Best for corner cases, complex formatting, consistent style.

---

## Key Takeaways

- Eval pipelines are the quality gate — test systematically before deploying
- The four techniques compound — apply all for maximum improvement
- Specificity produces the largest single score jump
- XML tags are essential when injecting any external content

---

## Personal Notes

- The eval workflow is identical across Anthropic, Bedrock, and Vertex AI — the pattern is universal, only the SDK call changes
- Vertex AI's access to Google's Haiku equivalent makes dataset generation cost-efficient on the Google Cloud stack

---

## Follow-Up Questions

- Does Vertex AI offer a batch inference API for running large eval datasets more efficiently?
- Can Vertex AI Pipelines be used to automate scheduled eval runs?
