# Common Challenges, Iteration & AI Fluency

**Course:** Claude 101
**Section:** Getting Started
**Status:** ✅ Complete

---

## Objective

Understand the most common failure modes when prompting Claude, develop an iteration mindset, and learn the 4D Framework for AI Fluency.

---

## Key Concepts

### Common Prompt Challenges

| Challenge | What's Happening | Fix |
|---|---|---|
| Response is too generic | Not enough context in the prompt | Add audience, role, constraints, and specifics |
| Response is too long or short | Claude is guessing at appropriate length | Be explicit: "Keep this under 100 words" or "length isn't a concern" |
| Wrong format | Claude knew what but not how | Show an example or describe the structure explicitly |
| Confident but wrong information | Claude can generate plausible but incorrect facts | Verify high-stakes facts independently; ask Claude to cite sources or indicate confidence; enable web search |
| Wrong tone | Claude defaults to professional and helpful | Describe tone in plain language or provide a writing sample |

---

### The Iteration Mindset

Your first prompt is a starting point, not a final answer. Effective Claude users:

- **Treat first drafts as starting points** — review, identify what's working, then refine
- **Give specific feedback** — "Cut the first two paragraphs and make the conclusion more action-oriented" beats "make it shorter"
- **Know when to start fresh** — if a conversation has gone off track, a new chat with a clearer prompt is often faster than redirecting

---

### The 4D Framework for AI Fluency

Developed through research by Professor Rick Dakan (Ringling College of Art and Design) and Professor Joseph Feller (University College Cork).

| Dimension | Definition |
|---|---|
| **Delegation** | Deciding what work should be done by humans, what by AI, and how to distribute tasks strategically |
| **Description** | Communicating clearly with AI — defining outputs, guiding processes, specifying behaviors |
| **Discernment** | Critically evaluating AI outputs for quality, accuracy, and appropriateness |
| **Diligence** | Using AI responsibly and ethically — maintaining transparency and accountability |

> The prompt framework (setting the stage, defining the task, specifying rules) is rooted in **Description**. Troubleshooting draws on **Discernment** and **Diligence**.

---

### Evaluating Claude for Your Workflows (Evals)

Evals are systematic ways to test how well Claude performs on tasks that matter to you.

**Simple eval approach:**
1. **Gather examples** — collect 5–10 examples of a task you do regularly
2. **Create test prompts** — write prompts that would generate similar outputs
3. **Compare outputs** — does Claude capture key info? Is the tone right? What's missing?
4. **Refine** — adjust prompts, add examples, identify where human review is essential

---

## Key Takeaways

- Most prompt failures are fixable with more specific context, format guidance, or tone direction
- Iteration is the core workflow — first drafts are starting points
- The 4D Framework (Delegation, Description, Discernment, Diligence) is the mental model for AI fluency
- Evals don't require complex infrastructure — 5–10 examples and honest comparison is enough

---

## Personal Notes

- The "confident but wrong" challenge is the most important one for enterprise/data work — independent verification is non-negotiable for anything client-facing
- Evals map directly to how I'd QA a data pipeline — sample inputs, expected outputs, gap analysis
- The 4D Framework is a useful lens for scoping AI work in client engagements: start with Delegation (what should AI do here?), then Description (how do I communicate that?)

---

## Follow-Up Questions

- Is there a standard eval framework Anthropic recommends for production use cases?
- How does the 4D Framework apply to agentic workflows where Claude is making decisions autonomously?
- What's the best way to get Claude to indicate its own confidence level on a claim?
