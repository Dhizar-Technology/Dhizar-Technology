# Vector 1 (V1) — Prototype Build Guide
### Market hardware, open-source software, datasets, and competitive landscape

This document exists so the team **stops evaluating options and starts building**. Everything below is either purchasable today or downloadable today. Prices are approximate (USD) and will drift — re-verify before purchase orders.

---

## 1. Direct prior art — study these before designing anything

| Product | What it was | Status | Why it matters to us |
|---|---|---|---|
| **NextMind** (Paris, founded 2017) | A $399 dev-kit headband reading visual-cortex signals via dry EEG electrodes, using flickering "tags" on tracked objects to trigger actions — almost exactly our concept, aimed at gaming/AR | Acquired by **Snap Inc.** in March 2022 for undisclosed terms after raising $5.6M total; tech absorbed into Snap Lab for AR Spectacles, dev kit discontinued | This is the closest comparable exit in our space. It proves (a) investors will fund this, (b) a strategic acquirer (social/AR platform) is a plausible exit path, and (c) the market gap it leaves (no active consumer SSVEP gaming controller) is currently **open**. Study any public teardown/reviews of the NextMind dev kit and SDK design. |
| **Anthriq** (Bengaluru, founded 2020, formerly Nexstem) | 13-channel wireless research-grade EEG headset ("Instinct") + biosignal SDK infrastructure (EEG/EMG/EXG), positioned as infrastructure for other builders, not a gaming product | Active, $6.23M raised (Info Edge, RedStart Labs, Gruhas) | Our inspiration. Note they position as **infrastructure/platform**, not an end consumer gaming product — this is a gap we occupy instead: a vertically-integrated gaming controller, not a generic SDK. Also a potential India-based hardware/component partner or watch-list competitor. |
| **Emotiv** (San Francisco) | Consumer/prosumer EEG headsets (Insight, EPOC X, Flex, MN8), used broadly in BCI research including SSVEP experiments | Active, established since 2011 | Not gaming-specific but the most mature consumer EEG hardware+SDK ecosystem; good for early prototyping and as a competitive/pricing benchmark. |
| **g.tec medical engineering** (Austria) | Research-grade SSVEP BCI systems, including turnkey "intendiX" SSVEP spelling products | Active, clinical/research focus | Gold-standard accuracy benchmark; too expensive/clinical for consumer gaming but useful for algorithm validation. |
| **OpenBCI** (Brooklyn) | Fully open-source EEG hardware (Cyton board, Ultracortex headset) with 400+ published research papers using it, explicitly lists SSVEP as a supported paradigm | Active | **Our primary v1 prototyping hardware family** — see Section 2. Its single-channel-friendly boards make it a good fit for our single-channel-first strategy. |
| **NeuroSky** | Low-cost single-channel consumer EEG chips (used in toys like Star Wars Force Trainer, MindWave) | Active, low-end | Directly relevant now: a proven, cheap, single-channel feasibility path. Channel placement is usually forehead (frontal), not occipital, so treat as a fast sanity-check on signal chain electronics, not a source of real SSVEP accuracy numbers. |

**Action item:** assign one team member to obtain and review any public teardown, patent filings, or SDK documentation from NextMind's dev-kit era (patents often stay public post-acquisition) — this de-risks our hardware design by learning from a company that already solved and shipped this exact problem.

---

## 2. Hardware to buy for the prototype — single-channel first

Per our sequencing decision (see `01-TECHNICAL-PRIMER.md §3`): prove the core loop on **one occipital channel (Oz)**, wired/USB, before spending on multi-channel hardware. Save 8-channel arrays and wireless headgear for after the core loop is validated and funded.

### Recommended single-channel v1 stack (~$150–350 total)

| Item | Vendor | Approx. price | Why |
|---|---|---|---|
| **A single-channel-capable bioamplifier board** (e.g., OpenBCI Cyton used in 1-channel mode, or a lower-cost single/few-channel biosignal amplifier module) | OpenBCI Shop or equivalent open-hardware biosignal amplifier vendor | ~$150–500 depending on board chosen | Only one occipital channel (Oz) plus reference/ground is needed for Phase 0 — using a multi-channel board in single-channel mode is fine and keeps a clear upgrade path later; a dedicated cheap single-channel board is also acceptable if budget is the binding constraint. |
| **Dry comb electrode (5mm) or a simple gelled electrode + headband** | OpenBCI Shop or generic EEG electrode supplier | ~$30–60 | One electrode at Oz, one reference (typically ear/mastoid), one ground. Avoids the cost and assembly time of a full headset frame for the feasibility spike. |
| **Monitor with a known, fixed refresh rate (60Hz or 144Hz)** | Any | — | Needed because flicker frequencies are constrained by refresh-rate divisibility (see Technical Primer §5). |

**Upgrade path (do this only after Phase 0 succeeds):** move to the **Ultracortex Mark IV headset** (~$900 complete, or ~$350 as a frame you assemble) for a proper multi-electrode mount once the team is validating 4–8-channel classification, per the milestones document. There is no need to buy this before the single-channel spike is done.

**Budget alternative for an even faster sanity check:** a low-cost single-channel consumer EEG toy/dev chip (NeuroSky-class) can validate basic amplifier/signal-chain electronics in days, but its typical forehead placement means it is **not** a substitute for real occipital SSVEP validation — treat any accuracy numbers from it as informal, not reportable.

### Links to bookmark
- OpenBCI Shop: https://shop.openbci.com/
- OpenBCI Cyton Board product page: https://shop.openbci.com/products/cyton-biosensing-board-8-channel
- OpenBCI Ultracortex Mark IV (upgrade path, not v1): https://shop.openbci.com/products/ultracortex-mark-iv
- OpenBCI GUI (free, open-source visualization/streaming software): included with hardware, docs at openbci.com
- Emotiv store (benchmark reference): https://www.emotiv.com/

---

## 3. Open-source software stack

| Layer | Tool | Notes |
|---|---|---|
| Raw signal streaming | **BrainFlow** (open-source library, supports OpenBCI + many other boards) | Unifies data acquisition across hardware — future-proofs us if we later switch boards or add channels. |
| Cross-platform streaming protocol | **Lab Streaming Layer (LSL)** | Industry-standard for syncing EEG stream timing with stimulus timing — critical because SSVEP classification accuracy depends on precise stimulus-EEG time alignment. |
| SSVEP classification | Implement **CCA / FBCCA** in Python (scikit-learn + numpy/scipy is sufficient) — well-documented algorithms, many open reference implementations exist in academic GitHub repos attached to the papers below | This is not "research" — it's implementation of known, published methods. Do not over-invest here for v1; a single-channel FBCCA implementation is genuinely simple. |
| Deep-learning baseline (for later phases) | **EEGNet** and its SSVEP-focused derivatives (open PyTorch/TensorFlow implementations exist on GitHub from multiple academic groups) | Use as a starting architecture rather than designing from scratch. |
| Game engine | **Unity** or **Unreal Engine** (game-dev partner's choice) | Needs to (a) render precisely-timed flicker stimuli tied to actual display refresh, (b) receive classifier output over a local socket/USB HID and map to in-game actions. |
| Visualization/debug | **OpenBCI GUI** for raw hardware signal inspection, **plus our own in-house visualization/testing/debugging web tool** (see `07-SOFTWARE-DEVELOPMENT-GUIDE.md`) for everything hardware-independent: stimulus authoring, classifier debugging, replaying public datasets, and team-wide access without needing physical hardware on hand. |

---

## 4. Open, human SSVEP-EEG datasets — use these to build and test *before* real hardware exists

This is the most important section for the software track described in `07-SOFTWARE-DEVELOPMENT-GUIDE.md`. Training and validating a classifier needs data that actually resembles what our headset will produce: **human scalp EEG, occipital electrodes, real SSVEP flicker stimuli.**

> **A note on the Allen Institute Brain Observatory data (brain-map.org):** this dataset is calcium-imaging and Neuropixels electrode recordings taken directly from the visual cortex of head-fixed **mice** during invasive experiments. It is excellent neuroscience data, but it is not human scalp EEG, does not use an SSVEP flicker paradigm matched to our stimulus design, and would not validate anything about our pipeline's real-world performance. **Do not use it as a substitute for human SSVEP data.** It is listed at the bottom of this section as an optional, unrelated resource only for the ML engineer's general background reading — never as pipeline validation data.

| Dataset | Size | Notes | Where to find |
|---|---|---|---|
| **"Benchmark Dataset for SSVEP-Based BCIs" (Tsinghua, Wang et al.)** | 64-channel EEG, 35 subjects, 40-target speller, frequencies 8–15.8 Hz | The most widely cited SSVEP benchmark in the field. **Because it's 64-channel, we can extract just the Oz channel (or Oz + a couple of neighbors) to simulate our single-channel setup** — this is our primary tool for developing and validating the single-channel classifier before hardware exists. | Published via IEEE TNSRE; dataset hosted by the Tsinghua BCI group — search "Tsinghua SSVEP Benchmark Dataset" (also referenced as "Wang et al. 2016") |
| **BETA dataset** | 64-channel, 70 subjects, same 40-target paradigm | Larger companion dataset to the benchmark above; same single-channel-extraction approach applies. Good for deep-learning training set size once the ML track starts. | Search "BETA SSVEP dataset Tsinghua" |
| **Open dataset for wearable SSVEP-BCI (Zhu et al.)** | 8-channel, 102 subjects, 12-target, compares wet vs. dry electrodes | Directly relevant because it validates **dry electrodes** (what we plan to use) against wet/gel, and has few enough channels that isolating a single occipital channel is straightforward. Important evidence for technical credibility with investors. | PMC article: https://pmc.ncbi.nlm.nih.gov/articles/PMC7916479/ |
| **Open dataset for human SSVEPs across 1–60 Hz** | 64-channel, 30 subjects, single-target sweep across many frequencies | Useful for choosing our optimal stimulation frequency range and understanding user fatigue/comfort trade-offs. | Published in *Scientific Data* (Nature): https://www.nature.com/articles/s41597-024-03023-7 |
| **OpenBMI dataset** | 62-channel, 54 subjects, motor imagery + ERP + SSVEP (4-target SSVEP subset) | Multi-paradigm dataset; useful if the team later explores hybrid control schemes. | Referenced widely in academic literature under "OpenBMI" |
| **Asynchronous SSVEP dataset (63-channel, 24 subjects)** | Includes "non-control state" data (user not focusing on any target) | Critical for real gameplay: a real game needs to know when the user *isn't* trying to trigger anything, not just classify among fixed targets. | Tandfonline: https://www.tandfonline.com/doi/full/10.1080/27706710.2024.2418650 |

**Action item:** the first task for whoever leads the software/ML side is to download the Tsinghua Benchmark dataset, extract the Oz channel only, and reproduce a basic single-channel FBCCA classifier. This validates the team's implementation *before* any custom hardware exists, and gives a credible internal milestone to show investors early ("our single-channel pipeline already reproduces a reasonable accuracy on the standard academic benchmark, using only one electrode").

---

## 5. Competitive & adjacent landscape (for awareness, not imitation)

- **Neuralink, Synchron, Paradromics** — invasive, medical-first, not consumer gaming competitors in the near term; different regulatory universe entirely.
- **Meta's discontinued EEG wristband research / CTRL-labs acquisition** — Meta pivoted to **EMG** wrist-based control (reading muscle signals, not brain signals) for its own AR/VR input; not a direct competitor to visual-cortex SSVEP but shows a major player validated "thought-adjacent hands-free input" as a category worth building for.
- **Valve / OpenBCI collaboration (publicly discussed in gaming/BCI circles)** — Valve has publicly expressed interest in biometric/EEG input for gaming; worth monitoring, not a current shipping competitor.
- **Cognixion, Neurable, Cumulus Neuro** — mostly assistive/communication-focused rather than mainstream gaming — another gap V1 can differentiate into.

---

## 6. Rough total cost of proving feasibility (hardware side, before any investor money)

| Item | Cost |
|---|---|
| Single-channel bioamplifier board + electrode + reference/ground | ~$180–560 |
| Monitor (if team doesn't already have a fixed-refresh-rate display) | ~$150–300 |
| Dev/compute (a mid-range laptop most teams already have) | $0 (existing) |
| Misc (electrode gel/comb spares, cables) | ~$50–100 |
| **Total** | **~$380–960** |

This is deliberately smaller than an 8-channel-from-day-one budget, in line with the "start smallest possible, show growth" approach the whole roadmap is built on. The software track (below) costs effectively $0 in hardware, since it runs entirely on public datasets and a laptop.

---
*Next: see `03-PRODUCT-JOURNEY-MILESTONES.md` for how this hardware/software/dataset stack turns into a phase-by-phase plan, and `07-SOFTWARE-DEVELOPMENT-GUIDE.md` for the full software track.*
