# Competition Tools

A collection of tools to assist Jury Presidents and other competition officials with common (and uncommon) competition management tasks.

---

## Paraclimbing Qualification Planner (`jenga.html`)

Builds a combined qualification starting grid for para climbing competitions.
This tool could also be used for youth competitions, but the task complexity probably doesn't warrant this.
 
Shows when each sport class and athlete is scheduled to start on each qualification route together with spacing and/or cleaning breaks. The calculated schedule can be printed out for reference.


Every athlete appears twice (once per route) with a distinct `order` per route.

### Features

- **Drag-and-drop import** - Reads CSV or JSON files containing the athletes and starting orders for all sport classes/categories. Displays a set of coloured "pills", one for each comination of sport class/category and qualification route. e.g. W-B01-R1 and W-B01-R2 respectively group athletes in the Women B1 sport class on their twocqualification routes. 
- **Drag-and-drop route allocation** — sport class and route pills can be dragged onto physical routes, labelled A - H. Route allocation is nominative, i.e. it does not follow that "route 1" must be placed ahead of "route 2", ; Pills may be dragged onto any of the 8 route columns, and moved between or reordered within columns. 
- **Auto-Arrange** — Global reordering optimization. Reorders all routes across columns to minimize time conflicts between Route 1 and Route 2 of each sport class. Processes largest groups first, brute-forces insertion positions to minimize required padding, then rebuilds columns in the new optimal order.
- **Auto-Spacing** — Local gap-filling. Preserves current column order but inserts padding blocks (PAD X') before the later-starting route of each sport class to enforce the required minimum time gap. Padding is rounded to 5-minute increments and includes a delete button for manual adjustment.
- **Break Additions** — Cleaning and spacing intervals (15 min) are added manually - I tried various algorithms to automate break addition, but manual insertion seems to be more reliable.

### How to use

1. Open `schedule.html` in a browser.
2. Drop a JSON or CSV file onto the page.
3. Drag sport-class groups from the pool onto the route columns (A–H).
4. Click **Auto-arrange** to automatically space the two route appearances of each class.
6. Juggle the schedule by adding **Cleaning Breaks**  and **Spacing Intervals** and using the **Auto-Spacing** button.

The scheduler uses a GLOBAL_CADENCE of 5 min/athlete and GLOBAL_CLEAN period of 15 min to calculate route durations and required gaps:

---

## Boulder Restart Assistant (`rotation.html`)

Plan **gaps**, **restarts**, and **interruptions** in the rotation schedule for boulder qualification / semifinals.
Multi-language interface (EN / DE / FR).

### Features

- **Setup & Start List** — configure phase values (start time, rotation interval, number of boulders, transit time, shuttle capacity) and import athlete start lists.
- **Call Zone (CZ)** — live view of which athlete is on which boulder in each rotation. GAP slots and restarts are highlighted.
- **Gap Assistant** — add restart requests by athlete and boulder; the tool calculates the earliest possible gap and restart rotation, detects conflicts, and merges restarts into shared gaps where possible.
- **Transit Zone (TZ)** — shuttle schedule with departure times and CZ wait times, adjusted for gaps and pauses.
- **Interruptions / Pauses** — hold the entire wall for a number of rotations (e.g. weather, repairs); all subsequent athletes and times shift accordingly.
- **Print** — print the CZ and TZ schedules.

### How to use

1. Open `rotation.html` in a browser.
2. **Setup tab:** configure start time, interval (seconds per rotation), number of boulders, transit time, and shuttle capacity.
3. Drop a JSON or CSV start list onto the import card, or add athletes manually with **+ Row**.
4. **CZ tab:** use the **?** button for workflow help. Add restart requests in the **Single Insert** table — select athlete, enter boulder number and decision rotation; the tool suggests gap and restart rotations automatically.
5. Add **Stop / Pause** entries to model full-wall interruptions.
6. **TZ tab:** view shuttle groupings, departure times, and CZ wait times.
7. Use **Print** for a combined CZ + TZ printout.

---

## Input data

Both tools accept data by dropping a **`.json`** or **`.csv`** file onto the page.

**CSV format** — header row, comma- or semicolon-delimited:

| bib | lastname | country | order |
|-----|----------|---------|-------|
| 1   | BERTONE  | FRA     | 2     |
| 2   | SEKIKAWA | JPN     | 17    |

- `bib` — competitor number (integer)
- `lastname` — competitor surname (only `lastname` is accepted, never `name`)
- `country` — 3-letter nation code
- `order` — starting position within the round (integer) or on a route.

### Additional input fields

| field | description |
|-------|-------------|
| `group` | Sport class in the format `{Gender}-{Category}{Level}`, e.g. `M-AL1`, `W-B02`, `M-RP3`. Gender is `M` or `W`; category is one of `AL` (leg amputee), `AU` (arm amputee), `RP` (neurological), or `B` (visually impaired). |
| `route` | Qualification route number (`1` or `2`). |

**JSON format** — array of objects with the same fields:

```json
[
  {"bib": 1, "lastname": "BERTONE", "country": "FRA", "order": 2}
]
```

For `rotation.html`, a JSON file may also be an object with an `athletes` array.

Tool-specific additional fields are documented under each tool below.

---


## Test files

Sample data files are in `testfiles/`:

| File | Tool | Description |
|------|------|-------------|
| `ex-pr.json` | `schedule.html` | Para competition data — 24 groups, 2 routes each |
| `ex-bd.json` | `rotation.html` | Boulder qualification start list — 24 athletes |
| `ex-bd.csv` | `rotation.html` | Same data in CSV format |

---

## Requirements

No build step, no server, no dependencies. Open any `.html` file directly in a modern browser (Chrome, Firefox, Safari, Edge).
`schedule.html` uses a CDN-hosted copy of [SortableJS](https://sortablejs.github.io/Sortable/) for the pool-to-route drag-and-drop.
