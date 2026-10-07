# Vector 1 (V1) — Milestone-Based Product Journey

**Purpose:** This is the north star document. Every task the team picks up should map to a milestone below. If it doesn't, it's a distraction — flag it, don't just do it.

**Guiding principle:** start with the smallest possible working thing, prove it, then grow it in visible, demonstrable steps — because the growth curve itself is what investors are buying into.

**No timelines are stated in this document on purpose** — durations are not yet known with confidence. Phases are sequenced by dependency, not by calendar. Move to the next phase when the current phase's definition of done is actually met, not when a date arrives.

This roadmap runs **two tracks in parallel**: a hardware track (gated on funding/incubation) and a software track (can run today, unblocked, against public data). They converge once real hardware exists.

---

## Phase 0 — Feasibility Spike
**Goal:** Prove, on our own team's heads, that we can detect which of 2 flickering targets a person is looking at, above chance, using a **single occipital channel**, off-the-shelf hardware, and public algorithms.

**Hardware-track definition of done:**
- [ ] Single-channel bioamplifier + Oz electrode streaming live EEG via BrainFlow/LSL.
- [ ] A simple on-screen test with **2 flickering circular patches** at two distinct, refresh-rate-compatible frequencies (e.g., 10 Hz and 12 Hz).
- [ ] Live classification of which of the 2 patches a live human is looking at, tested on at least 3 team members, logged accuracy and latency, on the single channel.

**Software-track definition of done (can start immediately, no hardware needed):**
- [ ] Single-channel FBCCA classifier reproduces reasonable accuracy on a real human SSVEP benchmark dataset (Oz channel extracted from the Tsinghua Benchmark dataset — see `02-PROTOTYPE-BUILD-GUIDE.md §4`).
- [ ] The in-house visualization/testing/debugging tool (see `07-SOFTWARE-DEVELOPMENT-GUIDE.md`) can render a 2-target flicker stimulus, replay recorded EEG against it, and show classifier output live — entirely in the browser, no hardware attached.

**Explicitly out of scope for Phase 0:** game integration, custom neural network, wireless hardware, multi-channel hardware, more than 2 targets, any UI polish beyond what's needed to run the test.

**Why this phase exists:** it's the cheapest possible test of the riskiest assumption (can we reliably detect SSVEP with a single consumer-grade dry electrode and a small team, without a neuroscientist on staff). If this fails or shows very weak accuracy, everything downstream needs to be re-planned before more money is spent — this is the single highest-leverage phase in the whole project.

---

## Phase 1 — Minimum Playable Prototype
**Goal:** A real (even if ugly) game scene, built by the India dev partner, where a small number of flickering targets each map to a distinct game action (move left/right, jump, select, fire), controlled live by a person wearing the single-channel headset, wired via USB.

**Hardware-track definition of done:**
- [ ] Game partner has a minimal Unity/Unreal scene with SSVEP-tagged UI elements or in-world objects, each flickering at a frequency validated against the display's refresh rate.
- [ ] Classifier output (FBCCA, no custom neural net yet) streams to the game engine in real time (target: under ~1.5 seconds per selection to start; will improve later).
- [ ] End-to-end loop demonstrated: person looks at target → game character performs the mapped action, live, wired, single channel, on a single PC.
- [ ] Basic "non-control state" handling — the system tolerates the user *not* trying to trigger anything without spamming false actions (using the asynchronous SSVEP dataset/methodology as a reference).
- [ ] Safety warning screen implemented before any flicker stimulus is shown (see `01-TECHNICAL-PRIMER.md §6`) — non-negotiable, ships with the very first version anyone outside the team sees.
- [ ] Decision point: if single-channel accuracy/latency is insufficient for a playable game at this target-count, expand to a second or third channel here — as a deliberate, evidence-based upgrade, not a default assumption.

**Software-track definition of done:**
- [ ] Classifier upgraded to handle the target count the game partner needs, still validated primarily against public datasets.
- [ ] Visualization/debug tool supports the full target count, shows live confidence scores per target, and can log/export session data in the same format the eventual real headset sessions will use — so nothing has to be rebuilt when hardware arrives.
- [ ] Company website (v1) is live, describing the product and the team, ready to link from any outside conversation (investor, tester, press).
- [ ] AI/ML engineer onboarded (if not already) and running their first experiments comparing FBCCA against an early EEGNet-style baseline on public datasets.

**This is the artifact used to recruit the first outside testers and refine the concept — not yet the investor demo.**

---

## Phase 2 — Investor-Demonstrable Prototype
**Goal:** A polished, reliable, repeatable live demo that a non-technical investor can watch (or even try on themselves) without the team needing to explain away glitches.

**Definition of done:**
- [ ] Accuracy and latency numbers are measured and documented (e.g., "X% selection accuracy, Y s average decision time across N testers") — investors respond to real numbers, not vague claims.
- [ ] A short, well-produced demo game loop (simple but visually clean) — think "one small level, one clear win-condition, controlled entirely by gaze-triggered SSVEP targets."
- [ ] Headset setup time for a new person is minimized and demoable quickly (electrode placement, impedance check, quick calibration if using TRCA).
- [ ] A visible, simple dashboard or overlay showing "the computer sees your brain signal" (e.g., a live frequency-spectrum visualization, reused directly from the in-house visualization tool) — this is a **storytelling tool**, not a technical necessity, but it dramatically increases investor trust and "wow factor" versus a black box.
- [ ] Repeatability tested: same demo works reliably across several different people's heads, not just the founding team's.
- [ ] Photosensitivity safety disclosure process is a standard part of every live demo.
- [ ] Pitch deck (`08-PITCH-DECK-SCRIPT.md`) and project report (`09-PROJECT-REPORT.md`) are finalized with real, measured numbers substituted for placeholders.

**This is the artifact that goes in front of investors.** Everything before this phase is internal; everything after is what the raised money is for.

---

## Phase 3 — Seed Funding & Team Scale-Up
**Goal:** Convert the funded prototype into a repeatable R&D program with a real team, not a garage project.

**Milestones:**
- [ ] Hire/contract additional ML and embedded/hardware engineering capacity as needed; formalize the game-dev partnership (contract, revenue/equity terms, IP ownership of jointly-built game content).
- [ ] Begin custom dataset collection protocol (consented users, IRB-style ethics review even if informal, clear data-ownership and privacy policy — this becomes a real asset and a real liability if mishandled).
- [ ] Begin training first custom deep-learning classifier (EEGNet-derived architecture) on combined public + proprietary data, targeting measurable improvement over the FBCCA baseline (faster decision windows and/or higher accuracy and/or less calibration needed).
- [ ] Expand channel count from single-channel toward a small multi-channel array (2–8 channels) if evidence from Phases 0–2 shows the accuracy/target-count ceiling requires it.
- [ ] Expand target count toward a usable "full control scheme" (8–12+ targets) for richer game genres.
- [ ] File provisional patents on any genuinely novel elements (stimulus design tricks, classifier architecture, headset ergonomics, single-channel accuracy techniques) — do this before showing the tech to too many external parties.

---

## Phase 4 — MVP Hardware: Custom Headgear
**Goal:** Move off borrowed/off-the-shelf boards onto a Vector 1-branded headset design.

**Milestones:**
- [ ] Industrial design pass focused on gamer-acceptable form factor (this is a real differentiator — existing research headsets look nothing like consumer gaming gear).
- [ ] Move from wired USB to **wireless** (Bluetooth LE or proprietary 2.4GHz link) for the final product, with a documented, tested latency budget (wireless must not meaningfully hurt reaction-time-sensitive gameplay).
- [ ] Onboard computation for at least the signal-cleaning/preprocessing stage (reduces wireless bandwidth and latency), full classification either onboard or on a companion app depending on power/compute trade-offs.
- [ ] Manufacturing feasibility study (unit cost at increasing volumes) with the India hardware/manufacturing ecosystem already accessible via the game dev partnership region.
- [ ] Regulatory/compliance scoping (consumer electronics + wireless certification requirements per target market — a real, sometimes slow, cost center to plan for early, not late).

---

## Phase 5 — Product Launch Readiness
**Goal:** A shippable, supportable consumer product with a real content pipeline.

**Milestones:**
- [ ] Multiple shipped/launch-partner games (not just one demo title) — this is where the first partner game dev company's role expands into an SDK-and-multiple-titles relationship, or where an SDK opens to other studios.
- [ ] Cross-user generalization: new users get good accuracy with **minimal or zero calibration** (this is the single hardest and most valuable ML milestone — it's what separates a lab demo from a real product).
- [ ] Customer support, replacement/warranty process, and safety/compliance documentation finalized.
- [ ] Go-to-market: pricing, channel (direct-to-consumer vs. bundled with the partner studio's game), and a follow-on fundraising round positioned around real user/unit numbers rather than a prototype demo.

---

## Anti-scope-creep checklist (revisit at every planning meeting)

Ask of every proposed task: **"Which phase and milestone does this serve?"** If the honest answer is "none, but it's cool," it goes in a parking-lot backlog, not the sprint. Specific traps to watch for, based on how these projects typically drift:

- Building a fully custom deep-learning model **before** the single-channel CCA/FBCCA baseline prototype works end-to-end. (Sequence matters — Phase 0/1 first.)
- Jumping to 8-channel or wireless hardware before the single-channel wired prototype has proven the core loop. (Phase 3/4 work leaking into Phase 0–2.)
- Designing final wireless headgear industrial design before the wired single-channel prototype has proven the signal pipeline. (Phase 4 work leaking into Phase 0–2.)
- Expanding target count/game complexity before accuracy and latency numbers are actually measured and good. (Feature creep masking an unproven core.)
- Chasing "read emotions / attention / other cognitive states" feature requests from partners or investors — that is a different signal type and product than SSVEP visual-cortex target selection; politely defer, don't silently absorb into scope.
- Negotiating exclusive, broad IP-ownership terms with the game dev partner before Phase 1 proves the collaboration actually works well day-to-day.
- Treating the open-source Allen Institute mouse dataset (or any non-human, non-SSVEP dataset) as a substitute for real human SSVEP validation data. See `02-PROTOTYPE-BUILD-GUIDE.md §4`.
