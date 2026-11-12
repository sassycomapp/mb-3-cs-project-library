---
title: "GLOSSARY"
doc-id: "mb-614e0345"
updated: "2026-11-12"
---
# Mybizz Consulting Services

The domain glossary of Mybizz Consulting Services, shared by all its apps. It defines each term the business uses, once, so that every step, document and agent means the same thing by it. It is written at PDLF's Step 16 (domain modelling), in the format of Matt Pocock's `domain-modeling` skill, and is a glossary only: no implementation detail.

## Language

Every term is marked **common** (every Mybizz application shares it) or **trade** (this business's own trade: consulting and service delivery). *(shared)* names a term defined once in [[[default:specifications-global/spec-nomenclature]]](C:/dev/project-library-global/specifications-global/spec-nomenclature.md); it keeps that shared meaning here unchanged.

### The parties

**Client** *(common; shared)*:
The business or individual that subscribes to a Mybizz application and is given its own client instance. In Mybizz CS it is the business whose records, customers and money the application holds.
_Avoid_: using "client" for the person the client's business serves.

**Customer** *(common; shared)*:
A client's own customer, who uses the client's instance. In Mybizz CS: the person who books an appointment, orders delivered work, buys a package, accepts a quote or signs in to the customer portal.
_Avoid_: client, buyer, account.

**Contact** *(common)*:
One person a client's business knows, recorded once, whatever the person becomes later: a lead, a customer, or inactive.
_Avoid_: customer (for the record as a whole), account.

**Lead** *(common)*:
A prospective customer of a client's business, captured from a form, enquiry or email and created together with its contact in one step; it becomes a customer or is archived.
_Avoid_: prospect, enquiry.

**Provider** *(common)*:
A user of a client's business who delivers a service and keeps a calendar. An Owner or Manager who also delivers is a provider too.
_Avoid_: staff member (when the delivering person is meant), resource (for a person).

**Resource** *(trade)*:
A bookable room or piece of equipment of a client's business, reserved by a booking when the service requires one and freed when the booking ends or is cancelled.
_Avoid_: room, kit, asset.

**Company** *(common)*:
A business customer of a client's business, grouping that business's contacts.
_Avoid_: account, organisation.

**Deal** *(trade)*:
An opportunity a client's business is pursuing, tracked through its pipeline stages to won or lost, optionally carrying a quote.
_Avoid_: opportunity, sale.

**Roles** *(common; shared)*:
Owner, Manager, Admin, Staff and Customer — the same five roles in every application, as [[[default:specifications-global/spec-nomenclature]]](C:/dev/project-library-global/specifications-global/spec-nomenclature.md) fixes them.
_Avoid_: inventing a sixth role or renaming one per app.

**Mybizz** *(common)*:
The business that builds and sells the Mybizz applications. The developer, David, runs Mybizz.
_Avoid_: owner (for Mybizz or for the developer).

**The developer** *(common)*:
David, who runs Mybizz.
_Avoid_: owner (for Mybizz or for the developer).

**Owner** *(common; shared)*:
The role of the person responsible for a client instance: the client who signed up, once onboarded. Never Mybizz and never the developer.
_Avoid_: administrator, boss.

**User** *(common)*:
A person with a sign-in to a client instance, holding exactly one role: Owner, Manager, Admin, Staff or Customer. A customer with a portal sign-in is a user with the Customer role; a visitor is never a user; a client is a business, never a user; Mybizz and the developer are not users of a client instance.
_Avoid_: user where one role is meant — name the role.

**Team member** *(common)*:
A user with the role Owner, Manager, Admin or Staff: the client's own people.
_Avoid_: staff (for the whole team — Staff is one role).

**Visitor** *(common; shared)*:
Someone viewing a client's public website without signing in.
_Avoid_: guest (for a signed-in person), user.

**Prospect** *(common)*:
A business trying the demo apps or considering Mybizz, before it becomes a client.
_Avoid_: lead (a captured prospect inside a client's business), demo user.

**Demo prospect** *(common)*:
A prospect signed in to a demo app, whose account is removed at the nightly reset.
_Avoid_: test user, sample user.

**Package** *(trade)*:
A named bundle of sessions of one service that a client's business offers for sale. The concept and architecture documents keep the feature's name, Session packages (F-13).
_Avoid_: session credit, membership.

**Customer package** *(trade)*:
A customer's purchase of a package: the sessions bought, the sessions remaining, and its expiry and state.
_Avoid_: package balance, subscription.

**Session** *(trade)*:
One appointment's worth of a customer package: drawn down when a booking uses it, returned when the booking is cancelled inside the cancellation window.
_Avoid_: credit, appointment.

### The trade

**Service** *(trade)*:
One thing a client's business offers and publishes: either bookable time with a provider (pricing model duration) or delivered work bought per unit (pricing model unit).
_Avoid_: product, item, offering.

**Booking** *(trade)*:
An appointment — or one or more places in a group session — between a customer and a provider at a set time, carrying its meeting type, total price and payment state. A booking is never deleted; it ends in its status.
_Avoid_: appointment (for the record), reservation.

**Meeting type** *(trade)*:
How a booked appointment takes place: at the business, at the customer's location, by video, or by phone — chosen from what the service supports.
_Avoid_: mode, channel, delivery mode.

**Intake** *(trade)*:
The client-defined questions a customer answers when booking a service or ordering delivered work, defined once per service.
_Avoid_: metadata (except where the schema names the column), custom form.

**Intake answer** *(trade)*:
One customer's answers to a booking's intake, stored with the booking.
_Avoid_: intake form (the definitions are the form; the answers are the answers).

**Availability rule** *(trade)*:
A provider's — or, with no provider, the business's — weekly hours from which slots are computed.
_Avoid_: schedule, working hours.

**Availability exception** *(trade)*:
A blocked date or one-off change for a provider, or for the whole business when no provider is named.
_Avoid_: holiday, day off.

**Opening hours** *(trade)*:
The business's own availability rules — the hours the business is open.
_Avoid_: business hours (as a system concept), schedule.

**Slot** *(trade)*:
One bookable start time for a duration service with a given provider, offered by the availability calculation, taken by a booking and freed when the booking ends or is cancelled.
_Avoid_: time slot, opening, window.

**Group session** *(trade)*:
One scheduled time of a group service, with a limit on places and a count of places taken.
_Avoid_: class, event, workshop.

**Place** *(trade)*:
One customer's seat in a group session, limited by the session's place limit.
_Avoid_: seat, spot, ticket.

**Recurring series** *(trade)*:
A customer's repeating booking pattern — weekly, fortnightly or monthly — whose bookings carry the series with them.
_Avoid_: subscription (reserved for Mybizz's own billing), standing booking.

**Payment hold** *(trade)*:
The 15-minute reservation of a slot and any required resource while a booking awaits online payment; the emailed payment link is honoured until it expires.
_Avoid_: cart, reservation (as a noun for it), pending payment.

**Order** *(trade)*:
A customer's order of delivered work — a unit service, no slot — which is never deleted: it moves from ordered to in progress to delivered, or is cancelled.
_Avoid_: purchase, project, sale order.

**Brief** *(trade)*:
The customer's description and attachments of the work ordered.
_Avoid_: instructions, spec.

**Delivery** *(trade)*:
What the provider sends back for an order: a message and files, sent once the work is done.
_Avoid_: deliverable, handover.

**Time entry** *(trade)*:
Recorded work by a provider for a contact — optionally from a booking — at an hourly rate from the service, billable or not; a billable entry becomes an invoice line exactly once.
_Avoid_: timesheet, hours log.

### Money

**Invoice** *(trade)*:
A request for payment a client's business issues to a contact — for a booking, an order, a package, or recorded time — never deleted. Its lines make its subtotal, tax is fixed at issue, and the amount paid is the succeeded payments less refunds. Draft, sent, paid; sent or overdue may be cancelled; overdue is set nightly, with the reminder; the PDF is kept in the client's storage.
_Avoid_: bill, statement.

**Invoice line** *(trade)*:
One line of an invoice — description, quantity, unit price, amount — which may come from one time entry, marked invoiced in the same call that creates the line.
_Avoid_: row, charge.

**Quote** *(trade)*:
A priced offer a client's business sends to a contact or company, valid until a date and optionally part of a deal. Draft, sent, then accepted or declined; it expires nightly when past its valid-until date. The customer accepts it at a private web address, accepting the Terms and Conditions and the Privacy Policy there; acceptance creates the invoice — exactly one, however often it is asked.
_Avoid_: estimate, proposal (proposal is a deal stage).

**Quote line** *(trade)*:
One line of a quote — description, quantity, unit price, amount — optionally naming the service priced.
_Avoid_: row, charge.

**Payment** *(trade)*:
One record of money actually taken or received — by card gateway, PayPal, cash, bank transfer or other — for a booking, order, package or invoice; pending, succeeded or failed; carrying the gateway's reference and fee when the gateway took it; never deleted. Every amount is the server's, never the browser's.
_Avoid_: transaction (the gateway's own word), charge.

**Refund** *(trade)*:
A payment record of kind refund: it refunds one succeeded payment, through the original gateway where that payment was online, and never exceeds the payment. The late-payment path refunds in full when a reinstated slot is no longer free.
_Avoid_: reversal.

**Deposit** *(trade)*:
The part of a service's price paid at booking, with the rest paid on the day or invoiced.
_Avoid_: down payment, part payment.

**Payment option** *(trade)*:
What a service or order offers the customer for paying: pay in full, deposit, pay on the day, invoiced, or from a customer package.
_Avoid_: payment method (the method is card, PayPal, cash and the like), payment plan.

**Expense** *(common)*:
Money the client's business has spent: supplier, amount with its tax, receipt image kept in the client's storage; recorded or void, and a void expense is left out of every report and export; never deleted.
_Avoid_: cost, outgoing, receipt (the receipt is the image).

**Expense category** *(common)*:
One of the spend categories an expense belongs to, seeded with common ones and editable.
_Avoid_: cost centre, chart of accounts.

**Tax** *(common)*:
The client's business tax settings — a rate, a label such as VAT, GST or Sales Tax, and an optional registration number — fixed onto each invoice and quote at issue.
_Avoid_: VAT, GST as system terms (they are labels).

**Payment terms** *(trade)*:
How many days until an invoice is due — zero meaning due on receipt; a default for the business, overridable per service for invoiced bookings and per invoice.
_Avoid_: credit terms, due period.

### Relationships and outreach

**Contact event** *(common)*:
One recorded thing that happened with a contact — a booking, order, payment, email, enquiry, note, review, no-show, quote or deal — pointing at the row it came from.
_Avoid_: activity feed entry, history entry.

**Timeline** *(common)*:
A contact's history: its contact events in order.
_Avoid_: activity log (the instance's own audit trail), feed.

**Enquiry** *(common)*:
A message sent through the client's contact form — name, email, phone and a message of at most 1,000 characters; new, responded or archived; an automatic reply sent once. An enquiry is not a lead: it is the message; a lead is captured from a capture form.
_Avoid_: contact request, message thread.

**Segment** *(common)*:
A named set of contacts chosen by rules — prebuilt or custom — that campaigns send to; its contact count is recalculated nightly.
_Avoid_: list, group, tag (a tag is one rule inside it).

**Task** *(common)*:
A piece of work for a contact, assigned to a team member, with a due date; created by hand, by an automation, by voice, or by the system; done when completed.
_Avoid_: to-do, reminder (a reminder is an email the system sends).

**Automation rule** *(common)*:
When a trigger event happens and conditions are met, take actions: create a task, send an email, enrol the contact in a sequence, add a tag.
_Avoid_: workflow, trigger (the trigger is the rule's event).

**Automation run** *(common)*:
One execution of an automation rule for one triggering event — one run per rule and trigger — recorded as success, skipped or failed.
_Avoid_: job, execution.

**Campaign** *(common)*:
An email campaign of one of two kinds: a sequence (a series of emails sent over days to each contact in it) or a broadcast (one email to many); draft, scheduled, active, paused or completed; sent to a segment, or to every contact that has not opted out; opens, clicks, unsubscribes and revenue tracked.
_Avoid_: newsletter (a broadcast is the term), drip.

**Enrolment** *(common)*:
A contact's place in a sequence: enrolled, completed or paused, with the day it reached.
_Avoid_: subscription, membership, signup.

**Email address** *(common)*:
One address on the client's own domain, of two kinds: personal (one team member's own, seen by them) or desk (shared by the team members who belong to it).
_Avoid_: mailbox, sender.

**Desk** *(common)*:
A shared email address, such as help@ or accounts@, read and sent from by the team members who belong to it.
_Avoid_: shared inbox, role account.

**Team Inbox** *(common)*:
Where the team reads and replies: each member sees their own addresses and their desks, and no one else's.
_Avoid_: inbox (alone, for the Team Inbox), shared inbox.

**Email message** *(common)*:
One email the instance holds — received, sent or drafted — with folders inbox, drafts, sent, archive, spam and trash; Trash is deleted for good after 30 days; a body older than 90 days moves to the client's storage with its row kept as an index.
_Avoid_: email (when the record is meant), thread (a conversation groups messages; the record is the message).

**Suppression** *(common)*:
An email address blocked from further mail, in scope: a hard bounce or a complaint suppresses every email; an unsubscribe suppresses marketing email only.
_Avoid_: blocklist, opt-out (the opt-out is the act; the suppression is the state).

**Email template** *(common)*:
A ready-made email of the client's business — a confirmation, reminder, invoice, receipt and the rest — held one per language.
_Avoid_: snippet, draft, campaign email (a campaign email is written in its campaign).

**Lead capture** *(common)*:
A form on a landing page that captures a lead — the lead and its contact created together in one step — of three kinds: a lead form, an event registration, or a video sales letter page.
_Avoid_: signup form, opt-in.

**Event registration** *(common)*:
A capture page where a visitor registers for a group session; the schema keeps the name event_registration for its kind.
_Avoid_: event (an event is out of scope), booking.

**Review** *(trade)*:
A customer's star rating and comment after an appointment, asked for by email: requested, awaiting approval, then approved or declined; only an approved review shows publicly.
_Avoid_: rating (alone, for the whole review), feedback (support feedback is a different thing).

**Testimonial** *(common)*:
A recommendation the client types in itself, shown on the public website when the client approves it.
_Avoid_: quote, review.

### Content and legal

**Public website** *(common)*:
Everything a visitor sees at the client's own address without signing in.
_Avoid_: site, front end.

**Page** *(common)*:
One editable page of the public website — home, about, contact, services, a service's detail, and the legal pages.
_Avoid_: post (a blog post is not a page), site.

**Edit mode** *(common)*:
How the Owner and Manager edit the public website's pages from inside the application; every save is recorded in the audit trail.
_Avoid_: editor, draft mode.

**Homepage template** *(common)*:
One of the four ready-made shapes of the public home page: classic, services, booking or minimalist.
_Avoid_: theme, layout.

**Homepage section** *(common)*:
One arranged block of the home page — hero, services, booking, video and the rest.
_Avoid_: block, widget.

**Blog post** *(common)*:
An article the client publishes, filed in one or more blog categories.
_Avoid_: article, news item.

**Blog category** *(common)*:
A filing category of the blog.
_Avoid_: tag, topic.

**Comment** *(common)*:
A visitor's remark on a blog post, shown only after the client approves it, where comments are switched on.
_Avoid_: review, reply.

**Landing page** *(trade)*:
A standalone page built for a campaign — outside the public website's own pages — whose job is to capture a lead.
_Avoid_: page (alone, for a public page), funnel (the funnel is the path, not the page).

**Media file** *(common)*:
A file the instance holds in the client's media library; the same home holds an order's brief and delivery files.
_Avoid_: upload, attachment (an email attachment is a message part).

**Palette** *(common)*:
The colour scheme the client picks from the named palettes for its whole application.
_Avoid_: theme, brand.

**Clause Builder** *(common)*:
Where the client builds the three legal documents from pre-written clauses and publishes them.
_Avoid_: contract builder, template editor.

**Domain** *(common)*:
The client's own domain name, which its public website and its email use; the Settings tab of that name shows its website and email records.
_Avoid_: email domain, website domain.

**Legal document** *(common)*:
One of the three — the Privacy Policy, the Terms and Conditions, the Cookie Policy — built and published in one system; publishing raises its version and keeps a copy of the published text in the client's storage; the instance does not go live until all three are published.
_Avoid_: policy (alone — the Cookie Policy is one of them), terms (alone), Terms of Service (the client's document is the Terms and Conditions).

**Legal acknowledgement** *(common)*:
The Owner's recorded, one-time acceptance of responsibility for the legal documents' content.
_Avoid_: consent (that is the customer's or visitor's choice), agreement.

**Client service agreement** *(common)*:
Mybizz's own terms with its clients — the agreement between Mybizz and the subscribing business. It is never the client's own document.
_Avoid_: Terms of Service (never call the client's document this).

**Consent record** *(common)*:
One record of a person's choice or acceptance: a cookie choice (the visitor's, anonymous — a consent number, the choice, the version, the time) or the acceptance of the Terms and Conditions and Privacy Policy (carrying the contact) made at a booking, an order, a package purchase, an online payment, portal sign-up or quote acceptance. Cookie records are kept two years, acceptance records seven.
_Avoid_: consent log, GDPR record.

**Cookie banner** *(common)*:
The notice that asks every first visitor before any embedded map or video loads — Accept all, Refuse all, More info — and loads placeholders when they refuse.
_Avoid_: cookie popup, GDPR banner.

**Consent cookie** *(common)*:
The first-party necessary cookie that remembers a visitor's choice so the banner does not ask again until a new version is published.
_Avoid_: tracking cookie (it is a necessary first-party cookie).

### Mybizz's billing

**Plan** *(common)*:
Which way a client pays Mybizz for its client instance: the monthly plan or the annual plan; chosen at registration and changed through Paddle's customer portal.
_Avoid_: tier, package.

**Subscription** *(common)*:
The client's agreement with Mybizz, held in Paddle, on one plan — monthly or annual; its status is the subscription status.
_Avoid_: membership, contract.

**Monthly plan** *(common)*:
One month at a time, billed in advance, no contract; the client can leave at the end of the paid month.
_Avoid_: pay-as-you-go, rolling plan.

**Annual plan** *(common)*:
Twelve months for the price of ten, billed in advance once a year; the plan the referral credit rewards and applies to.
_Avoid_: yearly plan, annual subscription.

**Renewal** *(common)*:
The start of the client's next paid month or year, when the plan is billed again — the date the Plan and billing page shows.
_Avoid_: anniversary, rebill, expiry.

**Assisted setup** *(common)*:
The paid, one-off service where Mybizz sets a client's business up for it, at one month's subscription price, paid once.
_Avoid_: setup fee, onboarding service, professional services.

**Merchant of Record** *(common)*:
Paddle, in its role for Mybizz: it bills each client, takes the payment, issues the tax invoice and is the seller of record — so Mybizz never takes a client's card details.
_Avoid_: payment processor, gateway (the gateways take the clients' customers' card payments), billing provider.

**Referral** *(common)*:
The fact that a new client registered through a referring annual client's link or code, recorded by the management app as one row in one of four states: pending — registered, no annual payment yet, or on the monthly plan; confirmed — the referred client's first annual payment has completed, which makes one credit; used — the credit has been taken off a renewal of the referring client; void — the referred client left Mybizz while pending. A refund of the first annual payment returns the referral to pending; a client that moves from the monthly plan to the annual plan confirms its referral at the first annual payment.
_Avoid_: affiliate, invite, code (alone, for the whole referral).

**Referral credit** *(common)*:
The reward: one month off the referring client's next annual renewal for each confirmed referral, at most five per renewal, never paid in cash; set as one flat discount on the referring client's subscription, in the subscription's currency, for the next billing period only.
_Avoid_: session credit (a package session), discount (alone), cashback.

**Reserve** *(common)*:
The confirmed credits beyond the five a renewal can take — kept for the next renewal, replacing a confirmed credit that a refund returns to pending, and carried forward.
_Avoid_: bank, balance (a customer package has a balance of sessions).

**Referral kit** *(common)*:
The ready-made sharing material an annual client uses: its referral link; posts for LinkedIn, Facebook, X and WhatsApp; an email banner added to its campaign emails (sequences and broadcasts); and a badge and a pop-up for its public website — each switched on and off by the client, the badge and pop-up off until switched on.
_Avoid_: affiliate programme, referral programme material.

**Referral link** *(common)*:
The client's own address, at Mybizz's domain, which opens Mybizz's registration with the code filled in.
_Avoid_: affiliate link, invite link.

**Referral code** *(common)*:
The code itself, which a new client may also type at registration. The referring client sees the referred business's name and whether it has subscribed.
_Avoid_: promo code, voucher.

**Plan summary** *(common)*:
The one-row picture of the client's plan, its next renewal and its referrals, written by the management app daily and after every change, shown on the Owner's Plan and billing page. Its amounts are display text in the subscription's currency, exactly as Paddle states them — they are not money records of the client's business.
_Avoid_: billing summary, invoice (the mock-up is a preview, not an invoice).

**Mock-up invoice** *(common)*:
The preview on the Plan and billing page of the next annual renewal: twelve months at the plan price, the annual saving of two months, one month off for each confirmed referral (at most five), then the total as Paddle previews it, before tax — and below the total, each pending referral by the referred business's name, the confirmed referrals in reserve, and the referral link.
_Avoid_: draft invoice (a draft invoice is a real one), sample invoice.

**Plan and billing** *(common)*:
The Owner-only Settings tab: the client's plan, its renewal, the mock-up invoice, and the referral kit.
_Avoid_: billing page, subscription page.

### The platform

**Blank client template** *(common)*:
The client app every client instance runs — the schema, one startup module, the schedule, with the master template as its dependency — and which never holds a form, a server module or a secret of its own.
_Avoid_: empty app, client shell.

**Release image** *(common)*:
The built image of the master template and the blank client template at one release, which every client instance runs.
_Avoid_: build, deployment.

**Release** *(common)*:
One approved, tagged release of the development workspace, pushed into the master template and built into the release image; it reaches every client instance through the management app's Release action, and after the first release it only ever adds tables and nullable columns.
_Avoid_: deploy.

**Provisioning** *(common)*:
The management app's New client action: it creates the client's container and database from the release image, writes the client's keys and token into the Vault, loads the sample data for the client's trade, and sets up the client's email sending.
_Avoid_: onboarding (onboarding is the Owner's wizard), setup (assisted setup is a paid service).

**Sample data** *(common)*:
The realistic demonstration records loaded at provisioning for the client's trade, every one recorded so it can be removed exactly; sample records are never emailed, never paid and never exported.
_Avoid_: test data, demo data (the demo apps have their own), dummy.

**Clear sample data** *(common)*:
The Owner's action that removes every recorded sample row, empties the links from real rows, and recomputes the summaries.
_Avoid_: reset (the demo apps reset; this clears), delete.

**Demo app** *(common)*:
One of the two public applications where a prospect tries the whole product without paying — there is no free trial.
_Avoid_: trial (there is none), sandbox.

**Demo mode** *(common)*:
The flag inside an app that makes it a demo: email is delivered only to the signed-in demo prospect, payments run on the gateways' test keys, and the nightly reset removes the prospect's account and restores the sample state.
_Avoid_: test mode (that is the gateways'), staging.

**Secret** *(common)*:
One credential a client's instance keeps in the Vault, encrypted, masked on every read, never logged and never round-tripped — the gateway keys, the email keys, the storage keys, the management token, the Vault's own authenticator code.
_Avoid_: key (alone), password, credential (as the stored item).

**Audit log** *(common)*:
The instance's record of who did what, when, and what changed — before and after — for every significant action; written by the server; no role can change it; the last 30 days stay in the instance and older rows move to the client's storage nightly.
_Avoid_: activity log (the schema's name is the implementation), event log.

**Rate limit** *(common)*:
The per-identifier counter that refuses too-frequent requests — ten a minute from a signed-out address, a hundred a minute for a signed-in user — and also locks sign-in after repeated failures and the Vault after five wrong codes.
_Avoid_: throttle, quota.

**Heartbeat** *(common)*:
The report every client instance sends the management app every 15 minutes — status, release, schema fingerprint, task status, error count, queued retries — carrying no personal data, and degrading to every 30 minutes rather than skipping.
_Avoid_: ping, health check (the health endpoint answers one).

**Usage report** *(common)*:
The daily report of what a client used: memory, storage, email volume, heavy jobs, last sign-in and last booking — the numbers the at-risk alerts and the cost-per-client figures come from.
_Avoid_: metrics, telemetry.

**Management endpoint** *(common)*:
One of the seven Bearer-token endpoints the management app calls on a client instance: health, status, suspend, unsuspend, push-notification, push-update-alert and plan-summary. A wrong token is refused with nothing changed.
_Avoid_: admin API, internal API.

**Webhook** *(common)*:
An event a gateway or the email service sends the instance: verified by signature before anything is read, refused when its event has been seen before, stored once with its payload in the client's storage, answered quickly, and applied once by a background task; one that cannot be applied is reconciled by the five-minute task.
_Avoid_: callback, notification (a notification is for a user).

**Client storage** *(common)*:
The client's own bucket in its own cloud account, holding everything cold: backups, history, archived audit rows, webhook payloads, email bodies and attachments, expense receipts, published legal documents, consent archives and exports. The instance keeps only index rows; the client takes it all when it leaves.
_Avoid_: backup (backups are one folder of it), cloud storage (alone).

**Retention** *(common)*:
How long each kind of record is kept before it moves to the client's storage or is deleted — audit rows 30 days in the instance, email bodies 90 days, consent records per the legal documents, email Trash 30 days.
_Avoid_: expiry (a customer package expires; retention is the record-keeping rule), cleanup.

**Summary** *(common)*:
A precomputed table the dashboards read — the day's figures, each service's performance, each provider's performance, each campaign's metrics — written by scheduled tasks keyed by period, so a task run twice leaves exactly one row per period; dashboards never compute on load.
_Avoid_: report (a report is what a person reads), cache.

**Notification** *(common)*:
A message for one user, shown in the application's bell and feed, written by the client's own server code or pushed by the management app, sent once for each event, read or expired.
_Avoid_: alert (an at-risk alert is a management-app warning), message (an email message is a different thing), push (the push is how the platform sends one).

**Notification engine** *(common)*:
The part that sends notifications — honouring each user's preferences and quiet hours, and logging every delivery attempt.
_Avoid_: notifier, push service.

**Notification preferences** *(common)*:
Each user's per-channel, per-kind choices — enabled or not, and at most so many a day.
_Avoid_: settings (alone), opt-in.

**Quiet hours** *(common)*:
A user's window in which notifications are held until it ends.
_Avoid_: do-not-disturb, schedule.

**Delivery log** *(common)*:
The record of every notification attempt: channel, status, tries, error.
_Avoid_: email log (that is sent email's own record), audit.

**At-risk alert** *(common)*:
The warning the management app raises when a client's usage or inactivity suggests it may leave — so Mybizz can reach the client before it does.
_Avoid_: churn alert, red flag.
