---
title: "mb-3-cs-b15-app1-data-flow-scope"
doc-id: "mb-7e50e4f7"
updated: "2026-11-07"
---
# Scope Card — mb-3-cs-b15-app1-data-flow

**intent:** Show the booking flow's three round trips and the payment paths including the D5 late-payment shadow, so the reader trusts that one transaction guards the slot and money never moves on the browser's word. The load-bearing claim: `confirm_booking` re-checks slot, resource and places inside the same transaction that confirms, and a late webhook either reinstates inside that discipline or refunds in full (§22 row 4; §14.8 D5).
**type:** sequence — the RT1→RT2→RT3 ordering and the webhook's later arrival are the meaning; vertical order carries the D5 shadow path. The config's file name stays `-data-flow`; the Archify guide check runs at authoring and the developer's subject takes priority if it disagrees.
**quality_profile:** standard
**node inventory (12):** customer-browser · booking-flow-form · get_booking_context (RT1) · get_available_slots (RT2) · confirm_booking-transaction (RT3) · gateway-payment-session · webhook-endpoint · webhook-log-row · process-webhook-task · payments-invoice-rows · d5-refund-path · booking-status
**main path:** customer-browser → booking-flow-form → get_booking_context → get_available_slots → confirm_booking-transaction → gateway-payment-session → webhook-endpoint → process-webhook-task → payments-invoice-rows
**deferred to cards:** intake forms; booking_flow_state caching; the confirmation email
**corridor plan:** participants ordered left to right — browser, form, server, gateway, webhook, task, tables — so the D5 shadow crosses at the empty band between task and tables
**layout budget source:** renderers/sequence/README.md (read at authoring)
**probe declaration:** expected corridors — the D5 refund path crosses at the band between process-webhook-task and payments-invoice-rows; abort if the shadow path collides with the main path's lifelines or an edge survives the repair order
**decisions (verbatim):** "A) Approve card 2 (recommended)" — D10, 2026-11-07
**status:** approved
