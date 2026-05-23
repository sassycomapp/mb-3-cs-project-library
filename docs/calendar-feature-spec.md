Calendar Feature Plan — Anvil + Material 3
Core Decision

This will be implemented as a feature module inside the host app, not as a reusable Custom Component.

Implementation style:

pure Anvil
pure Material 3
no Anvil Extras
minimal custom CSS
responsive
dynamic rendering
no static hardcoded day buttons
1. FORM STRUCTURE
Main Feature Form
BookingCalendarForm

Contains:

hdg_calendar_title (Heading) — Calendar title
col_view_selector (ColumnPanel) — View toggle buttons
content_panel (ColumnPanel) — Dynamic view injection
dd_staff (DropDown) — Staff selector (optional)

Component Structure
Component	Type	Purpose
col_calendar (ColumnPanel)	Container for entire calendar
hdg_calendar_title (Heading)	"Calendar" title
col_view_selector (ColumnPanel)	Container for view buttons
btn_view_month (Button)	"Month" view toggle
btn_view_week (Button)	"Week" view toggle
btn_view_day (Button)	"Day" view toggle
content_panel (ColumnPanel)	Dynamic view injection point
dd_staff (DropDown)	Staff filter (optional)
2. VIEW ARCHITECTURE
REQUIRED APPROACH

Use:

3 separate forms dynamically loaded into content_panel
frm_calendar_month — HTMLTemplate with CSS Grid
frm_calendar_week — RepeatingPanel with tmpl_week_timeslot
frm_calendar_day — RepeatingPanel with tmpl_day_event

Do NOT:

keep all views loaded with visible=False

Reason:

cleaner lifecycle
lower DOM complexity
simpler responsiveness
simpler state management
aligns better with Anvil form architecture

Form Tree
BookingCalendarForm
│
├── col_calendar (ColumnPanel)
│   ├── hdg_calendar_title (Heading)
│   ├── col_view_selector (ColumnPanel)
│   │   ├── btn_view_month (Button)
│   │   ├── btn_view_week (Button)
│   │   └── btn_view_day (Button)
│   └── content_panel (ColumnPanel)
│
├── frm_calendar_month (HTMLTemplate-based)
│   └── tmpl_month_day_cell (template subform)
│
├── frm_calendar_week (RepeatingPanel-based)
│   └── tmpl_week_timeslot (template subform)
│       └── tmpl_week_event (nested template)
│
└── frm_calendar_day (RepeatingPanel-based)
    └── tmpl_day_event (template subform)
3. FORM RESPONSIBILITIES
BookingCalendarForm
Responsibility

Controller form — manages view switching and staff filtering.

frm_calendar_month
Responsibility

Render month grid view.

Layout:

HTMLTemplate with CSS Grid (7 columns)
tmpl_month_day_cell repeated for each day cell
No DataGrid — pure HTML/CSS grid
Dynamic day rendering based on month

frm_calendar_week
Responsibility

Render week view with timeslots.

Layout:

RepeatingPanel (rp_week_timeslots)
tmpl_week_timeslot for each timeslot (e.g., 9 AM, 10 AM, 11 AM)
tmpl_week_event nested inside for events in each timeslot
Vertical timeslot layout with horizontal event containers

frm_calendar_day
Responsibility

Render single day schedule.

Layout:

RepeatingPanel (rp_day_events)
tmpl_day_event for each event in the day
InteractiveCard for each event
Vertical event list with time indicators
4. RESPONSIVE STRATEGY
APPROVED COMPONENTS
Use	Component
Vertical layout	ColumnPanel
Horizontal layout	FlowPanel
Event containers	InteractiveCard
Clickable events	InteractiveCard
Dynamic rows	RepeatingPanel
Main content injection	add_component()
HTML rendering	HTMLTemplate
5. TEMPLATE SUBFORMS
tmpl_month_day_cell
Purpose: Single day cell in month grid

Structure:
hdg_day_number (Heading) — Day number (1-31)
flow_events (FlowPanel) — Container for events on this day

Designer-Only Properties:
Component	Type	Text/Label	Property
hdg_day_number	Heading	15	body-medium
flow_events	FlowPanel	—	Visible = True

tmpl_week_timeslot
Purpose: Single timeslot row in week view

Structure:
hdg_time (Heading) — Time label (e.g., "9:00 AM")
rp_week_events (RepeatingPanel) — Repeats tmpl_week_event

Designer-Only Properties:
Component	Type	Text/Label	Property
hdg_time	Heading	9:00 AM	body-small
rp_week_events	Repeating Panel	—	Visible = True

tmpl_week_event
Purpose: Single event within week timeslot

Structure:
card_event (InteractiveCard) — Clickable event container
card_content (CardContentContainer) — Event details
hdg_title (Heading) — Event title
hdg_time (Heading) — Event duration

Designer-Only Properties:
Component	Type	Text/Label	Property
card_event	Interactive Card	—	role = outlined-card
hdg_title	Heading	Consultation	title-medium
hdg_time	Heading	30 min	body-small

tmpl_day_event
Purpose: Single event in day view

Structure:
card_event (InteractiveCard) — Clickable event container
card_content (CardContentContainer) — Event details
hdg_title (Heading) — Event title
hdg_time (Heading) — Event time range
hdg_description (Heading) — Optional description

Designer-Only Properties:
Component	Type	Text/Label	Property
card_event	Interactive Card	—	role = outlined-card
hdg_title	Heading	Massage Therapy	title-medium
hdg_time	Heading	9:00 AM - 10:00 AM	body-small
hdg_description	Heading	—	body-small

6. MATERIAL 3 COMPLIANCE
STRICT RULES

Use ONLY:

Heading component (hdg_) — Never use Label for titles
InteractiveCard — For all clickable events
ColumnPanel — For vertical layouts
FlowPanel — For horizontal layouts
RepeatingPanel — For dynamic lists
HTMLTemplate — For month grid
standard Buttons — For navigation
standard DropDown — For staff selection

Do NOT use:

Labels for titles — Use Heading (hdg_) instead
Chips — Not part of M3 calendar design. Chips are custom components composed from `m3.Button` or `m3.Card` per ADR-014.
LinearPanel — Use ColumnPanel or FlowPanel
Anvil Extras — Pure Anvil only
custom JS widgets — No external libraries
7. STAFF SUPPORT
Selected Model

Single visible staff calendar.

Implementation:

dd_staff (DropDown)

Changing dropdown:

reloads current view
filters event source by staff_id
Designer-Only Properties:
Component	Type	Property
dd_staff	DropDown	items: [Staff list], appearance = outlined
8. DATA FLOW
Calendar receives data externally

The calendar feature does NOT manage scheduling.

It only:

displays supplied data
filters by staff selection
switches between views

Required Input Structure
events = [
    {
        "event_id": 1,
        "staff_id": 5,
        "title": "Consultation",
        "start": datetime_obj,
        "end": datetime_obj,
        "color": "#6750A4"
    }
]

Data Flow:
1. BookingCalendarForm receives events list from parent
2. View button click triggers load_view(view_name)
3. content_panel.clear() removes previous view
4. Appropriate form instantiated and added via add_component()
5. Form filters events by staff_id if dd_staff selected
6. tmpl_* subforms render individual items
9. VIEW SWITCHING
Implementation

Inside BookingCalendarForm:

def btn_view_month_click(self, **event_args):
    self.load_view("month")

def btn_view_week_click(self, **event_args):
    self.load_view("week")

def btn_view_day_click(self, **event_args):
    self.load_view("day")

def load_view(self, view_name):
    self.content_panel.clear()

    if view_name == "month":
        form = frm_calendar_month()
        form.set_events(self.events, self.selected_staff_id)
        self.content_panel.add_component(form)

    elif view_name == "week":
        form = frm_calendar_week()
        form.set_events(self.events, self.selected_staff_id)
        self.content_panel.add_component(form)

    elif view_name == "day":
        form = frm_calendar_day()
        form.set_events(self.events, self.selected_staff_id)
        self.content_panel.add_component(form)

def dd_staff_change(self, **event_args):
    self.selected_staff_id = self.dd_staff.value
    self.load_view(self.current_view)  # Reloads with filter
10. MONTH VIEW IMPLEMENTATION
frm_calendar_month
Purpose: HTMLTemplate-based month grid

HTMLTemplate Structure:
<div class="calendar-month-container">
  <div class="calendar-toolbar">
    <button class="nav-btn">&lt;</button>
    <div class="calendar-title">{{ current_month }}</div>
    <button class="nav-btn">&gt;</button>
  </div>
  <div class="calendar-weekdays">
    <div>Mon</div><div>Tue</div><div>Wed</div>
    <div>Thu</div><div>Fri</div><div>Sat</div><div>Sun</div>
  </div>
  <div class="calendar-grid" anvil-slot-repeat="day_cells">
  </div>
</div>

CSS Grid (applied programmatically):
.calendar-month-container { width: 100%; }
.calendar-toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}
.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 8px;
  margin-bottom: 8px;
}
.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 8px;
}
@media (max-width: 768px) {
  .calendar-grid { gap: 4px; }
}

frm_calendar_month Implementation:
class frm_calendar_month(frm_calendar_monthTemplate):
    def form_show(self, **event_args):
        # Generate day cells for current month
        day_cells = self.generate_month_grid()
        self.day_cells = day_cells  # Populates anvil-slot-repeat

    def generate_month_grid(self):
        # Returns list of day cell data
        # Each cell: {'day': 15, 'is_today': False, 'events': [...]}
        pass

Designer-Only Properties:
Component	Type	Property
HTMLTemplate	HTMLTemplate	anvil-slot-repeat="day_cells"
11. WEEK VIEW IMPLEMENTATION
frm_calendar_week
Purpose: RepeatingPanel-based week view

Structure:
rp_week_timeslots (RepeatingPanel)
  └── tmpl_week_timeslot (repeated for each timeslot)
      ├── hdg_time
      └── rp_week_events (RepeatingPanel)
          └── tmpl_week_event (repeated for each event)

frm_calendar_week Implementation:
class frm_calendar_week(frm_calendar_weekTemplate):
    def form_show(self, **event_args):
        # Generate timeslots for current week
        timeslots = self.generate_week_timeslots()
        self.timeslots = timeslots  # Populates rp_week_timeslots

    def generate_week_timeslots(self):
        # Returns list of timeslot data
        # Each timeslot: {'time': '9:00 AM', 'events': [...]}
        pass

Designer-Only Properties:
Component	Type	Property
Repeating Panel	rp_week_timeslots	items: timeslots, template = tmpl_week_timeslot
12. DAY VIEW IMPLEMENTATION
frm_calendar_day
Purpose: RepeatingPanel-based day schedule

Structure:
rp_day_events (RepeatingPanel)
  └── tmpl_day_event (repeated for each event)

frm_calendar_day Implementation:
class frm_calendar_day(frm_calendar_dayTemplate):
    def form_show(self, **event_args):
        # Generate events for selected day
        events = self.get_day_events()
        self.events = events  # Populates rp_day_events

    def get_day_events(self):
        # Returns filtered events for current day
        # Sorted by start time
        pass

Designer-Only Properties:
Component	Type	Property
Repeating Panel	rp_day_events	items: events, template = tmpl_day_event
13. NAVIGATION
View Switching

Use:

standard Buttons (btn_view_month, btn_view_week, btn_view_day)
outlined/filled roles
No icon libraries required

Navigation Buttons Designer-Only Properties:
Component	Type	Text	Property
btn_view_month	Button	Month	Outlined
btn_view_week	Button	Week	Filled
btn_view_day	Button	Day	Outlined
14. FINAL RECOMMENDED FORM TREE
BookingCalendarForm
│
├── col_calendar (ColumnPanel)
│   ├── hdg_calendar_title (Heading)
│   ├── col_view_selector (ColumnPanel)
│   │   ├── btn_view_month (Button)
│   │   ├── btn_view_week (Button)
│   │   └── btn_view_day (Button)
│   └── content_panel (ColumnPanel)
│
├── frm_calendar_month (HTMLTemplate)
│   └── tmpl_month_day_cell (template subform)
│
├── frm_calendar_week (RepeatingPanel)
│   └── tmpl_week_timeslot (template subform)
│       └── tmpl_week_event (nested template)
│
└── frm_calendar_day (RepeatingPanel)
    └── tmpl_day_event (template subform)
15. OVERALL RECOMMENDATION

This approach provides:

Best balance of Anvil-native architecture
Material 3 compliance
responsiveness
maintainability
performance
clean rendering
future extensibility

without introducing:

Anvil Extras
custom JS
unsupported patterns
excessive CSS complexity

Key Benefits:

Pure Anvil implementation
No external dependencies
Dynamic rendering (no hardcoded components)
Template-based scalability
Easy to extend with new features

Key Benefits:

Pure Anvil implementation
No external dependencies
Dynamic rendering (no hardcoded components)
Template-based scalability
Easy to extend with new features

Key Benefits:

Pure Anvil implementation
No external dependencies
Dynamic rendering (no hardcoded components)
Template-based scalability
Easy to extend with new features