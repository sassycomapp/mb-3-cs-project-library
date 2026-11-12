---
title: "mb-3-cs-architecture-explainer"
doc-id: "mb-dc62ff9a"
updated: "2026-11-12"
---
# Mybizz CS — Architecture Explainer

This document explains, in plain language, how Mybizz CS is built and why it is built that way. It is written for the developer, not for the build agent. The build agent works from `mb-3-cs-architecture-reference.md`, which holds every technical fact; where the two documents ever differ, the reference is correct.

Every feature carries the same number and name as in `mb-3-cs-concept.md` and in the reference (F-01 to F-37).

---

## 1. The Big Picture

Mybizz CS is one application, written once, that runs separately for every client. Each client gets their own copy of the application with their own database, on their own web address. No client can ever see another client's data, because each client's data lives in a different database altogether — not in a shared database behind a filter.

The difficulty with "one copy per client" is keeping 100 copies up to date. Mybizz CS solves this with five apps that each do one job.

---

## 2. The Five Apps

**App 1 — the workshop.** All building happens here. App 1 holds every form, every piece of server code and test data. When a version is proven, it is tagged as a release. No client ever runs App 1.

**App 2 — the master (`master_template`).** App 2 receives each proven release from App 1. Every client's app runs its code and screens. App 2 holds no client data.

**App 3 — the client app (`blank_client_template`).** App 3 is an empty client app: the database structure with no data, one small startup module, the scheduled jobs, and a link to App 2. App 3 and App 2 are packed together into one release image, which every client's app runs. The image builds each new client's tables from App 3's database structure.

**App 4 — the client apps.** One per client: a container running the release image, with the client's own database. A client app holds the client's data and nothing else; all code and screens come from App 2. The client uses the app but never sees its code, its container or its database.

**App 5 — the management app.** Mybizz's own control room: registering clients, setting up new client apps with their storage, email and domain, watching usage and health, billing, handling support tickets and feedback, and passing AI requests to the AI services. App 5 exists before the first paying client, because Mybizz cannot run the business without it.

**Why App 3 must stay empty.** If anyone adds a screen or server code to App 3, it quietly overrides App 2 for every client, and later fixes to that screen or code stop reaching clients. So the rule is absolute: App 3 holds the database structure, the startup module and the scheduled jobs only, and the image build stops if it holds anything more. Any change for one client is built in App 1 and released through App 2 to everyone. App 3 carries a README stating this rule.

---

## 3. Where the Apps Live

Apps 1, 2, 3 and 5 live on Mybizz's own Anvil Business account.

Each client app runs on DigitalOcean, in a container of its own with its own database: the free Anvil App Server runs the release image, and DigitalOcean looks after the servers, updates, failover and database backups. A client therefore has its own memory and its own database, and no client's work can slow another's. Mybizz pays for the hosting, so the client is Mybizz's client and never has to deal with Anvil or DigitalOcean.

Heavy work (imports, backups, AI, sending campaigns) runs in the background, so the client's pages stay fast, and Mybizz watches each client's usage from App 5.

Each client app runs on the client's own domain. The client owns the domain, and every email address of the client is on it.

---

## 4. How a New Client Is Set Up

1. The client registers and opens a free Cloudflare account, which holds the client's storage and the client's domain settings, and invites Mybizz into it. App 5 records the client and issues the client reference number.
2. App 5 creates the client's database and starts the client's container from the current release image, which builds the client's tables.
3. App 5 sets the client app's secret key and gives the client app a private token for talking to App 5.
4. App 5 creates the client's storage bucket and the client's email identity, and loads the sample data.
5. The client works through the onboarding wizard inside their own app. The wizard's domain step points the client's domain at the client's app and at the email service, so the website and every email address work at once.

---

## 5. How a Change Reaches Every Client

1. An agent builds the change in the local copy of App 1; screen layout work in the Anvil designer always comes first and is pulled into the local copy before code is written.
2. The change is tested and released from App 1.
3. The developer pushes the release into App 2, and the release's database changes into App 3.
4. App 2 and App 3 are packed into the new release image, which is checked and tested, and run first on a test client app.
5. App 5 moves every client app to the new release image. Each client app adds the release's new tables and columns to its own database as it starts.

A release only ever adds tables and columns; it never removes or renames one, so no client's data is lost and a release can be rolled back.

---

## 6. Data, Backups and History (F-29)

The client app's database holds what the business is working with now: current bookings, contacts, invoices, recent emails and their current status. Everything that is history — backups, the messages payment and email services send back, older audit records, emails older than 90 days, attachments and receipts — goes to the client's own storage bucket on Cloudflare R2, each kind in its own folder.

Keeping history out of the app does three things. It keeps each client well inside the account's limits. It means each client's history is held in storage that belongs to that client. And it means a client can leave with all of their data at any time.

The last 30 days of the audit trail stay in the app, so the owner can check recent activity quickly. Anything in the bucket can be brought back into the app, either to restore it or simply to look at it.

Each backup in the bucket is locked for as long as it is kept, so nobody, not even someone who has stolen Mybizz's keys, can delete or scramble it. App 5 checks every day that each client's backup arrived, and once a month a backup is restored on the test client app to prove it works.

---

## 7. Security (F-01, F-02, F-30)

**Roles.** Every user has one of five roles: Owner, Manager, Admin, Staff or Customer. The screens hide what a role cannot use, but the real protection is on the server: every server function checks the role before it does anything, so a user cannot get around a screen to reach data.

**The Vault.** Every key the client's app needs — payment, email service, storage — is encrypted and kept in the Vault. Only the Owner can open the Vault, only with a code from an authenticator app every time, and the Owner is emailed every time it is opened. One secret, the encryption key, sits in Anvil's own secret store; everything else is encrypted with it.

**Card data.** Card details never pass through the client's app. Stripe and PayPal handle them, which keeps the app clear of the heaviest card-security duties.

**Compliance.** The design follows the UK GDPR, each client's own privacy law and PCI DSS (card payments). Mybizz never collects the client's customers' details for itself.

---

## 8. How the Code Is Organised

The code is split into packages by feature — bookings, services, CRM, blog, invoices and so on — with matching server packages. A developer looking for booking code looks in the bookings packages. Existing package names never change while the app is being built, so nothing breaks; new features get new packages.

Every screen follows the same pattern: the screen shows things, client modules handle screen logic, server modules do the real work and hold all money and date calculations, and pure logic modules hold rules that can be tested on their own.

---

## 9. Navigation and Layout

Staff use the admin area, with a side menu grouped the way a business owner thinks: the Inbox, Sales and Operations, Customers and Marketing, Content and Website, Finance and Reports, then Settings, the Vault and Help. Customers use the customer portal, with a short menu of their own bookings, packages, quotes, invoices, reviews and account.

The public website uses proper web addresses for every page, so pages can be found by search engines and shared as links, with a proper preview of each page when it is shared. The admin area does not need public addresses, so it uses simpler screen switching, which is faster.

When anyone opens the app, the app checks who they are before showing anything: staff go to the dashboard, customers go to their portal, and everyone else sees the sign-in page.

Step-by-step processes, such as onboarding and booking, show their steps in a side panel and save everything to the server in one go at the end, which keeps them fast and avoids half-finished records.

---

## 10. The Features

### The client's day-to-day business

**F-01 Accounts, roles and access.** Sign-in, password reset and lockout after repeated failed attempts protect accounts. The five roles decide what each person can see and do, and the server enforces them. Every significant action is written to the audit trail.

**F-02 The Vault.** One locked place for every key, so keys are never scattered across settings screens (section 7).

**F-03 Registration, onboarding and setup.** Registration happens through Mybizz, which creates the client's reference number and their app (section 4). Inside the app, an onboarding wizard walks the owner through the essentials in a natural order — which language, who you are, how you look, which currency, the legal acknowledgement, how you get paid, connecting your domain, bringing in your existing contacts — with a progress bar and a short help video on every step. Each step is saved, so the owner can stop and come back. The contact import reads the files that HubSpot, Mailchimp, Acuity and other tools export, and matches their columns for the owner. A paid done-for-you setup is available.

**F-04 Dashboard and analytics.** The dashboard shows revenue, bookings and customers against the previous period. The figures are worked out in the background and stored ready, so the dashboard opens instantly instead of calculating on every visit. Deeper analytics break revenue down by service and provider, show which marketing earns money, export to PDF and CSV, and can be emailed on a schedule.

**F-05 Settings Hub.** One place for every setting, in tabs. Each role sees only the tabs it is allowed to change. Feature switches let a business hide what it does not use, such as marketing or the blog, so the menu stays clean.

### The client's public presence

**F-06 Public website.** Every client gets a complete website — home, about, contact, services, blog and legal pages — with no code. An Owner or Manager edits a page on the page itself: Edit makes each text, image and video editable where it sits, the AI helper writes or rewrites any text, and Save publishes at once. The contact form feeds the CRM directly. The site runs on the client's own domain and works on every screen size.

**F-07 Homepage templates.** Several homepage layouts are built from the same set of sections; a layout simply decides which sections appear and in what order. Sections that have nothing to show, such as video when no video is set, hide themselves.

**F-08 Landing pages.** Focused single-purpose pages, with no menu to distract the visitor, for capturing leads, registering for events, or a video sales pitch. Each counts its own visits and conversions. A landing page is set up in its editor and its content is edited on the page itself, as on the website.

**F-09 Visual and media presentation.** A gentle parallax effect on the homepage, rotating showcases, and video from the common video sites, all handled by shared components so they behave the same everywhere.

### Services, bookings and billing

**F-10 Service catalogue.** A service is priced either by time (a 60-minute session) or by deliverable (a logo). Time-priced services create calendar slots; deliverable-priced services do not. Each service also says how it is delivered: at the business, at the customer's place, online live, as delivered work, or to a group.

**F-11 Bookings and scheduling.** The booking flow runs as one continuous sequence, from choosing a service to paying, without leaving the page. Behind it, the app works out real availability from business hours, each provider's schedule, blocked dates, travel time for visits to customers, places left in group sessions, and whether any needed room or equipment is free. The final check for a clash happens on the server at the moment of booking, so two customers can never take the same slot. Each service decides how the customer pays: in full or a deposit at booking, on the day, or by invoice with payment terms. If an online payment fails, the slot is held for 15 minutes while the customer is sent a payment link, so a slot is never lost to a card problem and never blocked for long. Reminders go out automatically, with the video link where needed.

**F-12 Orders for delivered work.** Work with no appointment, such as a logo or a report, is ordered and paid online, then tracked from ordered to in progress to delivered, with the customer told at each step.

**F-13 Session packages.** A customer can buy a bundle of sessions up front. Each booking uses one session from the bundle, and both the customer and the business can see what is left.

**F-14 Time tracking.** Providers record time worked for a customer — by hand or with a start and stop timer — with the rate and whether it is billable. Billable hours turn into invoice lines, and the same step marks them as invoiced, so time can never be billed twice. Reports show hours and value by provider, customer and service.

**F-15 Invoicing and payments.** Invoices are created automatically when a booking completes or a quote is accepted, and also from time entries, orders, packages or by hand. Each invoice carries payment terms, such as 30 days, and its due date follows from them; an invoice that passes its due date becomes overdue and the customer is reminded. Each invoice moves through draft, sent, paid, overdue and cancelled. One payment service sits behind Stripe and PayPal, so the rest of the app never needs to know which one a customer uses, and a client can switch without redoing services or bookings. PayPal's own buttons take the customer's approval, while the server creates and captures the payment, so the amount can never be changed in the browser. Each payment's gateway fee is recorded, so the accounts show what the business really received.

**F-16 Dual currency.** The business keeps its books in one currency, fixed once trading starts so the records stay consistent. It can show prices to overseas customers in another currency, while every sum behind the scenes stays in the home currency.

**F-28 Accounts.** The app already holds every invoice, payment and refund, so adding expenses, each with a photo of its receipt, makes it a complete record of the business's money. Reports show profit and loss by month, what is owed, the tax collected and paid, and a statement per customer. One click exports everything in the formats Wave, Xero and QuickBooks import, so the client's accountant can use any system, and the client pays for no accounting subscription.

### Customers and marketing

**F-17 CRM and contacts.** Every person who deals with the business has one record with their full history, including every email, created automatically from bookings, forms and emails. A contact can belong to a company. Ready-made groupings and a builder for custom groupings let the business target the right people. The whole list can be imported and exported.

**F-18 Email.** Every email of the business, in and out, travels through one email service, Amazon SES, paid by Mybizz. The client creates as many addresses on the client's domain as the business needs — a personal address for each person, and desk addresses such as help@ or accounts@ shared by a team — and every email to them arrives in the Team Inbox inside the app, where each person sees their own address and their desks, and works it like a full mail program. Emails from known contacts appear on their timelines. The same service sends the business's confirmations, reminders and invoices, and its newsletters and campaigns, all from the client's own domain and signed so that they reach the inbox. Using one service means one set of domain settings, one place for bounces and unsubscribes, and no email account or bill for the client. Detail: `spec-team-inbox.md`.

**F-19 Lead capture and marketing funnel.** A blog post leads to a landing page, which leads to a booking. A form on the landing page creates the lead and the contact together, so nothing needs converting by hand, and every step records where the person came from.

**F-20 Tasks and automation.** Tasks keep follow-ups from being forgotten. Some are created by hand, some automatically by events, and some by voice. Automation rules let a business say "when this happens, and these conditions are met, do this" — create a task, send an email, start a sequence.

**F-21 Blog and content.** A full blog with three post layouts, search-engine settings per post, categories, view counts and moderated comments. New posts are written in the blog editor; published posts are fixed on the page itself. A Share button sends each published post to Facebook, LinkedIn, WhatsApp or Instagram through each platform's own share window, with an AI-written caption and a full preview, so Mybizz never holds anyone's social media password. Detail: `spec-sm-syndication.md`.

**F-22 Clause Builder.** Legal pages are built from ready-written clauses that the business edits, arranges and publishes, instead of copying text from the internet.

**F-23 Reviews and testimonials.** After an appointment the customer is invited to leave a rating and comment. Nothing appears publicly until the business approves it, and approved reviews become the testimonials on the homepage. A new client can also type in testimonials it already has, so the homepage is never empty while reviews build up.

**F-24 Customer portal.** Customers sign in to see and manage their own bookings, packages, quotes, invoices, reviews and details. Every request is limited on the server to that customer's own records. Customers contact the business through the contact form; there is no separate ticket system for them.

**F-35 Sales pipeline, companies and quotes.** A team that sells needs to see every opportunity. A deal follows a sale from lead to won or lost, with its value and its owner, so each salesperson works the deals assigned to the salesperson. A company groups the contacts of one business customer. A quote is priced like an invoice, sent by email and accepted online by the customer, and the accepted quote becomes the invoice, so nothing is typed twice.

### Help, AI and voice

**F-25 Ease of use, in-app help and language.** Screens follow the order in which a business owner thinks. Each feature's tooltip opens a short help video in a pop-up, so a business owner learns by watching, exactly where the question arises; tooltips can be switched off. The client chooses the app's language — English, French, German, Spanish or Italian — and screens, emails and videos follow it. Every piece of screen text, every email and every document is looked up by a short key from one phrase file per language instead of being typed into the screens, which is what makes a new language possible without rebuilding a screen. AI translates each key from English with a fixed glossary of product terms, and a language is offered only when every key is translated.

**F-26 AI assistance.** One AI helper is used everywhere instead of separate AI tools on each page. The helper opens in a floating window over the current page and picks the right assist for that page. Because every assist runs through the same helper and the same server path, the first assist is the hard work and each further assist is mostly a new prompt. AI requests pass through App 5, so Mybizz's AI keys never sit in client apps and Mybizz can see each client's AI use. If the AI fails, the page still works and says so. The assists that need a history of data show their worth from the first day on the sample data (section 11).

**F-27 WhatsUp voice.** The owner speaks into their phone's browser; the speech is turned into text on the server and handled by the same AI path as typed requests; the answer is shown and spoken back. This gives voice input to every assist, voice task creation, spoken answers to everyday questions, a short spoken morning briefing, follow-up questions in one conversation, filling in a whole form by speaking it, and business questions that need analysis across the data. For those analysis questions the app works out the figures itself and the AI only puts them into words, with people's names replaced by labels, so no customer name reaches the AI. WhatsUp is a home screen shortcut, not an app, and uses no WhatsApp, so there is no third party to pay or depend on.

### Data, security and design

**F-29 Data, backups and history.** Section 6.

**F-30 Security and compliance.** Section 7.

**F-31 Design system.** Every screen is built on the Anvil Material 3 theme with a small set of refinements, using named colour tokens instead of fixed colours. Because colours are tokens, a client can switch between eight palettes and the whole app changes at once; and when a user's device is in dark mode, the app switches to the palette's dark colours by itself, on every screen. The admin area is compact and quick; the public website is warmer and more spacious — the same design system, tuned for two jobs.

### Mybizz running the business

**F-32 Help, support tickets and feedback.** The Help button in every client app sends a problem or a suggestion straight to App 5, where the founder is notified and replies. A problem that stops a client's business is acknowledged within hours, and the fix reaches every client through App 2. The reply appears back in the client's Help view, so the conversation stays in one place.

**F-33 Hosting.** Section 3.

**F-34 Mybizz management app.** Section 2. App 5 is where Mybizz runs Mybizz CS as a business: clients, setup, usage, health, billing, updates, support, and the AI gateway. Each client app regularly reports simple signs of life — when staff last signed in, when the last booking was made — and App 5 raises an alert when a client goes quiet or a ticket waits too long. That gives the founder the chance to step in before a client leaves.

App 5 also looks after the shared resources without the founder watching. When a client app is running short of memory, App 5 gives it more; when the business reaches 60 clients, App 5 enlarges the shared database. Every client has fair-use limits on the shared database, email and AI, set far above heavy use, so one busy or misused client can never slow down or cost the others. App 5 warns the client before a limit, acts at the limit, and tells the founder in one daily summary, with an immediate message only when a decision is needed.

**F-36 Demo apps.** A prospective client tries Mybizz CS before paying, in two apps on Mybizz's own account: one running a business full of sample data, one empty for trying setup. Both reset every night, so a visitor can change anything. Emails really go out, but only to the visitor's own address, and payments run in the gateways' test modes, so the visitor sees everything a customer would and no real money moves.

**F-37 Plan, billing and referrals.** The Owner sees the business's own subscription in Settings: the plan, the renewal date and a mock-up of the next annual invoice. App 5 sends these figures to the client's app, because only App 5 talks to Paddle, and the total on the mock-up is Paddle's own preview of the renewal. On the annual plan, every business that joins on the annual plan through the client's referral link takes one month off the client's next renewal, up to 5 months, so the client can see each referral turn from pending to confirmed on its own invoice. Mybizz supplies a referral kit (posts, an email banner, a website badge and a pop-up), so satisfied clients can spread the word in a few clicks. Each kit item says the client gains a discount, because advertising rules require it, and the website badge and pop-up stay off until the client switches them on, because the client's website belongs to the client.

---

## 11. Sample Data

A new client's app arrives with three months of sample data for a business in the client's own trade, so every report, assist and voice question shows what it does on the first day, instead of an empty screen. Every sample row is recorded in one list, which is how the app tells sample data from the client's own: a banner says sample data is present, sample records never receive an email or a payment, and one button lets the Owner clear the sample data while everything the client created stays.

---

## 12. Working Within Anvil

Anvil shapes some decisions:

- Code in the browser cannot use all of Python, so all money and date work happens on the server.
- Every call from a screen to the server is a trip over the internet, so screens are designed to make as few calls as possible.
- Anvil has no live push to the browser, so screens that need fresh information check for it on a timer.
- Large tables are always searched, never read in full, so pages stay fast as data grows.
