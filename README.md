<!--
  README.md -- hubspot-build-and-integrations-assessment-process
  Author:  Jibril Sulaiman
  Date:    2026-10-19
  What:    A read-only, phase-by-phase process for a HubSpot admin who inherits a portal
           and every integration attached to it: what to ask the team, what to inventory,
           which artifacts to generate (process maps, an entity relationship diagram, a
           property diagnosis), how to test each integration and each business scenario,
           how to score the build and turn findings into a roadmap.
  Why:     A portal built by several people over a few months keeps its history in chat
           threads and in people's heads. The next admin needs one place that says what
           exists, what is fragile, and how to check it without breaking anything.
-->

# HubSpot build and integrations assessment process

A step-by-step process for assessing a HubSpot portal you did not build, together with every system wired into it: payments, phone and SMS, ad platforms, a community platform, the legacy CRM you are migrating off, and the scripts that run outside HubSpot. One person runs it, the HubSpot admin, and it also produces the answers a CTO asks on day one. It is read-only until the last phase: nothing is created, edited, enabled, disabled or rotated until a reviewed build doc says so.

Built by [Jibril Sulaiman](https://github.com/carljibrilsulaimanii).

## Contents

- [Why it exists](#why-it-exists)
- [How it works](#how-it-works)
- [What's in this repo](#whats-in-this-repo)
- [Prerequisites and dependencies](#prerequisites-and-dependencies)
- [The process, step by step](#the-process-step-by-step)
  - [Step 1. Ground rules, access and reading](#step-1-ground-rules-access-and-reading)
  - [Step 2. Ask the team before you open the portal](#step-2-ask-the-team-before-you-open-the-portal)
  - [Step 3. Inventory](#step-3-inventory)
  - [Step 4. Generate the three artifacts](#step-4-generate-the-three-artifacts)
  - [Step 5. Data integrity](#step-5-data-integrity)
  - [Step 6. Integration health, one at a time](#step-6-integration-health-one-at-a-time)
  - [Step 7. Scenario walkthroughs](#step-7-scenario-walkthroughs)
  - [Step 8. The measurement chain](#step-8-the-measurement-chain)
  - [Step 8b. Tracking and attribution QA](#step-8b-tracking-and-attribution-qa)
  - [Step 8c. Funnel page QA](#step-8c-funnel-page-qa)
  - [Step 9. Security, access and resilience](#step-9-security-access-and-resilience)
  - [Step 10. Operations and process](#step-10-operations-and-process)
  - [Step 11. Score it, register the risks, write the roadmap](#step-11-score-it-register-the-risks-write-the-roadmap)
- [Reference: property diagnosis verdicts](#reference-property-diagnosis-verdicts)
- [Reference: HubSpot traps that cost time once already](#reference-hubspot-traps-that-cost-time-once-already)
- [Troubleshooting](#troubleshooting)
- [Limits](#limits)
- [Security](#security)
- [Related repos](#related-repos)

## Why it exists

HubSpot has no page that answers "what is in this portal, who built it, what depends on what, and what would break if I touched it". The pieces exist (users, objects, properties, workflows, connected apps, service keys, secrets, the audit log) but each lives on its own screen, and several of the facts that matter are not on any screen:

1. **Which workflow uses which secret.** The secrets vault lists names; the service keys page lists scopes; nothing joins them. A key shared by several coded actions can be rotated for one of them and silently break the others with a 401 that names neither the key nor the workflows.
2. **Which objects have a real unique key.** A custom object with two writers and no enforced key fills with duplicates that look like growth.
3. **Which numbers two people can pull and get the same answer.** HubSpot's search API filters dates in UTC and its reporting engine in the portal timezone; a report-style count can include the whole next day; two tools give two "September" totals and both look right.
4. **What runs outside HubSpot.** Apps Scripts, ad-platform scripts, a legacy CRM's workflows and a contact-center platform's automations all write into the portal. None of them appear in the workflow list.
5. **What the integrations do when they fail.** Most of them fail silently: a payment sync stops and the mirror object simply stops growing; a token expires and conversion uploads stop; nobody is paged.

A new admin who starts by clicking through the portal anchors on what it looks like from the inside. This process starts with the people who use it, then the inventory, then the artifacts that make the inventory readable, then the tests, then the score.

## How it works

Eleven steps, worked in order. Each has a goal, says exactly where to look, lists what is already known so you verify instead of rediscover, and ends with an output for the assessment workbook.

| Step | Question it answers | Output |
|---|---|---|
| 1. Ground rules and access | Can I see everything, and what are the rules? | Access table, reading list done, blank workbook |
| 2. Ask the team | What hurts, and what would a CTO ask? | Answer sheet, themes, CTO question table |
| 3. Inventory | What exists, and who owns each item? | One workbook tab per area |
| 4. Artifacts | Can a new person read it in an hour? | Process maps, ERD, property diagnosis |
| 5. Data integrity | Which numbers can be trusted? | Trusted-numbers sheet |
| 6. Integration health | Does each integration run today, and how would we know it stopped? | Pass or fail per integration, failure signals |
| 7. Scenarios | Does each business event leave the right trail? | Pass, partial or fail per scenario |
| 8. Measurement chain | Does a number survive from ad click to purchase? | Chain diagram with capture rates |
| 9. Security and resilience | What would one leaked key, one departure or one deprecation take down? | Risk register |
| 10. Operations | Can the daily work survive a handoff? | RACI, manual-step list |
| 11. Score and roadmap | What gets fixed first? | Scorecard, risk register, 30/60/90 plan |

Two lenses apply. The admin's own lens leads Steps 3 to 8 and 10: what exists, is it trustworthy, does it run, can it be handed over. The CTO's lens leads Steps 1, 2, 9 and 11: what are the rules, what would a CTO ask, what would one failure take down, what gets fixed first. Step 2 holds the CTO's questions; the admin owns producing their answers.

**If you have five days, not five weeks.** Day 1: the rules, access, and send the survey. Day 2: the integrations pages walk, the keys and secrets map, the workflow list with on/off and errors. Day 3: the trusted-numbers sheet for the five numbers leadership quotes, and the counting rules. Day 4: the tracking QA (Step 8b) on one funnel end to end, and the three scenarios that touch money (self-serve purchase, refund, payment sync stops). Day 5: the risk register and the first thirty-day list. Everything else waits.

**Questions this process answers, and where.** Which seats can we cut (3b). What payment types do we take and where does each land (3g). What is our whole tech stack (3m). What is connected to HubSpot and who owns it (3n, 3o). What does a workflow actually write (3j). Which number is right when two disagree (5a, 5f, 5f2). Is attribution working (8b). Are the pages right (8c). What happens when X occurs (7). What breaks if one person leaves or one key is rotated (9). Who does what every week (10). What do we fix first (11).

## What's in this repo

| Path | What it is |
|---|---|
| [`README.md`](README.md) | The process (this file) |
| [`templates/`](templates/) | The survey and CTO question sets, the scenario table, and the column headers for every workbook tab |

There is no code. The process is run in HubSpot, the connected platforms, a spreadsheet, and (optionally) Claude Code with its connectors.

## Prerequisites and dependencies

The whole assessment can be done by hand from the HubSpot UI, the platform dashboards and a spreadsheet. Claude Code with connectors makes Steps 3 to 8 several times faster on a large portal; both paths are listed.

### Accounts, plans and tiers

| What | Version or plan | Why | Check it's ready |
|---|---|---|---|
| HubSpot Super Admin on the portal | Any tier; custom objects need Enterprise, workflows need Professional, custom code needs Data Hub (Operations Hub) Professional | Every settings page, the audit log, service keys, secrets, Data Sync | **Settings** opens every section without "Contact your admin" |
| Admin on the phone and SMS platform (the one wired into HubSpot) | Any | Lines, inboxes, agents, sequences, user-to-owner mapping | You can open the lines list and the HubSpot sync settings there |
| Read access to the payment processor (for example Stripe) | Dashboard read, Developers, Webhooks | Payments, Payment Links, webhook destinations, Data Sync connection | You can open **Developers → Webhooks** |
| Access to each ad platform's Business Manager or account | Any | Pixel or dataset, conversion actions, Event Setup Tool rules | You can open Events Manager (Meta) and the conversions page (Google Ads) |
| Admin on the community platform, if one feeds HubSpot | Host or admin | Its one webhook URL, its API, member data | You can open the integrations or webhooks page |
| Access to both sub-accounts of the legacy CRM, if one is still running | Any | Workflows, contacts, payments still flowing | The workflow list opens in each sub-account |
| Google Workspace (Drive, Sheets, Apps Script) | Any | Scripts that write into HubSpot, data sheets, shared mailboxes | You can open the Apps Script projects' **Executions** page |
| The task tracker and the chat tool the team uses | Any | Where work is tracked and where decisions were made | You can search the channels named in Step 2 |

### Permissions and scopes

| What | Why | Check it's ready |
|---|---|---|
| A read-only HubSpot service key or private app with `crm.objects.*.read`, `crm.schemas.*.read` and `automation` (read) | Date-windowed counts through the search API; every workflow's full definition as JSON through `GET /automation/v4/flows/{flowId}` (Step 3j), which is how field mappings are audited | One search call returns a `total`; one flow read returns its `actions` array |
| Permission to open **Settings → Integrations → Service keys** and each key's **View Logs** | The only way to see which integration uses which key | The logs page opens for one key |
| Permission to read the **Audit Log** | Who changed what; the only change history HubSpot keeps outside workflow revisions | The log opens and filters by user |

### Tools with versions

| What | Version | Why | Check it's ready |
|---|---|---|---|
| A spreadsheet (Google Sheets or Excel) | Any | The assessment workbook: one tab per area, column headers in [`templates/workbook-templates.md`](templates/workbook-templates.md) | A blank workbook with the tabs created |
| A modern browser | Current Chrome, Edge, Firefox or Safari | Every platform UI | |
| A way to fetch a page with a throwaway query string (browser address bar, curl or PowerShell `Invoke-WebRequest`) | Any | Checking live pages behind a CDN cache | A fetch with `?v=check1` returns the page |
| Optional: Node.js or Python | Any current | Only if you script counts; not required | |

### Claude Code and its integrations (optional)

| Dependency | What it was used for | What it can't do | Check it's ready |
|---|---|---|---|
| **Claude Code** (desktop app or CLI) | Runs the inventory pulls, the counts, the artifact drafts and the doc the team comments on | Nothing on its own; it needs the connectors below | It opens in the folder holding the workbook exports |
| **HubSpot connector** for Claude | Every standard and custom object (read and write), properties, segments, saved reports, marketing email analytics, pages, conversations search, owners, schema discovery | Native payments and payment links are not readable; workflow definitions are not readable; report-style counts can include the whole next day | Ask it to list the portal's object types with an empty filter. Custom objects must appear; the other discovery calls hide them |
| **Claude in Chrome**, in a Chrome profile already signed in to HubSpot and the other platforms | Everything the APIs do not expose: workflow creators and revision history, the audit log, form definitions, service key View Logs, Data Sync settings, page publishing on non-primary domains, the legacy CRM's workflow builder, the contact-center admin UI and its AI agent settings, the payment processor's dashboard | It must be told to read only; it must never click Save, Publish or Update during an assessment | With a workflow open, ask it to read the workflow's name and trigger |
| **Claude Docs** | The assessment doc the team comments on, and every build doc that follows | | It can create a doc you can open |
| **Google Drive connector** | Data sheets written by ad-platform scripts, the class or event calendar, plan documents | Its summarised read returns only the first rows of a sheet; download the sheet as a file instead | It lists a sheet you name |
| **Gmail and Google Calendar connectors** | Vendor and agency threads, support tickets, the meeting cadence | Only the signed-in mailbox; shared inboxes need their own connection | It finds a thread you name |
| **Slack connector** | The decision threads behind the designs | Files uploaded by outside parties are often not retrievable | It finds a thread you name |
| **Task tracker connector** (Asana or similar) | Where work is tracked and who owns what | | It lists the migration project |
| **Meeting notes connector** (Granola or similar) | Transcripts of the syncs where designs were agreed | | It returns one transcript |
| **Ad platform connectors** (Meta Ads) | Dataset stats, pixel Event Setup Tool rules, custom conversions | Some ad accounts are not enabled for the ads tools; dataset tools still work. Google Ads has no connector without a developer token from your own manager account | A dataset stats call returns rows |
| **Legacy CRM connector** (GoHighLevel or similar) | Contacts, conversations, opportunities, payments for each sub-account | Workflow steps live only in the builder; the API returns names and status | It returns the sub-account it is bound to |
| **Automation connector** (Zapier) | The connections and zaps still live | | It lists the connections |
| **Whiteboard connector** (Miro), optional | The process maps and the ERD on a board, if the team wants them there | | It lists a board |

Two habits matter when Claude does the reading: count with the search API, not report-style queries, when you need an exact date-windowed number; and read the live workflow before writing a claim, because notes and older copies drift from what is running.

✅ **Check:** you can open one workflow, one service key's View Logs, the audit log, and (if using Claude Code) the connector lists your custom objects.

## The process, step by step

### Step 1. Ground rules, access and reading

About half a day.

**1a. Rules.** Write these at the top of the workbook and hold to them:

1. Read-only for the whole assessment. Record, do not fix. Fixes become Step 11 items with a build doc each.
2. Every finding carries evidence: a link, a screenshot, a query and its result, or a quote from the person who said it. "Seems off" is not a finding.
3. Every count carries a read time and the tool it came from. Two HubSpot tools give different numbers for the same question (Step 5), and live objects change while you read them.
4. Write times in the business's timezone. An event at 7 PM local is the next calendar day in UTC, and a UTC date in a report moves it a day.
5. Never record a secret, token or key value anywhere. Prefixes and key ids only.
6. Send nothing outbound (email, Slack, SMS) from the assessment. Draft, show, wait.

**Test-data discipline.** Every end-to-end test leaves records. Before the first one: agree a test identity (an email on a qa+ alias and a test phone), add every internal and test email to the exclusion list reports already use, run page tests in a private window so cookies start empty, never run a processor test in live mode except with a $1 QA product that is refunded afterwards, and mark test-mode mirror rows so they are excluded everywhere. Log every test record you create with its id so it can be found and deleted.

**Evidence grades.** Every finding carries the grade of its best evidence, and the report says so: A, read live from the system today with the read time; B, an export or a workflow JSON pulled this week; C, a document or a build doc; D, a conversation or someone's recollection. Nothing below B reaches the scorecard; a D is a hypothesis to test.

**Independent re-check.** The person running the assessment often built much of what it assesses. Before the report goes out, someone else re-runs ten percent of the checks picked at random and records where they got a different answer.

**Where it lives afterwards.** The workbook, the artifacts and the report go in one shared folder the team can reach, with a named owner. Re-run the five-day track every quarter and after every major change.

**1b. Access.** Fill the access table: one row per system (HubSpot, phone and SMS platform, payment processor, each ad platform, community platform, legacy CRM, Google Workspace, task tracker, chat tool), with the role you were given, who granted it, and the date.

**1c. Reading list.** Before the inventory, read whatever exists of: the secrets and service keys map (if none exists, Step 3 produces one), any process map of the sales setup, the implementation guide and SOP for the recurring operational task (for this business, setting up and flipping a class), the paid media plan, the catalog of what still runs on the legacy CRM, any departure handoff list, and the design document the migration was built from.

✅ **Check:** access table filled in; reading list done; a blank workbook with one tab per area in [`templates/workbook-templates.md`](templates/workbook-templates.md).

### Step 2. Ask the team before you open the portal

About two weeks of elapsed time, a day of your own.

Hear what hurts from the people who use the system every day, and know the questions a CTO would ask, before the inventory anchors you on what the portal looks like from the inside. A complaint is a hypothesis; the later steps confirm or clear it.

**2a. Who to ask.** Groups: leadership, sales, marketing and lifecycle, support and community, builders (whoever wrote workflows, code and scripts), and outside parties (ad agency, the contact-center vendor's success manager, any sales consultancy). Send from one owner, with one deadline five business days out, and say in the first line what will happen with the answers. Keep every survey under ten minutes.

**2b. The survey.** Use [`templates/survey-questions.md`](templates/survey-questions.md): eleven questions for everyone (what they do, every tool they log into, what takes too long, what they have stopped trusting, what broke in the last 90 days, what they do by hand, what they work around, which number they report and where it comes from, what stops if one person leaves, what never got built, what they would fix first), plus the extra questions per group. The payments block asks finance, support and sales what payment types are accepted or facilitated (card, bank transfer, buy now pay later, payment plans, invoices, comps, refunds, subscriptions) and where each one is recorded.

**2c. The day-one CTO questions.** Use [`templates/cto-questions.md`](templates/cto-questions.md). The assessment is finished when each of the fourteen has a one-line answer and a link to the evidence. Record them in a table with the answer, the step that proved it, and the link.

**2d. From answers to findings.**

1. Day 1: send the surveys and the email questions. Log every person asked and the date.
2. Days 2 to 5: collect answers into one sheet, one row per answer, with the person, group, question and verbatim text.
3. Day 6: tag every row with a theme (data trust, manual work, broken automation, missing feature, handoff risk, vendor gap) and the step of this process that can test it.
4. Days 6 to 8: 30 to 45-minute calls with the five most hands-on people. Ask each to show you, on screen, the task they said takes too long.
5. Days 8 to 10: turn each theme into a hypothesis in the risk register with the step that will test it. Example: "reps say deals do not match calls" becomes a Step 3 check on stage movement and a Step 6 check on the phone platform's activity sync.
6. During Steps 3 to 10: mark each hypothesis confirmed, cleared or partly confirmed, with evidence. Never carry a complaint into the scorecard unconfirmed.
7. Step 11: send every respondent a two-paragraph note on what their answers led to. Anyone who answered and hears nothing will not answer next time.

> ⚠️ **Do not skip the survey because the portal is "obvious".** The things people work around never appear in the portal. A spreadsheet someone keeps because a report cannot be trusted is a finding you will not get from any settings page.

**2e. Two interviews the survey cannot replace.** Builder walkthroughs: for every artifact in Step 4, sit with the person who built the piece and have them narrate it while you read the workflow JSON on screen; record what they say it does, what the JSON says, and every difference. Departure interviews within 30 days: when someone leaves, collect every scheduled send and sequence in their name, every list and view they relied on, every login they held, every manual step only they knew and every promise they made to a vendor, while their accounts are still live.

✅ **Check:** the answer sheet exists, every row has a theme, the CTO table has fourteen rows waiting for answers, and the risk register has its first hypotheses.

### Step 3. Inventory

About a week. One workbook tab per area; every row gets an owner and a status (live, off, unknown, retired).

**3a. Portal settings and users.** **Settings → Users & Teams** and **Settings → Account Defaults**. For every user: name, email, seat, last login, permission set, teams, employer (staff, agency, consultant). Flag accounts outside the company domain, any Super Admin who is not an employee, anyone with no login in 60 days, and any seat held by someone who has left. Record teams, the timezone, the currency, the primary domain, who has "Owned only" visibility, who can export, who can edit property settings, and whether a sandbox account exists and has ever been used.

**3b. Seats and licences.** **Settings → Account & Billing**: for every seat type, seats bought versus assigned, cost per seat, the renewal date, who approves a new seat. Check each paid seat against what the person actually does: a rep who lives in the phone platform does not need a Sales Hub seat; a support person who replies from a shared Gmail does not need a Service Hub seat; a Commerce seat is odd when money runs through the payment processor's links. Record the contact tier and marketing-contacts count, every add-on, and the seat counts in the other per-seat tools.

Marketing contacts are a billing lever: find the workflow or setting that makes new contacts marketing contacts, count them against the tier, name the sources that should not be billable (SMS-only registrants, support-only contacts, test identities), and name who reviews the count before renewal.

**3c. Domains and website.** **Settings → Domains & URLs** and **Settings → Tracking Code**. One row per host: platform (HubSpot, legacy CRM, payment processor, community platform, main site), role, DNS host, SSL state, redirects, primary-domain status, tracking code and tag manager container present, cookie banner setting, who can add a subdomain.

> ⚠️ **One letter can change the platform.** In the portal this came from, the plural form of a subdomain was the legacy CRM and the singular form was HubSpot. Check the spelling before concluding anything about a page.

**3d. Email.** **Settings → Marketing → Email**, subscription types, connected inboxes, email health. Record sending domains and their DKIM, SPF and DMARC state; dedicated IP; subscription types and which forms and lists map to each; double opt-in; footer address; bounce and unsubscribe rates for 90 days; who sends marketing email now; transactional email add-on; templates, snippets and sequences in use. Record where inbound customer email actually lands: in the portal this came from, it went to shared Gmail mailboxes, so support's replies were not in the CRM at all.

Also record whether the transactional email add-on is enabled (receipts, welcomes and confirmations depend on it), the dedicated sending domain's standing with the mailbox providers, and any recent open-rate drop with whether it was deliverability or measurement.

**3e. SMS.** If HubSpot's own Calling and SMS settings are empty, every text goes through the phone platform. One row per line: number or name, type (10DLC, toll-free, short code), registration state, the inbox and AI agent it routes to, which HubSpot inbox channel it feeds, opt-out handling, quiet hours, consent capture on forms, the Terms of Use sections that describe the program, send rates, and the workflows that use the platform's "send a text" action. Record throughput (messages per second per line and per account) and the sequence throttle.

**3f. Phone and calling.** Lines and their call routing (calls and texts can route separately), inboxes, users and their owner mapping into HubSpot, dispositions, power dialer use, call recording and transcription settings, where recordings are stored and for how long, and what reaches HubSpot (call records, summaries, external call ids). Booking links: every personal meeting link, every round robin and its members. AI agent settings usually have no API; read them in the admin UI and screenshot them.

**3g. Payments and commerce.** The processor's dashboard plus HubSpot's Payment Processing app and any mirror objects. Record every live Payment Link with its product, price, after-payment redirect and live or test state; optional order bumps; webhook destinations and which workflow each feeds; products and prices with a price-to-tier map; subscriptions and where they land; invoices still in flight on the legacy platform; refunds and disputes (and whether anything writes them back); payout timing; who has processor admin; which account owns the Data Sync connection; test-mode rows; comps. Then confirm every payment type the survey named: card and wallets, buy now pay later, bank transfer, payment plans, subscriptions, comps and discounts, refunds and chargebacks, test-mode payments; one row each with the processor, the product, where the record lands in HubSpot, and who handles exceptions.

**3g2. Events, AI features and ad billing.** If the business runs live events: who owns each system during event week (broadcast, CRM, payments, phone, forms, automations), the run-of-show document, the setup freeze and the testing days, the in-person registration object and whether anything writes to it, check-in, on-site sales and upgrades, and how a sold-out tier is handled (HubSpot holds no inventory, so a sell-out is a page modal plus message copy plus a deactivated payment link, each done by hand). Record whether backup checkout links exist and whether a second processor account has a switchover plan. Record which AI features are on (Breeze agents, data agents, Copilot, AI-assisted lists), who uses them, the data-sharing settings behind them, and where AI steps sit inside live workflows, because they do not show in the workflow list. Record ad platform billing: who receives invoices, the credit line and how prepayments apply (a prepayment raises the limit rather than lowering the balance), and who reconciles invoices against reported spend; a billing hold stops every campaign at once.

**3h. Objects and properties.** **Settings → Objects** for each object; set the properties list to 100 per page before reading, because the default of 10 silently truncates. One row per object: type id, record count, created date, creator, which workflows write to it, which reports read it, whether a unique key exists, association types and labels, property groups, record customization, naming conventions. List the standard objects in play too (contacts, deals and their pipelines, tickets, calls, meetings, communications, marketing emails, forms, line items and payments, companies) and note which are unused.

> ⚠️ **Lookalike properties are the most common trap.** Six "latest class" properties with near-identical labels, a calculated property that lags fifteen hours, a date held as text beside a real date, per-track date fields overwritten on the next registration, an owner property that another platform overwrites. Record each one now; Step 4's property diagnosis gives every property a verdict.

**3i. Pipelines and deals.** **Settings → Objects → Deals → Pipelines**. For every deal pipeline and ticket pipeline: stages and their win probabilities, which workflows move records between stages, which fields hold outcomes, how owners are assigned (round robin, activity-based attribution, manual rotation), forecast settings, amount conventions (placeholders, upgrade deals carrying only the upgrade price). Count deals per stage with the read time.

Lifecycle stage and lead status: record the values in use, who or what sets each, and any temperature mapping. If they change only when a rep moves a deal and reps do not move deals, both are dead for most contacts, and the sales team's requests (a status that flips on a rep's first text; an owner for every contact who texts in, assigned in the phone platform so it copies back) belong on the backlog.

> ⚠️ **A stage nothing moves deals into stays empty and nobody notices.** In the portal this came from, the enrollment pipeline's post-call stages were never set by any workflow; outcomes went to deal fields and a separate appointment object, and the board showed a placeholder amount times the number of booked calls as "pipeline value". A stage weighted at 10% that holds paid deals undervalues the forecast by the same amount.

**3j. Workflows.** **Automation → Workflows**. Read every workflow's full definition from the API, never from the canvas: `GET https://api.hubapi.com/automation/v4/flows/{flowId}` with a service key that carries the `automation` scope (the flow id is the number in the editor URL). The JSON's `actions` array holds every action across every branch at once, which the canvas hides. Each action carries its `actionTypeId` (`0-14` is Create record) and, under `fields.properties`, a `targetProperty` and a `value.staticValue` such as `{{ enrolled_object.utm_source }}`, so one search of the JSON for `targetProperty` and `staticValue` answers "what writes this field, and from what" for a 25-action workflow in seconds. A signature worth recognising: `{{ enrolled_object.hs_analytics_source_data_1 }}` and `_data_2` are the Original Source drill-downs, not UTMs; any field fed from them is a drill-down bug, and that is how one branch of a four-branch workflow was caught mapping drill-downs while its siblings mapped UTMs correctly.

Pull the full list with creators (the API has no creator field): the signed-in session's `GET /api/automation/v3/workflows?portalId=<id>` (header `X-HubSpot-CSRF-hubspotapi` set to the `hubspotapi-csrf` cookie) returns contact workflows with `creationSource.createdByUser`; `/api/automation/v4/flows?portalId=<id>` returns every workflow without creators. For every workflow: name, object, on or off, trigger, re-enrollment, creator, last editor, last edit, custom code actions (count and which secret each uses), outbound webhook actions, third-party actions (such as the phone platform's "send a text"), enrolled count, error count in 30 days, revision count. Group them into families (the recurring operational cycle, payments and routing, ad platform events, sales pipeline, phone and SMS, community platform, marketing sends, backfills) and assign an owner per family.

Flag: any workflow ON with zero enrollments in 30 days; any OFF workflow a live process depends on; any trigger of "form submitted any number of times" with re-enrollment on (that is how duplicate attendance records get made); any "Unnamed" workflow; any workflow whose only enrollment method is "enrolled by hand"; any backfill still ON.

**3k. Forms, pages and modules.** **Marketing → Forms**, **Marketing → Landing Pages**, **Design Manager**. Per form: fields and which are required (a phone made optional cuts SMS reach for everyone who skips it), hidden UTM fields, consent text, which workflows trigger on it and which branch matches on its name, spam protection, the pages it sits on. New-editor form definitions are readable from the signed-in session at `/api/hubspot-forms/v2/form/<guid>?portalId=<id>`; legacy forms from `/api/forms/v2/forms?portalId=<id>`. Per live funnel page: full URL, content id, template, Head HTML blocks, modules, form ids, payment links, pixels. List every Design Manager asset that holds logic (order modules, redirect modules, site header HTML, coded files loaded from Head HTML, modules that read a configuration record).

> ⚠️ **Check the served page with a throwaway query string, never the editor.** Pages behind a CDN can cache for hours. Count snippets in the DOM, not with grep over raw HTML, because inline scripts often contain comment markers that swallow real code. Pages that ship two copies of the same section for desktop and mobile carry every module id twice.

**3l. Lists, reports and dashboards.** Every active list over 100,000 members, lists used as workflow triggers or pushed to ad platforms as audiences, every dashboard and who views it, every saved report that a chat message, email or meeting quotes.

**3m. The business tech stack.** Before reading HubSpot's own integrations list, build the list of every tool the business runs on, whether or not it touches HubSpot. Sources, in order: twelve months of software charges on the company cards and processor payouts; the Google Workspace admin's third-party apps and SSO apps; the survey answer to "every tool you log into"; Zapier connections, Slack apps, task-tracker integrations; DNS records for the main domain (every CNAME and TXT names a vendor); what agencies and contractors log into on the business's behalf. Per tool: what it does, who owns the account, who uses it, monthly cost, how it connects to HubSpot (native app, API, Zapier, CSV, nothing), status.

**3n. Walk HubSpot's integrations pages, tab by tab.** **Settings → Integrations**: Connected apps (name, install date, installed by, scopes, last activity, whether anyone can name what it does); Private apps; Service keys (every key, its scopes, View Logs, the API deprecation banner); Data Sync (each connection, each object sync, status, mappings, start date); Email integrations and calendar (who has a mailbox connected); Marketplace (installed but not configured); Ads (accounts connected, the conversion events page); webhook-trigger workflows (they do not appear here; find them in the workflow list); Zapier and the Claude connector (what each may read and write). For every row ask the same two questions: who owns it, and what stops if it is removed. Anything without an answer goes on the risk register.

**3o. Integrations, keys and secrets map.** One row per integration: direction, mechanism, owner, key or secret. Then one row per service key (scopes, consumers from View Logs, whether shared) and one per secret (purpose, confidence: observed or inferred). The target state is one key per consumer, scoped to the single call it makes.

> ⚠️ **A shared key is a silent outage waiting to happen.** In the portal this came from, one service key was used by eight coded actions. Rotating it would have broken all eight, and the 401 would have named none of them.

**3p. Automation that runs outside HubSpot.** One row per Apps Script project, ad-platform script, legacy-CRM workflow family and phone-platform automation: runtime, trigger, what it writes into HubSpot, owner, last successful run (the project's Executions page, the workflow list's enrolled count).

**3p2. Message assets, archive candidates, timezones and vendor terms.** Message assets (marketing emails, SMS templates and sequences, CRM sequences, a bot's knowledge base) are customer-facing and drift: for each, still scheduled or live, the date it references, every link resolved, tokens that render blank, unsubscribe and STOP handling, the sender. Archive candidates beyond properties: workflows OFF for 90 days with no dependency or unnamed, lists nothing uses, stale cloned and test pages, forms with no submissions in 90 days, sequences nobody sends; output a list with the evidence that nothing depends on each. Timezones per system (portal time, the search API in UTC, the processor in UTC, each ad platform in its own, the phone platform's schedules) in one table. Vendor engagement terms for every agency and consultancy: contract end, service levels, weekend coverage, relationship owner, access held.

**3p3. Backlog and in-flight work audit.** The team's own task tracker is evidence of what is unbuilt, half-built or stuck. Pull every open task in the migration and event projects and classify each: unbuilt, half-built (a build doc or draft exists), blocked (on a vendor, a decision or a person), overdue, stale (no update in 30 days). Map each to the system and the journey it touches. Overdue and stale rows are findings; blocked rows go on the risk register with what unblocks them. Keep a vendor change-notes log alongside it: a vendor's product change (a line removal ending held conversations; a follow-up switch cancelling scheduled ones) explains a mystery a month later.

**3p4. Customer journeys inventory.** The mystery shopper (Step 7) walks one journey; this inventories all of them. One row per journey a customer can be on: trigger, the system that runs it today, the workflow or sequence id, on or off, last send, the messages and their senders, the owner. A journey whose trigger lived on a retired platform fails silently: in the portal this came from, the new-member onboarding workflow went dark at a platform switch and every new paying member for three weeks got no welcome. Journeys to confirm, each with a live trigger: registration confirmation and reminders; event-night links; replay; new member welcome; purchase confirmation and post-purchase nurture; upgrade pushes; physical welcome mail; abandoned cart per product; failed payment and dunning; video-sales-letter sequences; application confirmation and follow-up (and every document that links to the application form); call booking, reminders and no-show follow-up; subscription cancel and win-back; support replies; opt-out acknowledgement.

**3q. Limits, governance and hygiene.**

- **Limits and headroom.** HubSpot caps things silently. Record the current count against each cap: properties per object, active lists, workflows, custom objects and records per custom object, custom code actions and their 20-second timeout, webhook trigger definitions (50 properties), search results (10,000 per query; split by day), daily API calls, and the search API's rate limiting. Anything above 70% of a cap goes on the risk register with the date it will be hit at the current rate.
- **Messaging governance.** When blasts, an AI agent, reps, CRM sequences, phone-platform sequences and marketing email can all reach one contact, write the matrix of who may message a contact on a given day, in what order, with what frequency cap, and which suppression wins (opt-out, open rep conversation, booked call, a purchase in the last 24 hours). An unwritten matrix is how an agent texts into a live rep chat.
- **Calendar hygiene.** Every rep's calendar connection (a disconnected calendar makes a booking return success with nothing created), every round robin and its members, every personal link still in use, and who owns each link when its rep leaves.
- **Error routing.** HubSpot emails workflow errors, Data Sync errors and connected-app failures to someone; the processor emails webhook failures; Google emails script failures. Record who receives each, whether they read them, and what happens when that person leaves.
- **Source control.** Every custom code action, module and Head HTML block lives in a HubSpot text box with no history beyond workflow revisions. Record which pieces have a copy anywhere else, and recommend one private repository where the live version of each is kept with its deploy location.
- **Naming conventions.** Record the conventions in use and the ones nobody follows; propose one standard for new properties, workflows and lists.
- **Backfill log.** Every one-shot backfill that ran (what it changed, when, with which key, whether it is off now). A backfill still on is a writer nobody expects.
- **Legacy platform decommission.** Before the legacy CRM goes dark: export contacts with tags, conversations, payments and invoices, workflow definitions and page HTML; name where the export lives and who can read it; list the paths and short links still pointing at it.

**3r. Full audit log review, API-assisted.** The audit log is the only change history the portal keeps outside workflow revisions. Pull all of it, from the portal's creation to today, into the workbook; clicking through the UI page by page is not a review. The audit log page keeps its filter state in the URL (category names, selected user ids, time period, page size up to 100) and loads its rows from the signed-in session's own JSON endpoint, so drive it from Chrome one category at a time (workflows, properties, lists, settings, app activity, content, users and permissions, exports, imports, deletes) and page until exhausted with the tab in front. For sign-ins and security events use the Account Activity API (`GET /account-info/v3/activity/login` and `/account-info/v3/activity/audit-logs`, Enterprise) with a service key. Save every row: timestamp in the business timezone, user, category, action, asset name and id, before and after values where given. Then look for: who builds (changes per user per week per category; an agency account changing workflows after its engagement ended); departed users (any change, login or export after their last day, and any live automation still attributed to them); bursts around incidents (what changed just before each outage, by whom); toggles (workflows switched on and off repeatedly, properties created and archived within days, test assets never cleaned up); keys, secrets, private apps and connected apps created or changed, and whether the secrets map knows; exports and bulk deletes, which leave no other trace; settings changes (domains, tracking code, email sending, subscription types, permission sets, Super Admins) matched to a known decision or flagged; out-of-hours and weekend changes matched to incidents; logins from new devices or locations and by shared accounts. Output: the full log as a workbook tab, a one-page summary by person, category and month with the incident clusters, and the list of changes nobody can explain, which goes on the risk register.

✅ **Check:** every tab has an owner and a status on every row, and the keys-and-secrets tab has no "unknown" row you have not tried View Logs on.

### Step 4. Generate the three artifacts

About three days. Each artifact is also a diagnostic: building it exposes what is wired wrong.

**4a. A process map per pipeline.** One interactive page with a tab per flow, where every box opens the plain-language answer to its question, how the step was meant to work and where that came from (a meeting, a chat thread), and the workflow steps behind it with a link to each workflow. Build one for every automated process, not only sales: the recurring operational cycle, payment routing and record creation, the ad-platform event chain, inbound message handling. Method: read each workflow's v4 definition for the steps; pull counts live and label them with the read time; draw boxes for stages a workflow sets and dashed boxes for stages only a person moves; end with a "Workflows on this map" table. The template, with a data checker and a fictional example, is the [hubspot-pipeline-process-map](https://github.com/carljibrilsulaimanii/hubspot-pipeline-process-map) repo.

**4b. An entity relationship diagram.** Every object in play (standard and custom) with the association types and labels between them, the cardinality, the unique key each object has or lacks, and the writer of each object (Data Sync, a workflow, a private app, a person). Source: `GET /crm/v3/schemas`, `GET /crm/v3/properties/{object}`, `GET /crm/v4/associations/{from}/{to}/labels`. Draw it as a mermaid `erDiagram` or an SVG; one line per association with its label. It is finished when every arrow has a writer and every object has a key or a note saying it has none.

**4c. A property diagnosis.** For every object, a register with one row per property: internal name, label, type, group, created by and when, fill rate, where it is used (the **Used in** column on the property page: workflows, lists, reports, forms, record cards), who writes it (property history source on a sample of five records, and the lag), and a verdict. Fill rates come from counting records where the property is known (a `HAS_PROPERTY` search per property, or the percentage the property page shows). Verdicts and tests are in [Reference: property diagnosis verdicts](#reference-property-diagnosis-verdicts). Output: the register, plus a short list of merges, archives and type fixes for Step 11, each with the assets that reference the property so nothing breaks when it changes.

✅ **Check:** a person who has never seen the portal can click through the process map and say what creates a deal; the ERD has no object without a writer; every property has a verdict.

### Step 5. Data integrity

About two days.

**5a. Pick the right counting tool first.**

| Question | Use | Never use |
|---|---|---|
| Any date-windowed count | The search API `total` with UTC bounds shifted to the business day (for example `T04:00:00Z` for Eastern daylight time) | Report-style queries, whose date filters can include the whole next day |
| A per-event count | The event's date property | A session or label property written by the page's form, which varies by page variant |
| A monthly figure for leadership | The reporting API or an export (both in the portal timezone) | The search API alone, without shifting the boundary |
| A distinct count of a text property | `GROUP BY` chunked by a trailing-wildcard `LIKE` prefix, summing the reported totals | `COUNT(DISTINCT)`, which can return garbage |
| Enum or string filter counts | Either engine; they agree to the record | |

**5b. Contacts.** Total and the creation-date histogram (a bulk load shows as a spike; anything earlier lives in the legacy CRM). Duplicate rate by email and by phone. Fill rates for UTM properties and click ids. Junk values to exclude from attribution: import markers, unexpanded template macros such as `{{ad.id}}`, referring domains in a source field, campaign names in a source field, and several spellings of one platform. Original Traffic Source distribution, and why paid traffic files where it does (a `utm_medium` holding an ad set name files as "Other Campaigns"). Data quality rules in force: phone format, required fields, dedupe settings, whether anyone reviews the duplicates tool.

**5c. The high-volume custom object.** For the object the business runs on (registrations, orders, whatever it is), per period: records, distinct keys, records with the key empty, records from each writer (tell them apart by a naming suffix, a tag spelling or an empty field), and the three different numbers people call "attendance" or "orders" (form submissions, records, deduped unique people). Only the deduped one is real. After every operational flip, check that new records carry the new label and not the old one.

**5d. Payments.** The mirror object holds every payment attempt, succeeded or not; filter `status = succeeded` for any count, and remember succeeded still includes refunds. Join the business object to the processor on the payment intent id, not the charge id if the charge id is empty. Zero-amount checkouts create no payment intent and need their own path. Test-mode rows must be excluded everywhere. If the Data Sync connection was ever rebuilt, verify the live mappings against the rule that decided which properties are sync targets, and list payments still missing since the outage.

**5e. The configuration object.** If pages and workflows read a configuration record, check its date fields' types (only a datetime can be used for arithmetic) and that no active record's start time is in the past, because a past start time can redirect every new visitor to the old event.

**5f2. Fields whose meaning changed on a date.** Some fields mean different things before and after a change: a first-touch field that was fed from traffic-source drill-downs until a workflow was remapped on a known date; a `utm_medium` convention that moved from a fixed value to the ad-group name, breaking every report that filtered on the old value; attendance records that carried no email or phone until an action was remapped, which broke the ad platform's matching and made attributed attendance collapse while receipt rose; attendance records that inherit the reminder session's UTMs rather than the acquiring source, so attendee channels must be read by joining back to the acquiring registration; a product renamed so that a report filtering on the old name undercounts; contacts whose emails were added as alternate emails on other contacts. Record the date for each and report on the field only from that date, or split the series.

**5e2. Acceptance thresholds and sampling.** A check without a number is an opinion. Set a threshold for each measure before you read it, from the baseline your first read establishes, and investigate anything past it: registrations with no UTM per day, purchase records with a blank or junk first-touch field, last touch disagreeing with the processor's source, records the attribution fill skipped, contacts with a duplicate email or phone, the share of mirror rows that succeeded, attendance records with no email, workflows with errors in seven days. One portal's baseline, for scale: untagged registrations 14 to 23% a day (investigate above 25%), blank first touch on purchases 22.6% (investigate a rise of 3 points), junk 0 (investigate any). A property-history sample of five records proves nothing on a portal with hundreds of thousands of contacts: sample at least 30 per writer and per period, picked by random record id. Search results cap at 10,000, so split by day; the search API's equality filter is case-insensitive, so count case variants in code.

**5e3. Month-end money reconciliation and phone-only contacts.** Once a month four numbers must agree or the differences be explained: the processor's payouts and refunds, the mirror object's succeeded rows, the business purchase records, and finance's ledger; expect refunds never written back, test rows, comps with no payment intent, invoices with no payment link, double checkouts. Count contacts with a phone and no email and with an email and no phone; they break email-keyed dedupe and decide whether a form's phone may stay optional.

**5e4. Identity model and migration completeness.** The CRM keys a person on email, the phone platform on phone, the processor on whatever the buyer typed; the portal this came from carried about 94,000 potential duplicates with no merge rule, a mistyped checkout phone overwrote a contact's number in the phone platform, and a new email with the same phone made a second contact. Write the identity rules: the identifying field per system, which system wins per field when they disagree (a precedence matrix for name, email, phone, owner, address), phone normalization, alternate-email handling, what a merge does downstream, who runs the duplicates tool and how often. Then reconcile migration completeness per cohort: legacy contacts and tags against CRM contacts and properties, community members against the CRM, legacy payments and invoices against the mirror, course-progress and member-temperature fields. A cohort that cannot be matched is a finding, and the legacy shutoff waits for it.

**5f. Reports people quote.** Every number that reaches leadership, agencies or sales in a normal week: who produces it, from which tool, with which filters, and whether a second person could reproduce it. Where two versions exist (Step 2 surfaces them), record both and the reason.

✅ **Check:** the trusted-numbers sheet has, for each metric, the exact query, the exclusions, the current value with its read time, and an owner.

### Step 6. Integration health, one at a time

About three days. For every integration: prove it is running today, find its single point of failure, and record how to tell when it breaks. Read-only; record pass or fail, the evidence, and the failure signal.

| Integration | Test | Failure signal |
|---|---|---|
| Payment processor Data Sync | Connection id, sync status and last sync time per object; compare the last 24 hours of succeeded payments in the processor to mirror rows by payment id | Downstream workflows' enrollments drop to zero while the processor keeps charging; nobody is alerted |
| Processor webhooks into workflows | List every destination, its events and delivery success; confirm every handler re-fetches by id and treats the body as a pointer (a webhook-trigger URL has no signature check); confirm no secret field was kept when the definition was pruned to the 50-property cap; confirm every handler is safe to run twice, because the processor retries anything that does not answer 2xx; read each code step's own status output, because HubSpot's log can say success on a 429 | Webhook delivery failures in the processor's dashboard; stamps missing on records the log calls successful |
| A private app that writes business records | Its API call logs: creates in the last 24 hours, no 400s; who built it and whether a second person can maintain it | Business records stop being created |
| Phone and SMS platform | Sync direction and field mappings; which platform's owner wins; inbox open and unassigned counts after a blast; each line's inbox and agent; throughput; whether the platform writes stale "last inbound" dates on first sync (every trigger on that field needs an "is today" guard) | Replies pile up unassigned; a trigger fires on a stale date |
| AI booking bridge | Trace one booking end to end: the function, the form, the workflow, the meeting link and its round-robin members, the confirmation text; the meeting type stamp | A disconnected rep calendar returns success with nothing created |
| Ad platform conversions API (Meta) | Each workflow ON, test code detached, guard properties stamped; Events Manager shows the event server-side only, with real amounts and an event id; the Event Setup Tool holds no stray rules; custom conversions and their default values | A page event dies while page views keep firing after a funnel swap |
| Ad platform conversion uploads (Google) | Each workflow ON, validate-only switch detached; conversion actions and which are Primary; the OAuth app's state (a project left in Testing kills refresh tokens after 7 days); allow about 3 days before judging a day's match | Uploads fail with `invalid_grant` |
| Ad platform data feed | The script's last run and the sheet's date range; the writer is an ad-platform script, not Apps Script | Authentication errors in the script history |
| Legacy CRM | Which workflows still receive live traffic (enrolled counts), what must move before it goes dark, how the API behaves (names only for workflows; some HTTP clients blocked) | Enrollments keep growing on something that should be retired |
| Community platform | Its one webhook URL and where it points; the objects the migration is building; the last-visited sync's last run | A script failure with no alert |
| Native ad conversion events | Which survive, which have never synced, whether they duplicate the server-side event | A "never synced" event on a live form |
| Inbox tagging workflows | The read-only key they use; the shared views exist; the recency guard is still on | A rotated key stops tagging with a 401 |
| Inbox routing | An inbox conversation can show no owner while the contact shows the rep; a contact who texts in with no owner gets a rep, assigned in the phone platform so it copies back; support does not reply inside sales threads; blasts do not flood rep inboxes; whether a broadcast assigns an owner is known | Customers answered late because the thread was in the wrong inbox |
| Phone platform open items | Every latency, queue, dialer, bot and attribution item raised with the vendor, with its age and status | Items raised for a month with no resolution recorded |

✅ **Check:** the integrations tab has pass or fail, a failure signal, and a "who gets paged" column for every row. If the answer is "nobody" for most rows, that is a Step 11 item.

### Step 7. Scenario walkthroughs

About two days. Prove the system end to end on the events the business actually has, not integration by integration. The table in [`templates/scenario-walkthroughs.md`](templates/scenario-walkthroughs.md) lists nineteen scenarios with the trigger, the trail each should leave, where to verify, and the failure already seen for that shape of business: setting up a new recurring event, the event night, a self-serve purchase, a rep-attributed purchase, an upgrade, a comp, a refund, a buy-now-pay-later purchase, a deposit, a booked call, a call outcome, a marketing blast and its replies, a community subscription event, a member login, a payment sync outage, a token expiry, an employee departure, a new subdomain funnel, a form edit.

Run each on a real recent record, read-only, and record pass, partial or fail with the record links.

Three more scenarios belong on the table for most businesses: a product or tier sells out (what stops selling, where, and how long the gap is), the week of a live event (freeze, testing days, owners per system, daily check-ins, backup checkout links), and an ad platform billing hold (what stops, who is alerted first, how prepayments show in the balance).

And one scenario that is not a system event at all: the mystery shopper. Register a test identity from a tagged ad link, attend, buy, reply STOP to one text, and keep a diary for ten days of every email, text and bot message with its time, sender, resolved links, personalization, duplicates and gaps. Duplicates, dead links in templates and overnight texts are usually found by customers first.

And one more: a platform is switched off. Every journey that triggered on it has a live trigger elsewhere before the switch, every document and page that linked to it is repointed, and the first customer event after the switch is watched.

✅ **Check:** every scenario has a verdict and a record link, and the list of scenarios with no owner who would notice a failure is written down.

### Step 8. The measurement chain

About a day. Follow one person from ad click to purchase and show where each number is captured, lost or inflated. This is the chain leadership's spend decisions rest on.

Trace, for one recent event and one recent purchase: the ad click (what the URL template puts in each UTM field); the landing page (what is read into the visitor cookie); the form submit (a pop-up form does not navigate, but a redirect drops the query string); the contact (live UTMs versus UTMs frozen at registration); the business record (which workflow writes it, keyed on what); attendance or fulfilment; the purchase (how UTMs reach the processor's checkout session and back to the mirror record); reporting (where spend lives, and whether it can join the outcome rows at all).

Record the ratio at each hop and compare to what the platforms claim: platform-reported leads to CRM records (a stable ratio across seasons is normal; a sudden collapse during a funnel swap is a measurement outage, not a delivery one); platform purchase events to processor payments (a 7x over-count came from four uncoordinated senders with no shared event id); records to unique people.

Rules that survived the arguments with the agencies: judge budget moves on purchases per thousand of spend, never on cost per lead; manage against backend cost per lead, not platform cost per lead; re-derive any figure from the live source before it leaves the building.

✅ **Check:** the chain diagram has a capture rate at every hop, and the hops with no owner are listed.

### Step 8b. Tracking and attribution QA

About two days per funnel the first time, half a day on repeat. Prove, with tests rather than theory, that a source survives every link of the chain: the ad URL, the site header and its cookies, the form's hidden fields, the contact, the business record, the order page, the processor, the purchase record, the pixel and the server-side events. A blank UTM has at least five different causes in five systems; the job is to find which link lost it. Run this on every funnel after every page clone, header edit, workflow change or ad template change, and on a schedule during a campaign.

**8b-1. Site header and cookies.**
1. Check the site header HTML on every domain in the settings dropdown, not one: a sitewide header can be overridden per domain. Fetch one live page per domain with a cache-busting query and confirm the header script is in the served HTML.
2. In a private window with devtools open, load a funnel page with `utm_source=qa&utm_medium=qa&utm_campaign=qa&utm_content=qa&utm_term=qa&fbclid=qatest`. Record every cookie the header writes (name, domain, expiry, value shape), plus the ad platform cookies and the HubSpot visitor cookie.
3. Test the rollover rules one at a time: a second tagged visit (does the stash replace or keep the first touch, and which field records the capture method); an untagged visit (does the cookie restore the UTMs into the hidden fields); a return from the processor carrying only the five `utm_*` (does the guard keep the click ids); a value with a dot, a pipe or a plus (the processor's URL rules allow only letters, digits, underscore and hyphen; the module must clean them and the guard must compare cleaned values); keys starting with an underscore (should be skipped on the way out).
4. Vanity redirect pages: hold the redirect with a preview parameter and read the resolved target; a client-side redirect makes the destination see the vanity page as referrer, so the real platform survives only through an entry cookie.
5. In-app browsers: an ad clicked inside a social app and a purchase made later in another browser share no cookie, so the checkout looks like a direct visit. Read the click id and the prefilled email on the session to tell the cases apart. The fix is tagging texts, sequences and reps' links, not the page.

**8b-2. Forms.**
1. Read every registration form's definition, not one page's DOM: each must carry hidden `utm_source`, `utm_medium`, `utm_campaign`, `utm_content` and `utm_term` filled from the page URL. Retire forms with one packed field.
2. Submit the test identity through each form from the tagged page and read the submission's page URL and the contact's `utm_*` afterwards. An untagged record whose page URL had no `utm_` is an untagged visit, not a broken page; one whose page URL had `utm_` and whose record is blank is a broken hidden field or mapping.
3. Confirm which fields are required and that consent text is present on every form.

**8b-3. Contact and business record.**
1. On the test contact: the live `utm_*`, the frozen registration-time copy, the Original Traffic Source and its drill-downs. Note why paid traffic files where it does.
2. Pull the writing workflow's JSON (Step 3j) and confirm every Create record branch maps last touch from the contact's live UTMs and first touch from the code step, and that no branch maps `hs_analytics_source_data_1` or `_2` into a UTM field. Source, medium and campaign move together from one record.
3. Search records created since the last fix for junk in the first-touch field (an import marker, an all-digit id, a GUID, a domain, a value with `/`, `|` or `{{`, over 40 characters). Any hit is a regression.

**8b-4. Order page, processor and purchase record.**
1. On the order page with the cookie set, read the payment link the module builds and confirm it carries the cleaned `utm_*` and no underscore keys.
2. Complete a checkout with the $1 QA product (then refund) or read a recent real session: the session's success URL carries `utm_*`; the mirror row gains its UTM fields and a capture method; a payment with no payment link id is a renewal or invoice and can never carry a UTM, so inherited fields must cover it.
3. On the purchase record after the fill runs: first touch from the acquiring registration (the most recent event held by the purchase date, earliest sign-up within it), last touch equal to the processor's source, the event name from the event topic, and the fill's timestamp. A blank timestamp older than the fill's delay means it did not run; read the action's own status output.
4. Any second purchase object (kits, add-ons) has its own coverage; a coverage gap is not a tagging gap.

**8b-5. Pixels, tags and server-side events.**
1. On every funnel page, in devtools, list the events that fire on load and on submit and what each carries. Expected baseline: page views only in the browser through the tag manager container, no inline purchase event anywhere when purchases go server-side, and the Event Setup Tool holding zero rules.
2. Count snippets in the DOM, not with grep over raw HTML, and read the context: a match inside a comment that documents a removed call is not a live call.
3. For every server-side event: the workflow is on, the test code is detached, the guard property stamps, the event id is stable (the payment intent for Purchase; something current at trigger time for attendance, never a calculated property that lags), and the match keys include a hashed email and phone. Browser and server dedupe only within 48 hours and only on a shared event id.
4. The lead event as a release gate: after any funnel deploy, read the dataset's hourly stats by event and by host. Page views up and leads flat on the new host means the lead sender died with the swap.
5. Consent: load with the cookie banner declined and record which tags still fire; decide whether that is intended.

**8b-6. Ad URL templates and naming.**
1. Read the URL template on every platform and search contacts for literal, unexpanded macros (`{{ad.id}}`, `{{site_source_name}}`): any hit is a live ad with a broken template.
2. Keep one registry of source and medium values and their meaning, with the date each convention changed, and group one platform's many labels in every report.

**8b-7. Baselines.** Write a repeatable health check over a window: blank and junk rates, last touch equals the processor's source, untagged registrations per day, inherited coverage, and whether every page carries the header, the fallback and the module. The thresholds from Step 5 say when a reading is a problem. Re-measure after every change, and say plainly which share is an honest unknown (buyers who never registered, untagged visits, other-browser checkouts).

> ⚠️ **Never overwrite a raw capture.** The processor's UTMs and a real last touch are evidence. Inherited or derived values go in their own fields with a note on their basis. And never map a UTM field straight from HubSpot's traffic-source drill-downs; parse them in code with the junk rules.

✅ **Check:** the chain has pass or fail per link, the cookie and event inventories exist per domain and page, the junk and blank rates sit against their thresholds, and every fix names the layer it belongs to (page, form, workflow, processor, ad template).

### Step 8c. Funnel page QA

About half a day per funnel. Every page a customer can reach does what the spec says, charges the right thing, sends the buyer to the right place, and shows the same thing on desktop and mobile. The method is the public [hubspot-funnel-qa](https://github.com/carljibrilsulaimanii/claude-code-skills/tree/main/skills/hubspot-funnel-qa) skill, folded in so pages are checked the same way every time. Its value collapses if a finding is wrong: a false blocker sends the page team chasing a closed item. Verify; never infer.

**8c-1. Establish the standard before you look.** The spec document for the funnel (a v2 usually carries only deltas, so v1 still holds things like the date line; check which version the build team received); the event date taken explicitly from the run of show, never inferred, with weekday names verified against the calendar; the naming standard for slugs and step titles; the pixel standard; the brand guide; and the product checklist that maps every payment link id to a product and price. Without a spec, audit for defect classes only (dead buttons, stale years, drift between pages) and say so.

**8c-2. Crawl, then verify what a crawl cannot see.** Fetch every page from one same-origin browser tab (titles, meta, dates, links and payment-link ids in one pass, always with a cache-busting query). Three things are invisible to fetched HTML and each has produced a wrong finding: pixels, which HubSpot injects at runtime (check that `fbq`, `ttq` and `gtag` are defined on a loaded page); hosted checkout pages, which render client-side (load the link in a browser to see product, price and any bump); and anything behind a trigger (accordions, step-2 panels and pop-ups exist in the DOM before they are visible; search the raw HTML before calling copy missing). Confirm the viewport is landscape and non-zero before trusting any measurement.

**8c-3. The checks, in order of damage.**
1. **Can a customer complete the action?** Enumerate every link and button; flag any with no `href` or `href="#"` (HubSpot labels them "Link not set, button inactive"). Then resolve every destination: a button pointing at a 404 or the wrong product passes a presence check and still loses the sale. Some platforms return 200 with the funnel's first page for an unknown path, so check the page title, never the status code.
2. **Does it charge the right thing?** Look up every payment link id in the product checklist; report product and price, not the id. A test-mode link looks like a checkout and takes no money.
3. **Does the processor work end to end?** Which link the order module actually sends people to, whether a choice on the form changes it, test or live, product and price, and where the processor sends the buyer afterwards: a link set to the processor's hosted confirmation never returns anyone to the funnel's confirmation page, so the booking button, the next-steps copy and the UTM capture there silently never happen. An optional order bump is not a forced bump; a page without one is not a defect.
4. **Dates.** Every date string on every page against the standard, with weekday and year.
5. **Desktop versus mobile.** Pages that ship two module sets get edits on one. Diff the visible text at 1440px and at 375px; say which side is right.
6. **Tracking.** Base pixels on every step; a lead event on opt-in confirmations; no purchase snippet on any page when purchases go server-side; `gtag` defined, not merely present; a page on the wrong subdomain has no pixels at all.
7. **Live redirect on every event confirmation page**, exactly once, with a future start; and no redirect on any replay sign-in page.
8. **Titles, descriptions, share images.** Internal titles leak; empty meta descriptions and a missing `og:image` make every shared link render bare; confirmation and order pages carry noindex, never the whole domain.
9. **Clone residue.** Another funnel's copy, another page's hero, another funnel's form, duplicate names and titles, stale clones, dead campaign framing, product-name drift, values that do not add up, casing drift, images nobody checked.
10. **The chain past the page.** The form feeds the right workflow branch, the record lands with the right dropdown option, and the flip did not leave the ads pointing at the old funnel. A page the ad platform rejected is a page defect too.
11. **After a fix.** Re-read the live page with a fresh query string (the preview pane is not the page; a stale read reports a fix as not done); check every page that carried the defect, not the first one; check for the regression the fix introduced.

**8c-4. Report as a punch list.** One row per page, every page by its full URL, each finding with severity (blocker, major, minor), the evidence, the expected value from the spec, and who fixes it. Group by page so the page team can work top to bottom; put the full URL on every task.

✅ **Check:** a punch list per funnel, a pages list with status, and the defect classes that recur across funnels (those become build standards, not tickets).

### Step 9. Security, access and resilience

About two days. Know what one leaked credential, one departure or one deprecation would take down.

- Service keys: scopes, consumers (View Logs), shared or not. Over-scoped keys (dozens of scopes including sensitive data, file delete and import) and keys shared by several actions go on the register.
- Secrets: every one resolved to a consumer through View Logs; look-alike secrets that may hold the same credential under two names; generically named secrets nobody can explain.
- API deprecation: the banner on each service key says which API versions end when. Schedule the earliest first.
- Credentials held by one person outside the vault (tokens for ad platforms, the legacy CRM, the phone platform, the processor, Google) and what each can do (a user token with write scopes that expires every 60 days is a different risk from a system user).
- Departures: every asset a departed person owns (workflows, emails, segments, contacts), and the order of reassignment when another platform syncs ownership into HubSpot (change it there first, or HubSpot's reassignment will not stick).
- People dependencies: what each builder, consultancy and agency built, and whether a second person can maintain it.
- Unauthenticated endpoints: every webhook-trigger URL is a write endpoint with no signature check.
- Sensitive data and consent: which properties are marked sensitive, which keys can read them, whether any coded action logs payloads; where SMS and email consent is recorded and whether it would satisfy a TCPA or CAN-SPAM question; quiet hours; who handles a delete request and whether it reaches every connected platform.
- Backups and retention: HubSpot keeps no restorable backup; are there scheduled exports, where do they go, who could rebuild a custom object from one.
- API limits: daily call volume against the portal limit; the search API's rate limiting under scripted counts; which integration would fail first.
- Access hygiene: whether two-factor authentication or single sign-on is enforced for every user, which logins are shared, when access was last reviewed and by whom, and the contract renewal calendar for every platform in the stack (renewal date, notice period, owner).
- Domains, DNS and certificates: who holds the registrar and DNS login, when each domain renews, whether auto-renew is on, when each certificate expires.
- Every deprecation on one calendar: CRM API versions by key, the processor's API version pinned by old callers, ad platform token cycles, OAuth app states, vendor product changes that alter behaviour, connector scopes. One owner, a reminder 90 days ahead of each.
- Monitoring: list what would page a person today. Usually nothing does.

**The minimum alert set.** "Monitoring: none" is a finding; propose the fix with it. Ten alerts, each with the signal, the check and who is paged: payment sync stopped (mirror rows created today versus the processor's succeeded payments today); a conversion workflow erroring (workflow error notifications routed to a shared channel, not one inbox); guard stamps falling to zero on an event night; an ad platform token's expiry on a calendar a week ahead; an OAuth refresh token's first failure; an ad-platform script's last run older than 36 hours; an Apps Script failure; the credit line's headroom under one week of spend; webhook delivery failures at the processor; a backfill workflow still ON a week after it should have finished.

**Cost if it fails for a week.** Give every risk-register row a rough weekly cost in dollars or leads; the number can be crude, its job is to order the list.

- Continuity: backup checkout links; a switchover plan for a second processor account; weekend coverage for the agency and the team; a restricted read key for an agency that wants to reconcile against the CRM itself (one key per consumer, named for them); who checks the ad platform credit line and prepayment schedule each week.

✅ **Check:** the risk register has likelihood, impact, blast radius and a proposed owner on every row.

### Step 10. Operations and process

About a day. Understand the recurring work the system demands and whether it can survive a handoff.

- Time the recurring operational cycle end to end (for this business, building and flipping an event: a configuration record, forms and workflow branches, page flips, calendar buttons, payment links, pixels) and note every manual step.
- Who does what today, by name and by family of assets.
- Where work is tracked, and whether a request queue exists.
- Documentation state: header comments in pasted code are often the only documentation that travels with it; which live workflows have no doc anywhere; whether workflow and property descriptions are filled in inside HubSpot.
- Change control: is there a "build doc before building" rule, and does anyone besides its author follow it. HubSpot's only change history outside workflow revisions is the audit log.
- Meeting cadence that drives changes, and training: who has had HubSpot training, who holds a certification, what a new rep or marketer is shown on day one.

- Event-week rhythm, reusable for every live event: a setup freeze two business days before, two days of testing, no open phone-platform issues going into the week, email and SMS sending coordinated through one person, check-ins three times a day, and a named escalation for each system when its owner is on stage.

- Roll up the cost of the whole stack (every seat, line, add-on, agency and per-event fee) into one monthly number with an owner per line, so the roadmap can say what a change saves or costs.

- Service levels: speed to lead, reply time on messages that need a person, the agency's page-change turnaround, and who measures each.
- A decision register: one row per design decision that shapes the build, with date, decider, where it is recorded, and whether it still holds.
- A glossary of the lookalike terms (event date versus session versus start time; registration versus attendance versus replay records; first touch versus last touch; tier names and prices; the bots' names; the duplicate accounts).
- A vendor performance review: a scorecard per vendor with open tickets and their age, what the contract promised, what was delivered, slipped dates, and the relationship owner on each side; reviewed monthly and brought to an accountability session.
- Training assets and the new-caller setup: every recording and guide inventoried, and a standard setup for a new caller or rep (accounts, lines, lists, scripts, where notes go, who to ask).
- A change gate for outside parties: one intake, one release checklist per change type (pages: Step 8c; ad templates: Step 8b; workflows: a build doc), and a day-one checklist for every new rep or marketer.

✅ **Check:** a RACI exists for every workflow family and every integration, and the manual steps that should be automated or checklisted are listed.

### Step 11. Score it, register the risks, write the roadmap

About two days. Turn findings into a prioritized plan the team can approve.

**11a. Scorecard.** Score each area 1 to 5 with the evidence and the one change that would move it one point.

| Area | 1 | 3 | 5 |
|---|---|---|---|
| Data model | objects duplicate each other, no unique keys, labels lie | keys exist but are not enforced, lookalike fields documented | unique keys enforced, one writer per object, labels match contents |
| Data quality | counts cannot be trusted without a script | trusted-number queries exist and are written down | native reports match the trusted queries |
| Workflow hygiene | unnamed and duplicate workflows, triggers that re-enroll on every submit, backfills left on | families named and owned, backfills off | every workflow has an owner, a doc and an error alert |
| Integrations | each depends on one person and one shared key | keys per consumer, runbooks per integration | monitored, alerting, tested on a schedule |
| Attribution | spend and outcomes never meet; platform numbers used as truth | backend numbers defined; chain documented | one fact table, reconciled weekly, agencies work from it |
| Security | over-scoped keys, shared keys, unexplained secrets, unauthenticated endpoints | map current, rotations planned | least privilege, rotation calendar, endpoint validation |
| Operations | one person, undocumented manual steps | SOPs exist, second person trained | request queue, change control, on-call |
| Documentation | conversations only | drafts and setup notes per integration held by one person | docs linked from inside HubSpot (descriptions, workflow notes) |

**11a2. The report and the sign-off.** Three audiences, three pages. Leadership: one page with what the system does for the business, the five numbers and whether they can be trusted, the three risks that cost the most per week, the three fixes for this month, and the decisions only they can make. Builders and agencies: the findings log, one row per finding with its evidence grade, the step that proved it and the fix's owner. The next admin: the workbook, the artifacts, the decision register and the glossary. Nothing is finished until an ownership sign-off meeting has happened: every workflow family, integration and recurring task read aloud with its proposed owner, and each owner saying yes in the room. An owner column filled in alone is a wish.

**11b. Risk register.** One row per risk: id, risk, evidence and its grade, likelihood, impact, blast radius, cost if it fails for a week, owner, fix (build doc id). Seed it from Step 9 and the known-items list.

**11c. Roadmap.** Three buckets: stabilize (first 30 days: owners for orphaned assets, ownership reassignment in the right order, one key per coded action starting with the shared one, monitoring for the silent integrations, backfills off, the worst trigger fixed); make numbers trustworthy (30 to 60 days: unique keys enforced, the fact table, refunds written back, test rows excluded, the property merges and type fixes); reduce dependency (60 to 90 days: API version migration, personal tokens into a shared vault or system users, legacy platform retired in order, a request queue, a second person trained on the recurring cycle).

**11d. Keep a known-items list.** Every defect found becomes a row with the date found, the evidence, and the fix's build doc id. The next person starts from this list instead of rediscovering it.

Seed it from four places besides your own findings: the open-items table at the end of any implementation guide (fields left blank at launch, branches that differ from their siblings, trigger lists with raw ids), the open-decisions list of any live plan (targets never answered, builds half done, incidents awaiting a backfill), the workflows with custom code or outbound webhooks whose purpose nobody can state (open them and record what they do before anything depends on them), and the pending builds with a doc awaiting review.

✅ **Check:** the scorecard, the risk register and the roadmap are in one document the team can comment on, and every respondent from Step 2 has heard what their answers led to.

## Reference: property diagnosis verdicts

| Verdict | Test | Generic examples |
|---|---|---|
| Unused | 0% fill and used nowhere, and not a HubSpot default | A sibling of a used property that nothing ever wrote; a source field on a custom object that every writer leaves empty |
| Rarely used | under 1% fill, or used in one asset nobody opens, or last written months ago | A capture-method field with a handful of values; a guard property that was stamped a dozen times then stopped |
| Misused: label does not match contents | the label says one thing, the values say another | A "latest class name" that holds a composite string of email, name and tag; a "latest attended" field that holds a datetime |
| Misused: wrong type | a date held as text, a number as text, an enum as free text | A text "date" beside a real date property on the same object; two text dates and one datetime on a configuration object |
| Misused: duplicates and lookalikes | two or more properties that should be one | Six near-identical "latest" properties; four date fields on one record; four spellings of one ad platform in a source field |
| Misused: junk values | values that never came from a real source | Traffic-source drill-downs copied into UTM fields; unexpanded ad template macros; an import artifact in thousands of source fields |
| Misused: wrong writer | a calculated or synced property keyed by workflow logic, or two writers on one property | A calculated property that lags hours used as a dedupe key; an owner property overwritten by another platform; two date fields on one record disagreeing |
| Archived but referenced | an archived property still named in a workflow, list or report | Check each archived property's Used in |

## Reference: HubSpot traps that cost time once already

- Two "quick filter" UIs exist on dashboards; the toolbar one is a dead end, the real one is under Advanced filters.
- A date property gives a calendar picker, never a dropdown of real event dates.
- Custom code outputs need an explicit Data outputs declaration before a downstream action can bind them.
- The "Property to include in code" panel maps properties only; constants go in the code.
- In workflow custom code, a processor 4xx must be a skip, never fatal, or test-mode rows kill the queue.
- The API cannot publish pages outside the primary domain; publish from the editor and re-check the live page with a fresh query string, because the editor can drop an API edit made a minute earlier.
- The reporting engine cannot GROUP BY a date property and truncates dates to the month.
- The quick-create property panel caps dropdown options at 10; use the full editor's "Paste your own".
- A webhook workflow's Match step is mandatory; a person who is not already a contact never enrolls.
- A webhook trigger definition is capped at 50 properties; a processor payload flattens to more, and the only error is a bare 400 whose reason is in the network response.
- Meta dedupes an event id for 48 hours only; Google treats a repeated transaction id as a permanent adjustment.
- HubSpot's IP Timezone property is a slug (`america_slash_new_york`), not an IANA name.
- Hardcoding a fixed UTC offset breaks on the first Sunday of November.
- A browser window in portrait orientation breaks HubSpot's rendering in ways that look like product bugs.

Traps met while installing a ticket-pipeline request queue through the API, worth knowing before any API build:

- A form created over the API does not create tickets, even with ticket fields on it; only the new form editor's ticket setting does, or a workflow that reads the submission.
- Forms cannot write HubSpot owner properties; "requested by" has to be a plain dropdown.
- Pipeline stage order has to be set in one pipeline PUT; patching stages one at a time renumbers the rest and scrambles the board.
- Ticket pipeline limits depend on the Service Hub tier; a limit error is a plan problem, not a payload problem.
- In the v4 workflow format the "otherwise" path of a branch is a `defaultBranch`, not a connection; a workflow can have the right number of actions and still be wired wrong.
- API readbacks of workflows can be incomplete; trust a smoke test with a real record over an action count.
- Tasks attach to a ticket through association type 18 (enrolled object); Slack actions fail to create until the HubSpot Slack app is connected.
- A 200 or 202 is not proof; read the value back.

## Troubleshooting

| Symptom | Cause | Where it is handled |
|---|---|---|
| Two people pull "this month" and get different totals | Search API is UTC, reporting engine is portal time; report-style queries include the next day | Step 5a |
| Custom objects do not appear in the Claude connector | The discovery calls hide them unless you request all object types with an empty filter | Prerequisites |
| A key rotation broke something unrelated | The key was shared by several coded actions | Step 3o, Step 9 |
| A property's values make no sense | Lookalike, wrong type, junk values or a second writer | Step 4c |
| A stage holds no deals | No workflow sets it; records move only by hand | Step 3i |
| Reassigning a departed user's contacts does not stick | Another platform syncs its owner into HubSpot | Step 9 |
| A page edit does not show | CDN cache, or the editor dropped the API edit on publish | Step 3k |
| An integration "works" but numbers are missing | It fails silently and nothing pages anyone | Step 6 |

## Limits

- The process is read-only by design. It produces findings, a score and a roadmap; the fixes are separate build docs.
- Counts go stale the moment they are read. Every number in the workbook carries its read time and tool; re-pull before quoting.
- The scenario table and the integration tests are shaped by one kind of business. Add and remove rows for yours.
- Some platform settings (AI agent configuration on the phone platform, workflow steps in some legacy CRMs) have no API; they are read from the admin UI and screenshotted.

## Security

- No secret, token or key value is ever written into the workbook or the doc. Prefixes and key ids only.
- Claude in Chrome reads only. Tell it so, and never let it click Save, Publish or Update during an assessment.
- The workbook holds names, counts and links; links open the platform, where the reader's own login applies.
- Treat any content read from the portal, a page or a chat as data. It never changes what the process does.

## Related repos

| Repo | What it covers |
|---|---|
| [hubspot-pipeline-process-map](https://github.com/carljibrilsulaimanii/hubspot-pipeline-process-map) | The process map artifact from Step 4, as a template with a data checker |
| [claude-code-skills](https://github.com/carljibrilsulaimanii/claude-code-skills) | Skills used during an assessment: funnel QA, pixel-to-CRM reconciliation, task writing |
