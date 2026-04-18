---
name: p2-mission-vision
description: P2 — CMO drafts mission/vision from P0; Founder-Persona revises in the founder's voice
---

# P2 — Mission & Vision

The voice pass. CMO produces a stub mission and vision from the P0 output. The Founder-Persona revises both in the founder's actual voice — calibrated from `get_user_context` (chapter, values, forward anchor, lens).

This is the only **serial** step in the standup. Founder-Persona must run after CMO P2 draft is complete.

## CMO P2 Draft (Stub)

The CMO draft is a deterministic synthesis from P0 output, per `synthesiseCmoP2Draft` in `layers/genesis/server/services/genesis-orchestrator.ts`:

```typescript
{
  mission: `To ${seed.product_concept.replace(/\.$/, '')}.`,
  vision: `A world where ${
    p0.cmo.problem_statement.split(/[.!?](?:\s|$)/)[0]?.trim() ??
    seed.product_concept
  } is no longer a problem.`,
  positioning: p0.cmo.positioning_draft,
}
```

This is intentionally simple. The point of the stub is to give Founder-Persona something concrete to revise — not to be the final product.

## Founder-Persona P2 Output

```typescript
interface FounderP2Output {
  mission: string                  // 1 sentence, founder's voice
  vision: string                   // 1-2 sentences, founder's voice
  positioning: string              // 1 paragraph, founder's voice
  voice_notes: string              // brief note on what was changed and why
  honest_gaps: string[]            // things the founder needs to fill (e.g., "personal why")
}
```

## Founder Voice Calibration

Pull from `get_user_context` returned in the parent command:

- **`chapter`** — what stage the founder is in (e.g., "Early-stage founder building a strategy platform — in the intense push toward product-market fit and first revenue"). Frames the urgency and pragmatism in mission/vision tone.
- **`values`** — e.g., "agency, velocity". The mission should *embody* the values, not name them.
- **`forward_anchor`** — what the founder is trying to achieve in the next 30 days. The vision should *connect* to this, not contradict it.
- **`lens`** — how the founder reads the world. Used to choose vocabulary (e.g., infrastructure vs. application framing).

If `get_user_context` was not called or returned thin context, Founder-Persona surfaces this as an `honest_gap`:

> "I don't have enough founder context to fully personalise the voice. Mission/vision are CMO-tone-leaning — revise as you go."

## Reference Prompt

- `layers/genesis/prompts/founder-p2.md` — canonical Founder-Persona P2 prompt

## Voice Anti-Patterns

The Founder-Persona revision must NOT produce:

- Generic startup copy ("revolutionising the way X is done", "empowering Y to achieve Z")
- Buzzword stacking ("AI-powered, blockchain-enabled, web3-native")
- Vague aspirations without a specific human ("building a better future")
- Founder pronouns the founder doesn't use (if the founder uses "I" in their seed paragraph, don't flip to "we" without flagging it)

## Reference Output (Legal ODR seed)

CMO draft (stub):

```
Mission: To build a two-sided online dispute resolution platform: AI
         coordinator triages disputes, human lawyer panel adjudicates.
Vision:  A world where UK small-claims-tier disputes (£0-£10k) where
         court backlog is the alternative is no longer a problem.
Positioning: <CMO P0 positioning_draft verbatim>
```

Founder-Persona revision (founder voice — values: agency + velocity, chapter: early-stage UK legal-tech founder):

```
Mission: To make small-claims-scale dispute resolution work — fairly,
         quickly, and without anyone needing to wait two years for a
         court date.
Vision:  Disputes resolved in days, not years. Lawyers paid for
         judgment, not paperwork. Claimants and respondents never have
         to navigate the courts again.
Positioning: <revised positioning emphasising speed and access>
Voice notes: Cut "AI coordinator" and "human lawyer panel" jargon
             from mission — that's the architecture, not the mission.
             Mission and vision now lead with the human outcome.
Honest gaps: Personal why — founder's own reason for caring about
             this isn't in the seed; founder should add.
```

## Quality Gates

Before passing P2 output to P3:

- [ ] Mission is one sentence
- [ ] Vision is 1-2 sentences and connects to the founder's forward anchor
- [ ] Positioning uses the canonical template (`<product> is a <category> for <beachhead> who <pain>; unlike <alternative>, we <differentiator>`)
- [ ] Voice notes explain what changed from CMO draft and why
- [ ] Honest gaps surfaced if founder context is thin
