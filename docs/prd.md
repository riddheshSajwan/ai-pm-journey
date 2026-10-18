# **distro.ai PRD v1.1**

_distro.ai  ·  Riddhesh Sajwan_

**PRODUCT REQUIREMENTS DOCUMENT (PRD)**

**Feature:** distro.ai v1: Sourced Research Brief and First Draft for LinkedIn Posts

**Version:** 1.1

**Date:** October 19, 2026

**Author:** Riddhesh Sajwan

**Status:** Draft

## **DOCUMENT OVERVIEW**

### **Document Purpose**

This PRD defines the requirements for distro.ai v1 to help lean B2B marketing teams publish consistent LinkedIn posts by turning a topic they have already picked into a sourced research brief and a first draft in their page's voice, which they edit, get approved and post themselves.

### **Revision History**

|**Version**|**Date**|**Author**|**Changes**|
|---|---|---|---|
|1.0|October 1, 2026|Riddhesh Sajwan|Initial draft, built from 4 user interviews, 1 watching session and a 7-day concierge test|
|1.1|October 19, 2026|Riddhesh Sajwan|Added success criteria and a focus metric with a guardrail. Targets left TBD until I have a baseline. Every pain point now carries the interview sentence it came from. User stories rewritten as jobs, not features. Technical context marked TBD for Weeks 7 to 8|

### **Document Reviewers**

- **Mentor / Reviewer:** Abhik Chowdhury (Upivot Pro instructor)

- **Engineering Lead:** Riddhesh Sajwan (solo build)

- **Design Lead:** Riddhesh Sajwan (solo build)

- **Product Manager:** Riddhesh Sajwan

- **QA / User Validation:** 5 beta marketers from the concierge test (P1 to P5)

- **Peer Reviewers:** Upivot Pro cohort

## **EXECUTIVE SUMMARY**

### **Feature Overview**

I'm building a LinkedIn content assistant for lean B2B marketing teams. The marketer sends a topic and their angle in one line; distro sends back a one-page research brief where every fact carries its source link, plus an optional first draft in the page's voice. The marketer stays in charge: they edit, get approval and post from their own account.

### **Problem Statement**

Marketers who own a company LinkedIn page say one post can take "half a day, or a whole day and night", and that AI tools have "not cut down my time to produce content". Finding relevant content, getting the facts right and waiting on approvals eat that time, so the page posts less often, or with lower quality, than it should.

### **Success Criteria**

**Focus metric:** Weekly returning marketers, meaning marketers who sent a request this week and also sent one last week. It is the one number that tells me distro has become part of their weekly routine.

**Baseline:** TBD, after Week 6. Nothing is live yet, so I will take it from the first two weeks of the AI brief beta.

**Target:** TBD. I will set it once the baseline is in, not before.

**Guardrail:** At least half the facts in a brief get ticked as used. If marketers throw most of the brief away, the research is not earning its place, however many of them come back.

**Hard line:** Zero facts shown without a source link that actually contains them.

### **Timeline**

12-week build from Week 5 to Week 16 of Upivot Pro (October to mid December 2026) across 3 phases: manual concierge, AI brief, then draft and launch.

## **BACKGROUND & CONTEXT**

### **User Problem**

**Current State:** Marketers pick the topic in a content call or from what is trending, research it by hand, draft with some help from ChatGPT, then wait for a manager, and sometimes an expert, to approve it.

**Pain Points:**

- **Research and finding content eat the time:** "Most of the time half a day, or a whole day and night, goes into creating just one post." (Naji, Interview 1) and "Maintaining the quality is the biggest thing, and finding relevant content." (Naji, Interview 1)

- **AI tools have not saved the time:** "It has helped a lot, but it has not cut down my time to produce content." (Naji, Interview 1) and "By low quality I mean slop. In today's time you'd call it AI slop." (Naji, Interview 1)

- **Getting the facts wrong is a risk:** "You have to be extra careful with the creative stuff and how you state things." (Naji, Interview 1) and "I'd do my research on the topic, go a bit in depth, take screenshots, give it context, so the AI isn't wrong about the ideas." (Interview 2)

- **Approvals and experts slow everything:** "All the content styles, the content plan, everything has to be approved." (Naji, Interview 1) and "Mostly the challenges are coordination-related." (Shoumoh, Interview 4)

**Evidence:** 4 user interviews: Naji (call), a social media marketer (call), a marketer running a B-school page (form response) and Shoumoh, ex Goldman Sachs Asset Management (call). Each pain point above quotes the sentence it came from. My watching session and concierge test are still to be run with real marketers, so I have left their numbers out of this version.

### **Business Context**

**Strategic Alignment:** Supports my thesis that distribution is the real differentiator for B2B companies: a page that posts consistently builds an audience that compounds, while paid reach stops the day the budget stops. distro starts with the smallest part of that problem that users will actually pay attention to: getting one good post out per week.

**Competitive Context:** Hootsuite and Zoho Social schedule and report on posts but leave the research and writing to the marketer. SocialKaptan-style tools automate comments and engagement, which runs against LinkedIn's User Agreement (section 8.2) and puts accounts at risk. ChatGPT drafts fast but has no sources and no page voice. Nobody hands the marketer verified facts for the post they already want to write.

**Success Metrics:** Focus metric is weekly returning marketers, with the guardrail that at least half the facts in a brief get used (see Success Criteria). Baseline and target are TBD until the beta runs. Revenue is not a v1 goal; willingness to pay gets tested after Week 12.

### **Technical Context**

**Current System:** Nothing built yet.

**Technical Constraints:** LinkedIn offers no sanctioned API to read other people's posts or the feed, and automated commenting or engagement breaks its User Agreement. Whatever I build has to work without touching LinkedIn.

**Integration Requirements:** TBD, Weeks 7 to 8. I am not in the solution yet; the build decisions go in my build spec once the problem and the metric are settled.

## **USER STORIES & ACCEPTANCE CRITERIA**

**Priority legend:** P0 = Must-have for beta, P1 = Should-have before launch, P2 = Could-have if time allows, P3 = Later.

### **Primary User Stories**

#### **User Story 1: Facts I Can Defend**

**As a** marketer who owns a company LinkedIn page **I want** to walk into my manager's approval with facts I can stand behind **so that** I stop losing half a day to research for one post.

**Acceptance Criteria:**

- Marketer can submit a topic and angle in a single text field, from the web app or by email

- Brief arrives within 3 hours in the concierge phase and within 10 minutes once the AI brief is live

- Brief has 5 to 6 facts or angles, one page, every fact with a working link

- Only sources from the last 18 months and from allowed domains are used

- Marketer can mark each fact as used or not used

**Priority:** P0 / Must-have

#### **User Story 2: Sounding Like Our Page**

**As a** marketer **I want** what we post to sound like our page and not like generic AI **so that** it earns reach instead of reading like slop.

**Acceptance Criteria:**

- Marketer can paste up to 10 past posts at signup

- Marketer can add up to 10 house rules (for example: no emojis, no em dashes, links go in the first comment)

- Voice profile can be edited any time and applies to the next request

**Priority:** P0 / Must-have

#### **User Story 3: Not Starting from a Blank Page**

**As a** marketer short on time **I want** to start writing from something instead of nothing **so that** a post gets finished the week I pick the topic.

**Acceptance Criteria:**

- Draft is off by default and switched on per request

- Draft uses only facts from the brief, under 1,300 characters, no links in the caption

- House-style checks run before the draft is sent (banned phrases, character count, no em dashes)

- Marketer can rate the draft as used as-is, partly used or not used

**Priority:** P1 / Should-have

#### **User Story 4: Knowing What Worked**

**As a** marketer **I want** to know which of my topics and posts actually worked **so that** I can plan next week's content with less guessing.

**Acceptance Criteria:**

- Recap lists the week's briefs, which ones became posts and the facts used

- Marketer can enter impressions and reactions for each post manually

- Recap is sent every Monday morning before the usual content planning call

**Priority:** P2 / Could-have

### **Edge Cases & Error Scenarios**

**Edge Case 1: Stat with no traceable source** When a fact cannot be matched on the page it links to, then it is dropped from the brief before sending, because one invented number on a company page does more damage than a thinner brief.

**Edge Case 2: Brand-new page with no past posts** When a marketer has fewer than 3 past posts, then the draft uses only their house rules and a neutral professional tone and says so, because a faked voice is worse than a plain one.

**Error Scenario 1: Not enough sources found** When search returns fewer than 3 usable sources, then the marketer gets a partial brief with a clear note instead of a padded one, and the user sees "I found only 2 solid sources for this angle. Want me to widen the date range or try a different angle?" with both options as buttons.

## **FUNCTIONAL REQUIREMENTS**

### **Core Features**

#### **Feature 1: Topic Intake**

**Description:** One-line intake for the topic and angle, from the web app or email, with the page's voice profile attached.

**Requirements:**

- Single text field for topic and angle, with an optional audience note

- Links the request to the marketer's page and voice profile

- Confirms receipt immediately and shows the expected delivery time

- Keeps a history of all requests per page

**Priority:** P0 / Must-have

#### **Feature 2: Sourced Research Brief**

**Description:** Finds, filters and summarises sources into a one-page brief where every fact carries its link. This is the core of v1.

**Requirements:**

- Searches the open web plus the company's own blogs and case studies

- Filters to the last 18 months and to allowed domains

- LLM picks the 5 to 6 most useful facts and angles for the stated angle

- Rules-based check drops any number not found on its linked page

- Delivers the brief as a clean page by email or Slack, with a link to the web view

**Priority:** P0 / Must-have

#### **Feature 3: Voice Profile**

**Description:** Stores past posts and house rules per page and applies them to drafts and style checks.

**Requirements:**

- Up to 10 past posts and 10 house rules per page

- Editable at any time

- Used as examples for the draft and as rules for the style checks

**Priority:** P0 / Must-have

#### **Feature 4: First Draft and Style Checks**

**Description:** Optional draft in the page's voice, built only from the brief, passed through house-style checks before delivery.

**Requirements:**

- Draft toggle per request, off by default

- Uses the brief and 2 past posts as examples

- Regex checks: under 1,300 characters, no links in caption, no em dashes, banned slop phrases removed

- Captures the marketer's rating of the draft

**Priority:** P1 / Should-have

#### **Feature 5: Hand-off and Weekly Recap**

**Description:** Tracks each brief from sent to posted and sends a Monday recap.

**Requirements:**

- Marketer marks a brief as posted and pastes the post link

- Manual entry of impressions and reactions

- Monday morning recap of the past week

**Priority:** P2 / Could-have

### **Business Rules**

**Rule 1: Every fact needs a source**

- **When:** A fact is added to a brief

- **Then:** It must carry a link, and the number or claim must appear on that page

- **Exception:** If the source is behind a paywall, the fact is marked "paywalled, verify before use" instead of being dropped

**Rule 2: A human always posts**

- **When:** A brief or draft is delivered

- **Then:** distro never posts, comments, likes or messages on LinkedIn on anyone's behalf

- **Exception:** None. This rule has no exceptions

**Rule 3: Draft follows the brief**

- **When:** A draft is generated

- **Then:** It may only use facts that appear in that request's brief

- **Exception:** Opinions and hooks are allowed if they carry no factual claim

### **Data Requirements**

**Data Collection:**

- **Requests:** topic, angle, timestamps, page; used to generate briefs and to measure weekly usage

- **Voice profile:** past posts and house rules pasted by the marketer; used only for their own drafts

- **Usage signals:** facts used, draft rating, posted link, manually entered impressions; used for the recap and my metrics

**Data Storage:**

- Requests and briefs kept for 12 months, then deleted

- Voice profiles kept until the marketer deletes them or closes the account

- Company content is never used to train models; LLM calls use providers with no-training settings

**Data Integration:**

- LLM and search APIs receive only the request, the voice profile and fetched sources

- Analytics tool receives event data with no post content

## **NON-FUNCTIONAL REQUIREMENTS**

### **Performance Requirements**

- **Response Time:** Brief delivered within 10 minutes of the request once automated; draft within 2 minutes after the brief

- **Throughput:** 20 marketers with up to 5 requests each per week during beta

- **Availability:** Best effort during beta, with a manual fallback where I build the brief myself if the pipeline fails

- **Scalability:** Design for 100 pages within 6 months without re-architecture

### **Security Requirements**

- **Authentication:** Email magic-link login

- **Authorization:** Each marketer sees only their own page's requests, briefs and voice profile

- **Data Protection:** Encryption at rest and in transit; no company content used for model training

- **Compliance:** India DPDP Act 2023 consent and deletion rights; no LinkedIn scraping or automation, in line with the LinkedIn User Agreement

### **Usability Requirements**

- **Accessibility:** WCAG 2.1 AA for the web app

- **Browser Support:** Latest two versions of Chrome, Safari, Edge and Firefox

- **Mobile Support:** Briefs readable on mobile email; the web app is desktop-first

- **Internationalization:** English only for v1

### **Technical Requirements**

- **Browser Compatibility:** Responsive web app, no install

- **Device Compatibility:** Desktop-first, since marketers write posts on laptops

- **Third-party Integrations:** LLM API, web search API, transactional email, Slack incoming webhook (P1)

- **Monitoring:** Pipeline error logging, LLM cost per brief, share of facts dropped by the source check

## **USER EXPERIENCE REQUIREMENTS**

### **User Interface Guidelines**

**Design System:** Plain, text-first interface. The brief is the product, so it should read like a clean one-page memo, not a dashboard.

**Key UI Elements:**

- **Request box:** One field for topic and angle, one optional note, a draft toggle and a send button

- **Brief card:** Numbered facts, each with its source link and a "used" checkbox

- **Voice setup:** Paste area for past posts and a short list of house rules

- **History:** List of past requests with status (sent, posted) and the posted link

**User Flow Requirements:**

- **First use:** Sign up → paste past posts and rules → send first topic → receive brief

- **Weekly use:** Pick topic in content call → send one line → receive brief (and draft) → write and get approval → mark as posted

### **Interaction Design**

**Input Methods:** Typing in the web app or replying to an email thread.

**Feedback Mechanisms:** Instant receipt with expected delivery time; email when the brief is ready.

**Error Prevention:** The request box asks for an angle, not just a topic, because vague topics produce vague briefs.

**Help & Documentation:** A sample brief shown during signup and one-line tips on writing a good angle.

### **Accessibility Requirements**

- **Keyboard Navigation:** Full keyboard access to request, voice setup and history

- **Screen Reader Support:** Semantic HTML and labelled source links

- **Color Contrast:** WCAG AA contrast (4.5:1) for all text

- **Font Size:** Readable at 200% zoom

## **TECHNICAL SPECIFICATIONS**

TBD, Weeks 7 to 8. APIs, data model and integrations are solution decisions, so they move to my build spec (distro_Build_Spec_v2, October 18, 2026) and stay out of this PRD.

## **ANALYTICS & MEASUREMENT**

### **Key Metrics**

**Focus Metric:**

- **Metric:** Weekly returning marketers

- **Definition:** Marketers who sent a request this week and also sent one last week

- **Baseline and Target:** TBD, after Week 6. Baseline from the first two weeks of the AI brief beta, target set from it

- **Tracking Method:** request_created events grouped by marketer and week

**Guardrail:**

- **Facts used:** At least half the facts in a brief get ticked as used, tracked with fact_marked_used events

**Secondary Metrics:**

- **Brief usefulness:** Share of posted briefs where 3 or more facts were used; target TBD after the baseline

- **Hands-on time per post:** Baseline TBD from my first real watching session; target set after that

- **Fact accuracy:** 0 unverifiable facts delivered (the hard line); tracked by the source checker and marketer flags

- **Draft keep rate:** Share of drafts rated used as-is or partly used; target TBD after the baseline, and the draft stays optional if it stays low

### **Event Tracking**

**Key Events to Track:**

- **Request Created:** request_created, page_id, include_draft, source (web or email)

- **Brief Delivered:** brief_delivered, request_id, fact_count, facts_dropped, minutes_to_deliver

- **Fact Used:** fact_marked_used, request_id, fact_id

- **Post Marked Live:** post_marked_live, request_id, facts_used, draft_rating

**Conversion Funnels:**

- **Activation:** Signup → voice set up → first request → first post marked live

- **Weekly loop:** Request → brief opened → post marked live → next week's request

### **Dashboard Requirements**

**Real-time Monitoring:**

- Briefs in queue and minutes to deliver

- Alert if a brief takes more than 30 minutes or the pipeline fails twice in a row

**Regular Reporting:**

- Weekly: returning marketers, posts marked live, facts used, LLM cost per brief

- Shared with Abhik and the cohort in the weekly Upivot Pro update

## **LAUNCH PLAN**

### **Launch Strategy**

**Rollout Approach:**

- **Phase 1 (Weeks 5 to 7, concierge):** Brief-only, built by me with a custom GPT and manual source checks, for the 5 concierge marketers. Success: TBD, after Week 6

- **Phase 2 (Weeks 8 to 12, AI brief beta):** LLM brief plus automated source check in a simple web app, 10 marketers. Success: baseline for weekly returning marketers measured in the first two weeks

- **Phase 3 (Weeks 13 to 16, draft and launch):** Draft switched on for users who want it, landing page and public launch. Success: weekly returning marketers at or above the target set from the Phase 2 baseline

**Feature Flags:**

- draft_enabled: turns the first draft on per page

- slack_delivery_enabled: sends briefs to Slack instead of email

- Rollback: switch any page back to concierge mode, where I build the brief by hand

**Launch Criteria:**

- 0 unverifiable facts delivered in the last 2 weeks of beta

- Brief delivered in under 10 minutes for 90% of requests

- At least 3 beta marketers say they would be disappointed if distro went away

### **Communication Plan**

**Internal Communication:**

- Weekly progress update in the Upivot Pro Skool thread

- Monday, Tuesday and Thursday Q&A sessions for blockers and reviews with Abhik

**External Communication:**

- Personal WhatsApp and LinkedIn messages to beta marketers, one at a time

- LinkedIn post series on building distro in public, written with distro itself

**Documentation:**

- One-page "how to write a good angle" guide

- Public GitHub repo (ai-pm-journey) with the PRD, tree, AI scope and observation logs

### **Success Measurement**

**Week 1 Goals:**

- Brief-only rerun sent to all 5 concierge marketers

- At least 3 briefs turned into live posts

**Month 1 Goals:**

- AI brief live for 10 beta marketers

- Baseline for weekly returning marketers measured

- First real watching session done, giving the time per post baseline

**Quarter 1 Goals:**

- Weekly returning marketers at or above the target, with the guardrail held

- 0 unverifiable facts delivered

- First test of willingness to pay with the 5 most active marketers

**Success Review Schedule:**

- Weekly metrics review every Sunday before the Upivot Pro submission

- Phase review at the end of Weeks 7, 12 and 16

- One watching session per phase to check time saved

## **RISKS & MITIGATION**

### **Technical Risks**

**Risk 1: The LLM invents stats or links**

- **Probability:** High

- **Impact:** High

- **Mitigation:** Automated source check that drops any number not found on its linked page; marketer can flag a bad fact in one click

- **Contingency:** Fall back to concierge mode for that page and review every brief by hand until the check is fixed

**Risk 2: Search returns thin or outdated sources for niche B2B topics**

- **Probability:** Medium

- **Impact:** Medium

- **Mitigation:** Add the company's own blogs and case studies as a source; let the marketer add 1 to 2 links of their own

- **Contingency:** Deliver a partial brief with a clear note instead of padding it

### **Product Risks**

**Risk 1: Marketers try it once and drift off**

- **Probability:** High

- **Impact:** High

- **Mitigation:** Brief-only focus, timing the brief before the Monday content call, weekly recap

- **Contingency:** Narrow to the 2 to 3 marketers who return and interview the rest on why they stopped

**Risk 2: The approval wait, not the writing, is the real bottleneck**

- **Probability:** Medium

- **Impact:** High

- **Mitigation:** Measure start-to-live time as well as hands-on time in every watching session

- **Contingency:** Explore a manager-ready version of the brief (facts plus sources) that speeds up approval

### **Business Risks**

**Risk 1: Wrong segment; enterprise brand teams do not feel this pain**

- **Probability:** Medium

- **Impact:** High

- **Mitigation:** Target lean teams with 1 to 2 people on LinkedIn, as Shoumoh's interview suggests

- **Contingency:** Leave enterprise brand teams out of v1 entirely

**Risk 2: LinkedIn policy changes or users expect automation**

- **Probability:** Low

- **Impact:** High

- **Mitigation:** distro never touches LinkedIn directly; the human always posts

- **Contingency:** Explain the no-automation choice upfront as a trust feature, not a limitation

## **OUT OF SCOPE**

### **Features Not Included**

- **Auto-posting to LinkedIn:** Not included; the marketer always posts. Revisit only through LinkedIn's official posting API and only if users ask

- **Automated commenting, liking or DMs:** Not included and not planned; it breaks LinkedIn's User Agreement and puts accounts at risk

- **Topic suggestions:** Not included; marketers already know what they want to post about

- **Best time to post and format prediction:** Needs months of performance data I do not have; considered after Quarter 1

### **Future Considerations**

- **Automatic performance recap:** Pull own-post analytics once gated API access is approved

- **Manager approval view:** A shareable brief and draft link a manager can approve or comment on

- **Other channels:** Newsletter and X versions of the same brief

### **Platform Limitations**

- **LinkedIn:** No sanctioned access to the feed or other users' posts; own-post analytics gated. v1 relies on manual input

- **Mobile:** No native app in v1; briefs are readable in mobile email

## **FAQs**

### **External FAQs (Customer Facing)**

**How do I use distro?**

Send your topic and the point you want to make in one line. You get a one-page brief with sourced facts, and a draft if you switch it on.

**How will it help me?**

It removes the research hunt, which is the slowest part of writing a post, and every number comes with a link you can show your manager.

**Will distro post or comment on LinkedIn for me?**

No. You always post from your own account. That keeps your page within LinkedIn's rules.

**What if the draft does not sound like us?**

The draft is optional. Add past posts and house rules to improve it, or switch it off and use only the brief.

**What happens if a fact is wrong?**

Every fact links to its source and is checked against that page before you see it. Flag a bad fact and it is removed.

### **Internal FAQs**

**What is this product for?**

Helping lean B2B marketing teams publish one well-sourced LinkedIn post a week without losing half a day to research.

**Who will benefit from this product?**

Content marketers and founders running a company or personal page with little or no writing support.

**Why am I well placed to solve this?**

I build pitch and proposal content for B2B clients every week, so I know the research-then-write loop, and I have already interviewed 4 marketers about it.

**How far ahead or behind are we from competitors?**

Scheduling tools (Hootsuite, Zoho Social) are far ahead on publishing and reporting, which I am not building. On sourced research for a specific post, nobody I found does it.

**Do we have the resources to build this product?**

Yes for v1: it is a solo build on off-the-shelf LLM, search and email services. The main cost is my time and LLM usage per brief, which I track weekly.

## **APPENDICES**

### **Appendix A: User Research**

- **User interviews:** 4 (Naji, content marketer; a social media marketer; a marketer running a B-school page, by Google Form; Shoumoh, ex Goldman Sachs Asset Management). Naji and the social media marketer talked most about time per post; Shoumoh's pain was coordination and data, not drafting

- **Anchor quote:** "Most of the time half a day, or a whole day and night, goes into creating just one post." (Naji, Interview 1)

- **Watching session:** Still to be run with a real marketer. I will add the numbers once it is done

- **Concierge test:** Still to be run with real marketers. I will add the results once it is done

### **Appendix B: Competitive Analysis**

|**Tool**|**What it does well**|**Gap for this problem**|**Risk**|
|---|---|---|---|
|Hootsuite|Scheduling, multi-channel publishing, reporting|No research or sourced facts; AI captions are generic|Low|
|Zoho Social|Scheduling and analytics at a lower price|Same gap: writing and research left to the marketer|Low|
|SocialKaptan|Automated engagement and commenting|Automation conflicts with LinkedIn's User Agreement|High|
|ChatGPT|Fast drafts on any topic|No sources, no page voice, output reads like slop|Medium|

### **Appendix C: Technical Architecture**

- **Pipeline:** Intake → voice profile load → source search and filter (rules) → brief selection (LLM) → source check (rules) → optional draft (LLM) → style checks (regex) → delivery

- **AI split:** 2 of 9 steps use an LLM; the other 7 are plumbing and rules, detailed in docs/ai-scope.md

- **Human-in-the-loop:** Mode B, AI drafts and the human approves; nothing goes out without the marketer

### **Appendix D: Design Mockups**

- Wireframes for request box, brief card, voice setup and history to be added by Week 7

- Sample brief from the concierge test used as the reference layout

### **Appendix E: Legal/Compliance Review**

- **LinkedIn User Agreement, section 8.2:** No bots, scraping or automated engagement; distro stays outside LinkedIn

- **India DPDP Act 2023:** Consent at signup, deletion on request, clear notice on what is stored

- **Source content:** Facts are summarised with links, not copied, to respect publisher copyright

## **PRD Development: RACI Matrix**

_distro.ai v1: Sourced Research Brief and First Draft for LinkedIn Posts_

## **RACI Legend**

- **R** = Responsible (does the work)

- **A** = Accountable (ultimately answerable, decision maker)

- **C** = Consulted (provides input, two-way communication)

- **I** = Informed (kept updated, one-way communication)

distro is a solo build during Upivot Pro, so I wear most of the hats. I have kept the roles separate so the matrix shows which hat does the work, and named the outside people who challenge, test and review it. Support, DevOps and Business Lead from the standard template are folded into PM, ENG and MENTOR.

## **Team Roles & Responsibilities**

|**Role**|**Team Member**|**Primary Responsibilities**|
|---|---|---|
|**PM**|Riddhesh Sajwan|Overall PRD ownership, scope, priorities, user research|
|**ENG**|Riddhesh Sajwan (builder hat)|Architecture, pipeline build, LLM and search integration, hosting|
|**UX**|Riddhesh Sajwan (design hat)|Brief layout, request flow, voice setup, wireframes|
|**QA**|Riddhesh Sajwan with beta marketers|Acceptance criteria checks, source-check testing, bug reports|
|**DATA**|Riddhesh Sajwan (analytics hat)|Metric definitions, event tracking, weekly dashboard|
|**LEGAL**|Riddhesh Sajwan (policy hat)|LinkedIn User Agreement fit, DPDP consent and deletion, source copyright|
|**MKT**|Riddhesh Sajwan (growth hat)|Beta recruiting, launch posts, landing page|
|**MENTOR**|Abhik Chowdhury (Upivot Pro instructor)|Scope challenge, review gates, final sign-off on the plan|
|**BETA**|5 to 10 beta marketers (P1 to P5 first)|Real usage, feedback, validation of briefs and drafts|
|**PEERS**|Upivot Pro cohort|Peer review of PRD sections and launch plan|

## **RACI Matrix by PRD Sections**

|**PRD Section**|**PM**|**ENG**|**UX**|**QA**|**DATA**|**LEGAL**|**MKT**|**MENTOR**|**BETA**|**PEERS**|
|---|---|---|---|---|---|---|---|---|---|---|
|**DOCUMENT OVERVIEW**|||||||||||
|Document Purpose & Structure|A/R|I|I|I|I|I|I|C|I|C|
|Revision History Management|A/R|I|I|I|I|I|I|I|I|I|
|Reviewer Coordination|A/R|I|I|C|I|I|I|C|C|C|
|**EXECUTIVE SUMMARY**|||||||||||
|Feature Overview|A/R|C|C|I|I|I|C|C|I|C|
|Problem Statement|A/R|I|C|I|C|I|I|C|C|I|
|Success Criteria Definition|A/R|C|I|C|R|I|I|C|I|C|
|Timeline Coordination|A/R|R|C|I|I|I|C|C|I|I|
|**BACKGROUND & CONTEXT**|||||||||||
|User Problem Research|A/R|I|C|I|C|I|I|C|R|I|
|Business Context|A/R|I|I|I|C|C|R|C|I|C|
|Technical Context|A|R|I|I|I|C|I|I|I|I|
|**USER STORIES & ACCEPTANCE CRITERIA**|||||||||||
|User Stories Creation|A/R|C|R|C|I|I|I|C|C|I|
|Acceptance Criteria|A/R|C|C|R|I|I|I|I|C|I|
|Edge Cases & Error Scenarios|A|C|C|R|I|C|I|I|C|I|
|**FUNCTIONAL REQUIREMENTS**|||||||||||
|Core Features Definition|A/R|R|C|C|I|C|I|C|C|I|
|Business Rules|A/R|C|I|C|I|R|I|C|I|I|
|Data Requirements|A|R|I|I|R|R|I|I|I|I|
|**NON-FUNCTIONAL REQUIREMENTS**|||||||||||
|Performance Requirements|A|R|C|C|I|I|I|I|C|I|
|Security Requirements|A|R|I|C|I|R|I|I|I|I|
|Usability Requirements|A|C|R|C|I|I|I|I|C|I|
|Technical Requirements|A|R|C|C|I|I|I|I|I|I|
|**USER EXPERIENCE REQUIREMENTS**|||||||||||
|UI Guidelines|A|C|R|C|I|I|I|I|C|C|
|Interaction Design|A|C|R|C|I|I|I|I|C|I|
|Accessibility Requirements|A|C|R|C|I|C|I|I|I|I|
|**TECHNICAL SPECIFICATIONS**|||||||||||
|API Requirements|A|R|I|C|C|I|I|I|I|I|
|Database Requirements|A|R|I|I|C|C|I|I|I|I|
|Integration Requirements|A|R|I|C|C|C|I|I|I|I|
|**ANALYTICS & MEASUREMENT**|||||||||||
|Key Metrics Definition|A/R|C|I|I|R|I|C|C|I|C|
|Event Tracking Specifications|A|R|I|C|R|C|I|I|I|I|
|Dashboard Requirements|A|C|C|I|R|I|I|I|I|I|
|**LAUNCH PLAN**|||||||||||
|Launch Strategy|A/R|C|C|I|C|I|R|C|C|C|
|Communication Plan|A|I|I|I|I|C|R|I|C|C|
|Success Measurement|A/R|C|I|C|R|I|C|C|I|I|
|**RISKS & MITIGATION**|||||||||||
|Technical Risks|A|R|I|C|I|C|I|C|I|I|
|Product Risks|A/R|C|C|C|C|I|C|C|C|C|
|Business Risks|A/R|I|I|I|C|C|R|C|I|C|
|**OUT OF SCOPE**|||||||||||
|Scope Decisions|A/R|C|C|I|I|C|I|C|I|C|
|Future Considerations|A/R|C|C|I|I|I|C|C|C|I|
|Platform Limitations|A|R|I|I|I|R|I|I|I|I|
|**APPENDICES**|||||||||||
|User Research Documentation|A/R|I|C|I|C|I|I|C|C|I|
|Competitive Analysis|A/R|I|I|I|I|C|R|C|I|C|
|Technical Architecture|A|R|I|I|I|C|I|I|I|I|
|Design Mockups|A|C|R|C|I|I|I|I|C|C|
|Legal/Compliance Review|A|C|I|I|I|R|I|C|I|I|

## **Key Decision Points & Approvals**

|**Decision Point**|**Primary Decision Maker**|**Required Approvals**|**Escalation Path**|
|---|---|---|---|
|**Feature Scope & Priority**|PM (A)|MENTOR, ENG|PM → MENTOR|
|**Technical Architecture (LLM and search choice)**|ENG (A)|PM|ENG → PM → MENTOR|
|**LinkedIn Policy & Data Compliance**|LEGAL (A)|PM|LEGAL → PM → MENTOR|
|**Timeline & Weekly Scope**|PM (A)|MENTOR|PM → MENTOR|
|**Beta and Launch Readiness**|PM (A)|QA, BETA, MENTOR|PM → MENTOR|
|**Risk Acceptance (for example, turning the draft on)**|PM (A)|BETA, MENTOR|PM → MENTOR|

## **Communication & Review Cycles**

### **Weekly Sync During PRD Development**

|**Meeting**|**Attendees**|**Purpose**|**Duration**|
|---|---|---|---|
|**Self review (Sunday)**|PM(A), ENG(R), UX(R), DATA(R)|Check the week's build against the PRD and metrics before submission|60 min|
|**Upivot Pro Q&A (Mon, Tue or Thu, 9 to 10:30 pm IST)**|PM(A), MENTOR(C), PEERS(C)|Scope challenge, blockers, review of PRD sections|30 min|
|**Beta check-ins**|PM(A), BETA(R)|What they used, what they skipped, why they stopped|15 min per marketer|

### **Formal Review Gates**

|**Gate**|**Reviewers**|**Criteria**|**Timeline**|
|---|---|---|---|
|**Requirements Complete**|PM(A), BETA(C)|All sections filled, brief-only rerun results added|October 11, 2026 (Week 6)|
|**Technical Review**|ENG(A), LEGAL(A), PM(A)|LLM and search choice made, source check tested, no LinkedIn automation|October 18, 2026 (Week 7)|
|**Mentor Review**|MENTOR(A), PM(A)|Scope and metrics challenged and agreed|October 20, 2026 (Week 8)|
|**Final Sign-off**|PM(A), MENTOR(C)|PRD frozen for the AI brief beta|October 25, 2026 (Week 8)|

## **Conflict Resolution Matrix**

|**Conflict Type**|**Primary Resolver**|**Escalation**|
|---|---|---|
|**Speed of build vs quality of the brief**|PM decides, using beta feedback|→ MENTOR if it changes scope|
|**Draft feature vs brief-only focus**|PM decides on draft keep rate data|→ MENTOR if beta results are mixed|
|**Scope vs 12-week timeline**|PM cuts scope before moving dates|→ MENTOR at the next Q&A|
|**User requests for automation vs LinkedIn rules**|LEGAL rule wins: no automation|No escalation; this is fixed|

## **Success Metrics for PRD Process**

|**Metric**|**Target**|**Owner**|
|---|---|---|
|**PRD Completion Timeline**|Final sign-off by October 25, 2026|PM|
|**Review Cycle Efficiency**|Fewer than 3 major revision rounds|PM|
|**Stakeholder Alignment**|Mentor sign-off at the Mentor Review gate|PM|
|**Development Handoff Quality**|Fewer than 5 open questions when the build starts|PM|
|**Scope Creep Control**|Less than 10% feature change during the beta|PM|

## **Working Backwards**

**ETA to deliver:** October 25, 2026, before the AI brief beta starts in Week 8.

|**Date**|**Milestone**|**Owner**|
|---|---|---|
|October 25, 2026|Final doc readiness (cut-off, no change after this)|Riddhesh|
|October 22, 2026|Final doc prep, circulate to Abhik and cohort for offline review|Riddhesh|
|October 20, 2026|Final review and inputs|Riddhesh and Abhik|
|October 15, 2026|Input from beta marketers (brief-only rerun and first AI briefs)|Riddhesh and beta marketers|
|October 8, 2026|Input from mentor and cohort at the Upivot Pro Q&A|Abhik and cohort|
|October 1, 2026|Kick-off|Riddhesh|
