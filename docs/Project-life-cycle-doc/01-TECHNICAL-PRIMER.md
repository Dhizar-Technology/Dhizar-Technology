# Vector 1 (V1) — Technical Primer: SSVEP & Visual-Cortex Control

**Purpose of this document:** Every person on the Dhizar Technology team — engineers, game devs, designers, and non-technical stakeholders — should read this before touching hardware or code. It defines the one core mechanism the whole product is built on, so nobody accidentally scopes in something adjacent-but-different (like motor imagery, P300 spellers, or EMG "muscle reading," which are different technologies with different accuracy/latency/hardware profiles).

---

## 1. What we are actually building

We are **not** reading thoughts. We are **not** doing general "mind control." We are building an **SSVEP-based BCI** — Steady-State Visually Evoked Potential.

**The mechanism, in one paragraph:**
When a person looks directly at a light source that flickers at a constant frequency (say, 10 times per second), the neurons in their **visual cortex** (occipital lobe, back of the skull) synchronize their firing to that same frequency — and its harmonics (20 Hz, 30 Hz, etc.). This is an involuntary, reflexive response — it happens whether the person "concentrates" or not, as long as their fovea is pointed at the flickering stimulus. An EEG electrode placed over the occipital region (channel positions **Oz, O1, O2, POz, PO3, PO4, PO7, PO8** in the 10-20 system) picks up this synchronized electrical signal. If we put multiple flickering targets on screen, each blinking at a *different, unique frequency*, we can tell which one the user is looking at simply by detecting which frequency dominates their occipital EEG signal at that moment. **That frequency-to-target mapping is the entire "control scheme."**

This is the industry-standard approach — the same principle NextMind (acquired by Snap in 2022) commercialized as a $399 consumer devkit, and the same principle behind decades of SSVEP-speller academic research (see `02-PROTOTYPE-BUILD-GUIDE.md` for citations and datasets).

## 2. Why SSVEP and not other BCI paradigms (and why the team must not drift)

| Paradigm | Signal source | Training needed | Latency | Accuracy (typical) | Why we are NOT doing this |
|---|---|---|---|---|---|
| **SSVEP (our choice)** | Visual cortex, reflexive | Little to none (calibration only) | ~0.5–4 s per selection | 85–99% in controlled setups | — |
| Motor Imagery (MI) | Motor cortex, imagined movement | Weeks of user training | Slow, unreliable | 60–80%, huge inter-subject variance | Needs extensive per-user training; bad for demo-ability and gamer UX |
| P300 Speller | Parietal "oddball" response | Minimal | Slower (needs many flashes to average) | High but slow | Designed for spelling, not real-time directional control |
| EMG "muscle reading" (e.g. wristbands) | Muscle, not brain | Minimal | Fast | High | Not a BCI at all — different sensor, different company (this is what Meta/CTRL-labs did) |
| Invasive (Neuralink-style) | Direct cortical implants | N/A | Fastest | Highest | Surgical, not consumer, not our market for years |

**Rule for the team:** If a research idea, hire, or vendor conversation starts pulling us toward motor imagery, EEG-based "emotion detection," or invasive electrodes, that is scope creep. Flag it against this table before spending time or money on it.

## 3. Why we start with a single channel, not eight

The academic and consumer-devkit default is 8+ occipital channels (Oz, O1, O2, POz, PO3, PO4, PO7, PO8). Dhizar Technology is deliberately **starting with one channel (Oz)** instead. This is a sequencing decision, driven by what actually needs to be proven first:

- The riskiest assumption in this entire project is not "can 8 channels classify accurately" — it's "can *any* consumer-affordable setup, run by a 3-person team with no neuroscientist, reliably tell which of 2 targets a real human is looking at." A single channel answers that question at the lowest possible cost, in the least amount of engineering time.
- A single dry electrode over Oz is dramatically cheaper, faster to set up (seconds, not minutes), and far more demoable — a huge practical advantage for early investor/incubator conversations, where setup friction kills a live demo.
- Multi-channel spatial filtering (the main advantage of 8-channel setups) primarily boosts accuracy and reduces required calibration — it is a **quality/robustness upgrade**, not a requirement for the core mechanism to work at all. It belongs in the "grow it" phase, not the "prove it" phase.
- This mirrors the same "smallest possible working thing, then grow it visibly" principle the whole roadmap is built on (see `03-PRODUCT-JOURNEY-MILESTONES.md`).

**What single-channel costs us, honestly:** lower ceiling on accuracy and target count than an 8-channel array, and a harder time separating closely-spaced frequencies. This is an accepted, explicit trade-off for Phase 0–1, not an oversight — channel count expands in later phases once the core loop is proven (see the milestones document).

## 4. The core signal-processing pipeline (what the software actually does)

1. **Acquisition** — analog EEG voltage from the occipital electrode(s), amplified and digitized (typically 250 Hz sampling for SSVEP is sufficient; some benchmark datasets use 1000 Hz downsampled to 250 Hz).
2. **Preprocessing** — bandpass filter (roughly 5–40 Hz to keep the fundamental + first harmonics of typical stimulation frequencies of 6–20 Hz), notch filter at 50/60 Hz to remove mains interference, artifact rejection (blinks, eye movement via EOG contamination).
3. **Windowing** — segment into short overlapping time windows (0.5–4 seconds); shorter windows = faster response but lower accuracy, this trade-off **is** the product's core UX tuning knob.
4. **Feature extraction / classification** — this is where the "which target is the user looking at" decision is actually made. Standard, well-proven algorithms (no novel research needed to get started):
   - **CCA (Canonical Correlation Analysis)** — the classical, still-strong baseline. Compares the EEG signal against reference sine/cosine waves at each candidate frequency; highest correlation wins. Fast, interpretable, no training data required per user. Works on a single channel, though accuracy is lower than with multiple channels.
   - **FBCCA (Filter Bank CCA)** — CCA applied across several sub-bands (fundamental + harmonics), then combined. Meaningfully more accurate than plain CCA, still fast, still training-free. **Recommended as our v1 baseline algorithm**, even on a single channel.
   - **TRCA (Task-Related Component Analysis)** — higher accuracy than FBCCA but needs a short per-user calibration session (a few minutes) and benefits more from multiple channels. Good candidate for the multi-channel phase.
   - **Deep learning (CNN/EEGNet-style, or newer transformer-based SSVEP nets)** — this is what "our custom neural network" should mean in practice: a model trained on public SSVEP datasets + our own collected data, fine-tuned to squeeze out higher accuracy and shorter windows (faster game response) than CCA/FBCCA can achieve, including on constrained single-channel input. This is a **later-stage differentiator**, not a v1 requirement — CCA/FBCCA alone is enough to build a convincing, working prototype.
5. **Command output** — the winning frequency is mapped to a key/button event and sent to the game engine (this can be as simple as emitting a virtual keypress or joystick event over a USB HID or a WebSocket/UDP message the game listens for).

## 5. The stimulus side — designing the blinking targets correctly

This is equally important and is a **game-dev + hardware-team joint responsibility**, not just a signal-processing problem:

- **Frequency range:** 6–15 Hz is the sweet spot most literature and products use — strong SSVEP response, tolerable for the eyes, and safely away from photosensitive-epilepsy trigger ranges when contrast and screen area are kept moderate (see Safety section below — this is a hard constraint, not a suggestion).
- **Frequency spacing:** targets need at least ~0.4–1 Hz separation (or joint frequency+phase coding, as used in the 40-target academic benchmark dataset) so the classifier can tell them apart reliably. More targets on screen = closer frequencies = harder classification. On a single channel this ceiling is tighter than on 8 channels — **expect 2 targets for the Phase 0 feasibility spike, growing to 4–8 only after channel count grows.**
- **Rendering constraint:** flicker frequency is limited by the display's refresh rate (a 60 Hz monitor can only render exact frequencies that divide evenly into 60, e.g. 6, 7.5, 10, 12, 15, 20, 30 Hz, unless using frame-approximation tricks). This is a game-engine (and web-based visualization tool) implementation detail that anyone building a flicker stimulus needs to know **before** designing UI around arbitrary frequencies.
- **Visual design:** circular patches are standard and well-validated in the literature; checkerboard patterns and higher contrast generally elicit stronger responses than plain color changes.

## 6. Safety constraint (non-negotiable, applies to every build — hardware or software)

Flickering visual stimuli in the ~3–70 Hz range can trigger seizures in people with **photosensitive epilepsy**. This is a known, well-documented risk for any flicker-based interface (it's why broadcast standards restrict flash rates in TV/games). This applies to the real hardware prototype **and** to the browser-based visualization/testing tool described in `07-SOFTWARE-DEVELOPMENT-GUIDE.md`, since it also renders flickering stimuli on a real screen. Every build and demo must:
- Include a visible warning before use (similar to standard game photosensitivity warnings).
- Keep stimulus contrast and screen area within commonly cited safer bounds, and avoid stacking multiple large high-contrast flickering regions simultaneously.
- Never be presented to press, investors, testers, or the public without this warning, regardless of how minor the team judges the risk to be.

This is a product-liability and user-safety issue, not a nice-to-have — build it into every build from day one rather than retrofitting later.

## 7. What "custom neural network" should mean for this team

Be precise with investors and engineers about this phrase, because it's used loosely:
- **Phase 0–1 (prototype):** No custom network required. Use CCA/FBCCA — open-source, well-documented, works out of the box, and can be developed and validated entirely in software against public human SSVEP datasets before any hardware exists.
- **Phase 2+ (MVP+):** Fine-tune an existing open-source SSVEP deep-learning architecture (e.g., EEGNet variants, or newer CNN/Transformer SSVEP classifiers published in recent papers) on public benchmark datasets, blended with our own collected sessions once the headset exists.
- **Later (product):** Train/optimize our own architecture on our own large proprietary dataset (collected via the shipped headset from real users, with consent), targeting shorter decision windows (sub-second target selection) and cross-user generalization (works well on a new user with zero/minimal calibration) — **this is the real IP moat**, not the SSVEP concept itself, which is public-domain science.

---
*Next: see `02-PROTOTYPE-BUILD-GUIDE.md` for exact hardware, software, and datasets to acquire — including the single-channel-first parts list.*
