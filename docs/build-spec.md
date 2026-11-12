**UPIVOT PRO · BUILD PHASE · DISTRO.AI**

# **distro: From Spec to First Prompt**

_My nine-section build spec, and the six prompts I sent to Claude Code in VS Code to build v1_

|**PRODUCT SHAPE**|**STACK**|**VERSION**|
|---|---|---|
|Web app, desktop first, readable on a phone|All code in VS Code (written with Claude Code): React + Vite + Tailwind frontend, Supabase for database, auth and edge functions, Claude API|v4.0, November 12, 2026|

> **THE ONE RULE**
> I do not ask Claude Code to write any code until all nine sections below are written. Everything I do not write down, the model will decide for me, differently each time.

> **WHAT CHANGED FROM V3 (Prompts 4 and 5 built)**
> distro now takes a topic, researches it, checks every source and writes a draft in the page's voice, end to end. The code is on the develop branch, one commit per prompt.
> 1. Prompt 4 and Prompt 5 added below, exactly as I sent them. Prompt 4 builds every screen and event with no AI, so I could test it by filling in facts and a draft by hand.
> 2. Prompt 5 is split into two parts: research and the source check first, then the draft. That let me check every source link before adding the draft on top.
> 3. Three edge functions, not two: generate-brief, generate-draft, and mark-failed for the 5 minute timeout.
> 4. With the draft on, generate-brief calls generate-draft on the server over HTTP, so research and drafting each get their own time limit. Whichever function sets the request to ready logs brief_ready, exactly once per run.
> 5. Timeouts: the Brief screen notices when 5 minutes have passed, but the server decides and records the failure. The browser never marks a failure itself.

> **WHAT CHANGED FROM V2 (Prompt 0 review with Claude Code)**
> Claude Code read Prompt 0 and came back with 28 open questions, then 7 more conflicts in my answers. Everything we settled now lives in CLAUDE.md in the distro repo. The ones that change this spec:
> 1. Two new columns: facts.source_quote (the exact sentence the source check looks for) and requests.run_started_at (so timeouts count from the latest run, not from the first one).
> 2. No users table of our own. Every user_id points at Supabase auth.users.
> 3. Research asks for 8 candidate facts so 5 to 6 survive the source check. At most 6 are marked verified. If none pass, the request fails.
> 4. If the facts work but the draft fails, the request stays ready with its facts and a "Write draft again" button.
> 5. Tighter row level security: users can only tick facts as used, never insert them or set verified. brief_ready and brief_failed are logged only by the edge functions.
> 6. Prompt 2 and Prompt 3 below are the versions I actually sent. Prompt 3 now also builds the Login screen, because the list cannot be seen without signing in.

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
users     no table of our own. Every user_id references auth.users(id)
          (free from Supabase auth)

pages     id, user_id, page_name, voice_posts, house_rules, created_at
          page_name is the company LinkedIn page, required
          voice_posts is a text array, up to 10 past posts
          house_rules is a text array, up to 10 short rules
          one page per user in v1

requests  id, user_id, page_id, topic, angle, include_draft, status,
          draft_text, final_text, posted_url,
          prompt_version, error_message,
          created_at, run_started_at, ready_at, posted_at
          angle is optional
          run_started_at resets on every Try again and Write draft again
          status is one of: queued, ready, failed, posted

facts     id, request_id, user_id, fact_text, source_quote, source_url,
          source_title, published_on, verified, used, created_at
          source_quote is the exact sentence from the page, from the model
          verified is set by the source check, never by the model
          only facts with verified = true are ever shown, at most 6

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

_Events I log: request_created, brief_ready, brief_failed, fact_marked_used, voice_saved, draft_copied, request_posted. The browser logs five of them; brief_ready and brief_failed are logged only by the edge functions. Status values are lowercase, exactly as written. Row level security is on for every table, and each user can reach only their own rows. api_audit_log is for cost and debugging, and is what the later token limit (#6) reads from._

### **#6 The AI layer**

> **DISTRO**
>
> **SCOPE**  AI does two jobs. One: research the topic and angle into 8 candidate facts, so 5 to 6 survive the source check, each with the page it came from. Two: write a first draft in the page's voice using only those facts. Everything else is a request form, a rules-based source check and a history list.
>
> **MODE**  AI drafts, the marketer reads, edits and approves. distro never posts, comments or logs in to LinkedIn.
>
> **WHERE**  three Supabase edge functions, generate-brief, generate-draft and mark-failed, written in VS Code and deployed with the Supabase CLI. The model key is a Supabase secret (ANTHROPIC_API_KEY) and never touches the browser. The browser calls generate-brief, which replies straight away and keeps working in the background, then calls generate-draft on the server over HTTP when the draft is on. With the draft on, generate-draft is the one that marks the request ready. mark-failed records a timeout when the Brief screen reports one.
>
> **SOURCE CHECK**  plain code, not AI. The function opens each source link (10 second timeout) and keeps a fact only if its source_quote is on that page and every number in the fact is in the quote. The date on the page wins over the model's date, and anything older than 18 months is dropped. Paywalls, pages that need JavaScript and timeouts all count as not verified. Only the first 6 passing facts are marked verified = true. Everything else is saved with verified = false and never shown.
>
> **WHEN THINGS FAIL**  no facts pass: the request fails with "No facts could be checked against their sources. Try a different angle." 1 to 4 pass: the brief shows them with a note. The draft fails: the request stays ready with its facts and a "Write draft again" button. Still queued 5 minutes after run_started_at: the edge function marks it failed with "This took too long. Try again." Try again reuses the same request and re-runs everything.
>
> **MODEL**  Claude API, one key and one spend cap. Both functions use Claude Haiku (claude-haiku-5-5) to keep cost per request low.
> - generate-brief (research): Haiku with the web search tool, capped at 5 searches per request. The source check still drops any fact its link does not support, so a cheaper model cannot put an unsourced fact on screen. If too many facts fail the check, this is the first thing to switch to a larger model.
> - generate-draft (drafting): Haiku. The draft call sends only the verified facts, the voice posts and the house rules, and caps the reply with max_tokens.
>
> **PROMPT**  two prompts, versioned brief_v1 and draft_v1, with the model name in one shared file both functions import. The version is saved on every request as "brief_v1+draft_v1", or "brief_v1" when the draft is off.
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

_Sent to Claude Code inside VS Code, one at a time. Check the preview at localhost and commit with Git between each. Prompts 0 to 3 get one screen live on a URL. Prompts 4 and 5 finish v1. These are the versions I actually sent; after each commit I start a fresh chat with /clear, and CLAUDE.md in the repo carries everything we decided._

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

**What happened:** the summary matched. Claude Code flagged 28 open questions (who calls the draft, what "supports a fact" means in code, what happens when the draft fails, and more). I answered them in one message, it found 7 conflicts in my answers, and one more round settled it. Then I asked it to write every decision into CLAUDE.md in the repo, so nothing is lost when I clear the chat. The changes to this spec are listed at the top under "What changed from v2".

### **Prompt 1 · Design system**

**Type:** Scaffold.  _Still no features._

```
First, create the React + Vite + Tailwind project in this folder, load the
Inter font, and run git init. Then:

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
Prompt 2 · Data model. Read CLAUDE.md first. No screens yet.

Set up the Supabase CLI in this folder (as a dev dependency, run with npx), link it to my Supabase project, and write a migration that creates exactly these tables, with exactly these column names. Do not add columns beyond the ones listed. Do not rename anything.

  users     no table of our own. Every user_id references auth.users(id).

  pages     id, user_id, page_name, voice_posts, house_rules, created_at
            voice_posts is a text array, up to 10 past posts
            house_rules is a text array, up to 10 short rules
            one page per user in v1 (unique user_id)

  requests  id, user_id, page_id, topic, angle, include_draft, status,
            draft_text, final_text, posted_url,
            prompt_version, error_message,
            created_at, run_started_at, ready_at, posted_at
            status is one of: queued, ready, failed, posted
            angle is optional

  facts     id, request_id, user_id, fact_text, source_quote, source_url,
            source_title, published_on, verified, used, created_at
            verified is set by the source check, never by the model
            only facts with verified = true are ever shown

  events    id, user_id, name, properties, created_at
            name is one of: request_created, brief_ready, brief_failed,
            fact_marked_used, voice_saved, draft_copied, request_posted

  api_audit_log
            id, user_id, request_id, function_name, model,
            prompt_version, input_tokens, output_tokens,
            web_searches, status, error_message, latency_ms, created_at
            function_name is one of: generate-brief, generate-draft
            status is one of: success, error

Then:
  - My project has "Automatically expose new tables" switched off, so the migration must grant table privileges explicitly: grant the authenticated role only the operations the policies below allow, and grant the anon role nothing.
  - Turn on Row Level Security on every table.
  - pages and requests: a user can read, insert and update only rows where user_id is their own auth id.
  - facts: a user can read their own facts and update only the used column. Inserting and deleting facts is done only by the edge functions, so a user can never set verified themselves.
  - events: a user can insert only their own rows and only these names: request_created, fact_marked_used, voice_saved, draft_copied, request_posted. brief_ready and brief_failed are written only by the edge functions.
  - api_audit_log: no user policies at all. Only the edge functions write to it, using the service role key.
  - Set up email magic link login, with http://localhost:5173 allowed as a redirect URL. If any of this has to be done in the Supabase dashboard rather than in code, tell me the exact clicks.
  - Put the project URL and anon key in .env.local (never the service role key), and add a .env.example without real values.
  - Push the migration to my Supabase project.

Done when: I can create two accounts, add a request as account A, add a fact for that request in the Supabase table editor (users cannot insert facts), and confirm account B cannot see either, and neither account can read api_audit_log. Give me a way to run this test myself, explain each step in plain words, and do not just tell me it passed.

Commit with the message "Prompt 2: data model" only after I confirm the test.
```

**Check before moving on:** do the two-account test myself. Do not take its word for it. Account A adds a request; I add a fact for it in the Supabase table editor; account B sees neither, and neither account can read api_audit_log. I type the database password into my own Terminal, never into the chat.

### **Prompt 3 · First screen, read only**

**Type:** Feature.  _The first thing a human can look at._

```
Prompt 3 · First screen, read only. Read CLAUDE.md first.

Build the Login screen and the Requests screen as a read-only list. Login is needed now so I can see my own requests; build it exactly as described in CLAUDE.md (email field, "Send magic link", the "Check your email" state, invalid email and send failed states).

LOGIN
  After the magic link, go to Requests. Skip the "first login goes to Voice" rule for now, because the Voice screen does not exist yet. Leave a clear TODO where that redirect will go.

HEADER
  On every screen except Login: the distro name on the left (goes to Requests), then Voice and Sign out on the right. Sign out works. Voice is a link that does nothing yet.

REQUESTS (read only)
CONTEXT      The screen a marketer lands on after login.
GOAL         Show every request belonging to the logged-in user.
CONSTRAINTS  Use the existing Card, List row, Status badge, Empty state and PageContainer components. Existing tokens only. Mobile first.
DATA         Read from requests, with a count of its facts where verified = true.
             Show: topic, angle (one line, cut with an ellipsis, nothing if empty), status badge, facts count, created date.
             The facts count shows a dash while the request is queued or failed.
             Sort newest first. Tapping a row does nothing yet.
STATES       Empty: "Send your first topic and distro will come back with a sourced brief."
             Loading: a skeleton, not a blank screen.
             Error: a plain message and a retry.

Do not build the request box, the Brief screen or the Voice screen yet.

Also add what Vercel needs to serve this as a single page app (a vercel.json rewrite so refreshing any URL works), and tell me exactly which environment variables I must add in Vercel.

Done when: a new account sees the empty state, and an account with three requests I added by hand in the Supabase table editor sees them newest first with the right status badges and fact counts. Tell me which rows to add for the test.

Commit with the message "Prompt 3: login and read-only requests" only after I confirm it works.
```

**Check before moving on:** commit and push to GitHub. Import the repo in Vercel, add the Supabase URL and anon key as environment variables, and add the Vercel URL to Supabase under Authentication > URL Configuration > Redirect URLs. Then sign in on my actual phone, on mobile data. This is the "one screen live" homework.

### **Prompt 4 · Write path and events**

**Type:** Feature.  _Every screen works, no AI yet._

```
Prompt 4 · Write path and events. Read CLAUDE.md first. No AI yet.

Build the rest of the screens so a marketer can do everything except the AI call. Follow CLAUDE.md exactly for every screen, state and event.

VOICE
  The full Voice screen: page name, up to 10 past posts, up to 10 house rules, Save voice.
  Saving upserts the user's one pages row, logs voice_saved (with first_save true or false) and goes to Requests.
  Turn on the redirect: until the user has a pages row, every screen except Login goes to Voice. Remove the TODO from Prompt 3.

REQUESTS
  Add the request box on top: Topic, Angle, Include draft toggle, Get brief.
  On Get brief: insert a requests row (status queued, run_started_at now, page_id of the user's page), log request_created, and open that request's Brief screen.
  Do not call generate-brief yet. Leave one clearly named function, startBrief(requestId), that does nothing for now; Prompt 5 fills it in.
  Tapping a row in the list now opens its Brief screen.

BRIEF
  Build every state in CLAUDE.md: queued, failed, ready (facts and draft), posted.
  Queued polls every 3 seconds and stops when the status changes.
  Used ticks save facts.used and log fact_marked_used when ticked.
  The draft autosaves to final_text 1 second after typing stops, with a small "Saved".
  Copy copies the text, saves final_text, logs draft_copied and shows "Copied".
  Mark as posted: inline LinkedIn URL field, saves posted_url, posted_at and status posted, logs request_posted.
  Show the "Try again" and "Write draft again" buttons in their states, but leave them calling empty functions (retryBrief, retryDraft) for Prompt 5. Skip the 5 minute timeout for now; it needs the edge function from Prompt 5.

EVENTS
  The browser logs only its five events, with the properties listed in CLAUDE.md. Put event logging in one helper so every screen uses the same code.

Use only the existing components and tokens. Mobile first.

Done when: with a new account I can save a voice, send a topic, see the queued screen, then fill in facts and a draft by hand in the Supabase table editor, set the request to ready, and watch the Brief screen update by itself. I can tick facts, edit and copy the draft, mark it as posted, and see every one of these in the events table. Tell me exactly which rows and values to edit for this test.

Commit with the message "Prompt 4: write path and events" only after I confirm it works.
```

**Check before moving on:** play the AI by hand. Fill in facts and a draft in the Supabase table editor, set the request to ready, and watch the Brief screen update by itself. Then tick, edit, copy, mark as posted, and check every action in the events table.

### **Prompt 5 · Part A · Research and the source check**

**Type:** Feature.  _The first real AI call._

**Before sending:** an Anthropic Console account with a few dollars of credit and a monthly spend limit, and an API key I type into my own Terminal, never into the chat.

```
Prompt 5 · Part A · Research and the source check. Read CLAUDE.md first.

Build the generate-brief edge function exactly as described in CLAUDE.md (Pipeline and Source check), deployed with the Supabase CLI.

  - The model name (claude-haiku-5-5) and both prompt versions live in one shared file that every function imports.
  - The Claude API key is read from a Supabase secret called ANTHROPIC_API_KEY. Tell me the command to set it and let me run it myself in Terminal. The key never goes in a file or in this chat.
  - generate-brief checks the user's login and that they own the request, replies straight away, and keeps working in the background.
  - Research: Haiku with web search, max_uses 5, asks for 8 candidate facts, each with fact_text, source_url, source_title, source_quote and published_on if known.
  - Source check in plain code, exactly as in CLAUDE.md. Save every fact; mark only the first 6 passing ones verified = true. Give each fact an increasing created_at in the model's order, so the Brief screen shows them in that order.
  - Write one api_audit_log row per Claude call, success or error.
  - When done: set ready (only if still queued), set ready_at, log brief_ready. With 0 verified facts: set failed with the message in CLAUDE.md and log brief_failed.
  - For now, ignore include_draft. Part B adds the draft.
  - Also build the mark-failed endpoint for the 5 minute timeout, and wire up the timeout and Try again on the Brief screen.
  - Fill in startBrief and retryBrief from Prompt 4.

Done when: I send three real topics and each comes back in under two minutes; I open every source link and the fact is really on that page; api_audit_log has one row per call with tokens filled in; and a topic with no good sources ends as failed with the right message. Show me how to read the function logs if something goes wrong.

Commit with the message "Prompt 5a: research and source check" only after I confirm it works.
```

**Check before moving on:** open every source link myself and confirm the fact is really on that page. This is the hard line from #3.

### **Prompt 5 · Part B · The draft**

**Type:** Feature.  _v1 is done when this passes._

```
Prompt 5 · Part B · The draft. Read CLAUDE.md first.

Build the generate-draft edge function exactly as described in CLAUDE.md.

  - Haiku, with only the verified facts, the page name, the voice posts and the house rules. Cap the reply with max_tokens.
  - generate-brief calls it on the server when include_draft is on, before the request becomes ready.
  - The browser calls it for "Write draft again", with the login and ownership checks, the queued versus ready rules, and the 5 minute rewrite timeout from CLAUDE.md.
  - On failure: the request stays ready, error_message is "The draft could not be written."
  - One api_audit_log row per call. prompt_version is "brief_v1+draft_v1", or "brief_v1" when the draft is off.
  - Fill in retryDraft from Prompt 4.

Done when: my spec's #9 passes end to end. I sign in with a fresh account, paste 3 past posts and 2 house rules, send a topic and angle with the draft on, and get 5 to 6 facts and a draft in under two minutes, where every source link contains its fact and the draft sounds like the posts I pasted. I tick facts, edit, copy, mark as posted, and see it in my history. Then I do the same on my phone, on mobile data, on the Vercel URL.

Commit with the message "Prompt 5b: draft" only after I confirm it works.
```

**What happened:** before building, Claude Code flagged two choices: how the 5 minute rewrite timeout gets triggered, and how generate-brief calls generate-draft. I agreed with both readings and added one line: "Make sure brief_ready is logged exactly once per run, by whichever function sets the request to ready, with seconds_to_ready measured from run_started_at."

**Check before moving on:** the full #9 test, run myself with real past posts from a company page, then again on my phone, on mobile data, on the Vercel URL.
