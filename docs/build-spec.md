**UPIVOT PRO · BUILD PHASE · DISTRO.AI**

# **distro: From Spec to First Prompt**

_My nine-section build spec, and my first four prompts for Claude Code in VS Code_

|**PRODUCT SHAPE**|**STACK**|**VERSION**|
|---|---|---|
|Web app, desktop first, readable on a phone|All code in VS Code (written with Claude Code): React + Vite + Tailwind frontend, Supabase for database, auth and edge functions, Claude API|v2.0, October 18, 2026|

> **THE ONE RULE**
> I do not ask Claude Code to write any code until all nine sections below are written. Everything I do not write down, the model will decide for me, differently each time.

> **WHAT CHANGED FROM V1 (mentor review)**
> 1. Prompt 0 now describes every screen in detail, not just the four names.
> 2. Both AI jobs (research and drafting) use Claude Haiku to keep token cost down. Research keeps the web search tool.
> 3. Per-user token limit added to the backend as a later-phase item (section #6 and #8).
> 4. New api_audit_log table in the data model: one row per model call.
> 5. was_edited removed from the requests table.
> 6. Lovable removed. All code lives in VS Code, written with Claude Code; Supabase is the database.

## **1 · The spec**

_Nine sections. Sources: my AI Scope statement, PRD v1.0 and the Observation Log._

### **#1 One line**

> **DISTRO**
> distro takes a LinkedIn topic a B2B marketer has already picked and, within two minutes, hands back a one-page brief of sourced facts and a first draft in their page's voice, which they edit, get approved and post themselves.

### **#2 The user and the job**

> **DISTRO**
>
> **USER**  a marketer at a lean B2B company (one to three people in marketing) who owns the company LinkedIn page, posts a few times a week, and needs a manager to approve every post. Today the research happens across a pile of browser tabs and the draft in Google Docs or ChatGPT.
>
> **JOB**  When I have picked this week's topic, I want the facts, the sources and a first draft that sounds like our page, so I can get a post approved without losing half a day to research.

### **#3 The outcome and the focus metric**

> **DISTRO**
>
> **OUTCOME**  5 marketers sending at least one topic to distro every week, 4 weeks running, by Week 16.
>
> **FOCUS METRIC**  weekly returning marketers: marketers who sent a request this week and also sent one last week.
>
> **GUARDRAIL**  at least half the facts in a brief get ticked as used. If marketers throw most of the brief away, the research is not earning its place.
>
> **HARD LINE**  zero facts shown without a source link that actually contains them. One invented number on a company page costs more than a thin brief.

### **#4 Screens (four, maximum)**

|**SCREEN**|**WHAT IT DOES**|**WHY IT EXISTS**|
|---|---|---|
|Login|Email magic link|Needed for per-page data|
|Requests|A request box on top (topic, angle, draft on or off) and every past request below it with its status|The home screen. Where they land and where every brief starts|
|Brief|One request: the facts with source links, a Used tick on each, the draft in an editable box, Copy and Mark as posted|Where the AI call happens and where the work gets done|
|Voice|Paste up to 10 past posts and up to 10 house rules|Without it the draft sounds like generic AI. Shown once after first login, then linked from the header|

_Four screens. No dashboard, no weekly recap, no settings, no team page. Those go in #8. Prompt 0 describes each screen in full._

### **#5 Data model**

```
users     id, email, created_at                  (free from Supabase auth)

pages     id, user_id, page_name, voice_posts, house_rules, created_at
          voice_posts is a text array, up to 10 past posts
          house_rules is a text array, up to 10 short rules
          one page per user in v1

requests  id, user_id, page_id, topic, angle, include_draft, status,
          draft_text, final_text, posted_url,
          prompt_version, error_message,
          created_at, ready_at, posted_at
          status is one of: queued, ready, failed, posted

facts     id, request_id, user_id, fact_text, source_url,
          source_title, published_on, verified, used, created_at
          verified is set by the source check, never by the model
          only facts with verified = true are ever shown

events    id, user_id, name, properties, created_at   (Week 6 metrics)

api_audit_log
          id, user_id, request_id, function_name, model,
          prompt_version, input_tokens, output_tokens,
          web_searches, status, error_message, latency_ms, created_at
          one row for every Claude API call, written by the edge
          function after the call returns (success or error)
          function_name is one of: generate-brief, generate-draft
          status is one of: success, error
          written only by edge functions; marketers cannot read or
          write it
```

_Events I log: request_created, brief_ready, brief_failed, fact_marked_used, voice_saved, draft_copied, request_posted. Status values are lowercase, exactly as written. api_audit_log is for cost and debugging, and is what the later token limit (#6) reads from._

### **#6 The AI layer**

> **DISTRO**
>
> **SCOPE**  AI does two jobs. One: research the topic and angle into 5 to 6 facts, each with the page it came from. Two: write a first draft in the page's voice using only those facts. Everything else is a request form, a rules-based source check and a history list.
>
> **MODE**  AI drafts, the marketer reads, edits and approves. distro never posts, comments or logs in to LinkedIn.
>
> **WHERE**  two Supabase edge functions, generate-brief and generate-draft, written in VS Code and deployed with the Supabase CLI. The model key is a Supabase secret and never touches the browser.
>
> **SOURCE CHECK**  plain code, not AI. The function opens each source link and keeps a fact only if its number or key phrase is on that page and the page, where it shows a date, is from the last 18 months. Everything else is saved with verified = false and never shown.
>
> **MODEL**  Claude API, one key and one spend cap. Both functions use Claude Haiku (claude-haiku-5-5) to keep cost per request low.
> - generate-brief (research): Haiku with the web search tool. The source check still drops any fact its link does not support, so a cheaper model cannot put an unsourced fact on screen. If too many facts fail the check, this is the first thing to switch to a larger model.
> - generate-draft (drafting): Haiku. The draft call sends only the verified facts, the voice posts and the house rules, and caps the reply with max_tokens.
>
> **PROMPT**  two prompts in one server file, versioned brief_v1 and draft_v1. The version is saved on every request.
>
> **AUDIT**  every model call writes one row to api_audit_log: who, which function, which model, tokens in and out, web searches, success or error, and how long it took.

> **LATER PHASE (not in beta scope): PER-USER TOKEN LIMIT**
> Before each model call, the edge function adds up input_tokens + output_tokens in api_audit_log for that user in the current month. If the total is over the limit, it skips the call, sets the request to failed with error_message "Monthly limit reached", and the Brief screen shows that message. The limit number is a Supabase secret, so I can change it without a code change. Build this after the beta core works; api_audit_log is built now so the data is already there.

### **#7 Design tokens**

```
COLOURS  background  #FFFFFF     surface     #F6F7F9
         border      #E3E6EB     text        #14171F
         text-muted  #5F6673     accent      #2B4C7E
         danger      #B42318     success     #1F7A4D

TYPE     Inter.  32 / 20 / 16 / 13.  Regular and semibold.
         Brief text at 16 with 1.6 line height. It is read like a memo.

SPACING  Base 8.  Use 8, 16, 24, 32, 48.  Nothing else.

RADIUS   8px everywhere.  No shadows. Cards get a 1px border in border.

WIDTH    Content column max 720px. The brief is a page, not a dashboard.
```

_This section is also my design-system.md for /docs in the repo._

### **#8 Out of scope**

> **DISTRO: NOT IN V1**
> - No posting, scheduling, commenting or LinkedIn login. distro never touches LinkedIn. This is a product decision, not a v1 limitation.
> - No topic suggestions. The marketer brings the topic; distro does the research.
> - No weekly recap, analytics or impressions entry. That is P2 and comes after the beta.
> - No team accounts and no approval flow inside distro. One marketer, one page. Approval stays wherever it happens today.
> - No email or Slack delivery. The brief lives in the app.
> - No images, carousels, polls or video. Text posts only.
> - No payments or plans. Free during the beta.
> - No settings page. Voice is the only thing a marketer sets up.
> - No mobile app. Responsive web only.
> - No per-user token limit in the beta. It is a later-phase item (see #6); api_audit_log is built now so it can be added without a data change.

### **#9 Done when**

> **DISTRO: V1 IS DONE WHEN**
> I can sign in, paste 3 past posts and 2 house rules, send a topic and angle with the draft switched on, and get a brief of 5 to 6 facts back in under two minutes where every source link opens to a page that contains its fact. I can tick the facts I use, edit the draft, copy it, mark the request as posted with the LinkedIn URL, and see it in my history. And the brief reads cleanly on my phone, on mobile data.

## **2 · My prompts, in order**

_Sent to Claude Code inside VS Code, one at a time. Check the preview at localhost and commit with Git between each. Prompts 0 to 3 get one screen live on a URL._

### **Prompt 0 · Foundation**

**Type:** Foundation.  _Run once. Builds nothing._

```
I am building a web app with React, Vite, Tailwind and Supabase in this
empty folder. All of the code lives here: the screens, the database
migrations and the edge functions. Before we build anything, here is the
full brief. Read it and confirm you have understood it. Do not write any
code yet.

PRODUCT    distro. It takes a LinkedIn topic a B2B marketer has already
           picked and returns a one-page brief of sourced facts plus a
           first draft in their page's voice. The marketer edits it, gets
           it approved and posts it themselves.
USER       A marketer at a lean B2B company who owns the company LinkedIn
           page and needs manager approval on every post.
JOB        When I have picked this week's topic, get me the facts, the
           sources and a first draft that sounds like our page, without
           losing half a day to research.

SCREENS    Login, Requests, Brief, Voice. Only these four.
           Every screen except Login has the same header: the distro name
           on the left (goes to Requests), then a Voice link and a
           Sign out link on the right.

  1. LOGIN
     Purpose   Get the marketer in with no password.
     Shows     The distro name, one line on what it does, an email field
               and a "Send magic link" button.
     Does      On send, Supabase emails a magic link. The screen swaps to
               "Check your email for a sign-in link" with the address
               shown and a "Use a different email" link.
     States    Invalid email: inline error under the field.
               Send failed: a plain error message and the button stays
               active to try again.
     Goes to   First ever login (no row in pages yet): Voice.
               Every login after that: Requests.

  2. REQUESTS (home screen)
     Purpose   Start a new brief and see every past one.
     Top       The request box, in a Card:
                 - Topic (required, one line)
                 - Angle (one line: the point of view the post should take)
                 - Include draft toggle (on by default)
                 - "Get brief" button, disabled until Topic is filled
     Does      On send: create a requests row with status queued, log
               request_created, call generate-brief (and generate-draft
               after it if the toggle is on), and open that request's
               Brief screen.
     Below     Every request belonging to this user, newest first. Each
               row shows: topic, angle (one line, cut with an ellipsis),
               status badge, count of verified facts, created date.
               Tapping a row opens its Brief screen.
     States    Empty: "Send your first topic and distro will come back
               with a sourced brief."
               Loading: a skeleton, not a blank screen.
               Error: a plain message and a retry.

  3. BRIEF (one request)
     Purpose   Read the facts, use the draft, record what happened.
     Top       Topic as the title, angle under it, status badge, created
               date, and a "Back to requests" link.
     Queued    "Researching your topic. This usually takes under two
               minutes." The screen checks for updates on its own; the
               marketer does not need to refresh.
     Failed    The error_message in plain words and a "Try again" button
               that re-runs the request.
     Ready     FACTS: 5 to 6 facts, only those with verified = true. Each
               shows the fact text, the source title as a link that opens
               in a new tab, the published date if known, and a Used tick.
               Ticking saves used = true and logs fact_marked_used;
               unticking saves used = false.
               DRAFT (only if include_draft is on): the draft in an
               editable Textarea at 16px with 1.6 line height, starting
               from draft_text. Edits save to final_text.
               "Copy" button: copies the current text, saves it to
               final_text, logs draft_copied and briefly shows "Copied".
               "Mark as posted" button: asks for the LinkedIn post URL,
               then saves posted_url, posted_at, sets status to posted and
               logs request_posted.
     Posted    Same as Ready, plus a "Posted" badge and the LinkedIn link.
               The draft is read-only.

  4. VOICE
     Purpose   Teach distro how this company page sounds.
     Shows     Page name (one line).
               Past posts: up to 10 large text boxes, one post each, with
               "Add another post" until there are 10, and a remove link
               on each.
               House rules: up to 10 short one-line inputs (for example
               "No emojis", "Always say customers, never users"), with
               "Add a rule" and remove links.
               A "Save voice" button.
     Does      Saves to the user's one pages row (creates it the first
               time), logs voice_saved, then goes to Requests.
     When      Shown once straight after first login, then reached from
               the Voice link in the header. Existing posts and rules load
               into the boxes for editing.
     States    Nothing pasted yet: a short line explaining that 3 or more
               past posts make the draft sound like the page.
               Save failed: a plain error and nothing is lost from the
               boxes.

DATA
  users     id, email, created_at                  (free from Supabase auth)

  pages     id, user_id, page_name, voice_posts, house_rules, created_at
            voice_posts is a text array, up to 10 past posts
            house_rules is a text array, up to 10 short rules
            one page per user in v1

  requests  id, user_id, page_id, topic, angle, include_draft, status,
            draft_text, final_text, posted_url,
            prompt_version, error_message,
            created_at, ready_at, posted_at
            status is one of: queued, ready, failed, posted

  facts     id, request_id, user_id, fact_text, source_url,
            source_title, published_on, verified, used, created_at
            verified is set by the source check, never by the model
            only facts with verified = true are ever shown

  events    id, user_id, name, properties, created_at   (Week 6 metrics)

  api_audit_log
            id, user_id, request_id, function_name, model,
            prompt_version, input_tokens, output_tokens,
            web_searches, status, error_message, latency_ms, created_at
            one row per Claude API call, written only by edge functions

AI         Two jobs only: research the topic into 5 to 6 sourced facts,
           and draft the post from those facts in the page's voice.
           Both jobs use Claude Haiku to keep token cost low; research
           also uses the web search tool.
           A plain-code source check drops any fact its link does not
           support. The human approves and posts. Model calls run server
           side in Supabase edge functions that live in this folder and
           are deployed with the Supabase CLI. The key never goes in the
           browser. Every model call writes a row to api_audit_log.
NOT IN V1
  - No posting, scheduling, commenting or LinkedIn login. distro never touches LinkedIn. This is a product decision, not a v1 limitation.
  - No topic suggestions. The marketer brings the topic; distro does the research.
  - No weekly recap, analytics or impressions entry. That is P2 and comes after the beta.
  - No team accounts and no approval flow inside distro. One marketer, one page. Approval stays wherever it happens today.
  - No email or Slack delivery. The brief lives in the app.
  - No images, carousels, polls or video. Text posts only.
  - No payments or plans. Free during the beta.
  - No settings page. Voice is the only thing a marketer sets up.
  - No mobile app. Responsive web only.
  - No per-user token limit yet. It comes in a later phase and will
    read from api_audit_log.

Reply with a one paragraph summary of what we are building and a list of
anything in this brief that is ambiguous. Do not build.
```

**Check before moving on:** does the summary match what I meant, and did it flag anything I missed? Fix the brief, not the code.

### **Prompt 1 · Design system**

**Type:** Scaffold.  _Still no features._

```
Set up the design system only. No features, no pages, no data.

Create a tokens file with exactly these values:
  COLOURS  background  #FFFFFF     surface     #F6F7F9
           border      #E3E6EB     text        #14171F
           text-muted  #5F6673     accent      #2B4C7E
           danger      #B42318     success     #1F7A4D

  TYPE     Inter.  32 / 20 / 16 / 13.  Regular and semibold.
           Brief text at 16 with 1.6 line height. It is read like a memo.

  SPACING  Base 8.  Use 8, 16, 24, 32, 48.  Nothing else.

  RADIUS   8px everywhere.  No shadows. Cards get a 1px border in border.

  WIDTH    Content column max 720px. The brief is a page, not a dashboard.

Then build these base components, using only those tokens:
  Button (primary, secondary, danger)
  Input (with label and error state)
  Textarea (with label, error state and a character counter)
  Toggle (with label)
  Card
  List row
  Status badge: queued uses text-muted, ready uses accent,
                failed uses danger, posted uses success
  Empty state

Rules for the rest of this project:
  - Never introduce a colour, size or radius that is not a token.
  - Never create a second version of a component that already exists.
  - Mobile first. Every component must work at 375px wide.
  - Page content is never wider than 720px.

Done when: a page showing one of each component renders correctly at
375px and at 1440px.
```

**Check before moving on:** search the code for a hex colour outside the tokens file. If there is one, send it back now.

### **Prompt 2 · Data model**

**Type:** Scaffold.  _Still no screens._

```
Set up the Supabase CLI in this folder, link it to my Supabase project,
and write a migration that creates exactly these tables, with exactly
these column names. Do not add columns. Do not rename anything.

  users     id, email, created_at                  (free from Supabase auth)

  pages     id, user_id, page_name, voice_posts, house_rules, created_at
            voice_posts is a text array, up to 10 past posts
            house_rules is a text array, up to 10 short rules
            one page per user in v1

  requests  id, user_id, page_id, topic, angle, include_draft, status,
            draft_text, final_text, posted_url,
            prompt_version, error_message,
            created_at, ready_at, posted_at
            status is one of: queued, ready, failed, posted

  facts     id, request_id, user_id, fact_text, source_url,
            source_title, published_on, verified, used, created_at
            verified is set by the source check, never by the model
            only facts with verified = true are ever shown

  events    id, user_id, name, properties, created_at   (Week 6 metrics)

  api_audit_log
            id, user_id, request_id, function_name, model,
            prompt_version, input_tokens, output_tokens,
            web_searches, status, error_message, latency_ms, created_at
            function_name is one of: generate-brief, generate-draft
            status is one of: success, error

Then:
  - status on requests only accepts: queued, ready, failed, posted.
  - Turn on Row Level Security on every table that has a user_id.
  - The policy is: a user can read and write only rows where user_id
    matches their own auth id.
  - Exception: api_audit_log gets no user policies at all. Only the
    edge functions write to it, using the service role key.
  - Add email magic link login.
  - Push the migration to my Supabase project.

Done when: I can create two accounts, add a request and a fact as
account A, and confirm account B cannot see either, and neither account
can read api_audit_log.
```

**Check before moving on:** do the two-account test myself. Do not take its word for it.

### **Prompt 3 · First screen, read only**

**Type:** Feature.  _The first thing a human can look at._

```
Build the Requests screen only, as a read-only list.

CONTEXT      The screen a marketer lands on after login.
GOAL         Show every request belonging to the logged-in user.
CONSTRAINTS  Use the existing Card, List row, Status badge and Empty
             state components. Existing tokens only. Mobile first.
DATA         Read from requests, with a count of its facts where
             verified = true.
             Show: topic, angle (one line, cut with an ellipsis), status
             badge, facts count, created date.
             Sort newest first. Tapping a row does nothing yet.
STATES       Empty: "Send your first topic and distro will come back
             with a sourced brief."
             Loading: a skeleton, not a blank screen.
             Error: a plain message and a retry.

Do not build the request box, the Brief screen or the Voice screen yet.

Done when: a new account sees the empty state, and an account with three
requests I added by hand in Supabase sees them newest first with the
right status badges.
```

**Check before moving on:** open it on my actual phone, deploy it so I have a URL I can send (Vercel, connected to my GitHub repo), then commit. This is the "one screen live" homework.
