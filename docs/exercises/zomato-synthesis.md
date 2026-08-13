# Zomato Interview Synthesis — Wk 1 Extended Exercise
**Student Name** : Riddhesh Sajwan 

**Batch** : Aug-26 Date
 
**Submitted** : 13/08/2026 

**GitHub file URL** : [zomato-synthesis.md](https://github.com/riddheshSajwan/ai-pm-journey/blob/main/docs/exercises/zomato-synthesis.md)

## Section 1 - Quote Extraction

Verbatim quotes from the 6 interviews - 

### Interview A - Rohan Verma (SDE, Bangalore, WFH)

- "I ordered biryani at 8:30 PM. App promised 30 minutes. It arrived at 9:45 PM. Cold."
  - Signal: The order delivery is getting delayed by a lot, hampering with customer experience, trust and food quality.
- "75 minutes! And this isn't a one-off. I've timed it. Between 7-9 PM in Bangalore, if I order Zomato, I'm gambling. 60-70 minute deliveries are the norm, not the exception."
  - Signal: The customer has seen this pattern repeat again and again, delayed delivery isn't one time experience.
- "I've literally stopped opening Zomato before 9 PM. I check Swiggy first. If Swiggy has what I want, done. Zomato is my backup now."
  - Signal: During peak hours, the customer has already switched to the competitor.
- "When Gold was 200 rupees for unlimited free delivery, no restrictions, it was a no-brainer. Now they've added minimum order values, restricted restaurants, handling fees Gold doesn't cover... my monthly savings dropped from about 500 rupees to 150. Membership was 200. Math didn't work anymore."
  - Signal: No real benefit of Gold is being seen by the customer, as they still have to pay delivery fees below threshold and hence impacting their overall budget.
- "Match the ETA to reality. I'm not asking for magic. If the delivery is going to take 60 minutes, tell me 60. Don't promise 30 and give me cold food at 75. Just be honest about the promise."
  - Signal: Customer expects transparency in delivery ETA. Incorrect delivery promises impact customer trust on the platform. 

### Interview B - Meera Krishnan (Restaurant owner, Chennai)

- "Weekdays, maybe 40 percent delivery. Weekends... we try to reduce it. Ideally close to zero."
  - Signal: Demand is overwhelming the restaurant owner.

- "Saturday-Sunday, we get 3x the walk-ins. My kitchen has one dosa griddle, one idli steamer, one prep station. Weekdays fine, weekends chaos."
  - Signal: The bottleneck is kitchen capacity over the weekend due to heavy walk-ins.
- "We cancel. As many as we can. Maybe 20-30 percent of weekend orders. Sometimes more."
  - Signal: Low fulfilment of restaurant is mostly due to cancellation during the weekend.
- "Zomato thinks I don't want orders. I WANT orders — I'm running a business. I just cannot FULFILL delivery orders on weekends."
  - Signal: Restaurant wants to fulfil order during the weekend, but capacity is a problem.
- "Let me tell the app: \"Saturday 1-3 PM, only 5 delivery orders per hour.\" Right now it's binary. I need a knob."
  - Signal: The restaurant wants to be able to set their capacity on the Zomato app, so as to avoid low fulfilment penalty. 

### Interview C - Priya Nair (Marketing exec, Mumbai)

- "Delivery fees. Bruh, it's the worst."
  - Signal: high delivery fees is leading to customers backing out.
- "See: Total 257 rupees. Delivery fee 45. GST 12. Suddenly it's not 200. It's basically 260."
  - Signal: Price of the item to be ordered and final amount to be paid to zomato varies a lot and that scares customer away. 
- "If delivery fee is above 40 rupees, most of the time yes. Below 30, I don't even blink. Between 30-40, I think a bit. Above 40, I usually bail. Above 60... it literally never happens. I never checkout."
  - Signal: delivery fees under 40 seems to be a sweet spot, anything above it gets discarded.
- "Only at checkout. Which is annoying. I've already invested time picking food. Then boom — fee revealed. It feels like a trap."
  - Signal: Surprise amount at the time of checkout ruins customer experience and breaks trust on app.
- "Show me the total price on the restaurant page. Include delivery. Don't surprise me at the end."
  - Signal: customer wants to make a decision knowing everything at once. Don't want to be surprised in the end.

### Interview D - Karthik Reddy (PM, Bangalore, ex-Gold)

- "The value proposition eroded. Slowly. Then all at once."
  - Signal: the churn happened gradually.
- "In July, they added \"restrictions\". Suddenly it was \"Gold-eligible restaurants only\" — they cut the eligible list by like 40 percent. Then minimum order value went up from 199 to 249. Then they added a \"handling fee\" that Gold didn't cover."
  - Signal: the policy changes that led to customer gradually churning out of Zomato gold.
- "In June I saved 480 rupees. July, 320. August, 180. September I would've saved 150 max if I'd stayed."
  - Signal: customer savings have dropped gradually while being subscribed to gold.
- "4 out of my 6 Gold-subscribing friends quit in the same window — July, August, September. Q3 was a bloodbath."
  - Signals: this is not an isolated case.
- "But hiding changes behind fine print? That's a trust break."
  - Signals: zomato has lost the customer's trust.

### Interview E - Anjali Patel (Consultant, Delhi, one-time user)

- "I tried it once. Six months ago. Never again."
  - Signal: first time user, didn't like the first experience itself.
- "Finally arrived 65 minutes late — the app had promised 35, it took over an hour. Food was cold. Spring rolls soggy. And the dessert was missing entirely."
  - Signal: same story with this customer, late delivery, cold food but this time one item missing too.
- "50 rupees. For a 550-rupee order, 65 minutes late, cold food, missing item. 50 rupees credit. Not refund — credit."
  - Signal: after a bad delivery experience and even worse customer support experience. credit of 10% of order amount was done, not even refund.
- "First delivery experience determines whether I trust an app. Zomato failed the test."
  - Signal: a bad first impression leading to customer churning
- "If Zomato knew I was a first-timer, they should have treated me like gold. Instead they treated me like a rounding error."
  - Signal: first time customer expects to be given more priority and first timer perks

### Interview F - Vikram Iyer (Tech Lead, Bangalore, 5-year power user)

- "I WAS. I still USE them. But I don't recommend them anymore. Big difference."
  - Signal: An active user of the app, but is no more an advocate of the app.
- "Two years ago, 25-30 minute deliveries were standard for my area. Now it's 40-50 average, and 60+ during evening rush."
  - Signal: delivery time has increased over time within a specific area.
- "Especially between 7 and 9 PM. But Whitefield especially — highest concentration of tech workers ordering dinner, hasn't been matched with delivery supply."
  - Signal: dense demand are not being met by required supply.
- "My cousin joined Zomato last month for the first time. Terrible first delivery experience — cold food, late, rider couldn't find him. Uninstalled within a week."
  - Signal: another bad first time experience leading to churn.
- "fix the evening delivery times in high-density tech corridors. Whitefield, Koramangala, HSR, Indiranagar. That's your bleeding wound."
  - Signal: customer giving an advice to solve the bad customer experience issue.

---

## Section 2 - Themes

### Theme 1: Delivery time beyond promised ETA.

- **Who mentioned it:** Rohan, Vikram, Anjali
- **Anchor quote:** "75 minutes! And this isn't a one-off. I've timed it. Between 7-9 PM in Bangalore, if I order Zomato, I'm gambling." - Rohan
- **Why it matters:** This is a repeated issue and has led to multiple churns

### Theme 2: Value proposition by Gold membership declined over time.

- **Who mentioned it:** Karthik, Rohan, Vikram
- **Anchor quote:** "The value proposition eroded. Slowly. Then all at once." - Karthik
- **Why it matters:** Users saw no real value in gold subscription despite paying for it. this value declined gradually over time.

### Theme 3: Customers switching to competitor

- **Who mentioned it:** Rohan, Karthik, Vikram, Anjali
- **Anchor quote:** "I WAS. I still USE them. But I don't recommend them anymore. Big difference." - Vikram
- **Why it matters:** Even the old and retained customers have started considering competitor especially during peak dining hours.

### Theme 4: Surprise charges during checkout killing the order entirely

- **Who mentioned it:** Priya, Karthik, Rohan
- **Anchor quote:** "Only at checkout. Which is annoying. I've already invested time picking food. Then boom — fee revealed. It feels like a trap." - Priya
- **Why it matters:** Customer feels cheated and hence loses the desire to make the final payment at checkout.

### Theme 5: A bad first time experience leading to straight churn

- **Who mentioned it:** Rohan, Anjali, Vikram
- **Anchor quote:** "First orders should be sacred — like a job interview. You get one shot. And they blew it." - Anjali
- **Why it matters:** First impression is often the last impression and a bad first impression has indeed led to customer churning permanently.

---

## Section 3 - Triangulation Map

| # | Data Problem | Data Signal | Confirming Interview Quotes | Triangulation Strength |
| --- | --- | --- | --- | --- |
| 1 | Bangalore dinner time delivery delay | 7-9 PM Bangalore orders ≈ 61 min avg vs ≈ 31 min elsewhere (~131 orders sampled); ratings drop on late orders | Rohan: "Between 7-9 PM in Bangalore, if I order Zomato, I'm gambling." Vikram: "40-50 average, and 60+ during evening rush... between 7 and 9 PM... hasn't been matched with delivery supply." Anjali (Delhi, cross-city): "65 minutes late — the app had promised 35." | **STRONG** |
| 2 | Walk-in surge leading to order cancellations | Weekend cancellation ≈ 22.7% vs ≈ 10.3% weekday for South-Indian restaurants | Meera (owner): "We cancel... Maybe 20-30 percent of weekend orders." + "I WANT orders — I just cannot FULFILL delivery orders on weekends." Vikram (customer): favorite South-Indian place "marked \"temporarily closed\" every Sunday 12-3." | **Moderate** |
| 3 | surprise charges during checkout leading to dropping of order | Cart completion 78% (≤₹30) -> 61% (₹31-40) -> 32% (₹41-60) -> 10% (>₹60) | Priya: "Below 30, I don't even blink... Above 40, I usually bail. Above 60... it literally never happens." + "Only at checkout... it feels like a trap." | **Weak** |
| 4 | Gold subscribers dissatisfied with increasing charges on delivery | Pre-June cohort ~ 73% active vs post-June ~ 38% active (~ 2× worse) | Karthik: "In July, they added restrictions..." + "4 out of my 6 Gold-subscribing friends quit... July, August, September." Rohan: "savings dropped from about 500 rupees to 150." Vikram: "cancelled 3 months ago... value dropped without warning." | **STRONG** |
| 5 | Poor first impression leading to permanent churn | Cohort of users with total_orders_lifetime = 1 (never returned after first order) | Anjali: "I tried it once... Never again. First delivery experience determines whether I trust an app." Vikram: "My cousin joined... terrible first delivery... Uninstalled within a week." | **MODERATE** |

### What interviews revealed that data COULDN'T

**1. Meera isn't refusing orders. She physically cannot cook them.**
 
In the data, her restaurant just looks unreliable: weekend cancellations spike, fulfilment drops, the system flags her "low fulfillment" and quietly cuts her visibility. Sitting in her kitchen, the story inverts. She badly wants the orders — she's running a business. But she has one dosa griddle, one idli steamer and one prep station, and at 1 PM on a Sunday there are 60 people already seated in front of her. Every delivery order she accepts makes a walk-in customer wait 90 minutes. So she cancels, and then gets penalised for arithmetic the platform forced on her by offering only an on/off switch. The data said "bad partner." She said, "I need a knob." Those are not the same problem and they don't have the same fix.
 
**2. The Gold exodus had a date on it, and it wasn't really about the money.**
 
The retention curve shows people leaving after June. It cannot tell you why, and the obvious guess — price — is wrong. Karthik walks through exactly what happened in July: the eligible-restaurant list cut by roughly 40%, minimum order pushed from ₹199 to ₹249, a handling fee Gold didn't cover, then peak-hour surcharges it also didn't cover. Layer on layer, none of it announced. What actually pushed him out wasn't the ₹200. It was noticing his savings slide from ₹480 to ₹320 to ₹180 and realising someone had decided that on purpose. He said he'd probably have paid ₹400 for an honest, clearly-explained Gold. What he wouldn't do is keep paying for something being hollowed out behind fine print — and that's a trust cost that shows up in the churn number long after the decision was made.
 
**3. ₹40 isn't the real problem. Finding out at the last second is.**
 
The funnel shows carts dying above roughly ₹40 in delivery fee, and it's tempting to read that as "drop the fee." Priya's account points somewhere cheaper to fix. She browses, finds a ₹200 dal makhani, adds it, gets to checkout and sees ₹257. The fee wasn't hidden maliciously — it just arrives after she's spent ten minutes choosing food, which is precisely when it stings most. Her word for it is "trap," and she closes the app and makes Maggi instead. She's not asking for free delivery. She's asking to see the all-in number on the restaurant page so she can make one decision, once. The data gives you a price threshold. Only the interview tells you the damage is being done by the sequence, not the amount.

---

## Section 4 - Problem Statements

### Problem Statement 1

"When ordering dinner between 7-9 PM in Bangalore's high-density tech corridors (Whitefield, HSR, Koramangala), working professionals (aged 25-35) want their food to arrive close to the promised ETA, but actual delivery runs roughly 2× the promise (≈61 min against a ~31 min promise) - evidenced by ~131 QuickBites orders in that window averaging 61 min vs ~31 min elsewhere, and 2/6 interviews (Rohan, Vikram) naming evening Bangalore delivery as their top pain."

### Problem Statement 2

"When a paying Gold member goes to place a small order from a restaurant they actually want, they expect the free delivery their membership was sold on, but they get charged anyway - because the cart falls under the ₹249 minimum, or the restaurant was dropped from the Gold-eligible list, or a handling fee and peak-hour surcharge Gold doesn't cover are added on top - evidenced by the retention cliff (pre-June cohort ~73% active vs post-June ≈38%) and 3/6 interviews: Karthik naming each restriction added in July (eligible list cut ~40%, minimum order ₹199 -> ₹249, uncovered handling fee), Rohan's monthly savings collapsing from ~₹500 to ~₹150 against a ₹200 membership, and Vikram cancelling because 'value dropped without warning.'"

### Problem Statement 3

"When walk-in demand surges on weekend afternoons, small restaurant partners want to serve dine-in customers first and keep only limited kitchen bandwidth reserved for Zomato orders, but the app offers no way to do that — the only control is going fully offline — so they cancel instead and get hit with low-fulfilment penalties for it — evidenced by a ~22.7% weekend cancellation rate for South-Indian restaurants (vs ≈10.3% weekday) and the owner interview (Meera), who describes cancelling 20-30% of weekend orders, being marked 'low fulfillment' twice with visibility dropping and ratings suffering, and having her ratings drop anyway when she uses the offline switch — plus customer-side confirmation (Vikram) of those restaurants showing 'temporarily closed' every Sunday 12-3."

### Problem Statement 4

"When a user with tight budget constraints wants to order, they choose items accordingly but when they reach the checkout page they see a price much higher after taxes and delivery and decide to not go ahead with the order — evidenced by cart completion collapsing from 78% (≤₹30 fee) to 32% (₹41-60) to 10% (>₹60), and the Priya interview describing a hard ~₹40 ceiling and a late fee reveal that 'feels like a trap'."

### Problem Statement 5

"When a new user places their very first order, they want a flawless experience that earns their trust, but first-timers are exposed to the same late-delivery and weak-recovery failures as everyone else and churn permanently after one bad order — evidenced by a cohort of users with total_orders_lifetime = 1 and 2/6 interviews (Anjali first-hand; Vikram's cousin) describing uninstalling after a single failed delivery followed by a token ₹50 credit."

---

## Section 5 - 4-Quadrant Map

| # | Problem Statement (shortened) | Impact (1-5) | Feasibility (1-5) | Quadrant |
| --- | --- | --- | --- | --- |
| 1 | Bangalore 7-9 PM delivery runs ~2× the promised ETA | 5 | 4 | **SOLVE NOW** |
| 2 | Gold value eroded silently -> subscriber churn | 4 | 3 | STRATEGIC |
| 3 | Weekend cancellations from kitchen capacity limits | 3 | 4 | QUICK WIN |
| 4 | Fees revealed late at checkout -> cart abandonment | 4 | 4 | **SOLVE NOW** |
| 5 | First-order failures -> permanent one-and-done churn | 5 | 3 | STRATEGIC |


### Quadrant Reference

- **SOLVE NOW** : Impact 4-5, Feasibility 4-5 — Wk 1-12 MVP
- **STRATEGIC BETS** : Impact 4-5, Feasibility 1-3 — investigate deeply
- **QUICK WINS** : Impact 1-3, Feasibility 4-5 — batch for later polish
- **AVOID** : Impact 1-3, Feasibility 1-3 — do not commit resources

---

## Section 6 - Chosen Problem + JTBD

### My chosen problem

"When ordering dinner between 7-9 PM in Bangalore's high-density tech corridors (Whitefield, HSR, Koramangala), working professionals (aged 25-35) want their food to arrive close to the promised ETA, but actual delivery runs roughly 2× the promise (≈61 min against a ~31 min promise) - evidenced by ~131 QuickBites orders in that window averaging 61 min vs ~31 min elsewhere, and 2/6 interviews (Rohan, Vikram) naming evening Bangalore delivery as their top pain."

### Why this one?

It has the strongest triangulation of any candidate - the largest single data sample (~131 orders) and multiple independent interviews, including a 300+ order veteran (Vikram) who calls it "your bleeding wound". It is also a prerequisite: fixing evening reliability also de-risks the first-order-churn problem, since a new user's first order is most likely to fail in exactly this window.

### JTBD Statement

"When I finish work around 8 PM in Bangalore and I'm hungry, I want to order dinner and get an arrival time I can actually trust, so I can eat a hot meal without gambling an hour on whether it turns up cold."

### User Story Version

"As a WFH tech professional ordering dinner in Bangalore between 7-9 PM, I want the app's ETA to reflect real evening-rush conditions, so that I can decide with confidence instead of defaulting to Swiggy."

---

