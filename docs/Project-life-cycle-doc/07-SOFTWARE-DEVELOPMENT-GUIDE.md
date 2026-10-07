# Vector 1 (V1) — Software Development Guide

**Purpose of this document:** This is the reference for everything on the software side of Vector 1. It exists because hardware work is currently blocked on funding/incubation, while software work is not — and we intend to use that time fully. Anyone joining as the software/web developer or the AI/ML engineer should be able to read this document alone and know exactly what to build, in what order, with what tools, and why.

---

## 1. Why software runs ahead of hardware

We cannot build or test the physical headset until we're funded or incubated. But almost everything downstream of "raw EEG signal" — the classifier, the visualization and debugging tooling, the game-communication protocol, and even a first pass at the company's public presence — does **not** require our own hardware to exist. It requires:

- A source of realistic EEG data that behaves like what our headset will eventually produce.
- A software architecture that treats "where the data comes from" as a swappable detail, not a hardcoded assumption.

If we build it this way, the day real hardware arrives, integration is "point the pipeline at a new data source," not "start over."

**The one rule that makes this whole approach work: keep every piece modular.** The classifier should not care whether its input array came from a CSV replay of a public dataset or a live BrainFlow stream. The visualization tool should not care whether it's rendering a recorded session or a live one. Build to an interface, not to a specific data source, from the very first line of code.

---

## 2. Data strategy — what we train and test against before real hardware exists

This is the foundation everything else sits on, so get it right before writing any classifier code.

### 2.1 Use real human SSVEP-EEG datasets — not the Allen Institute data

It's worth being precise here, because it changes what "validated" means for our software: **the Allen Institute Brain Observatory dataset (brain-map.org / observatory.brain-map.org) is not a fit for this project.** It is two-photon calcium imaging and Neuropixels electrode recordings taken directly from the exposed visual cortex of head-fixed **mice** during invasive lab experiments — not human scalp EEG, not recorded through a headset, and not built around an SSVEP flicker-frequency paradigm matched to our stimulus design. A classifier that "works" on that data tells us nothing about how it will perform on a human wearing our headset. Do not use it to validate the SSVEP pipeline. (It may be interesting background reading for the ML engineer on general visual-cortex encoding, but it should never appear in a validation report or an investor-facing accuracy number.)

The datasets that **are** the right fit — real human scalp EEG, real SSVEP flicker stimuli, exactly the kind of signal our headset will produce — are already identified in `02-PROTOTYPE-BUILD-GUIDE.md §4`:

| Dataset | Why we use it |
|---|---|
| **Tsinghua SSVEP Benchmark (Wang et al.)** | 64-channel, so we can extract just the Oz channel to simulate our single-channel hardware. This is our primary dataset for building and validating the classifier. |
| **BETA dataset** | Same paradigm, larger subject pool — good for the ML engineer's training set once deep-learning work starts. |
| **Zhu et al. wearable/dry-electrode dataset** | Directly validates dry electrodes (what we plan to use) vs. wet/gel, on a smaller channel count closer to ours. |
| **1–60 Hz sweep dataset (Scientific Data / Nature)** | Useful for choosing our stimulation frequency range. |
| **Asynchronous SSVEP dataset** | Has "non-control state" data — essential for teaching the classifier when the user isn't trying to trigger anything, which real gameplay absolutely needs. |

### 2.2 Treat the data source as a plug-in, not a hardcoded pipeline step

Structure the codebase so there's a clear, swappable "data source" boundary:

```
[ Data Source ]  →  [ Preprocessing ]  →  [ Classifier ]  →  [ Command Output ]
  - Public dataset file (CSV/EDF/MAT/NWB reader)
  - Recorded session file (from our own future tester sessions)
  - Live BrainFlow/LSL stream (once real hardware exists)
```

Every data source should implement the same simple interface (e.g., a Python generator or class yielding fixed-size windows of channel data plus a timestamp and, when available, a ground-truth label). The preprocessing, classifier, and visualization code downstream should never need to know or care which kind of source is feeding them. This single design decision is what prevents a rebuild later.

### 2.3 What "our own data" will look like once hardware exists

Plan the session-recording format now, even before hardware exists, so the visualization tool, the classifier, and the eventual real tester sessions all agree on one format from day one (e.g., a simple structured file: channel(s), sample rate, event markers for stimulus onset/target ID, and session metadata). Base this format on how the public datasets are structured, so a public-dataset file and a real-session file can be swapped into the same pipeline with zero code changes.

---

## 3. Deliverable 1 — Company Website

**Purpose:** the public face of Dhizar Technology and Vector 1. This is the first thing an investor, journalist, potential hire, or curious gamer will see, and it can exist today — it does not need a working prototype to justify its existence, only a clear, honest description of what the company is building and why.

### What it needs to contain
- **Home / product page:** the one-line pitch, a short explanation of SSVEP in plain language (reuse the framing from `01-TECHNICAL-PRIMER.md §1` and this README), and — as soon as one exists — a video or GIF of the working demo. Until then, an honest "we're in prototype phase" status is fine and, per the investor-pitch tone, preferred over overclaiming.
- **Team / about page:** founders, the two roles being hired, the game-studio partnership.
- **Contact / press page:** a real way to reach the team.
- **Careers section:** since the company is actively hiring the software developer and ML engineer roles right now, this page should exist and be kept current — it doubles as a recruiting tool.
- **Safety/ethics statement:** a short, public commitment to the photosensitivity safety constraint (Technical Primer §6) and to responsible data handling. This builds credibility with investors and testers alike and costs almost nothing to write.

### Suggested stack
- A static site generator or lightweight framework is enough for this — there's no need for a heavy backend for a marketing/company site. Common, low-maintenance choices: a static site (e.g., Astro, Next.js in static-export mode, or even a well-structured plain HTML/CSS site) hosted on a static host (Vercel, Netlify, GitHub Pages, or Cloudflare Pages).
- Keep it a **separate codebase/deployment** from the visualization/debug tool below — they have completely different audiences (public vs. internal team) and different security/reliability requirements. Don't let the marketing site and the internal engineering tool become the same app.

### Where to start
1. Write the plain-language product description and status honestly (reuse the README and Technical Primer language — don't write it twice from scratch).
2. Get a placeholder version live immediately, even with just the home page and a "we're hiring" note — a real, working, linkable URL is more valuable early than a polished-but-unpublished draft.
3. Iterate as real demo footage, real numbers, and real team bios become available.

---

## 4. Deliverable 2 — In-House Visualization, Testing & Debugging Tool

**Purpose:** this is the tool the development team will live in every day. It has to let anyone on the team — hardware, software, or ML — see what's happening in the signal pipeline without needing to read raw code, and it has to work **without physical hardware attached**, using recorded/public data, so the software and ML tracks are never blocked waiting for a headset.

This is explicitly an **internal, web-based tool for the development team**, not a public-facing product.

### What it needs to do

| Capability | Why it matters |
|---|---|
| **Render flicker stimuli in-browser** at configurable frequencies, respecting the refresh-rate-divisibility constraint from `01-TECHNICAL-PRIMER.md §5` | Lets the team design and preview stimulus screens (2-target, later 4–8-target) without touching the game engine. |
| **Play back a public dataset or a recorded session** through the pipeline, frame-by-frame or at real-time speed | This is how the team tests the classifier daily without a headset — replay Tsinghua/BETA/Zhu et al. sessions and watch the pipeline classify them. |
| **Live-stream mode** (once hardware exists): accept a live BrainFlow/LSL feed, using the exact same downstream code path as playback mode | Confirms the "swap the data source, nothing else changes" architecture actually holds. |
| **Show the raw signal and its frequency spectrum** (a live/near-live FFT or similar plot per channel) | This is the single most useful debugging view for SSVEP work — the team needs to *see* the target frequency's peak in the spectrum, not just trust a black-box classifier score. |
| **Show classifier output and confidence per target, over time** | Lets the team see false triggers, missed triggers, and how confidence evolves within a decision window — directly informs tuning the window-length trade-off from the Technical Primer. |
| **Log and export session data** in the format decided in §2.3 above | So every test session (including team members testing on themselves) becomes reusable data later. |
| **Enforce the safety disclosure** before rendering any flicker stimulus, even in this internal tool | Per the Technical Primer, this constraint applies to every build that renders flicker, including internal tools, the moment anyone outside the immediate dev team might see it. |

### Suggested stack
- **Web-based**, so any team member (including the non-technical business co-founder, for demo purposes) can open it in a browser with no install step. A single-page web app is the right shape.
- **Frontend:** a modern component framework (React, Vue, or Svelte are all reasonable) for the UI shell, tabs/panels, and controls.
- **Signal rendering / plotting:** use existing open-source charting/plotting libraries for the spectrum and time-series views (e.g., Plotly.js, D3, or a lighter charting library) rather than writing a custom plotting engine from scratch — this is exactly the kind of "use open source if it exists" case the founder described.
- **Stimulus rendering:** plain Canvas or WebGL for the flicker patches — precise frame timing matters here (tie flicker toggling to `requestAnimationFrame` and the browser's actual refresh rate, per the Technical Primer's refresh-rate constraint), so keep this part hand-built and carefully tested rather than relying on a generic animation library that might not guarantee frame-accurate timing.
- **Backend / classifier bridge:** the classifier itself (CCA/FBCCA, later deep learning) is most naturally implemented in Python (numpy/scipy/scikit-learn, later PyTorch/TensorFlow for EEGNet-style models). Expose it to the web frontend via a small local API (e.g., a lightweight Python web server) or, if the team prefers a single-language stack, evaluate whether a JS/TS-native signal-processing implementation is mature enough — but don't force this; a Python backend behind a simple API is a perfectly good, fast-to-build option and is what most of the open-source SSVEP reference implementations already use.
- **No hardware-specific code inside this tool's core logic.** Hardware/live-stream support (BrainFlow/LSL) should be added as one more implementation of the same "data source" interface from §2.2 — not as a special-cased mode bolted on separately.
- Where a genuinely SSVEP-specific need has no good open-source option (e.g., a frequency-spectrum view tuned specifically to highlight target frequencies and their harmonics, or a stimulus-authoring UI for defining target layouts and frequency sets), build it in-house rather than forcing a generic tool to fit — this is exactly the "in-house designed if not available as open source" part of the brief.

### Where to start
1. Build the stimulus renderer first (2-target flicker screen, refresh-rate-correct) — it's the smallest, most self-contained piece, and the whole team can use it immediately to visually sanity-check timing even before any classifier exists.
2. Build the dataset-playback data source and wire it straight into a bare-bones CCA classifier — get an end-to-end (if crude) pipeline working before polishing any UI.
3. Add the spectrum view and confidence-over-time view — these are what make the tool actually useful for debugging, not just demoing.
4. Add session logging/export.
5. Add the live-hardware data source only once real hardware exists — by design, this should be a small addition, not a rewrite.

---

## 5. Deliverable 3 — The AI/ML Model (Classifier & Custom Neural Network)

**Purpose:** reduce the latency (decision time) and increase the accuracy of target detection, beyond what the classical CCA/FBCCA baseline achieves. This is explicitly the **last** software deliverable to reach maturity, but its groundwork can and should start as soon as the AI/ML engineer is hired — it does not need to wait for the other two deliverables to be "done."

### What "the AI/ML model" means, staged (see also `01-TECHNICAL-PRIMER.md §7`)

1. **Baseline (no ML engineer required to start):** CCA, then FBCCA. Training-free, well-documented, implementable directly from published methods. This is what powers the visualization tool and the earliest hardware prototype.
2. **First ML milestone (as soon as the ML engineer joins):** benchmark an existing open-source SSVEP deep-learning architecture (EEGNet or a published SSVEP-specific derivative) against the FBCCA baseline, on the same public datasets, on the single-channel-extracted data. The goal at this stage is a fair, documented comparison — not yet a production model.
3. **Second ML milestone:** fine-tune the more promising architecture specifically for our constraints — single channel (initially), short decision windows (targeting faster-than-FBCCA response), and the target-count/frequency-set the game partner is actually using.
4. **Later, once real headset data exists:** retrain/fine-tune on our own collected sessions (with consent — see the Risk Register and R&D Roadmap's consent workstream), and pursue cross-user generalization (good accuracy on a brand-new user with minimal or no calibration) — this is the real long-term IP moat, not the SSVEP concept itself.

### What the AI/ML engineer needs to know before starting
- Read `01-TECHNICAL-PRIMER.md` in full — especially why CCA/FBCCA come first and why "custom neural network" is a later-phase differentiator, not a Phase 0 requirement.
- Read `02-PROTOTYPE-BUILD-GUIDE.md §4` for the exact datasets to use, and this document's §2.1 for which datasets **not** to use.
- Their first practical task should be reproducible and comparative: take the single-channel-extracted Tsinghua Benchmark data, reproduce the FBCCA baseline's accuracy, then reproduce an EEGNet-style baseline on the same split, and report both numbers side by side. This gives the team (and eventually investors) an honest, evidence-based answer to "how much does the neural network actually help," rather than an assumed one.

### Suggested stack
- **Python**, PyTorch or TensorFlow (either is fine; pick based on what's easiest to find open-source SSVEP reference implementations in — historically most academic SSVEP deep-learning code is published in one or the other, so let available reference code guide the choice rather than a stack preference).
- Standard ML tooling for experiment tracking (even a simple spreadsheet or a lightweight tool like Weights & Biases/MLflow is fine at this scale) so that accuracy/latency comparisons across model versions are recorded, not just remembered.
- The trained model should be exportable/servable in a form the visualization tool's backend (§4) and, eventually, the real-time game-integration pipeline can call with low latency — keep inference-time performance in mind from the start, not just training-time accuracy, since the whole point of this deliverable is reducing response latency.

---

## 6. How the two new hires fit together

| Role | Owns | Should NOT own |
|---|---|---|
| **Software/Web Developer** | The company website; the in-house visualization/testing/debugging tool's frontend, stimulus rendering, and data plumbing (including wiring in whatever classifier the ML side produces) | Designing or training the classifier/neural network itself — they integrate it, the ML engineer builds it |
| **AI/ML Engineer** | The classifier: CCA/FBCCA baseline, later EEGNet-style deep learning, dataset preparation, accuracy/latency benchmarking | The web frontend, stimulus rendering UI, or the company website — they hand off a model/API, the software developer wires it into the tool |

The two roles should agree early on the API/interface between "classifier" and "everything else" (per §2.2's data-source abstraction, extended to a matching "classifier output" interface: a target ID plus a confidence score, at minimum). Once that contract is fixed, the two people can work almost entirely in parallel.

---

## 7. Where and how to start — a practical first-week guide

If you're joining as the software developer or the ML engineer and asking "where do I actually start":

1. **Read, in order:** this document, then `01-TECHNICAL-PRIMER.md`, then `02-PROTOTYPE-BUILD-GUIDE.md §3–4`, then `03-PRODUCT-JOURNEY-MILESTONES.md` for the phase you're currently in.
2. **Set up your environment:** Python 3.x with numpy/scipy/scikit-learn (and PyTorch or TensorFlow if you're the ML hire); a modern JS/TS toolchain (Node.js + your chosen frontend framework) if you're the software hire.
3. **Get one real dataset downloaded and loaded** — start with the Tsinghua SSVEP Benchmark dataset, since it's the most widely cited and best-documented. Confirm you can extract the Oz channel from it and plot a few seconds of raw signal.
4. **Build the smallest possible end-to-end slice** before anything else: read one dataset session → run a basic CCA classifier on it → print out which target it guessed vs. the true label. This "smallest possible working thing" mirrors the same philosophy the whole roadmap is built on, and it will surface most of your data-format and pipeline questions immediately.
5. **Only then** start on the fuller visualization UI, the FBCCA upgrade, or the deep-learning benchmark, depending on your role.

### What exactly is required for this project (checklist)
- A laptop capable of running Python and a modern browser-based dev environment (no special hardware needed for software work).
- Downloaded copies of the public SSVEP datasets listed in `02-PROTOTYPE-BUILD-GUIDE.md §4`.
- Standard open-source libraries: numpy, scipy, scikit-learn, a plotting/charting library, a frontend framework, and (for the ML track) PyTorch or TensorFlow.
- Access to this documentation set and to whatever version control / project management tools the team standardizes on (a shared git repository for all code, including the website, the visualization tool, and the ML experiments, is strongly recommended from day one).
- Once hardware exists: BrainFlow and Lab Streaming Layer (LSL), as already specified in `02-PROTOTYPE-BUILD-GUIDE.md §3`.

---
*Next: see `08-PITCH-DECK-SCRIPT.md` for how to present this whole project live, and `09-PROJECT-REPORT.md` for the full written version for investors and incubators.*
