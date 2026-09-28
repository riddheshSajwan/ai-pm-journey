# **Opportunity Solution Tree**

_distro.ai  ·  built from my 4 user interviews (3 calls + 1 form response)_

**Outcome ->** 5 users visiting the platform at least once a week.

### **Opportunities ->**

**1.** _“Half a day or a whole day goes into creating just one post, because of the research and then writing it up crisply”_ (Naji) **- pain**

**2.** _“AI helps me summarise, but it hasn't cut my time much, because the output still reads like slop and I have to fix that”_ (Naji, Speaker 2) **- pain**

**3.** _“A relevant comment takes real effort to analyse the post and write, and a generic one hurts more than it helps”_ (Naji) **- pain**

**4.** _“Every post has to be approved before it goes out, and that slows everything down”_ (Naji, Speaker 2) **- pain**

**5.** _“I want to catch a trend and turn it into a ready-to-post piece fast, before the moment passes”_ (Speaker 2) **- desire**

**6.** _“I depend on expert and design teams to give me content, and coordinating that is most of the delay”_ (Shoumoh, Naji) **- pain**

**7.** _“I want to know which posts worked and why, so I can do more of that”_ (Naji, Shoumoh, Form) **- desire**

**8.** _“LinkedIn doesn't give me enough data back about my audience, and the packages that do get expensive fast”_ (Shoumoh, Form) **- pain**

**9.** _“I want to find and reach the right people to engage with, without hunting through LinkedIn manually”_ (Form, Speaker 2) **- desire**

### **I scored each opportunity (1-3 on each, highest total wins)**

|**Opp**|**How**<br>**many**|**How often**|**How**<br>**much**|**Can reach**|**score**|
|---|---|---|---|---|---|
|**1. Drafting takes half a day**|**3**|**3**|**3**|**3**|**12**|
|**2. AI output is slop**|**3**|**3**|**2**|**3**|**11**|
|**3. Relevant commenting is hard**|**3**|**3**|**2**|**3**|**11**|
|4. Approval slows posting|2|2|2|2|**8**|
|5. Turn a trend into a post fast|2|3|2|3|**10**|
|6. Coordination with other teams|2|2|2|2|**8**|
|7. Can't tell what worked / why|3|2|2|3|**10**|
|8. Not enough data from LinkedIn|2|2|2|2|**8**|
|9. Finding the right people to engage|2|2|2|2|**8**|

_These scores are my own judgement from the interviews, not measured numbers. Four interviews give me direction, not proof._

## **My top 3 opportunities**

1. Drafting a good post takes half a day to a day (score 12)

2. AI output reads like slop and still needs fixing (score 11)

3. Making a relevant comment on others' posts is hard work (score 11)

## **The one I'm picking: Opportunity 1, drafting takes too long**

It scored highest, three of my four interviews raised it, and unlike commenting it doesn't depend on reading other people's LinkedIn feeds, so I don't have a platform-rules problem to solve first. It's the cleanest thing for me to build and test in 10 weeks.

## **My solutions for Opportunity 1**

**1.** A WhatsApp or Slack bot I send a topic to, and it sends back a research brief plus a draft I approve by replying. No new app to log into, it lives where I already work.

**2.** A research agent that takes the topic I've already chosen and returns a one-page brief of the key facts, stats and recent angles, so the only thing left for me to do is write. This removes the research half, which is the part my interviewees said eats the time.

**3.** A writing copilot that autocompletes the next line in my own voice as I type, trained on my past posts, the way GitHub Copilot works for code. I stay in control, so it never comes out as slop.

**4.** A prompt pack plus a custom GPT preloaded with my company's voice and five post formats. Nothing to build, I paste in the topic and get a draft. I can set this up for 5 users this weekend.

_Solution 4 looks too basic to be a product, but it's the version I can test on Monday, and it tells me whether people even want assisted drafting before I build an agent._

## **The riskiest assumption I need to test first**

The riskiest thing I'm assuming is desirability: that marketers will adopt assisted drafting and keep coming back, instead of trying it once and dropping off. My cheapest test is to run the prompt-pack version (solution 4) with 5 marketers for two weeks and count how many draft a second and third post with it. If they don't come back, no agent will save it.

## **One thing I'm keeping in mind**

Three of my four interviewees felt this drafting pain (Naji, Speaker 2, the form respondent). Shoumoh, an enterprise brand marketer at Goldman, didn't, his team writes manually by choice and his real pain was coordination with other teams and weak data back from LinkedIn. So this opportunity is strong for lean and individual marketers, but not for well-resourced enterprise brand teams. I need to decide which segment I'm building distro for before I lock this in.
