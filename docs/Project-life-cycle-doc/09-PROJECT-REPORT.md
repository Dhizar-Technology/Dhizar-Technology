# Vector 1 (V1) — Project Report
### Dhizar Technology

*Prepared for investors, incubators, and grant/accelerator applications. Fill in every `[bracketed]` placeholder with real figures before sharing externally — do not present placeholder figures as measured results.*

---

## 1. Executive Summary

Dhizar Technology is building **Vector 1 (V1)**, a brain-computer interface (BCI) that lets a person control a video game using only their visual cortex — no hand controller required. The system uses **Steady-State Visually Evoked Potential (SSVEP)**, a well-established, decades-researched EEG signal: when a person looks at a light flickering at a constant frequency, their visual cortex synchronizes to that frequency involuntarily. By placing several differently-flickering targets on screen and reading which frequency dominates the player's occipital EEG signal, the system determines which target the player is looking at and triggers the corresponding in-game action.

The closest prior commercial attempt at this category, NextMind, was acquired by Snap Inc. in 2022 and its consumer developer kit was subsequently discontinued, leaving no active shipping consumer product in this space. Dhizar Technology intends to fill that gap with a vertically-integrated product — hardware, control software, and a purpose-built game — rather than a general-purpose developer SDK.

The company is following a deliberately staged, capital-efficient approach: proving the core detection mechanism on a **single EEG channel** before investing in multi-channel or wireless hardware, and running software development (the classifier, an internal visualization/testing tool, and the company's public-facing website) in parallel against public, real, human SSVEP-EEG research datasets — so that software progress is not blocked on hardware funding.

## 2. The Problem

Game input hardware has not fundamentally changed in four decades: a handheld controller or a keyboard and mouse. This paradigm does not extend well to the next generation of interactive hardware — AR/VR headsets, always-on wearables, and accessibility-first devices — where a traditional controller is often impractical or impossible. Major platform companies (Snap, Meta, Valve) have each publicly invested in hands-free or "thought-adjacent" input research over the past several years, validating the category's importance, but **no mainstream consumer product currently ships a brain-signal-based game controller.**

## 3. The Solution

Vector 1 reads EEG signal from the player's visual cortex via a lightweight headset. A partnered game studio designs on-screen interactive elements — buttons, directions, abilities — each flickering at a distinct, carefully chosen frequency. Vector 1's software determines, purely from the player's brain signal, which element they are looking at, and triggers the corresponding game command. The underlying signal-processing methods (Canonical Correlation Analysis and its filter-bank extension) are established, published, and require no proprietary breakthrough to implement — the company's differentiation is in productizing this science into a reliable, affordable, consumer-ready gaming controller, and later, in a custom-trained neural network that reduces response latency and removes the need for per-user calibration.

## 4. Technical Approach

### 4.1 Signal and mechanism
Full technical detail is maintained in `01-TECHNICAL-PRIMER.md`. In summary: EEG electrodes placed over the occipital region of the scalp detect the frequency-locked response of the visual cortex to a flickering visual stimulus. A signal-processing pipeline (acquisition → bandpass/notch filtering → windowing → classification → command output) determines which of several candidate frequencies the incoming signal most strongly matches, and maps that to a game command.

### 4.2 Hardware strategy — single-channel first
Rather than beginning with an eight-or-more-channel array (the common default in academic and consumer-devkit designs), Dhizar Technology is validating the core mechanism on a **single occipital channel** first. This reduces the cost and complexity of the first proof point substantially, produces a faster and more reliable live demonstration, and defers the added cost and complexity of multi-channel hardware until there is measured evidence it's needed. Full reasoning: `01-TECHNICAL-PRIMER.md §3` and `02-PROTOTYPE-BUILD-GUIDE.md §2`.

### 4.3 Software strategy — building ahead of hardware
Because the physical headset cannot be developed further without funding or incubation support, the software team is developing and validating the classifier, the game-communication protocol, and an internal visualization/debugging tool entirely against **public, peer-reviewed human SSVEP-EEG datasets** (notably the Tsinghua SSVEP Benchmark dataset and its companion BETA dataset, and a dry-electrode wearable SSVEP dataset), with a single channel extracted from each multi-channel recording to match our hardware plan. This ensures that once physical hardware exists, integrating it is a matter of plugging in a new live data source to an already-built and tested pipeline, not a ground-up rebuild. Full detail: `07-SOFTWARE-DEVELOPMENT-GUIDE.md`.

The company explicitly does **not** use non-human or non-SSVEP neuroscience datasets (such as the Allen Institute's mouse visual-cortex recordings) as a substitute for this validation work — that data, while scientifically valuable in its own domain, is recorded invasively from a different species using a different paradigm and would not produce meaningful validation of a human scalp-EEG SSVEP product.

### 4.4 The machine learning roadmap
The team distinguishes clearly between what is proven today and what is planned:
- **Today / near-term:** classification via CCA and Filter-Bank CCA — established, training-free, published methods.
- **Near-to-mid-term:** benchmarking and fine-tuning an open-source deep-learning architecture (EEGNet or a comparable published SSVEP-specific model) against the classical baseline, on public datasets.
- **Longer-term:** training a proprietary model on the company's own consented user data, targeting sub-second decision windows and strong accuracy for brand-new users with little to no calibration — the company's intended long-term technical moat.

## 5. Product Roadmap

The company organizes its work into sequential phases, gated by evidence rather than calendar time (full detail in `03-PRODUCT-JOURNEY-MILESTONES.md`):

1. **Feasibility Spike** — prove single-channel, two-target SSVEP detection works, above chance, on real people.
2. **Minimum Playable Prototype** — a real, if small, game controlled end-to-end by the live classifier, built with the company's game-studio partner.
3. **Investor-Demonstrable Prototype** — a polished, repeatable, numbers-backed live demonstration.
4. **Seed Funding & Team Scale-Up** — hiring out ML/hardware capacity, beginning consented proprietary data collection, first custom deep-learning model.
5. **MVP Hardware** — a custom-designed, wireless, Vector 1-branded headset.
6. **Product Launch Readiness** — multiple launch titles, cross-user generalization with minimal calibration, go-to-market.

## 6. Market Opportunity

Multiple independent market research firms currently estimate the global brain-computer interface market in the low-billions-of-dollars range, with double-digit compound annual growth projected through the early-to-mid 2030s; estimates vary by firm and methodology. *(The team will substitute specific, named, and linked market research figures here before this report is shared with a specific investor, rather than relying on an unsourced aggregate number.)* Gaming and entertainment is consistently cited as one of the fastest-growing BCI application segments, alongside healthcare and rehabilitation. Unlike medical BCI applications, a consumer gaming input device does not require years of clinical/regulatory approval before it can ship, making it a comparatively fast path to a real product.

## 7. Competitive Landscape

| Player | Category | Status | Gap Dhizar Technology exploits |
|---|---|---|---|
| NextMind | Visual-cortex BCI for AR/gaming | Acquired by Snap (2022); consumer dev kit discontinued | Category currently has no active consumer player |
| Anthriq | Biosignal infrastructure/SDK | Active, funded | Positioned as infrastructure, not a vertically-integrated consumer gaming product |
| Emotiv / g.tec / OpenBCI | EEG hardware & research tooling | Active | Research/prosumer tools, not consumer gaming products |
| Neuralink / Synchron / other invasive BCI | Medical, invasive | Active, different market | Different regulatory category, years from consumer gaming relevance |

Full detail: `02-PROTOTYPE-BUILD-GUIDE.md §1, §5` and `05-INVESTOR-PITCH.md`.

## 8. Business Model

| Stage | Revenue driver |
|---|---|
| Near-term | Grant/prototype funding, developer-kit pre-orders, studio partnership/licensing deals |
| Mid-term | Direct-to-consumer headset sales, bundled with the flagship partner game |
| Long-term | Platform/SDK licensing to other studios, hardware plus a software subscription tier for advanced calibration-free models, potential strategic acquisition |

*(To be refined with real unit economics before fundraising conversations — see `05-INVESTOR-PITCH.md`.)*

## 9. Team & Partnerships

- **Founding team:** three members — one leading business development and fundraising, and technical leadership spanning hardware/signal-processing and software direction.
- **Currently hiring:** a software/web developer (company website and the internal visualization/testing/debugging tool) and an AI/ML engineer (the classifier and, later, the custom neural network) — role scope defined in `07-SOFTWARE-DEVELOPMENT-GUIDE.md §6`.
- **Strategic partner:** an India-based game development studio, already collaborating on prototype game content, which substantially de-risks the "will anyone build content for this" question common to BCI-for-gaming ventures.

## 10. Risk Factors

A full, phase-tagged risk register with mitigations is maintained in `06-RISK-REGISTER.md` and reviewed at the start of every project phase. Headline risks include: signal quality on a single dry electrode being insufficient for reliable classification (mitigated by the dedicated Feasibility Spike phase); scope creep into adjacent but different BCI paradigms; photosensitive-epilepsy safety exposure during demos (mitigated by a mandatory, non-negotiable safety disclosure process); and the standard hardware-startup risks of manufacturing cost, wireless engineering, and regulatory compliance, each scoped to the phase in which they become relevant rather than addressed prematurely or too late.

## 11. Current Status

*(Update this section to reflect real, current status before sharing — do not leave placeholder claims in an external-facing report.)*
- Hardware: `[status — e.g., "Feasibility Spike hardware ordered / in progress / complete"]`
- Software: `[status — e.g., "single-channel FBCCA classifier validated against Tsinghua Benchmark data at X% accuracy; visualization tool in development"]`
- Team: `[status — e.g., "software/web developer role open; AI/ML engineer role open"]`
- Partnership: `[status of game-studio partnership agreement]`

## 12. The Ask

*(To be completed with real figures before any external distribution: total raise amount, use-of-funds breakdown across team, hardware R&D, data collection, and partner/legal costs, expected runway, and the specific Phase 3/4 milestones this raise is intended to fund — see `03-PRODUCT-JOURNEY-MILESTONES.md` and `05-INVESTOR-PITCH.md`.)*

---

**A note on how to use this report:** every section above is written to be honest about what is proven today versus what is planned. When updating this report for a specific investor, incubator, or grant application, replace every bracketed placeholder with real, current, measured information — never leave a placeholder figure in a document that leaves the team's hands.
