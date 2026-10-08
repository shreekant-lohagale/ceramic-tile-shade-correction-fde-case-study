# Task 3 — Submission Form Answers

## In two sentences: what is the client's real problem, and how is it different from what they asked for?

The real problem is not simply comparing two tile images; it is the lack of a repeatable mapping from the measured difference on a fired tile to the printer correction that will produce an acceptable next sample. They asked for a fast vision-style system that tells them what to edit, but I would first prove that this correction mapping is stable and keep QC approval in the loop before automating the printer.

---

## Who are the people in this deal, and what does each of them need to hear from you?

Procurement needs ROI, timeline, deployment risk and a clear commercial structure. QC needs a measurable pass/fail rule and confidence that the system will not accept bad shade. Design/prepress needs recommendations in the same printer/file controls they use today. Printer/OEM and IT/OT teams need safe integration boundaries. Production needs fewer correction rounds without creating a new bottleneck, and the kiln/process team needs us to acknowledge process drift that may be outside the printer’s control.

---

## The five questions you would ask on the next call, ranked. For each one: why it matters, and what you assumed while you wait for the answer.

1. **What exactly counts as a QC pass for each SKU?**  
   Why: this defines the target and evaluation.  
   Assumption: the current spectrophotometer/lab procedure remains the source of truth.

2. **What printer controls can we safely recommend changing?**  
   Why: we need the exact six-channel/file control space and safe limits.  
   Assumption: the pilot recommends bounded changes through the existing prepress workflow and does not write directly to the printer.

3. **Do you have paired history of failed sample → exact file edit → next fired result?**  
   Why: that is the training data needed to learn the correction mapping.  
   Assumption: it is incomplete, so the pilot includes controlled data collection.

4. **Which batch/process variables can we log?**  
   Why: kiln, material, glaze/ink and machine state may change the final shade even with the same file.  
   Assumption: phase 1 logs these variables but does not try to control the full process.

5. **What is the real economic baseline?**  
   Why: we need affected batch starts/month, reject or rework cost per m² and current correction cost to calculate ROI.  
   Assumption for the illustrative business case only: 6 affected starts/month, 6,000 m²/event and ₹50/m² avoidable loss.

---

## Which options did you consider, and why did you pick the one you did? What did you rule out, and why?

I considered camera-only comparison, rule-based correction, a data-driven correction recommender, and a full closed-loop process model. I chose a data-driven recommender using existing lab measurements plus controlled correction history because it directly learns the missing relationship between fired-tile deviation and printer adjustment while remaining bounded and explainable. I ruled out camera-only RGB comparison as the primary signal because lighting/gloss can distort it, and I ruled out fully autonomous process control because the current evidence and data are not strong enough.

---

## How will the client know the pilot worked? Metric, baseline, threshold, how many runs, who measures, and what result would make you stop.

Baseline is the current 6–7 hour correction round with multiple manual variants. The main quality metric is the plant’s existing QC pass/fail result on the fired verification tile; operational metrics are recommendation latency, number of correction rounds and time to an accepted sample.

On at least 30 unseen batch-start correction events, I would require:
- ≥80% to reach QC pass after one recommended correction round
- recommendation in <5 minutes
- ≥50% reduction in median correction-loop time
- no increase in false QC acceptance

QC measures the final result, design records actual edits, and the system logs recommendation/time/confidence. I would stop or redesign if measurements are not repeatable, the correction mapping is unstable, recommendations frequently hit printer limits, or the model cannot beat a simple rule/operator baseline.

---

## What does this cost the client, and what does it cost us to deliver? Show the arithmetic and name the pricing structure. What value does the client get, in their money?

I would propose an **8-week fixed-fee pilot at ₹15 lakh**, paid 40% at kickoff, 40% when shadow pilot starts and 20% at final handover.

Estimated internal delivery cost (assumption):
- FDE / solution lead: ₹3.5 lakh
- ML/data engineer: ₹2.5 lakh
- integration/QA: ₹1 lakh
- travel/calibration/contingency: ₹1 lakh
- total: about ₹8 lakh

The discovery notes do not contain verified reject economics, so the ROI is illustrative. Assuming 6 affected batch starts/month, 6,000 m² exposure/event, ₹50/m² avoidable loss and 50% reduction:

6 × 6,000 × ₹50 = **₹18 lakh/month baseline exposure**

₹18 lakh × 50% = **₹9 lakh/month avoided cost**

₹15 lakh / ₹9 lakh ≈ **1.7 months illustrative payback**

I would replace those assumptions with the plant’s real numbers before commercial approval.

---

## Timeline. What is the first result the client sees, and when? Which steps depend on the client rather than on us?

The plan is 8 weeks. Week 1 is plant discovery and QC definition; Week 2 is measurement/data setup; Weeks 3–4 collect controlled correction examples and build baselines; Week 5 is the first client-visible recommendation demo; Weeks 6–7 are shadow use on real batch starts; Week 8 is the go/no-go review.

Client dependencies are access to QC/design/printer/process users, master and variation tiles, lab instruments, printer/OEM information, safe test permissions and enough batch-start events during the pilot.

---

## Where did you push back on the client, narrow their ask, or tell them not to do something?

I pushed back on the idea that this should start as a fully automated “vision analytics” system that removes the lab loop. The software can generate a correction in minutes, but the fired result still needs physical verification during the pilot, and shade can also move because of kiln/material/process changes. I narrowed the first deployment to a batch-start decision-support system for 3–5 repeat SKUs, with QC approval and no automatic write-back to the printer.

---

## What is most likely to go wrong with your plan, and what in these notes did you not trust?

The biggest risks are missing paired correction history, an undocumented QC threshold, unstable measurements, SKU-specific behaviour and process drift being larger than the printer correction can compensate for. I also would not treat the quoted 5,000–10,000 m² wrong-shade figure as reliable yet because the recording itself marks the exact number as unclear. I also would not assume the same printer/image correction works across plants without evidence.

---

## What did you use AI for? Which tools, where they helped, where they misled you, and what you threw away.

I used ChatGPT as a thinking and drafting assistant to structure the discovery notes, compare solution options, pressure-test assumptions and make the pilot/evaluation plan clearer. It helped most with organizing the FDE case and identifying where a “computer vision” framing was too narrow.

I did not use an AI model as the proposed production decision engine just because the brief mentioned vision analytics. I threw away the ideas of a generic camera-only classifier, an LLM in the correction loop and a fully autonomous printer controller because the missing issue is paired process/correction data, not language generation.

---

## Github Repo URL

Add your public repository URL here after creating the Task 3 repository. Do not publish the confidential discovery-call notes.
