# Note to Procurement Lead
## Ceramic Tile Shade-Correction Pilot

### What we propose

We recommend an **8-week pilot at one plant**, focused on **3–5 repeat, high-volume SKUs**.

The system will compare the fired batch-start sample with the approved master using the plant’s existing lab measurements, then recommend a bounded change to the printer input. A QC/design user will approve every recommendation during the pilot.

We are **not** proposing a fully autonomous printer controller at this stage. The first objective is to prove that the measured shade/gloss difference can be translated into a repeatable printer correction.

### Why this approach

Today, the team can spend roughly **6–7 hours on a correction round**, creating several variants, printing/firing them and checking them again. The main value is reducing repeated trial rounds and the production exposure that occurs while the correct shade is being found.

The client asked for a result in minutes. We can target **under 5 minutes for the software recommendation**, but the fired result still needs physical verification during the pilot.

### What we need from you

Before kickoff we need:

- one plant and 3–5 pilot SKUs
- access to QC, design/prepress, printer and process users
- master and variation tiles
- the current QC pass/fail rule
- spectrophotometer/gloss readings
- printer/OEM information and safe adjustment limits
- permission to record correction attempts and run bounded tests
- basic economics: affected batch starts/month and reject/rework cost per m²

### What you will see

**Week 1:** confirmed workflow, QC rule and pilot SKUs  
**Weeks 3–4:** first structured correction dataset and baseline model  
**Week 5:** first recommendation demo using plant data  
**Weeks 6–7:** shadow use during real batch starts  
**Week 8:** measured go/no-go result and scale-up recommendation

### Pilot success

We propose the pilot is successful if, on at least **30 unseen batch-start correction events**:

- at least **80%** reach the plant’s QC pass condition after one recommended correction round
- recommendation time is **under 5 minutes**
- median correction-loop time is reduced by at least **50%**
- no recommendation bypasses QC approval
- there is no increase in false QC acceptance

The plant’s own QC threshold remains the source of truth.

### Commercial proposal

**Fixed-fee 8-week pilot: ₹15 lakh**

Payment structure:
- ₹6 lakh at kickoff
- ₹6 lakh when shadow pilot starts
- ₹3 lakh at final pilot handover

### Illustrative value

The discovery call did not provide verified reject cost or batch-start frequency, so the example below is only a decision model.

Assume:
- 6 affected batch starts/month
- 6,000 m² exposure/event
- ₹50/m² avoidable reject/rework cost
- 50% reduction from the pilot

Then:

**6 × 6,000 × ₹50 = ₹18 lakh/month baseline exposure**

At 50% reduction:

**₹18 lakh × 50% = ₹9 lakh/month avoided cost**

A ₹15 lakh pilot would therefore have an illustrative simple payback of about **1.7 months**. Before approval, we would replace these assumptions with the plant’s actual numbers.

### Decision requested

Approve the 8-week pilot **only after** a short workshop with the plant QC/design team confirms the pass/fail rule, printer controls and data availability. If those basics are not measurable, we should not start model development.
