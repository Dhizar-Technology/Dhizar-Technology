# 🤖 Dhizar AI Model
### Project Journey & Milestone Map

---

> **What this document is:** a milestone map for Dhizar, our purpose-built model derived from reverse-engineering EEGNet. It marks the questions each stage of this journey must answer — not the answers themselves. How those questions get answered is entirely up to the team's own creativity and technical judgment.
>
> **What this document is not:** a spec, an architecture guide, or a deliverables list. Those live elsewhere and are tracked separately.

---

## 📍 Project Definition

Dhizar exists to **reduce latency in our controller** by predicting, ahead of the final confirmation, which button/trigger is about to be selected. It begins its journey as a reverse-engineering study of the EEGNet model.

---

## 🗺️ Journey Overview

```
 STUDY                    DEFINITION                 SCOPING                BUILD
 ───────                 ─────────────              ──────────             ───────
 ① EEGNet Architecture   ④ Input/Output Spec        ⑧ Current              ⑫ Build
 ② Modification Scope    ⑤ Data Requirement            Limitations         ⑬ Documentation
 ③ Pipeline Design       ⑥ Black Box vs                                    ⑭ Training
                             Mathematical Tool        ⑨ Future Scope        ⑮ Bottlenecks
                          ⑦ Minimum Data Volume       ⑩ Tool Stack          ⑯ Final Accuracy
                                                       ⑪ Accuracy Estimate
```

---

## Stage ① — EEGNet Architecture Study
**Focus:** Produce a detailed understanding and documentation of the EEGNet model's architecture, as the foundation Dhizar is reverse-engineered from.

☐ Stage acknowledged and closed

---

## Stage ② — Modification Scoping
**Focus:** Determine what can be modified in this architecture to serve our specific purpose — reducing controller latency by predicting the button/trigger before it happens.

☐ Stage acknowledged and closed

---

## Stage ③ — Pipeline Design
**Focus:** Design the complete pipeline, from raw input to final predicted trigger, expressed with proper visuals, block diagrams, and the underlying theory.

☐ Stage acknowledged and closed

---

## Stage ④ — Input / Output Definition
**Focus:** Define precisely what the model takes as input, and precisely what it provides as output.

☐ Stage acknowledged and closed

---

## Stage ⑤ — Data Requirement Definition
**Focus:** Define what kind of data this model will require to function.

☐ Stage acknowledged and closed

---

## Stage ⑥ — Black Box or Mathematical Tool?
**Focus:** Settle whether Dhizar is being built and understood as an interpretable mathematical tool or as a black-box model — and what that choice means for how we trust and validate it.

☐ Stage acknowledged and closed

---

## Stage ⑦ — Minimum Data Volume
**Focus:** Establish, at minimum, how much data this model will require to be viable.

☐ Stage acknowledged and closed

---

## Stage ⑧ — Current Limitations
**Focus:** Document the model's limitations as they stand at this stage of its development.

☐ Stage acknowledged and closed

---

## Stage ⑨ — Future Scope
**Focus:** Define where this model can go beyond its current stage.

☐ Stage acknowledged and closed

---

## Stage ⑩ — Tool Stack Finalization
**Focus:** Settle on the tools that will be used to build, train, and run Dhizar.

☐ Stage acknowledged and closed

---

## Stage ⑪ — Estimated Accuracy
**Focus:** Establish an estimated accuracy target for the model before training begins.

☐ Stage acknowledged and closed

---

## Stage ⑫ — Build
**Focus:** Build the model.

☐ Stage acknowledged and closed

---

## Stage ⑬ — Documentation
**Focus:** Document the built model.

☐ Stage acknowledged and closed

---

## Stage ⑭ — Training
**Focus:** Begin and carry out training.

☐ Stage acknowledged and closed

---

## Stage ⑮ — Bottlenecks
**Focus:** Identify what bottlenecks are being faced during and after training.

☐ Stage acknowledged and closed

---

## Stage ⑯ — Final Accuracy
**Focus:** Establish the final, measured accuracy achieved.

☐ Stage acknowledged and closed

---

## 🎯 Alignment Reminder

Dhizar has exactly one job: **make the controller feel instant by predicting a moment sooner.** Every stage above exists in service of that single purpose — not accuracy for its own sake, not sophistication for its own sake, but a real reduction in the latency the player feels.
