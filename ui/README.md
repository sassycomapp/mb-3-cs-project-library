---
title: "README"
doc-id: "mb-7d2ea785"
updated: "2026-10-30"
---
# ui

The wireframes and screens of App 1. Every intended user interface has a wireframe, created from the application's defined architecture. A folder per Anvil client package, named exactly as the package is named, holds a folder per form, named exactly as the form is named in Anvil: `ui\{package}\{FormName}\`.

Every form folder holds the same six files, modelled on `(PACKAGE-NAME)\(FORM-NAME)\`:

| File | What it is |
|---|---|
| `{FormName}-wireframe-definition.md` | What the screen must show and do |
| `{FormName}-wireframe.html` | The approved wireframe, chosen from the options in `wireframe\` |
| `{FormName}-wireframe-chklist.md` | The wireframe checklist, from `templates-global\tmp-chklist-wireframe.md` |
| `{FormName}-screen.html` | The screen, Step 27 |
| `{FormName}-screen.png` | The screen image, Step 28 |
| `{FormName}-screen-chklist.md` | The screen checklist, from `templates-global\tmp-chklist-screen.md` |

Every package folder is kept. An empty package folder is a package the application does not use. The model folder `(PACKAGE-NAME)\` stays as the pattern for every form folder.

| Package | What it holds | Used by |
|---|---|---|
| `auth` | Sign-in, sign-up, second factor and password reset | Every application |
| `layouts` | The page layouts: admin, customer, blank and error | Every application |
| `components` | Custom components shared across packages | Every application |
| `public_pages` | Home, about, contact, privacy policy and terms pages | Every application |
| `dashboard` | The admin dashboard and its cards | Every application |
| `onboarding` | The client's first-run setup | Every application |
| `settings` | Business settings, users, email, payment gateways, theme and vault | Every application |
| `analytics` | Reports and analytics | Every application |
| `crm` | Contacts, leads, segments, tasks and email campaigns | Every application |
| `blog` | Posts, categories and comments | Every application |
| `landing_pages` | Landing pages and their editor | Every application |
| `invoices` | Invoices | Every application |
| `services` | Services and service categories | Services businesses |
| `bookings` | Bookings, availability and the calendar | Services businesses |
| `storefront` | The store's public pages: catalogue, product page and search | E-commerce businesses |
| `products` | Products, variants, attributes, categories and collections | E-commerce businesses |
| `inventory` | Stock levels and stock locations | E-commerce businesses |
| `cart` | The cart | E-commerce businesses |
| `checkout` | Checkout and payment | E-commerce businesses |
| `orders` | Orders, fulfilment, returns and exchanges | E-commerce businesses |
| `customers` | Customer accounts, addresses, groups and company accounts | E-commerce businesses |
| `promotions` | Discounts, promotions and price lists | E-commerce businesses |
| `shipping` | Shipping options and fulfilment providers | E-commerce businesses |
| `regions` | Regions, currencies and tax | E-commerce businesses |
| `channels` | Sales channels and marketplace synchronisation | E-commerce businesses |
