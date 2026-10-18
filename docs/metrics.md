# **Measurement Plan**

_distro.ai  ·  Riddhesh Sajwan  ·  Week 6_

### **Focus metric: weekly returning marketers**

**A marketer who sent at least one request this week and at least one request last week.**

distro only works if it becomes part of a marketer's weekly posting routine. One good brief that never gets a second request tells me nothing, so this is the one number I watch.

|**Item**|**Detail**|
|---|---|
|How I count it|Take every request_created event, group by user_id and by week (Monday to Sunday, IST), and count the users who appear in this week and in the week before|
|Baseline|TBD, after Week 6. Taken from the first two weeks of the AI brief beta|
|Target|TBD. I set it once the baseline is in, not before|
|Where the data lives|events table in Supabase|

### **Guardrail: facts used**

**At least half the facts shown in a brief get ticked as used.**

If marketers keep coming back but throw most of the brief away, the research is not earning its place and the focus metric is flattering me.

|**Item**|**Detail**|
|---|---|
|How I count it|For every request that reached ready in the week, take its facts with verified = true (the only ones a marketer sees) and work out the share with used = true|
|Why the column and not the event|Unticking a fact sets used back to false without logging anything, so counting fact_marked_used events alone would overcount. I read the used column at the end of the week|
|Line I hold|50% or more, every week of the beta|

### **Events**

Seven events. Each one is a row in the events table with user_id, name, properties and created_at.

|**Event**|**When it fires**|**Properties**|**How I count it**|
|---|---|---|---|
|request_created|Marketer presses "Get brief"|request_id, page_id, include_draft, has_angle|Requests per marketer per week. This is what the focus metric is built from|
|brief_ready|The request's status changes to ready|request_id, verified_facts, dropped_facts, seconds_to_ready|Share of requests that turn into a brief, median seconds to ready, and dropped facts out of all facts found, which shows how often the model gets a fact wrong|
|brief_failed|The request's status changes to failed|request_id, error_message|Failed requests out of all requests each week, grouped by error_message|
|fact_marked_used|Marketer ticks Used on a fact|request_id, fact_id|Share of ready briefs with at least one tick. The guardrail itself comes from the used column|
|voice_saved|Marketer saves the Voice screen|page_id, post_count, rule_count, first_time|Share of new marketers who save a voice before their first request, and how many have 3 or more past posts for the draft to learn from|
|draft_copied|Marketer presses Copy on the draft|request_id, characters|Share of requests with the draft switched on where the draft got copied|
|request_posted|Marketer marks the request as posted and adds the LinkedIn link|request_id, facts_used, hours_ready_to_posted|Posted requests out of ready briefs each week: how many briefs become real posts|

### **Hypothesis**

**I believe that if every fact in a brief comes with a source the marketer can open and check, marketers will use the brief for real posts and come back the next week, because finding content and getting the facts right is where their time goes today.**

Where it comes from: "Most of the time half a day, or a whole day and night, goes into creating just one post." (Naji, Interview 1)

|**Item**|**Detail**|
|---|---|
|I am right if|In the first two weeks of the AI brief beta, more than half the marketers who send a request in week 1 send another in week 2, and the guardrail holds|
|I am wrong if|Marketers tick fewer than half the facts, or most of them do not come back for a second week|
|What I do if I am wrong|Talk to the marketers who stopped before I build anything more|
