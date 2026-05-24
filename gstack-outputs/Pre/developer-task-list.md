# Mybizz CS — Developer Task List

**Purpose:** Tasks that human developer must do manually in Anvil (cannot be done programmatically by AI)  
**Excludes:** Programmatic tasks (code, schema, configuration) — see `07-programmatic-task-list.md`

---

## Developer Tasks Only

**What follows are tasks that require manual interaction with the Anvil Designer or Project Manager:**
- Drag-and-drop UI component creation
- Form layout design in Designer
- Setting component properties in Designer
- Creating packages/forms/modules in Project Manager pane
- Creating data tables in Designer
- Setting up data bindings in Designer
- Configuring M3 theme properties in Designer

**AI does NOT create forms, modules, or edit YAML files.**

---

## Phase 0: Pre-Implementation

### [DONE] DT-1: Create Database Tables in Anvil Designer

**Task:** Create all 36 tables in Anvil Data Tables Designer

**Steps:**
1. Open Anvil.app → mb-3-cs → Data Tables
2. Create tables in order (respecting foreign key dependencies):
   - Core tables first (users, activity_log, business_profile, config, etc.)
   - Then booking tables
   - Then CRM tables
   - Then email tables
   - Then content tables
   - Then finance tables
3. For each table:
   - Add fields with correct types
   - Set field properties (indexed, unique, required)
   - Set up foreign key links
   - Save table

**Reference:** `authoritative-schema.md` for exact field specifications

---

### [DONE] DT-2: Install Dependencies in Anvil

**Task:** Install M3 Theme and Routing dependencies

**Steps:**
1. Open Anvil.app → mb-3-cs → Settings → Dependencies
2. Install M3 Theme:
   - Click "Install Dependency"
   - Enter ID: `4UK6WHQ6UX7AKELK`
   - Name: `Material 3 Theme`
   - Click "Install"
3. Install Routing:
   - Click "Install Dependency"
   - Enter ID: `3PIDO5P3H4VPEMPL`
   - Name: `Routing`
   - Click "Install"
4. Set M3 as default theme:
   - Settings → App Settings
   - Theme: `Material 3 Theme`
   - Save

---

### [DONE] DT-3: Create Client Code Packages and Forms

**Task:** Create all form folders and forms in Anvil Project Manager

**Steps:**
1. Open Anvil.app → mb-3-cs → Project Manager
2. Create package folders in `client_code/`:
   - `auth/`
   - `dashboard/`
   - `settings/`
   - `layouts/`
   - `services/`
   - `bookings/`
   - `crm/`
   - `invoices/`
   - `blog/`
   - `public_pages/`
   - `analytics/`
   - `shared/`
3. For each form folder:
   - Right-click → New Form
   - Name the form (e.g., `LoginForm`)
   - Designer will create `form_template.yaml` and `__init__.py`
4. Repeat for all 36 forms

**Reference:** `scaffold-spec.md` for form structure

---

### [DONE] DT-4: Create Server Code Packages

**Task:** Create all server package folders in Anvil Project Manager

**Steps:**
1. Open Anvil.app → mb-3-cs → Project Manager
2. Create package folders in `server_code/`:
   - `server_auth/`
   - `server_dashboard/`
   - `server_settings/`
   - `server_services/`
   - `server_bookings/`
   - `server_customers/`
   - `server_marketing/`
   - `server_payments/`
   - `server_blog/`
   - `server_analytics/`
   - `server_shared/`
3. **Note:** Anvil automatically appends `.py` — do NOT include `.py` in folder names

**Reference:** `scaffold-spec.md` for server package structure

---

### DT-5: Design UI Components in Anvil Designer

**Task:** Design all form layouts using drag-and-drop in Designer

**For each form:**
1. Open form in Designer (double-click form folder)
2. Drag components from toolbox:
   - Buttons, TextFields, Labels, DropdownMenus, DatePickers
   - Cards, DataGrids, Plot components
   - Navigation components (Links, Panels)
3. Set component properties:
   - Text, placeholders, labels
   - Colors (use `theme:` prefix for M3 colors)
   - Visibility rules
   - Validation rules
   - Data bindings (if applicable)
4. Set M3 styling:
   - Use `role='outlined'` for inputs
   - Use `role='outlined-error'` for validation errors
   - Set button hierarchy (filled, outlined, text)
5. Save form

**Reference:** `scaffold-spec.md` for M3 component standards

**Note:** AI will write the `__init__.py` code, but Designer layout must be done manually.

---

### DT-6: Configure Form Data Bindings

**Task:** Set up data bindings for forms that need them

**For forms with data bindings:**
1. Open form in Designer
2. Select component
3. In Properties panel → Data Bindings:
   - Enable "Write Back" toggle
   - Bind to `self.item` field
   - Set up read/write bindings
4. Save form

**Note:** Forms that manage their own server calls should NOT use data bindings.

---

### [DONE] DT-7: Set Startup Form

**Task:** Configure application startup form

**Steps:**
1. Open Anvil.app → mb-3-cs → Settings → App Settings
2. Startup Form: `auth.LoginForm`
3. Save

---

### DT-8: Configure M3 Theme Properties

**Task:** Set M3 theme customization

**Steps:**
1. Open Anvil.app → mb-3-cs → Settings → App Settings
2. Theme customization:
   - Primary color
   - Accent color
   - Font family
   - Header style
3. Save

---

### DT-9: Create Layouts in Designer

**Task:** Design all layout forms with navigation

**For each layout (AdminLayout, CustomerLayout, BlankLayout, ErrorLayout):**
1. Open layout in Designer
2. Create navigation structure:
   - Sidebar with navigation items
   - Main content area
   - Sign out button at bottom
3. Set up navigation click handlers:
   - Add Link components
   - Set up lambda click handlers with variable capture
4. Save layout

**Reference:** `architecture.md` for navigation architecture

---

### [Done] DT-10: Set GitHub Repository

**Task:** Link Anvil app to GitHub

**Steps:**
1. Open Anvil.app → mb-3-cs → Settings → GitHub
2. Connect GitHub account
3. Create new repository: `mb-3-cs`
4. Link repository to app
5. Push initial code

---

## Phase 1-5: Feature Implementation

### DT-11 to DT-XX: Implement Features in Designer

**For each feature module (Services, Bookings, Payments, CRM, etc.):**

**Steps:**
1. Open each form in Designer
2. Design UI layout:
   - List forms: Add search box, filter dropdowns, DataGrid
   - Editor forms: Add form fields, save/cancel buttons
   - Viewer forms: Add detail display, action buttons
   - Calendar forms: Add calendar component, event markers
3. Set component properties:
   - Labels, placeholders, validation
   - Visibility rules based on RBAC
   - Data bindings (if applicable)
4. Add M3 styling:
   - Use theme colors
   - Set proper roles
   - Ensure responsive design
5. Save form

**Note:** AI will write all `__init__.py` code, but Designer layout must be done manually.

---

## Task Execution Order

1. [DONE]**DT-1:** Create database tables 
2. [DONE]**DT-2:** Install dependencies
3. [DONE]**DT-3:** Create form folders
4. [DONE]**DT-4:** Create server package folders
5. **DT-5:** Design UI for all forms
6. **DT-6:** Set up data bindings
7. [DONE]**DT-7:** Configure startup form
8. **DT-8:** Set theme properties
9. **DT-9:** Design layouts
10.[DONE]**DT-10:** Set GitHub repository
11. **DT-11+:** Implement features in Designer (following Phase 1-5 order)

---

## Developer Task Notes

- **AI does NOT:** Create forms, edit YAML files, drag-and-drop components
- **Developer DOES:** All UI design work in Anvil Designer
- **AI DOES:** Write all `__init__.py` code, server functions, tests, schema
- **Reference:** `07-programmatic-task-list.md` for AI tasks

---

## Completion Checklist

- [DONE] All 36 tables created in Designer
- [DONE] All dependencies installed
- [DONE] All 36 form folders created
- [DONE] All 13 server package folders created
- [] All form layouts designed in Designer
- [] All data bindings configured
- [DONE] Startup form set
- [] Theme configured
- [] All layouts designed
- [DONE] GitHub repository linked
- [] All features implemented in Designer

---

*End of file — 09-developer-task-list.md*
