---
title: "README"
doc-id: "mb-7d2ea785"
updated: "2026-11-06"
---
# ui

The screen requirements of App 1. A screen requirement is the approved design of one form, written to section 14.1 of `spec-ui-standards` in `project-library-global`; `mb-form-builder` builds the form from it.

A folder per Anvil client package, named exactly as the package is named, holds a folder per form, named exactly as the form is named in Anvil. The path mirrors the form's module path in `client_code`: `ui\<package path>\<FormName>\`. A form held inside another form's folder in `client_code` sits one folder deeper here in the same way.

Every form folder holds one file, `<FormName>-requirement.md`, modelled on `(PACKAGE-NAME)\(FORM-NAME)\(FORM-NAME)-requirement.md`. Its front matter says `status: draft` until David approves it, then `status: approved`. The form's spec sheet, which the Anvil agent reads, lives in the app at `theme\assets\docs\app1\<package path>\<FormName>.md`.

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
