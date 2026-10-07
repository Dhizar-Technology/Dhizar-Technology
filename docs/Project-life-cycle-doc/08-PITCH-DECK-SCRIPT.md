# Vector 1 (V1) — Pitch Deck Script
### Dhizar Technology

**How to use this document:** this is the content and speaking script for a live investor pitch deck — not the final designed slides. Hand this to whoever builds the actual slide deck (in-house or via a designer), and hand the speaking notes to whoever presents. Fill in every `[bracketed]` placeholder with real figures before presenting — never present a placeholder as if it were a real number. Timelines are intentionally not specified anywhere in this script; speak about sequencing ("first we prove X, then Y"), not dates.

---

### Slide 1 — Title
**On slide:** Vector 1 (V1) — Dhizar Technology. "See it. Trigger it." *(or team's preferred tagline)*
**Say:** "We're building Vector 1 — a game controller you operate with your visual cortex. No hands, no buttons, no learning curve."

### Slide 2 — The problem
**On slide:** "Game input hasn't changed in 40 years. The next generation of hardware — AR glasses, wearables, accessibility devices — doesn't have room for a controller."
**Say:** "Every major platform company — Snap, Meta, Valve — has invested in hands-free input in the last few years. Nobody has shipped a consumer brain-signal gaming controller. The closest company to try, NextMind, got acquired by Snap in 2022 and their consumer product was discontinued. That gap is still open."

### Slide 3 — The solution
**On slide:** Diagram: player → flickering targets on screen → EEG headset → software detects target → game action fires.
**Say:** "When you look directly at something flickering at a steady rate, your visual cortex synchronizes to that exact frequency — involuntarily, every time, no training needed. We put several different flicker rates on screen. Our software reads which frequency dominates your brain signal and knows exactly what you're looking at. That's the entire control scheme."

### Slide 4 — Why this is real science, not hype
**On slide:** "SSVEP: decades of academic research. Hundreds of published studies. One prior commercial product."
**Say:** "This is called SSVEP — Steady-State Visually Evoked Potential. It's not speculative. It's one of the most well-validated signals in BCI research. We're not claiming to have discovered something new. We're claiming to be the team that turns known science into a real gaming product."

### Slide 5 — Our key strategic choice: start with one channel
**On slide:** "Most teams start with 8+ EEG channels. We start with one."
**Say:** "The riskiest question isn't 'can 8 channels do this accurately' — it's 'can any affordable setup, run by a small team, reliably detect this at all.' We're answering that with the cheapest, fastest, most demoable setup possible: a single electrode. That's a deliberate sequencing decision, not a limitation we're stuck with — we expand channel count once we have evidence, not before."

### Slide 6 — Live demo *(or backup video if live isn't available yet)*
**On slide:** [Live demo / recorded video]
**Say:** "Here's the loop working live: [presenter] is wearing the headset, looking at one of these two targets. Watch the game respond." *(Follow the safety disclosure script from `04-RD-ROADMAP.md` before any investor tries it themselves.)*

### Slide 7 — Traction / what's proven today
**On slide:** "[X]% selection accuracy. [Y] seconds average decision time. Tested on [N] people." *(fill in only once measured — see Milestones Phase 2 definition of done)*
**Say:** "These are real, measured numbers across multiple people, not a single best-case run. We'll be upfront about what's proven today versus what's still roadmap."

### Slide 8 — Software track: building ahead of hardware
**On slide:** "We're not waiting on funding to build software. Our classifier and tooling are already validated against real, public, human EEG research data."
**Say:** "While hardware is gated on funding, our software team — the classifier, and our own internal visualization and testing tool — has been building and testing against real published human SSVEP datasets. The moment hardware exists, it's a plug-in, not a rebuild. That derisks the technical side of this raise significantly."

### Slide 9 — Market opportunity
**On slide:** "[Cite 2–3 specific named market research reports and figures here, sourced and linked — see note below.] Gaming/entertainment: one of the fastest-growing BCI application segments."
**Say:** "We're not claiming the whole BCI market. We're targeting the gaming input wedge — the part of this market that doesn't require years of medical/clinical regulatory approval before shipping."
*(Note for whoever presents: cite specific market reports with firm names and links, not an unsourced aggregate figure — sophisticated investors respect the honesty.)*

### Slide 10 — Competitive landscape
**On slide:** Table — NextMind (acquired, discontinued), Anthriq (infrastructure play, not gaming), Emotiv/g.tec/OpenBCI (research tools), Neuralink et al. (invasive, medical). "We are the only vertically-integrated consumer gaming controller in this space today."
**Say:** "Everyone adjacent to us is either discontinued, an infrastructure/SDK company, a research-tools company, or in an entirely different regulatory category. We're building the one thing none of them are: an end-to-end consumer gaming product."

### Slide 11 — Business model
**On slide:** Near-term: grants, dev-kit pre-orders, studio licensing. Mid-term: direct-to-consumer + bundled with partner game. Long-term: SDK platform, subscription tier, strategic acquisition path.
**Say:** "NextMind proved the dev-kit-first model and the strategic-acquisition exit path both work in this category. We intend to follow a similar arc, but ship an actual game alongside the hardware from the start."

### Slide 12 — Roadmap
**On slide:** Feasibility prototype (single channel, in progress) → Playable game prototype → Investor demo (this stage) → Funded R&D (custom model, more channels) → Consumer product.
**Say:** "Every stage is a concrete, visible capability upgrade — not just a line on a roadmap slide. This raise funds the next two stages."

### Slide 13 — Team
**On slide:** Founding team (3), currently hiring: software/web developer, AI/ML engineer. First strategic partner: India-based game studio.
**Say:** "We already have the partner most BCI-for-gaming pitches struggle to answer for — someone actually building a game for this. We're growing the technical team now to match."

### Slide 14 — The ask
**On slide:** "[Raise amount]. Use of funds: [team / hardware R&D / data collection / partner & legal costs, with rough percentages]. This funds: [specific Phase 3/4 milestones]."
**Say:** "This raise takes us from a proven single-channel demo to a funded R&D program — hiring out the ML and hardware team, starting our own consented data collection, and beginning the custom model that becomes our long-term moat."

### Slide 15 — Close
**On slide:** "Vector 1. The first real gaming controller for the visual cortex." Contact info.
**Say:** "We're not asking you to believe in speculative neuroscience. We're asking you to fund the team that turns twenty years of published, validated science into the product nobody's shipped yet."

---

## Presenter notes — general

- Always run the safety disclosure (30 seconds, see `04-RD-ROADMAP.md`) before any investor tries the headset personally.
- Never present a number that hasn't actually been measured. If asked for a number that doesn't exist yet, say so plainly and explain what stage will produce it (per Milestones Phase 2 definition of done).
- Have the backup demo video ready at all times — a hardware glitch should never become the story of the meeting.
- Be explicit and comfortable saying "this part is proven, this part is roadmap" — investors in early deep-tech respect precision over hype.
