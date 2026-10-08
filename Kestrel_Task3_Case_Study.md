# Forward Deployed Engineer Case Study
## Ceramic Tile Shade-Correction Pilot

**Client:** Global ceramic tile manufacturer with plants in the UAE, India and Bangladesh  
**Prepared for:** Procurement, Plant QC, Design/Prepress, Production and IT/OT  
**Status:** Proposal based on the first discovery call only  
**Important:** Commercial figures and technical thresholds below are explicit pilot assumptions until confirmed with the plant team.

---

## Page 1 — Executive recommendation

### Decision

Do **not** start with a fully autonomous vision system that directly edits printer files.

Start with an **8-week, one-plant decision-support pilot** on **3–5 repeat, high-volume SKUs**. The pilot should measure the fired production tile against the approved master, recommend a bounded change to the printer input, and require a QC engineer to approve the recommendation before the next verification print/fire.

The first goal is not “remove the lab completely.” The first goal is to prove that a repeatable mapping exists between:

**measured fired-tile deviation → printer-input correction → improved fired result**

If that mapping is stable, the workflow can later move toward more automation.

### Why this is the right scope

The client’s stated symptom is shade mismatch when the same SKU is run again. Today, operators visually compare tiles, edit the print reference in Photoshop, generate 6–7 variants, print/fire them, measure again, and sometimes repeat. One correction round takes about **6–7 hours**.

The client wants the system to return a correction in **5–6 minutes**. A software recommendation can be produced in minutes, but the physical result still depends on printing and firing. Therefore the pilot separates:

- **Recommendation latency:** target under **5 minutes**
- **Physical verification:** still required during the pilot
- **Business objective:** reduce repeated trial rounds, reject exposure and operator-to-operator variation

The notes also say that, once the first sample for a batch is correct, the rest of that batch is usually fine. That makes a **batch-start calibration station** a better first deployment than an expensive always-on camera system.

### What success looks like

For the pilot, I would define success as:

- at least **80% of unseen batch-start correction events** reaching the client’s QC pass condition after **one recommended correction round**
- recommendation generated in **<5 minutes**
- at least **50% reduction in median correction-loop time** versus the current 6–7 hour baseline
- no automatic write to the printer; QC signs off every recommendation
- no degradation in final QC acceptance

A final absolute colour/gloss threshold must be confirmed by the QC team. Until then, the working assumption is that the existing lab procedure and spectrophotometer remain the source of truth.

---

## Page 2 — Problem understanding and stakeholders

### The real problem

The client asked for a system that compares the master tile with a current production tile and tells the operator what to change in the printer input.

The deeper problem is that the factory has **no reliable, repeatable mapping from measured fired-tile difference back to the printer correction needed for that SKU and process state**.

The current loop is slow because operators compensate by eye, create several variants, print/fire them, test them, and repeat. The same original image can drift over time because material, firing and production conditions change.

This is therefore not only a computer-vision problem. It is a **closed-loop manufacturing calibration problem** with colour measurement, printer controls, process drift and human sign-off.

### As-is process

1. A repeat SKU is scheduled.
2. The printer uses the existing reference/master image.
3. A sample is printed and fired.
4. QC compares it with the master tile.
5. If it fails, the design/prepress operator manually edits colour levels in Photoshop.
6. Around 6–7 candidate variants may be created.
7. Variants are printed, fired and checked.
8. If none pass, the loop repeats.
9. Once a sample is accepted, the batch proceeds.

The call notes indicate a correction round takes roughly **6–7 hours**, and the client believes **thousands of square metres** may be produced in the wrong shade while the issue is being corrected. The exact reject-area figure in the recording is unclear and must be validated before using it in a business case.

### Stakeholders

| Stakeholder | What they need from us |
|---|---|
| Head of IT & OT Procurement | Commercial structure, hardware/OEM risk, deployment model, security, supportability and ROI |
| Special Projects Lead, Procurement | A credible pilot scope, timeline, evidence of feasibility and clear dependencies |
| Plant QC lead | Pass/fail definition, measurement repeatability, safe thresholds and final sign-off authority |
| Design / Prepress / Photoshop operator | Recommendations expressed in the same controls they actually change today |
| Digital printer operator / OEM | Valid file format, allowable channel changes, adjustment limits and integration constraints |
| Kiln / process engineer | Visibility into process variables that can create colour drift and invalidate a correction |
| Plant manager / production lead | Reduced waste and delay without adding a fragile production bottleneck |
| IT/OT / cybersecurity | Local deployment, network boundaries, device access and auditability |

A critical gap in the first call is that the day-to-day QC and design users were not present. I would not freeze the product design until they join the next discovery session.

---

## Page 3 — What I do not know yet

### Five ranked questions for the next call

#### 1. What exactly counts as a QC pass for each SKU?
**Why it matters:** The system cannot be evaluated without the same acceptance rule used by the plant. We need the approved colour metric, gloss tolerance, sampling locations, number of readings, repeatability and whether tolerances vary by SKU/finish.

**Assumption until answered:** The existing lab spectrophotometer and current QC procedure remain the source of truth. We will not replace the acceptance method in the pilot.

#### 2. What printer controls can we safely recommend changing?
**Why it matters:** We need the exact input space: printer/OEM, file format, six colour channels, adjustment granularity, legal ranges, Photoshop workflow and whether changes are global or spatial.

**Assumption until answered:** Operators can change six bounded colour-channel values in the existing prepress workflow, and the pilot will recommend changes rather than write directly to the printer.

#### 3. Do we have historical paired correction data?
**Why it matters:** A useful model needs examples of: failed fired sample measurement → exact file change → next fired sample measurement → pass/fail.

**Assumption until answered:** Historical data is incomplete or unstructured. The pilot therefore includes controlled data collection and a small designed perturbation study.

#### 4. Which process variables can be logged at batch start?
**Why it matters:** Raw-material lot, glaze/ink lot, kiln recipe, firing time, speed, temperature and machine state may change the relationship between printer input and final fired shade.

**Assumption until answered:** Phase 1 does not attempt root-cause control, but it logs available process metadata so we can detect when the correction model is outside its valid operating range.

#### 5. What is the economic baseline?
**Why it matters:** Procurement needs the real number of affected batch starts per month, reject/rework cost per m², line rate, scrap disposition, overtime and cost of each correction round.

**Assumption until answered:** For an illustrative business case only, use 6 affected batch starts/month, 6,000 m² exposure/event and ₹50/m² avoidable internal loss. These are placeholders, not client facts.

---

## Page 4 — Solution options and recommended engineering

### Options considered

#### Option A — Camera-only visual comparison
Use calibrated RGB images and computer vision to compare master and current tile.

**Why not as the primary solution:** Lighting, gloss, texture and firing can make RGB comparison unstable. The client already has a spectrophotometer that measures L/a/b directly. Vision can add spatial information later, but it should not be the sole measurement system.

#### Option B — Rule-based correction table
Encode operator heuristics such as “if L is high, reduce a certain channel.”

**Strength:** Fast, explainable and easy to deploy.

**Weakness:** Likely brittle across SKUs, inks and process drift. Useful as a baseline, not the final pilot model.

#### Option C — Data-driven correction recommender
Learn the local relationship between measured deviation and printer-channel changes from paired before/after correction data.

**Recommended.** It directly addresses the missing mapping, can remain bounded and interpretable, and can start with simple models before any deep learning.

#### Option D — Full closed-loop root-cause model
Model raw materials, glaze, ink, kiln, printer and environment, then automatically control the process.

**Not in the first pilot.** This is a larger manufacturing-control program, not a safe first deliverable from the current evidence.

### Recommended architecture

**1. Reference data**
- approved master tile measurements
- current approved printer file
- SKU/finish metadata

**2. Batch-start sample**
- print and fire the first sample
- capture existing lab readings: L*, a*, b* and gloss
- optionally capture controlled, calibrated images for spatial defects/pattern shifts

**3. Context**
- printer/file version
- ink/glaze/raw-material lot where available
- kiln recipe / firing metadata
- plant, line and timestamp

**4. Correction engine**
Start with a constrained local model per SKU family:
- baseline: linear / regularized multivariate regression or response matrix
- upgrade only if justified: gradient-boosted model or other nonlinear regressor
- objective: recommend bounded channel adjustments that reduce the measured error while staying inside operator/OEM limits

**5. Human sign-off**
The QC/design operator sees:
- measured difference from master
- recommended channel changes
- confidence / “insufficient evidence” warning
- comparable historical examples where available

The operator approves, edits through the existing workflow, and runs one verification sample.

**6. Learning loop**
The result of every correction is stored:
- before measurement
- recommended/actual edit
- after measurement
- QC pass/fail
- process context

This is the dataset that improves the system over time.

### What I deliberately would not build in the pilot

- autonomous write-back to the printer
- an LLM in the correction path
- continuous line monitoring
- a plant-wide root-cause model
- a large deep-learning stack before we have paired correction data
- a replacement for the client’s QC acceptance process

---

## Page 5 — Evaluation and build plan

### Pilot evaluation

#### Baseline
Current process:
- roughly 6–7 hours per correction round
- 6–7 manually created variants in a round
- subjective visual decisions
- sometimes repeated rounds

#### Primary quality metric
**QC pass on the fired verification tile using the client’s existing lab procedure.**

If the client does not have a single documented tolerance, agreeing that tolerance is a prerequisite to the pilot. We should not invent an acceptance threshold on their behalf.

#### Secondary metrics
- recommendation latency
- number of correction rounds
- time from first failed sample to accepted sample
- percentage of recommendations overridden by QC
- percentage of cases where system refuses to recommend
- reject/rework area attributable to shade mismatch

#### Proposed pilot threshold
On at least **30 unseen batch-start correction events** across 3–5 SKUs:

- ≥80% reach QC pass after one recommended correction round
- <5 minutes recommendation latency
- ≥50% reduction in median correction-loop time
- no increase in false QC acceptance
- all recommendations stay within agreed printer limits

Thirty events is a pilot target, not a statistical guarantee. If changeover volume is lower, extend the pilot rather than weaken the test.

#### Who measures
- QC owns the master comparison and pass/fail result
- Design/prepress records actual edits
- FDE system records recommendations, timings and model confidence
- Production validates operational impact
- Procurement signs off the value calculation

#### Stop conditions
Stop or redesign the pilot if:
- the same input correction produces materially inconsistent fired results under apparently similar conditions
- the model cannot beat a simple operator/rule baseline after the initial controlled dataset
- recommendations frequently hit printer limits
- measurement repeatability is poor
- process drift is larger than the printer correction can compensate for
- client cannot supply a stable QC acceptance rule

### 8-week plan

**Week 1 — Discovery + site workflow**
- QC/design/printer/kiln workshop
- confirm pass/fail rule
- map printer controls and file formats
- collect 10–20 recent correction examples if available
- select 3–5 pilot SKUs

**Week 2 — Instrument and data contract**
- define measurement positions
- create correction-run logging template
- validate spectrophotometer repeatability
- set safe printer adjustment bounds

**Weeks 3–4 — Controlled data collection**
- run bounded perturbation/correction experiments
- record before/after measurements
- establish rule-based and linear baselines

**Week 5 — First client-visible result**
- offline recommender on pilot data
- replay historical/controlled runs
- review recommendations with QC/design users

**Weeks 6–7 — Shadow pilot**
- system recommends during real batch starts
- humans approve every action
- measure quality, time and overrides

**Week 8 — Decision**
- compare against baseline
- quantify avoided rounds/waste
- decide: scale, extend data collection, or stop

### Client dependencies
- access to QC/design/printer operators
- site access or representative samples
- master + variation tiles
- existing lab instruments
- printer/OEM documentation
- permission to run controlled bounded adjustments
- batch-start events during the pilot window

---

## Page 6 — Cost, value and risks

### Proposed commercial structure

**8-week fixed-fee pilot: ₹15 lakh**

Suggested payment:
- 40% at kickoff: ₹6 lakh
- 40% when the shadow pilot starts: ₹6 lakh
- 20% on final pilot report and handover: ₹3 lakh

Scale-up to additional lines/plants is priced only after the pilot proves the correction mapping.

### Estimated delivery cost to us — assumption

| Item | Estimated internal cost |
|---|---:|
| FDE / solution lead | ₹3.5 lakh |
| ML / data engineer | ₹2.5 lakh |
| Integration / QA support | ₹1.0 lakh |
| Site travel, calibration support, contingency | ₹1.0 lakh |
| **Estimated total** | **₹8.0 lakh** |

These are proposal assumptions, not facts from the discovery notes.

### Illustrative client value

The call did **not** provide verified reject cost per m² or affected batch-start frequency. Therefore I would show procurement a formula and one clearly labelled example.

**Illustrative assumptions only**
- 6 affected batch starts/month
- 6,000 m² exposed per event
- ₹50/m² avoidable reject/rework cost
- pilot reduces this exposure by 50%

Baseline monthly exposure:

**6 × 6,000 × ₹50 = ₹18,00,000/month**

Illustrative avoided cost at 50% reduction:

**₹18,00,000 × 50% = ₹9,00,000/month**

Under those assumptions, a ₹15 lakh pilot has a simple payback of:

**₹15,00,000 / ₹9,00,000 ≈ 1.7 months**

I would not present this as committed savings. Procurement must replace the three assumptions with verified plant economics before approval.

### Main risks

1. **No structured correction history**  
   Mitigation: controlled data-collection phase and simple models first.

2. **QC acceptance is subjective or undocumented**  
   Mitigation: agree a measurable pass/fail rule before model evaluation.

3. **Process drift dominates printer correction**  
   Mitigation: log process variables, detect out-of-scope conditions, and refuse to recommend when confidence is low.

4. **Different SKUs behave differently**  
   Mitigation: start with 3–5 repeat, high-volume SKUs; do not claim universal generalisation.

5. **Printer/OEM limits are unknown**  
   Mitigation: bounded recommendations and OEM/operator review.

6. **Measurement is not repeatable**  
   Mitigation: measurement-system check before training.

7. **Users do not trust the recommendation**  
   Mitigation: show measured deviation, recommended change, similar past examples and keep human approval.

8. **The “5–6 minute” expectation is interpreted as fully verified final shade**  
   Mitigation: explicitly separate software recommendation time from physical print/fire verification time.

### Final recommendation

Proceed with a paid, bounded pilot only after the plant QC/design team joins discovery. The strongest first product is a **batch-start correction recommender with human approval**, not a fully autonomous vision system.

The go/no-go decision after eight weeks is simple: if the system consistently reduces correction rounds and time while meeting the plant’s own QC standard, scale it. If not, stop before integrating it into production.
