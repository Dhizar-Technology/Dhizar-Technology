# 📊 Visualizer Tool
### Project Journey & Milestone Map

---

> **What this document is:** a milestone map for the internal team tool that lets us see, process, and record signals coming off Vector 1 (and future prototypes). It marks *what* the tool must become, stage by stage — not *how* it should be engineered. That is left entirely to the developers building it.
>
> **What this document is not:** a spec sheet or a deliverables checklist. Those are tracked separately.

---

## 📍 Project Definition

The Visualizer Tool is an internal **web application**, used only by our own team, that connects to the hardware prototype over **serial communication at 115200 baud**, displays live signal readings, lets the team apply and compare signal-processing algorithms, and records sessions for later analysis and model training.

---

## 🗺️ Journey Overview

```
 CONNECTION           OBSERVATION            ANALYSIS                 CAPTURE
 ──────────          ─────────────          ──────────               ─────────
 ① Serial Link    ② Live Signal View    ③ Processing Menu        ⑥ Save Graphs
                                          ④ Split-View Comparison   ⑦ Recording & Temporal Sync
                                          ⑤ Feature Highlighting    ⑧ Team Usability
```

---

## Stage ① — Serial Communication Link
**Focus:** Establish a live input channel into the tool from the hardware prototype over serial communication at 115200 baud.

☐ Stage acknowledged and closed

---

## Stage ② — Live Signal Visualization
**Focus:** Display the incoming reading from the hardware in real time, as a graph, alongside its basic parameters/signal information.

☐ Stage acknowledged and closed

---

## Stage ③ — Signal Processing Menu
**Focus:** Build the dropdown menu of relevant algorithms and formulas — the ones relevant to our need of extracting and detecting the flickering feature from the signal — that the team can select from.

☐ Stage acknowledged and closed

---

## Stage ④ — Split-View Comparison
**Focus:** When an algorithm/formula is selected, split the window into two live views — original signal on the left, processed signal on the right — so the effect of processing is directly comparable.

☐ Stage acknowledged and closed

---

## Stage ⑤ — Extracted Feature Highlighting
**Focus:** On the processed (right) side, visually highlight the specific features the chosen algorithm has extracted.

☐ Stage acknowledged and closed

---

## Stage ⑥ — Save Graphs
**Focus:** Give the team the ability to save the graphs they are viewing — original and processed — for later reference.

☐ Stage acknowledged and closed

---

## Stage ⑦ — Recording with Temporal Labeling
**Focus:** Allow the team to record both the input and processed data, with temporal labels attached, so that recordings can be synchronized in time with other signals/events later.

☐ Stage acknowledged and closed

---

## Stage ⑧ — Internal Team Usability
**Focus:** Confirm the tool genuinely makes the team's day-to-day signal work faster and easier — it exists purely to serve internal productivity, not as a customer-facing product.

☐ Stage acknowledged and closed

---

## 🎯 Alignment Reminder

The Visualizer Tool has one job: **make the invisible visible, and the manual fast.** Every stage above should be judged against that single purpose — if a stage doesn't make the team's own signal work clearer or quicker, it doesn't belong in this tool.
