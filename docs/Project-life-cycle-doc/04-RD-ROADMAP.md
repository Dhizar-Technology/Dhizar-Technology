# Vector 1 (V1) — R&D Execution Plan & Demonstration Setup

This translates `03-PRODUCT-JOURNEY-MILESTONES.md` into a concrete execution plan for Phases 0–2 (the pre-funding prototype work), plus a demo-day setup checklist. No durations are specified — move to the next stage when the current stage's definition of done is met, not on a calendar.

---

## Stage-by-stage execution plan

### Stage A — Feasibility Spike (Phase 0)
**Hardware track:**
1. Order a single-channel-capable bioamplifier board and one Oz electrode + reference/ground.
2. Assemble, get BrainFlow/LSL streaming working, first live raw-signal look in OpenBCI GUI (or equivalent).
3. Build the 2-target flicker test screen (fixed refresh-rate monitor, 10 Hz/12 Hz patches). Run live classification on 3+ team members. Log accuracy/latency.
4. **Go/no-go decision point:** if single-channel accuracy is far below chance-plus-margin across testers, pause and re-plan electrode placement, stimulus design, or amplifier quality before proceeding.

**Software track (runs in parallel, doesn't wait on hardware):**
1. Download the Tsinghua Benchmark dataset (or BETA / Zhu et al.), extract the Oz channel only.
2. Implement basic CCA, then extend to FBCCA on the extracted single-channel data. Validate against benchmark accuracy reported in the literature.
3. Stand up the first version of the in-house visualization/testing/debugging tool (see `07-SOFTWARE-DEVELOPMENT-GUIDE.md`): render a 2-target flicker stimulus in-browser, replay recorded EEG segments against it, plot classifier output live.
4. Start the company website with placeholder content — this can begin the moment there's a one-paragraph description of the product, it does not need to wait for a working prototype.

### Stage B — Minimum Playable Prototype (Phase 1)
1. Kick off with the game dev partner: share the Technical Primer + Build Guide, agree on engine (Unity/Unreal), agree on the communication interface between classifier and game (e.g., local UDP/WebSocket message with target ID + confidence).
2. Game partner builds a minimal scene with the target set the team has settled on, correctly frequency-locked to display refresh.
3. Software team upgrades the classifier to handle the agreed target count + "non-control state" detection (using the asynchronous SSVEP dataset/methodology as a reference).
4. Integration sprint: live classifier output driving real game actions, end-to-end, wired, single channel. Expect rough edges — this is normal.
5. Internal playtesting across the full team + a few trusted outside testers. Document accuracy/latency per person.
6. Re-visit the single-vs-multi-channel decision here with real evidence, per the milestones document's Phase 1 decision point.
7. Onboard the AI/ML engineer here at the latest (earlier is fine if hiring finishes sooner) and start their first EEGNet-style baseline experiments against public data, in parallel with the FBCCA integration work above.

### Stage C — Investor-Demonstrable Prototype (Phase 2)
1. Polish the demo game loop (visuals, clear win-condition, short session length).
2. Add the live "brain signal" visualization overlay for storytelling, reusing the in-house visualization tool's rendering rather than building a second, separate visualization.
3. Reduce headset setup time; streamline calibration flow; add the safety/photosensitivity disclosure screen if not already present from Phase 1.
4. Repeatability testing across several new people, not just the founding team. Fix whatever breaks.
5. Rehearse the live pitch + demo end-to-end multiple times; prepare a recorded backup video in case live conditions fail on demo day (standard practice for hardware demos).
6. Finalize `08-PITCH-DECK-SCRIPT.md` and `09-PROJECT-REPORT.md` with real, measured numbers.
7. Investor demo day / meetings.

---

## Demonstration setup checklist (use this literally on demo day)

**Before the demo (dry-run stage)**
- [ ] Full dry run in the actual room/lighting conditions the demo will happen in (screen flicker visibility and EEG signal quality can both be affected by ambient lighting and electrical interference).
- [ ] Confirm the demo laptop's display refresh rate matches what the stimulus frequencies were tuned for.
- [ ] Charge/test all cables; bring spares (electrode combs, USB cables, a spare board/electrode if budget allows — hardware demos fail on cables more often than on software).
- [ ] Record a clean backup video of a successful run.

**Day before**
- [ ] Re-run the calibration/impedance check process on the exact demo hardware.
- [ ] Print or screen-share the accuracy/latency numbers slide — investors remember numbers, not vibes.
- [ ] Prepare the safety disclosure script (30 seconds, said before any investor tries the headset themselves).

**Demo day**
- [ ] Arrive early enough to re-test the full loop once in the actual room.
- [ ] Offer investors the *choice* to try it themselves after watching one clean run — a hands-on try, if it works, is far more persuasive than any slide.
- [ ] Have the backup video ready to switch to instantly if live hardware has an issue — never let a hardware glitch become the story of the meeting.

---

## Parallel workstreams to start early (don't block on the critical path)

- **Data collection & consent process** — start drafting this from Phase 1, not Phase 3. If the plan is to eventually use every demo/tester session as training data, get consent language right from the very first outside tester.
- **IP/patent scoping conversation** with a lawyer — even a single early conversation (not a full filing) before Phase 2 demos, so the team knows what's safe to show publicly.
- **Partner agreement** with the game dev company — formalize scope, IP ownership of the demo game, and expectations before kickoff, even if informally at first; formalize fully before Phase 3.
- **Company website and software tooling** (see `07-SOFTWARE-DEVELOPMENT-GUIDE.md`) — start immediately; neither depends on hardware existing.
