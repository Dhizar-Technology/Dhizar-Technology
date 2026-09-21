# 🧠 VECTOR 1 — Mono-Channel Prototype
### Project Journey & Milestone Map

---

> **What this document is:** a milestone map, not a manual. It exists so every person on the team — regardless of what they're heads-down building on any given day — can look up and know exactly where their work fits in the bigger journey. It does **not** prescribe *how* any milestone is solved. That is intentionally left to the engineers' judgment and creativity.
>
> **What this document is not:** a spec, a deliverables list, or a technical guide. Deliverables for each stage are tracked separately and defined as the work unfolds.

---

## 📍 Project Definition

Vector 1 is our first hardware prototype: a **mono-channel** system that reads electrical signals from the visual cortex, processes them, and forwards the result to the main computing unit through a microprocessor over **serial (USB) communication at 115200 baud**.

Everything below is the journey this prototype must travel — from raw neuroscience understanding to a physical, limitation-tested circuit.

---

## 🗺️ Journey Overview

```
 FOUNDATION            DESIGN                  REALIZATION            HARDENING
 ──────────           ────────                ─────────────          ───────────
 ① Cortex          ④ Architecture         ⑥ Circuit Design       ⑧ Limitations
 ② Signal          ⑤ Block Diagram        ⑦ PCB Design           ⑨ Failure Points
 ③ Components                                                     ⑩ Efficiency/Accuracy Killers
                                                                   ⑪ Biological Resistance
                                                                   ⑫ Environmental Resistance
                                                                   ⑬ Usage Policy
```

---

## Stage ① — Understanding the Visual Cortex
**Focus:** Build a grounded, team-wide understanding of the visual cortex — its role, its behavior, and why it is the target region for this device.

☐ Stage acknowledged and closed

---

## Stage ② — Understanding Target Signal Characteristics
**Focus:** Understand the nature of the specific signal being targeted at the visual cortex — its properties, behavior, and what distinguishes it as a usable, detectable signal.

☐ Stage acknowledged and closed

---

## Stage ③ — Sensor & Component Feasibility
**Focus:** Survey what sensors and supporting components exist, what's realistically available to us, and what the mono-channel prototype can be physically built from.

☐ Stage acknowledged and closed

---

## Stage ④ — Overall Prototype Architecture
**Focus:** Define the complete system architecture of Prototype 1 — how signal acquisition, processing, and the serial hand-off to the main computer fit together as one coherent system.

☐ Stage acknowledged and closed

---

## Stage ⑤ — Block-Based Circuit Diagram
**Focus:** Represent the architecture as a block-level circuit diagram — the functional map before the physical one.

☐ Stage acknowledged and closed

---

## Stage ⑥ — Circuit Diagram / Design
**Focus:** Translate the block diagram into the actual circuit diagram and design of Prototype 1.

☐ Stage acknowledged and closed

---

## Stage ⑦ — PCB Designing
**Focus:** Turn the circuit design into a physical PCB layout, ready for fabrication.

☐ Stage acknowledged and closed

---

## Stage ⑧ — Limitations of Prototype 1
**Focus:** Honestly document what this prototype can and cannot do, by design.

☐ Stage acknowledged and closed

---

## Stage ⑨ — Failure Points
**Focus:** Identify where and how this prototype is most likely to fail — physically, electrically, or procedurally.

☐ Stage acknowledged and closed

---

## Stage ⑩ — Efficiency & Accuracy Killers
**Focus:** Identify the factors most likely to silently degrade signal efficiency or classification accuracy.

☐ Stage acknowledged and closed

---

## Stage ⑪ — Biological Barriers / Resistance
**Focus:** Understand and document the biological factors that resist or interfere with reliable signal acquisition, across different users.

☐ Stage acknowledged and closed

---

## Stage ⑫ — Environmental Resistance
**Focus:** Understand and document the environmental factors that resist or interfere with reliable prototype operation.

☐ Stage acknowledged and closed

---

## Stage ⑬ — Policy & Conditions of Use
**Focus:** Define the conditions, boundaries, and policies under which Prototype 1 is safe and valid to use.

☐ Stage acknowledged and closed

---

## 🎯 Alignment Reminder

Every stage above exists to answer one question the team should keep returning to:

> **"Do we now understand this well enough to build the next stage on top of it?"**

If the honest answer is no, the stage isn't closed yet — regardless of how much time has been spent on it. Sequence matters more than speed here: Vector 1 is the foundation every later hardware generation stands on.
