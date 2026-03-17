# Prompt Engineering Techniques

**Course:** Building with the Claude API
**Section:** Prompt Engineering
**Status:** ✅ Complete

---

## Objective

Apply a systematic set of prompt engineering techniques to improve Claude's output quality, measured through eval pipelines.

---

## The Approach

Start with a poor initial prompt → apply techniques one at a time → evaluate after each → observe score improvements over time.

**Example task:** Generate a one-day meal plan for athletes based on height, weight, physical goal, and dietary restrictions.
**Baseline score:** 2.32 (with basic prompt and less capable model)

---

## Technique 1: Being Clear and Direct

Use simple, direct language with **action verbs in the first line** to specify the exact task.

**Structure:** Action verb + clear task description + output specifications

```
"Generate a one-day meal plan for an athlete that meets their dietary restrictions"
```

**Score improvement:** 2.32 → 3.92

---

## Technique 2: Being Specific

Add guidelines or steps to direct model output.

**Type A — Attributes:** List qualities/attributes desired in output (length, structure, format)
**Type B — Steps:** Provide specific reasoning steps for the model to follow

| Type | Controls | When to Use |
|---|---|---|
| A (Attributes) | Output characteristics | Almost all prompts |
| B (Steps) | How model arrives at answer | Complex problems needing broader perspective |

**Score improvement:** 3.92 → 7.86

---

## Technique 3: Structure with XML Tags

Use XML tags to organize and delineate different content sections within prompts.

```xml
<athlete_information>
  height: 6'1", weight: 185lbs, goal: muscle gain
</athlete_information>
```

**When to use:** Whenever interpolating large amounts of content — XML tags help Claude distinguish between different types of information.

**Tag naming:** Use descriptive, specific names (`sales_records` beats `data`).

---

## Technique 4: Providing Examples (One-Shot / Multi-Shot)

Provide examples in prompts to guide model behavior.

- **One-shot** = single example
- **Multi-shot** = multiple examples

**Structure:**
```xml
<example>
  <input>...</input>
  <ideal_output>...</ideal_output>
  <reasoning>Why this output is ideal</reasoning>
</example>
```

**Best for:**
- Corner case handling (sarcasm, edge scenarios)
- Complex output formatting (JSON structures)
- Clarifying expected response quality or style

**Best practices:**
- Add context for corner cases ("be especially careful with sarcasm")
- Include reasoning explaining why the output is ideal
- Use highest-scoring eval examples as templates
- Place examples after main instructions

---

## Key Takeaways

- First line clarity is the highest-leverage single change in any prompt
- Specificity (Type A + Type B guidelines) produces the largest score jumps
- XML tags are essential when injecting external content into prompts
- Examples are most valuable for edge cases and complex formatting requirements
- Each technique is additive — combine them for compounding improvements

---

## Personal Notes

- The score jump from 3.92 to 7.86 from adding specificity alone is the most compelling evidence for structured prompting — that's nearly doubling quality
- XML tags are something I should be using in every prompt that interpolates data — it makes the structure explicit and unambiguous
- The "use highest-scoring eval examples as templates" tip is a feedback loop I should build into every eval pipeline

---

## Follow-Up Questions

- Is there a point of diminishing returns where adding more guidelines hurts performance?
- How do XML tags interact with system prompts vs user messages?
- Can multi-shot examples negatively bias Claude toward examples that don't fit new inputs?
