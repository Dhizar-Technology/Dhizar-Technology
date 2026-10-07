# Vector 1 (V1) — Investor Overview
### Built by Dhizar Technology

*A brain-controlled game input device, starting with a working single-channel prototype and a signed game-studio partner.*

---

## The one-line pitch

Vector 1 (V1) turns a gamer's eyes and visual cortex into a game controller — no hand controller, no learning curve, using a wearable headset that detects which flickering on-screen target a player is focused on, in real time.

## The problem

Game input has been stuck on the same paradigm — hands on a controller/keyboard — for 40 years. Every major platform company (Snap, Meta, Valve) has publicly invested in "hands-free" or "thought-adjacent" input research in the last several years, because the next generation of interactive hardware (AR glasses, always-on wearables, accessibility-first devices) doesn't have room for a traditional controller. There is currently **no shipping consumer product** that lets a mainstream gamer control a game with a lightweight, affordable, brain-signal-based headset. The closest attempt — NextMind, a Paris startup building almost exactly this — was acquired by Snap in 2022 for its AR research value, and its consumer developer kit was discontinued. The market gap it left is still open.

## The solution

A lightweight EEG headset reads signals from the **visual cortex** — the part of the brain that responds, involuntarily and measurably, when a person looks at a flickering light. Our game partner designs on-screen elements (targets, buttons, abilities) that each flicker at a slightly different rate. Our software detects which one the player is looking at, purely from their brain signal, and triggers the matching game action.

This is not speculative neuroscience — it is a well-established, decades-researched signal (SSVEP: Steady-State Visually Evoked Potential), validated across hundreds of published academic studies and at least one prior commercial product. **We are not the first to prove the science works. We are positioned to be the first to make it a real gaming product.**

## Our key strategic choice: prove it on one channel first

Most research and consumer devkits default to 8+ EEG channels from day one, which is expensive and slow to iterate on. We are deliberately starting with a **single-channel controller** — proving the core loop works with the minimum possible hardware, before spending on multi-channel arrays. This means:
- A dramatically lower cost to reach our first fundable proof point.
- A faster, more reliable, more repeatable live demo (seconds of setup, not minutes).
- A clear, evidence-based path to expand channel count only once the core mechanism is validated — not a guess baked into the budget upfront.

## Why now

- **The enabling hardware got cheap.** Research-grade EEG hardware that cost tens of thousands of dollars a decade ago is now available off-the-shelf for a few hundred dollars, with open-source software ecosystems (OpenBCI: 400+ published papers using its hardware).
- **A comparable company was just acquired.** NextMind's 2022 acquisition by Snap is a concrete signal that strategic acquirers value this category — and it left the consumer gaming use case unaddressed.
- **AI made the hard part easier.** Modern deep learning is directly applicable to squeezing much higher accuracy and speed out of brain-signal classification than was possible even a few years ago — this is our core technical differentiator and long-term moat, not the underlying SSVEP concept itself, which is public science.
- **India's gaming and hardware ecosystem is maturing fast**, giving us access to a strong, cost-effective game-development partner and a path to affordable manufacturing for the eventual headset.
- **We can build and validate our software today, without hardware**, using public human SSVEP-EEG research datasets — so our software team is not idle while we wait on funding for hardware.

## Market opportunity

Multiple independent market research firms currently size the global brain-computer interface market in the low-billions-of-dollars range, with projected double-digit compound annual growth rates through the early-to-mid 2030s (estimates vary meaningfully by firm and methodology — figures available on request, and the team will cite specific named reports before presenting to any individual investor rather than relying on vague aggregate figures). Gaming and entertainment is consistently named across these reports as one of the fastest-growing application segments, alongside healthcare/rehabilitation. We are not claiming to capture this whole market — we are targeting the **gaming input device** wedge of it, a category that, unlike medical BCI, doesn't require years of clinical/regulatory approval before shipping a first product.

## Business model (illustrative — refine with real unit economics before fundraising)

| Stage | Revenue driver |
|---|---|
| Near-term | Grant/prototype-stage funding, developer-kit pre-orders (à la NextMind's $399 dev kit model), studio partnership/licensing deals |
| Mid-term | Direct-to-consumer headset sales, bundled with flagship partner game(s) |
| Long-term | Platform model: SDK licensing to other game studios, hardware + software (subscription for advanced calibration-free/cross-user models), potential strategic acquisition by a platform player (AR/VR, social, or console company) — consistent with the exit path NextMind demonstrated |

## Competitive landscape (summary — full detail available on request)

| Player | Focus | Gap we exploit |
|---|---|---|
| NextMind (acquired by Snap, 2022) | Visual-cortex BCI for AR/gaming | Product discontinued post-acquisition; category currently has no active consumer player |
| Anthriq (India) | Biosignal infrastructure/SDK (EEG/EMG), our own inspiration | Platform/infrastructure play, not a vertically-integrated consumer gaming product |
| Emotiv, g.tec, OpenBCI | EEG hardware & research tools | Research/prosumer tools, not consumer gaming products |
| Neuralink, Synchron and other invasive BCI companies | Medical, invasive | Different regulatory category, different market, years from consumer gaming relevance |

## Our roadmap (see `03-PRODUCT-JOURNEY-MILESTONES.md` for full detail)

1. **Feasibility prototype** (single-channel, in progress) — off-the-shelf hardware, open-source algorithms, proof that we can detect focus on a flickering target from a live EEG signal using one electrode.
2. **Playable game prototype** — built with our first game-studio partner in India, wired USB headset, a handful of controllable actions in a real (if small) game.
3. **Investor demo** — polished, repeatable, numbers-backed live demonstration (this is the stage this document supports).
4. **Funded R&D** — custom deep-learning model trained on our own collected data, expanded channel count and control scheme, first custom hardware design.
5. **Consumer product** — wireless custom headset, multiple launch titles, SDK for other studios.

We are deliberately starting as small and cheap as possible and growing the demonstration step by step — every funding milestone is tied to a concrete, visible capability upgrade, not just a roadmap slide.

## The ask

*(Team to fill in with actual figures before sending to any investor: raise amount, use of funds breakdown [team, hardware/manufacturing R&D, data collection, partner/legal costs], runway, and specific milestones the raise will fund — tie directly to Phase 3/4 of `03-PRODUCT-JOURNEY-MILESTONES.md`.)*

## Team & partnership

- **Founding team:** 3 members today — business development/fundraising leadership, plus technical leadership across hardware/signal-processing and software direction.
- **Currently hiring:** one software/web developer and one AI/ML engineer — see `07-SOFTWARE-DEVELOPMENT-GUIDE.md` for exactly what these roles will own.
- **First strategic partner:** an India-based game development studio, already collaborating on prototype content — de-risks the "will anyone actually build games for this" question that most BCI-for-gaming pitches struggle to answer credibly.

---

**A note on positioning, for whoever presents this deck:** be honest that this is an early prototype-stage company, not a company with existing revenue or shipped hardware. Investors in deep-tech/hardware are used to funding pre-revenue technical proof points — the credibility comes from (a) grounding the pitch in real, cited prior art and market data rather than hype, (b) showing a working, repeatable live demo, and (c) being precise about what is proven today versus what is roadmap.
