---
name: p3-reconciliation
description: P3 — resolve cross-expert conflicts into a coherent L1 strategy proposal; flag unresolvable conflicts as pending decisions
---

# P3 — Reconciliation

The compression step. Take all expert outputs (P0 + P1 + P2) and reconcile them into a single coherent L1 strategy proposal — foundation + 3-5 top-level strategies + the critical signals/risks/assumptions that justify them.

This is where the "AI founding team" becomes one voice without losing the per-expert provenance.

## Input

```typescript
interface ReconcileP3Input {
  seed: GenesisSeed
  cmo_p0: CmoP0Output
  radar_p0: RadarP0Output
  gc_p1: GcP1Output
  cto_p1: CtoP1Output
  fd_p1: FdP1Output
  cmo_p2_draft: CmoP2Draft
  founder_p2: FounderP2Output
  // CHRO output if dispatched (V1 may skip CHRO)
  chro_p1?: ChroP1Output
}
```

## Output

```typescript
interface ReconcileP3Output {
  foundation: {
    mission: string                    // from founder_p2.mission
    vision: string                     // from founder_p2.vision
    values: Array<{ name: string; description: string; authored_by: ExpertRole }>
    principles: Array<{ name: string; description: string; authored_by: ExpertRole }>
    beliefs: Array<{ name: string; description: string; authored_by: ExpertRole }>
  }
  strategies: Array<{
    name: string
    strategy_type: 'product' | 'go-to-market' | 'finance' | 'people' | 'regulatory'
    description: string
    horizon_phase: 'now' | 'next' | 'later'
    priority: 'low' | 'medium' | 'high' | 'critical'
    authored_by: ExpertRole | 'reconciler'  // 'reconciler' if cross-expert merge
  }>
  signals: Array<{
    name: string
    domain: string
    direction: 'tailwind' | 'headwind' | 'neutral'
    impact_level: 'low' | 'medium' | 'high' | 'critical'
    rationale: string
    authored_by: ExpertRole
    linked_strategy_names: string[]
  }>
  risks: Array<{
    name: string
    severity: 'low' | 'medium' | 'high' | 'critical'
    mitigation_status: 'open' | 'mitigation_planned' | 'mitigated'
    authored_by: ExpertRole
    linked_strategy_names: string[]
  }>
  assumptions: Array<{
    statement: string
    confidence: 'low' | 'medium' | 'high'
    invalidation_test: string
    authored_by: ExpertRole
    linked_strategy_names: string[]
  }>
  pending_decisions: Array<{
    description: string
    conflict_summary: string          // "<expert A> says X / <expert B> says Y"
    options: Array<{ option: string; advocated_by: ExpertRole; reasoning: string }>
  }>
  capture_score_inputs: {
    founder_problem_fidelity: string
    market_framing_coherence: string
    strategic_coherence: string
    measurability_of_commitments: string
    risk_surface_adequacy: string
  }
}
```

## Reconciliation Logic

For each potential conflict, apply this resolution order:

### 1. Auto-resolve (silent merge)

- Two experts surface the same signal/risk/assumption with compatible framings → merge under the more specific framing, attribute to both as `authored_by: <expert with more specific>`, link to the union of strategies.
- Wedge naming differs cosmetically (CMO calls it "UK small-claims claimants", FD calls it "B2C dispute filers") → reconcile to one canonical name; flag the resolution in the wedge's description.

### 2. Auto-resolve (rule-based)

- **GC `proceed_with_caveats` regulatory landmines** become risks at severity ≥ `high`. No founder vote needed — they're surfaced for visibility, not decision.
- **FD `unit_economics` confidence: 'low'`** entries become assumptions, not metrics. Founder validates later.
- **CTO `hard_technical_bits` of difficulty: 'frontier'`** become risks AND assumptions (the assumption being "frontier capability is achievable on this timeline").

### 3. Surface as `pending_decision`

When two experts genuinely disagree on direction (not just framing), the conflict cannot auto-resolve. Examples:

- CMO says "expand to Tier-2 EU jurisdictions in Year 2"; GC says "regulatory clearance for Tier-2 EU is 18-24 months and requires local counsel — Year 2 is unrealistic". → pending decision: *expansion timeline*.
- CTO says "build the AI coordinator from scratch on open-source models for control"; FD says "buying a third-party LLM API saves 6 months of build time and $400K". → pending decision: *AI coordinator build vs buy*.

Each `pending_decision` includes the full options + advocacy chain so the founder sees who said what.

### 4. Reconciler attribution

Any entity that is the *result of* a reconciler merge — not authored by a single expert — gets `authored_by: 'reconciler'`. Tag the entity's description with which experts contributed the inputs.

## Reference Prompt

- `layers/genesis/prompts/reconcile-p3.md`

## Quality Gates

Before passing P3 output to write-through:

- [ ] Foundation has mission + vision (from P2)
- [ ] At least 3 top-level strategies, no more than 5
- [ ] Each strategy has `horizon_phase` and `priority` set
- [ ] Every signal/risk/assumption is linked to at least one strategy
- [ ] Every entity has `authored_by` set (either an expert or `'reconciler'`)
- [ ] Pending decisions list explicitly empty `[]` rather than omitted (founder must see "no unresolved conflicts" rather than infer it)
- [ ] `capture_score_inputs` populated for downstream Capture Score Engine

## Anti-Patterns

- **Averaging conflicting positions.** "CMO says expand fast, GC says go slow → expand at moderate pace." → No. Surface the conflict.
- **Dropping a signal because it's awkward.** If Radar surfaced a regulatory headwind that contradicts CMO's market framing, the signal stays — that's the point of having a radar.
- **Inflating strategy count.** Five strategies max. If you have six, two of them are sub-strategies of another — fold them.
- **Generic strategy names.** "Product strategy", "GTM strategy" — these are categories, not strategies. Use named strategies like "Build the lawyer-panel two-sided marketplace" or "Win UK small-claims-tier dispute volume in 18 months".
