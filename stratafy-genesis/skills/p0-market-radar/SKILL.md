---
name: p0-market-radar
description: P0 parallel pass — CMO produces problem/beachhead/positioning, Radar scans environmental signals
---

# P0 — CMO + Radar Parallel Pass

The first parallel pass of the standup. CMO and Radar dispatch concurrently against the structured seed.

## CMO P0 Output

```typescript
interface CmoP0Output {
  problem_statement: string         // 2-4 sentences naming the pain, the cost, and who feels it
  beachhead_validation: string      // does the founder's beachhead hypothesis hold? what would invalidate it?
  positioning_draft: string         // one paragraph: "<product> is a <category> for <beachhead> who <pain>; unlike <alternative>, we <differentiator>"
  signals: Array<{                  // signals the CMO surfaces from the market framing
    name: string
    signal_type: 'opportunity' | 'threat' | 'shift'
    rationale: string
  }>
  assumptions: Array<{              // assumptions baked into the framing
    statement: string
    confidence: 'low' | 'medium' | 'high'
    invalidation_test: string
  }>
}
```

## Radar P0 Output

```typescript
interface RadarP0Output {
  signals: Array<{
    name: string
    domain: 'market' | 'technology' | 'regulatory' | 'macro' | 'competitive'
    direction: 'tailwind' | 'headwind' | 'neutral'
    time_horizon: 'now' | '6-18mo' | '18mo+'
    rationale: string
    source_hint: string             // where the founder/team should look to validate
  }>
  market_dynamics: string           // 1 paragraph synthesising the signals into a "what's actually happening here" narrative
}
```

## Parallel Dispatch

Run CMO and Radar **in parallel** via the plugin's tool-restricted sub-agents:

- `subagent_type: "stratafy-genesis:cmo-expert"`
- `subagent_type: "stratafy-genesis:radar-expert"`

Single message, two Agent tool uses, one per persona. Each sub-agent is declared with `tools: Read` — no MCP tool surface inherits, so the input-token footprint stays tight. Pass the structured seed JSON in the Agent prompt; do NOT staple conversation history or the full canonical prompt file — the agent body already carries the persona and output schema.

Stream each back with attribution:

```
🎯 CMO weighing in on market framing…
   <CMO P0 output>

📡 Radar scanning the environment…
   <Radar P0 output>
```

## CMO Persona (V1 slim)

Voice: directional, market-first, opinionated about the wedge. The CMO does NOT hedge — if the beachhead hypothesis is wrong, they say so. If positioning is muddled, they sharpen it.

Anti-patterns:

- "It depends on the market." → name the market.
- "Could appeal to a broad audience." → name the specific beachhead.
- Vague positioning like "the leading platform for X". → use the canonical positioning template (`<product> is a <category> for <beachhead> who <pain>; unlike <alternative>, we <differentiator>`).

## Radar Persona (V1 slim)

Voice: scanning, evidentiary, cites where to look. Radar surfaces signals that the founder might not see — regulatory shifts, technology curve points, competitor moves, macro tailwinds/headwinds.

Anti-patterns:

- Generic "AI is changing everything" signals. → name the specific shift and its time horizon.
- Signals without source hints. → always tell the founder where to validate (industry report, specific regulator, specific competitor's recent move).

## Reference Prompts

The CMO and Radar personas + output schemas are defined in the plugin-local agent files, which are what the sub-agents actually load at runtime:

- `agents/cmo-expert.md`
- `agents/radar-expert.md`

(The upstream canonical prompts at `layers/genesis/prompts/cmo-p0.md` and `radar-p0.md` in the main Stratafy repo are the source of truth for the persona definitions — but the plugin runtime cannot access them. The agent files are compressed versions kept in sync manually.)

## Quality Gates

Before passing P0 output to P1:

- [ ] Problem statement names a specific pain felt by a specific group
- [ ] Beachhead validation is a directional yes/no with reasoning, not "let's test"
- [ ] Positioning draft uses the canonical template
- [ ] At least 3 signals across CMO + Radar combined
- [ ] At least 2 assumptions surfaced with invalidation tests
- [ ] No signal lacks a source hint
