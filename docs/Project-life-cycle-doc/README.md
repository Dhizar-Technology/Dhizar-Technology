# Vector 1 (V1)
### A visual-cortex game controller — by Dhizar Technology

**Vector 1 (V1) is a brain-computer interface that lets a player control a game using only their visual cortex — no hands, no controller, no learning curve.**

The player looks at flickering targets on screen — buttons, abilities, directions — each blinking at its own unique frequency. V1 reads the player's occipital EEG signal, detects which target the frequency of their visual cortex is synchronized to, and fires the matching in-game command in real time.

This repository is the team's single source of truth: the science, the hardware plan, the software plan, the roadmap, the risks, and the materials used to talk to investors. Every document here is meant to be read and used, not filed away.

---

## The one-line pitch

**V1 turns eye focus into a game controller, by reading the one part of the brain that reacts to light involuntarily and measurably: the visual cortex.**

## How it works, in short

When a person looks directly at a light that flickers at a constant rate, the neurons in their visual cortex synchronize their firing to that same rate. This is called a **Steady-State Visually Evoked Potential (SSVEP)** — it is involuntary, it requires no training, and it has been studied in BCI research for decades. Put several different flicker rates on screen at once, read which frequency dominates a scalp electrode placed over the visual cortex, and you know exactly which target the player is looking at. That target maps to a game action.

Full explanation: [`docs/01-TECHNICAL-PRIMER.md`](docs/01-TECHNICAL-PRIMER.md)

## What makes V1 different from "read your mind" claims

V1 does **not** read thoughts, emotions, or intent. It does not attempt motor imagery, P300 spelling, or EMG "muscle reading" — those are different technologies with different accuracy, latency, and hardware profiles, and mixing them into this product is treated as scope creep (see the anti-scope-creep checklist in the milestones document). V1 does exactly one thing, extremely well: detect which flickering visual target a person's occipital cortex is locked onto, as fast and as reliably as possible.

## Where V1 is different from prior art

The closest comparable product, NextMind, was acquired by Snap in 2022 and its consumer dev kit was discontinued — leaving no active shipping consumer SSVEP gaming controller on the market today. Anthriq, the company that inspired this project, positions itself as general biosignal *infrastructure* (an SDK for other builders). Dhizar Technology is taking a narrower, vertically-integrated path: **V1 is a gaming controller product**, built end-to-end — hardware, control software, and a game built specifically for it — not a general-purpose developer SDK.

Full landscape: [`docs/02-PROTOTYPE-BUILD-GUIDE.md`](docs/02-PROTOTYPE-BUILD-GUIDE.md) and [`docs/05-INVESTOR-PITCH.md`](docs/05-INVESTOR-PITCH.md)

## Our build strategy: single-channel first

Most SSVEP research and consumer devkits reach for 8+ EEG channels from day one. Dhizar Technology is deliberately starting with a **single-channel controller** (one electrode over Oz, the primary occipital site) instead. This is a strategic choice, not a technical limitation we're stuck with:

- It is the cheapest, fastest way to prove the one thing that actually needs proving — that a real person can reliably trigger a target with their visual cortex — before spending money on multi-channel hardware.
- A working single-channel demo is a concrete, fundable milestone. It gives investors and incubators something real to evaluate instead of a roadmap slide.
- Channel count is a scaling decision, not a validation decision — expanding to 2, then 4–8 channels is Phase work that happens *after* the core loop is proven, not before.

Full reasoning and hardware detail: [`docs/02-PROTOTYPE-BUILD-GUIDE.md`](docs/02-PROTOTYPE-BUILD-GUIDE.md)

## Two tracks running in parallel

Because hardware can't be built or tested until the team is funded or incubated, Dhizar Technology is running **hardware** and **software** as two parallel, independently useful tracks:

| Track | Status | What it needs |
|---|---|---|
| **Hardware** | Blocked on funding/incubation | Capital, a single-channel EEG front-end, an electrode, a fixed-refresh-rate display |
| **Software** | Can start today, no hardware required | Public human SSVEP-EEG datasets (not real-time hardware), a software/web developer, an AI/ML engineer |

The software track is designed so that when hardware finally arrives, the software team plugs a live data stream into a pipeline that has already been built, tested, and debugged against real human SSVEP data — instead of starting from zero.

Full detail: [`docs/07-SOFTWARE-DEVELOPMENT-GUIDE.md`](docs/07-SOFTWARE-DEVELOPMENT-GUIDE.md)

## The team

- **Founding team:** 3 members today — one leading business development and fundraising, and technical members driving the hardware/signal-processing and software direction.
- **Currently hiring:** one software/web developer (to build the company website and the in-house visualization/testing/debugging tool), and one AI/ML engineer (to own the classifier and, later, the custom neural network).
- **Partnership:** an India-based game development studio, already collaborating on prototype game content.

## Repository guide

| Document | What it's for |
|---|---|
| [`docs/01-TECHNICAL-PRIMER.md`](docs/01-TECHNICAL-PRIMER.md) | The core science (SSVEP), why we chose it over other BCI paradigms, the signal pipeline, and the mandatory safety constraints. **Read this first.** |
| [`docs/02-PROTOTYPE-BUILD-GUIDE.md`](docs/02-PROTOTYPE-BUILD-GUIDE.md) | Prior art, the single-channel-first hardware plan, open-source software stack, and real human SSVEP datasets to build and test against. |
| [`docs/03-PRODUCT-JOURNEY-MILESTONES.md`](docs/03-PRODUCT-JOURNEY-MILESTONES.md) | The phase-by-phase product roadmap, hardware and software tracks side by side, from feasibility spike to shipped consumer product. |
| [`docs/04-RD-ROADMAP.md`](docs/04-RD-ROADMAP.md) | How each phase actually gets executed, stage by stage, plus a literal demo-day checklist. |
| [`docs/05-INVESTOR-PITCH.md`](docs/05-INVESTOR-PITCH.md) | Business-facing overview — market, competitive landscape, business model, the ask. |
| [`docs/06-RISK-REGISTER.md`](docs/06-RISK-REGISTER.md) | Known risks per phase and how we're mitigating them. |
| [`docs/07-SOFTWARE-DEVELOPMENT-GUIDE.md`](docs/07-SOFTWARE-DEVELOPMENT-GUIDE.md) | Full plan for the three software deliverables: company website, in-house visualization/testing/debugging tool, and the AI/ML classifier — including where to start and exactly what's needed. |
| [`docs/08-PITCH-DECK-SCRIPT.md`](docs/08-PITCH-DECK-SCRIPT.md) | Slide-by-slide content and speaking script for live investor pitches. |
| [`docs/09-PROJECT-REPORT.md`](docs/09-PROJECT-REPORT.md) | Formal written project report for investors, incubators, and grant applications. |

## Project status

🟡 **Planning → early prototype phase.** Hardware work is gated on funding/incubation. Software work — the classifier pipeline, the visualization/debug tool, and the company website — is starting now, against public human SSVEP-EEG datasets, so that the moment hardware exists, integration is a plug-in, not a rebuild.

## Safety note

Flickering visual stimuli can trigger seizures in people with photosensitive epilepsy. Every build and demo of this project — including the software-only visualization tool, once it renders flicker stimuli on a real screen for anyone outside the immediate dev team — includes a mandatory safety disclosure before use. See [`docs/01-TECHNICAL-PRIMER.md §5`](docs/01-TECHNICAL-PRIMER.md) for the full constraint. This is a hard requirement at every stage, not an optional nicety.

## License

*(Add a license before making this repository public — TBD by the team.)*
