# Sophyworks Steppers

> A library of structured reasoning workflows for product, strategy, and architecture decisions.

## What This Is

A curated collection of structured thinking workflows for product managers, product owners, tech leads, and architects. Each file in this repository encodes a specific reasoning framework — from problem framing and root cause analysis to positioning, pricing, architecture decisions, and executive communication — as a sequence of prompts that guides a large language model through the framework one step at a time. The goal is to turn proven mental models into composable, repeatable artifacts that any team can drop into their own AI workflow.

## What Are Steppers

These workflows are called **steppers**, the core construct of [Sophyworks](https://sophyworks.ai) — an AI-assisted decision-making platform built to bridge the gap between strategy and execution for business leaders, product teams, and strategy groups. A stepper is a JSON-defined prompt sequence that walks an LLM through a framework in disciplined, sequential steps, with each step building on the previous output rather than collapsing the entire problem into a single prompt. This structure forces the rigor that complex decisions demand: articulate the thesis before defending it, decompose before deciding, stress-test before committing.

## Schema

Every stepper in this repository follows the same JSON schema. The top-level object describes the stepper itself; the `steps` array defines the ordered sequence of prompts.

```json
{
  "name": "Human-readable name of the stepper",
  "alias": "short-unique-id",
  "description": "One-sentence summary of what the stepper produces",
  "visibility": "private",
  "category": "discovery | strategy | decision | ...",
  "tags": ["tag1", "tag2"],
  "steps": [
    {
      "title": "Step title",
      "description": "What this step accomplishes",
      "prompt": "The full instruction sent to the LLM at this step",
      "outputType": "text",
      "autoPlay": false,
      "iconName": "LucideIconName"
    }
  ]
}
```

**Field reference**

- `name` — display name shown in the UI.
- `alias` — short unique identifier used for routing and references.
- `description` — single sentence describing the artifact the stepper produces.
- `category` — primary classification for browsing and filtering.
- `tags` — secondary classifications, including methodology names and target roles.
- `steps[].prompt` — the exact text fed to the LLM at that step. Steps are executed in order, with each output appended to the conversation context for the next step.
- `steps[].iconName` — icon identifier from the [Lucide](https://lucide.dev) icon set.
- `outputType` and `autoPlay` — reserved for future UI behaviors; current default values are `"text"` and `false`.

## How to Use

### With Sophyworks

Import the JSON file directly into your Sophyworks workspace. The platform handles step orchestration, context passing between prompts, and output capture automatically.

### With Any LLM

Each stepper is portable. To run one manually with any model that accepts conversational input:

1. Open the stepper file and read the steps in order.
2. Submit `steps[0].prompt` as the first user message, prepended with the context the framework operates on (the idea, decision, or problem under analysis).
3. Submit `steps[1].prompt` as the next user message in the same conversation, so the model has full access to the previous output.
4. Continue until the final step. The last step typically synthesizes the artifact.

For programmatic use, a minimal loop in pseudo-code:

```python
import json

stepper = json.load(open("stepper-file.json"))
context = "Your initial input — the thesis, problem, or decision under analysis."
messages = [{"role": "user", "content": context}]

for step in stepper["steps"]:
    messages.append({"role": "user", "content": step["prompt"]})
    response = call_llm(messages)
    messages.append({"role": "assistant", "content": response})
    print(f"--- {step['title']} ---\n{response}\n")
```

The same pattern works with any provider's chat completion API.

## Index

Steppers are organized by the stage of work they support. The library grows over time; this index reflects the current state.

### Discovery & Diagnosis

- **Problem Intelligence Brief** — Transforms a vague symptom into a well-formed problem with actors, context, constraints, and success criteria.
- **Root Cause Protocol** — Drills from observable symptom to structural cause using 5 Whys plus Ishikawa, separating cause from correlation.
- **JTBD Extraction** — Extracts the functional, social, and emotional jobs the customer is hiring the product to do.

### Structuring & Strategy

- **MECE Decomposition** — Breaks a problem into mutually exclusive and collectively exhaustive parts.
- **Hypothesis Tree** — Decomposes a central bet into testable sub-hypotheses with falsification criteria for each leaf.
- **TOWS to Action** — Crosses internal SWOT with external context and converts each quadrant into prioritized initiatives.
- **Divergence Mapping** — Forces seven or more structurally different solutions before any convergence, mapped on effort and novelty.

### Decision & Risk

- **Pre-mortem** — Projects failure eighteen months out and reverses to surface blind spots before execution.
- **Reversible vs Irreversible** — Classifies the decision as a one-way or two-way door and calibrates the level of rigor required.
- **Build vs Buy vs Partner** — Decides how to obtain a capability based on total cost, time-to-value, and strategic dependency.
- **Trade-off Matrix** — Compares competing paths across measurable dimensions and identifies dominance.

### Architecture & Execution

- **Architecture Decision Record (ADR)** — Documents a technical decision with context, options considered, choice, and accepted consequences.
- **Implementation Handoff Brief** — Produces a unified spec across three lenses (backend, frontend, QA) that lets a senior team start development independently and consistently.
- **Definition of Ready / Done** — Establishes DoR and DoD validated by PO, Dev, and QA for stories and tasks.
- **Feature Kill Criteria** — Defines, before launch, the metrics and timing that would justify killing the feature.

### Governance & Engagement

- **Skin in the Game Audit** — Maps who decides, who executes, and who bears the consequences, exposing dangerous asymmetries.
- **Stakeholder Power Map** — Crosses power, interest, and current position to define an engagement strategy per quadrant.

### Go-to-Market

- **Wedge Strategy** — Designs the narrow, defensible entry point before market expansion.
- **Positioning Canvas** — Articulates category, alternatives, unique attributes, value, and ICP in the Dunford pattern.
- **Pricing Hypothesis** — Formulates the value-capture hypothesis (model, metric, range) and the experiment to validate it.

### Planning & Communication

- **Roadmap Slicing** — Breaks strategic ambition into now / next / later with explicit promotion criteria between bands.
- **Pyramid Executive Summary** — Structures executive communication starting from the conclusion, in the Minto pattern.

## Contributing

Contributions are welcome. New steppers, refinements to existing prompts, translations, and real-world case studies all add value to the library. If you apply a framework consistently in your own product, strategy, or architecture work and want it captured as a stepper, open a pull request following the JSON schema used here, or open an issue first to discuss the framework. The library grows by accumulating field-tested thinking, not theoretical taxonomies.

**Pull request checklist**

- File follows the schema in the [Schema](#schema) section.
- `alias` is unique across the repository.
- Each step's prompt is self-contained and references previous outputs only when they exist in context.
- The final step produces a clear, named artifact.
- A short note in the PR description explains the framework, its source, and the typical use case.

**Translations**

Steppers are language-agnostic in structure. Translated versions of an existing stepper are welcome as separate files with a language suffix in the filename and a `language` tag in the JSON.

## License

This repository is released under the MIT License. You are free to use, modify, and redistribute the steppers in personal and commercial contexts, with attribution. See `LICENSE` for the full text.
