---
title: "ResourceListForm-requirement"
status: draft
doc-id: "mb-5ecb7daf"
updated: "2026-11-25"
---
# ResourceListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `services.ResourceListForm` |
| Purpose | Rooms and equipment that can be booked (ticket 21, F-11, F-10; UF-08 set-up) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `services.ServicesForm` (Services → Resources) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_resources`) | "Resources", `headline-large` | `page-title` | — | `services.resources` |
| Add Resource button | `m3.Button` (`btn_add_resource`, filled) | Adds a room or piece of equipment (UF-08 set-up) | `primary-button` | — | `services.resources.add` |
| Resource grid | Anvil `DataGrid` (`dg_resources`) | Name, type, description, active | — | mobile | — |
| Resource row | Anvil `DataRowPanel` (`drp_resource_*`) | One resource per row | — | — | — |
| Resource name field | `m3.TextBox` (`txt_resource_name_*`, outlined) | Editable in place | `outlined-input` | — | `services.resource.name` |
| Resource type selector | `m3.DropdownMenu` (`dd_resource_type_*`) | `room` / `equipment` | `outlined-select` | — | `services.resource.type` |
| Description field | `m3.TextBox` (`txt_resource_desc_*`, outlined) | The resource's description | `outlined-input` | — | `services.resource.description` |
| Active switch | `m3.Switch` (`sw_resource_active_*`) | The resource's availability | — | — | `services.active` |
| Delete button | `m3.IconButton` (`ibtn_delete_resource_*`, icon `mi:delete`) | Removes an unused resource | `icon-button` | — | `services.resource.delete` |
| Help icon | `components.HelpTooltipComponent` (help entry `services.resources`) | The feature's tooltip and help video | — | — | `help.services.resources.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `resources` | `name`, `resource_type`, `description`, `is_active` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_resources` | `resource_service` list read (`server_services/resource_service`) | The grid arrives |
| Add a resource | `btn_add_resource_click` | `resource_service` save | The new resource row appears for editing |
| Save a resource | the row field changes + save | `resource_service` save | The resource is saved |
| Delete a resource | `ibtn_delete_resource_*_click` | `resource_service` delete | An unused resource is removed; one a service requires or a booking reserves is refused with a message |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Meeting room icon (`mi:meeting_room`) |
| Heading | `services.resources.empty.title` |
| Body text | `services.resources.empty.body` |
| Primary action | `services.resources.add` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `services.resources` | Resources |
| `services.resources.add` | + Add Resource |
| `services.resource.name` | Name |
| `services.resource.type` | Type |
| `services.resource.type.room` | Room |
| `services.resource.type.equipment` | Equipment |
| `services.resource.description` | Description |
| `services.resource.delete` | Delete resource |
| `services.resources.empty.title` | No resources yet |
| `services.resources.empty.body` | Add rooms and equipment that bookings can reserve alongside a provider. |
| `services.active` | Active |

## 8. Help Videos

| Process | Help video |
|---|---|
| Adding rooms and equipment | `master_template` help entry `services.resources` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-08 Booking that needs a room or equipment | The set-up half: add the resource, mark it required on the service |
| UF-11 Admin daily operations | The calendar filters by resource |
