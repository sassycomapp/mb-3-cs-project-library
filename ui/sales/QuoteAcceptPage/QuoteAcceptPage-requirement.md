---
title: "QuoteAcceptPage-requirement"
status: draft
doc-id: "mb-a8a2f542"
updated: "2026-11-25"
---
# QuoteAcceptPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `sales.QuoteAcceptPage` |
| Purpose | The public quote page at `/quote/{token}`: the quote laid out like the invoice PDF, Accept (filled) and Decline (outlined), the unticked acceptance box, and the confirmation with the invoice's payment link (ticket 38, F-35; DESIGN.md Sales Design; UF-41) |
| Screen type and width | Public page — 900 px (the quote page; a single document card) |
| Layout | `layouts.BlankLayout` (the token page is a standalone public page) |
| Route | `/quote/{token}` — Anvil Routing (`quotes.accept_token`); the token page is intentionally public |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_quote_accept`) | The page's one content column | — | mobile | — |
| Quote card | `m3.Card` (`card_quote_document`, elevated) | The quote laid out like the invoice PDF: number, customer, dates, lines, totals | `elevated-card` | mobile | — |
| Status badge | `m3.Chip` (`chip_quote_state`, type `input`, removable=False) | The quote's state per the status badges | — | — | — |
| Acceptance box | `m3.Checkbox` (`cb_quote_accept`, never ticked in advance) + two `m3.Link` | "I accept the Terms and Conditions and have read the Privacy Policy", with both documents linked beside it; the confirm button stays disabled until ticked (DESIGN.md Terms Acceptance Box) | — | — | `consent.terms.checkbox` |
| Accept quote button | `m3.Button` (`btn_accept_quote`, filled) | Accepts the quote | `primary-button` | — | `quote.accept` |
| Decline button | `m3.Button` (`btn_decline_quote`, outlined) | Declines | `secondary-button` | — | `quote.decline` |
| Confirmation card | `m3.Card` (`card_quote_confirmation`, elevated, hidden) | After acceptance: a confirmation with the invoice's payment link | `elevated-card` | mobile | — |
| Declined card | `m3.Card` (`card_quote_declined`, outlined, hidden) | After decline: the business is notified | `surface-card` | mobile | — |
| Expired card | `m3.Card` (`card_quote_expired`, outlined, hidden) | An expired, declined or already-accepted quote's state | `surface-card` | mobile | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The quote | `quotes` | all the quote's columns; `accept_token`, `accepted_at`, `declined_at`, `invoice_id` |
| Lines | `quote_items` | the quote's lines |
| The invoice | `invoice` | created on acceptance |
| Consents | `consents` | the acceptance box's records with the contact |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `quote_service` token read (`server_sales/quote_service`) | The quote arrives; a used or expired token shows its state |
| Accept | `btn_accept_quote_click` | `accept_quote` (`server_sales/quote_service`; public token page) | The invoice is created from the quote in the same step and emailed with a payment link; a linked deal moves to `won`; the business is notified (UF-41 step 3; one invoice, however often it is asked — IN-06) |
| Decline | `btn_decline_quote_click` | `accept_quote` (decline path) | The business is notified and a linked deal stays open for follow-up (UF-41 step 4) |
| Accept the terms | `cb_quote_accept_change` | None | The Accept button enables; the consents are written with the contact |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Document icon (`mi:description`) |
| Heading | `quote.unavailable.title` |
| Body text | `quote.unavailable.body` — the token is used, expired or unknown |
| Primary action | None — the message asks the customer to contact the business |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

The token page is intentionally public.

## 7. Text

| Phrase key | English text |
|---|---|
| `quote.accept` | Accept quote |
| `quote.decline` | Decline |
| `quote.thanks.title` | Thank you — your quote is accepted! |
| `quote.thanks.body` | Your invoice is on its way, with a link to pay it. |
| `quote.declined.body` | Thanks for letting us know. The business will be in touch. |
| `quote.unavailable.title` | This quote isn't available |
| `quote.unavailable.body` | It may have been accepted already, declined, or expired. Contact the business for a new one. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-41 Customer accepts a quote | The whole flow: read, accept or decline, the invoice |
| UF-24 Customer portal | The quote also opens from My Quotes |
