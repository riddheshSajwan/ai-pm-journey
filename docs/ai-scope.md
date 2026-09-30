# **AI Scope Statement**

_distro.ai  ·  Riddhesh Sajwan_

### **One-line scope**

**distro takes a LinkedIn topic a marketer has already picked and, within minutes, hands back a sourced research brief and a first draft in their page's voice, which the marketer edits, gets approved and posts themselves.**

### **Human-in-the-loop mode: B (AI drafts, human approves)**

Nothing goes out without the marketer reading it, and distro never posts to LinkedIn on anyone's behalf.

### **Component table**

|**#**|**Step**|**What happens**|**Type**|
|---|---|---|---|
|1|Take the topic|Marketer types the topic and their angle in one line (web form or Slack)|**Plumbing**|
|2|Load the page's voice|10 past posts pasted at signup, plus a few do and don't rules for the page|**Plumbing**|
|3|Find sources|Web search plus the company's own blogs and case studies. Keep only the last 18 months and allowed domains|**Plumbing + rules**|
|4|Build the research brief|Pick the 5 to 6 most useful facts and angles, one page, every fact with its link|**LLM**|
|5|Check every fact|If the number isn't on the linked page, drop it|**Rules**|
|6|Write the first draft|Draft in the page's voice using the brief and 2 past posts as examples|**LLM**|
|7|House-style checks|No links in the caption, under 1,300 characters, cut a list of slop phrases|**Rules + regex**|
|8|Hand it over|Send brief and draft to the marketer, capture edits, mark approved and posted|**Plumbing**|
|9|Weekly recap|What they posted this week and how it did (entered manually in v1)|**Plumbing**|
