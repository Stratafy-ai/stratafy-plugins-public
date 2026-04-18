---
name: seed-intake
description: Structure a raw founder paragraph into the 6 P0 input fields with per-field signal attribution
---

# Seed Intake

Pre-P0 prerequisite. Take a raw founder paragraph (anywhere from one sentence to several), structure it into the 6 P0 input fields, and attach a signal to each field indicating where the value came from.

## Output Shape

```typescript
interface SeedIntakeOutput {
  seed: {
    product_concept: string         // REQUIRED — what is being built
    beachhead_hypothesis: string    // who's the first beachhead customer
    launch_jurisdiction: string     // where launching first
    revenue_model_hypothesis: string // how it makes money
    industry_vertical: string        // what industry / domain
    scale_ambition: 'bootstrapped' | 'lifestyle' | 'vc-scale' | 'category-defining'
  }
  field_signals: {
    [field]: 'extracted' | 'inferred' | 'missing'
  }
  questions_for_founder: Array<{
    field: string
    question: string
    inferred_value: string
  }>
  summary: string
}
```

## Signal Semantics

- **`extracted`** — the value is clearly stated or strongly implied by the founder's input. No founder confirmation required.
- **`inferred`** — the LLM filled the gap from context. **Founder MUST confirm before the seed proceeds to P0.** This is a generic-output risk vector.
- **`missing`** — could not infer with reasonable confidence. Surface as a question; founder must provide.

## Hard Rules

1. **`product_concept` cannot be `inferred` or `missing`.** If the founder hasn't given a recognisable product concept, the intake fails — ask for it.
2. **At most 4 inferred fields** before forcing a slow-down. If 5+ fields are inferred, the input is too thin — don't proceed; ask for more.
3. **Inferred values must be plausible, not generic.** Bad inferred: "B2B SaaS customers". Good inferred: "Mid-market UK law firms (5-50 lawyers) handling commercial dispute resolution".
4. **`scale_ambition` defaults to `vc-scale` only when other signals (founder language, market size language) point that way.** Default to `bootstrapped` when in doubt — this is more conservative and forces the founder to explicitly opt into the vc framing.

## Reference Prompt

The canonical seed-intake prompt lives at `layers/genesis/prompts/seed-intake.md` in the main Stratafy repo. Use the same instruction structure when running this skill conversationally.

## Reference Seed (Legal ODR)

```
Raw input: "We're building an online dispute resolution platform — claimants
and respondents file a dispute, an AI coordinator triages it, and a panel of
human lawyers adjudicates. Launching in the UK first because the small claims
court backlog is brutal. Charging a fixed fee per dispute to the filing party,
with a margin to the lawyer panel."

Structured output:
  product_concept: "Two-sided online dispute resolution platform: AI
                    coordinator triages disputes, human lawyer panel
                    adjudicates"                                    [extracted]
  beachhead_hypothesis: "UK small-claims-tier disputes (£0-£10k)
                         where court backlog is the alternative"    [extracted]
  launch_jurisdiction: "United Kingdom (England & Wales)"           [extracted]
  revenue_model_hypothesis: "Fixed fee per dispute paid by filing
                             party; margin shared with lawyer panel" [extracted]
  industry_vertical: "Legal tech / civil dispute resolution"        [extracted]
  scale_ambition: "vc-scale"                                        [inferred]

Questions for founder:
  - scale_ambition: I inferred vc-scale from the AI + panel architecture.
                    If you're aiming for a UK-only lifestyle/profitable
                    business, say so now — it changes the whole strategy.
```

## Output Quality Check

Before passing the structured seed to P0, verify:

- [ ] `product_concept` is a single coherent sentence describing what's being built (not what problem it solves)
- [ ] `beachhead_hypothesis` names a specific customer segment, not "everyone who needs X"
- [ ] `launch_jurisdiction` is a real place (country, region, or state)
- [ ] `revenue_model_hypothesis` names the payer and the price structure
- [ ] `industry_vertical` is recognisable to a domain expert
- [ ] `scale_ambition` is one of the four enum values
- [ ] All `inferred` fields have a question_for_founder

If any check fails, do not promote to P0 — ask the founder to clarify.
