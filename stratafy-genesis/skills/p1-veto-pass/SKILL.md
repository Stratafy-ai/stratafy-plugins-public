---
name: p1-veto-pass
description: P1 parallel veto pass — GC + CTO + FD each emit a verdict with critical caveats; verdicts aggregate per the P1 rule
---

# P1 — GC + CTO + FD Parallel Veto Pass

The veto pass. GC, CTO, and FD each look at the seed from their domain and ask: *"Is there anything here that would kill this venture before it ships?"* Each emits a verdict; verdicts aggregate.

This is the highest-leverage pass for catching unbuildable, illegal, or unprofitable ideas before any further work happens.

## Verdict Enum

```typescript
type P1Verdict =
  | 'proceed'                    // no critical issues from my domain
  | 'proceed_with_caveats'       // proceed, but founder should know X
  | 'proceed_with_refinement'    // proceed only if seed is refined per my recommendation
  | 'stop'                       // do not proceed; this is a red-line issue
```

## Aggregation Rule

```
if any verdict === 'stop' → stop
else if any verdict === 'proceed_with_refinement' → proceed_with_refinement
else if any verdict === 'proceed_with_caveats' → proceed_with_caveats
else → proceed
```

This is the canonical `aggregateP1Verdicts` rule from `layers/genesis/server/utils/p1-verdict.ts`. Do not invent variants.

## GC P1 Output

```typescript
interface GcP1Output {
  landmines: Array<{
    description: string
    severity: 'red-line' | 'high' | 'medium' | 'low'
    jurisdictions_affected: string[]
    mitigation_path: string
  }>
  verdict: P1Verdict
  critical_caveats: string[]
  specialist_counsel_needed: boolean
  rationale: string
}
```

GC asks: *"What regulatory or corporate-structure landmines does this seed have? Is the launch jurisdiction the right venue? Does the revenue model trigger licensing requirements?"*

Anti-patterns:

- "Consult a lawyer" without naming what to consult on. → name the specific question.
- Generic "regulatory risk" without jurisdiction-specific landmines. → name the specific regulator and rule.

## CTO P1 Output

```typescript
interface CtoP1Output {
  buildable_today: boolean
  hard_technical_bits: Array<{
    capability: string
    difficulty: 'standard' | 'hard' | 'frontier'
    de_risking_path: string
  }>
  budget_allocation_advice: string  // "spend N% on Y vs Z" — opinionated
  verdict: P1Verdict
  rationale: string
}
```

CTO asks: *"Can this be built with current technology by a small team? What's actually hard? Where should the engineering budget go first?"*

Anti-patterns:

- "It's all standard tech." (everything is hard at frontier scale) → name what's actually frontier vs standard.
- Budget advice that's just "hire engineers". → say where they go first.

## FD P1 Output

```typescript
interface FdP1Output {
  unit_economics_by_wedge: Array<{
    wedge: string
    revenue_per_unit: string
    cost_per_unit: string
    contribution_margin_estimate: string
    confidence: 'low' | 'medium' | 'high'
  }>
  capital_needs_estimate_usd: string  // "$1.5M - $3M to reach default-alive at <X> customers"
  verdict: P1Verdict
  critical_caveats: string[]
  rationale: string
}
```

FD asks: *"Does this make money per unit? How much capital before default-alive? What are the capital-efficiency death traps?"*

Anti-patterns:

- "Margins to be determined." → estimate them. Bad estimate is better than none.
- Capital range so wide it's meaningless ("$500K to $50M"). → tighten with explicit assumptions.

## Parallel Dispatch

Run GC + CTO + FD in parallel. Single message, three Agent tool uses. Stream each verdict back with attribution:

```
🛡️  GC verdict: proceed_with_caveats
   Landmines: <N>, specialist counsel needed: <yes/no>
   <rationale>

⚙️  CTO verdict: proceed
   Buildable today: yes; <N> hard technical bits identified
   <rationale>

💰 FD verdict: proceed_with_refinement
   Unit economics by wedge: <N>; capital needs: <range>
   <rationale>

→ Aggregated P1 verdict: proceed_with_refinement
```

## On `stop` Verdict

If any expert returns `stop`, halt the standup. Do NOT auto-continue. Present the `stop` reasoning and let the founder decide:

- Pivot the seed and re-run intake + P1
- Override (rare — only when the founder has domain knowledge the LLM doesn't)
- Abort the run entirely

## Reference Prompts

- `layers/genesis/prompts/gc-p1.md`
- `layers/genesis/prompts/cto-p1.md`
- `layers/genesis/prompts/fd-p1.md`

## Reference Output (Legal ODR seed)

| Expert | Verdict | One-line rationale |
| --- | --- | --- |
| GC | `proceed_with_caveats` | UK MoJ regulated activity rules apply; firm panel needs SRA-authorised supervision. Specialist counsel needed for QC of T&Cs. |
| CTO | `proceed` | Buildable; hard bits are evidence verification + LLM coordination determinism. AI coordinator is frontier but de-riskable with human-in-loop fallback. |
| FD | `proceed_with_refinement` | Margins look acceptable but capital needs depend heavily on lawyer panel cost structure — refine the wedge between B2C claimants and B2B legal-aid contracts before raising. |
| Aggregated | `proceed_with_refinement` | |
