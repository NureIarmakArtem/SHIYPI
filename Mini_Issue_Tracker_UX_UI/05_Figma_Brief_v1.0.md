# Mini Issue Tracker — Figma Design Implementation Brief v1.0

## 1. Purpose and Scope
Design a low-fidelity desktop web interface for **Mini Issue Tracker**, an internal tool used by a small development team to register, track, prioritize, and resolve software issues [KC-01, KC-03]. 
The scope strictly covers **Release MVP (`US-01` through `US-08`)** [§8]. 
System users are limited to three pre-configured internal team members: `Member A` (Team Member), `Member B` (Team Member), and `Lead` (Team Lead) [BR-01, D-08]. `Assignee` is an issue-level relation, not a global system role [US-03].

## 2. Design Constraints
- **Format & Visual Style:** Desktop web layout, strict grayscale palette, low-fidelity wireframes with clear typographic hierarchy [D-01].
- **Figma Structure:** All frames must reside on a single canvas page using editable native Figma text and vector layers, Auto Layout, and reusable local components (buttons, inputs, badges, table rows) with semantic layer naming.
- **Separation of Concerns:** Keep reviewer annotations, permission matrices, and state tables strictly outside product frames inside the `REF-01` zone.
- **Visual Prohibitions:** Do not include decorative page icon placeholders, empty square boxes next to headings, row-selection checkboxes, or bulk action toolbars.
- **Economy of Frames:** Build canonical product frames plus compact component state variations in `REF-01`. Do not duplicate full-page screens for all 16 role-by-status combinations. Prototype connections are documented as annotations only; do not implement backend logic or interactive wiring during generation.

## 3. Canonical Screens and States (Exact IDs)
- **`S-01` — Authentication Boundary:** Neutral entry placeholder block labeled `"Authentication entry mechanism TBD"` [BR-01, NFR-02, D-02]. Contains no password inputs, login forms, or SSO buttons.
- **`S-02` — Issues List (Single Product Screen with 3 States):**
  - **`S-02-A` (All Issues):** Default view displaying all registered issues (`ISS-001` to `ISS-008`) [US-02, AC-02.1].
  - **`S-02-B` (Important Issues):** Filtered view of the exact same list component showing only high-priority open/active issues (`ISS-001`, `ISS-002`, `ISS-003`, `ISS-007`) [US-08, D-01].
  - **`S-02-C` (Empty List State):** State of `S-02` when zero issues exist in the repository, displaying an empty-state message [AC-02.5].
- **`M-01` — Create Issue Modal:** Overlay dialog triggered from `S-02` for submitting a new issue [US-01, D-01].
- **`S-03` — Issue Details:** Canonical detail view showing full issue data, inline management controls (`Assignee`, `Priority`, `Status`), and the comment thread [US-02..US-07]. Canonical frame context: `Actor = Lead` inspecting `ISS-003` (where `Assignee = Member A`).
- **`REF-01` — Reviewer State Reference (Outside Product UI):** Dedicated non-product documentation frame displaying the 4 role/assignment contexts, 4 status transition states, and validation/error examples.

## 4. Required Labels and Visible Data
- **On `S-02` (`S-02-A`, `S-02-B`):**
  - Header title: `Issues List`; primary button: `Create Issue`; view switcher tabs: `All Issues` and `Important Issues` [D-01].
  - Table columns: `ID`, `Title` (clickable link), `Status`, `Priority`, `Assignee` [AC-02.2, AC-05.3].
  - Empty value representations: `Not Set` for unset priority [BR-04, D-03]; `Unassigned` when no assignee is selected [AC-02.2, D-04].
- **On `M-01`:**
  - Form inputs: `Title` (required text input) and `Description` (required multiline textarea) [AC-01.1..01.3].
  - Buttons: `Create` and `Cancel` [D-01].
  - System-generated fields (`ID`, `Status = Open`, `Priority = Not Set`, `Assignee = Unassigned`, `Author`, `Created at`) must NOT appear as editable inputs in `M-01` [AC-01.4, AC-01.5, D-04].
- **On `S-03`:**
  - Navigation link: `Return to Issues List` [D-07].
  - Issue metadata: `ID`, `Title`, `Description`, `Status`, `Priority`, `Assignee`, `Author`, `Created at` [AC-02.4, AC-05.2].
  - Management controls: `Assignee` selector + `Apply`; `Priority` selector + `Apply`; `Status` selector + `Apply` (grouped cleanly without a `"Control Actions Area"` header) [D-06].
  - Comments section: `Comment` multiline input, `Add Comment` button, and chronological list of saved comments showing comment text, `Author`, and `Creation date/time` [AC-06.1..06.4].

## 5. Controls and Permissions Matrix
Ineligible role-dependent controls must remain visible in a `disabled` state rather than hidden [D-05]. Each state change on `S-03` requires selecting a value and clicking its adjacent `Apply` button [D-06].

- **Team Member (`non-assignee`):**
  - View list/details, Create issue (`M-01`), Add comment: `Enabled` [BR-02, US-02, US-06].
  - `Assignee` (`C-03.2`), `Priority` (`C-03.3`), `Status` (`C-03.4`): `Disabled` [US-03, AC-04.2, AC-07.4, D-05].
- **Team Member (`current assignee`):**
  - View list/details, Create issue, Add comment: `Enabled`.
  - `Assignee` (`C-03.2`), `Priority` (`C-03.3`): `Disabled` [US-03, AC-04.2].
  - `Status` (`C-03.4`): `Enabled` for allowed transitions from current status; `Disabled` if status is `Closed` [BR-08, BR-08.1].
- **Team Lead (`non-assignee`, e.g., Lead viewing `ISS-003` assigned to `Member A`):**
  - View list/details, Create issue, Add comment: `Enabled`.
  - `Assignee` (`C-03.2`, single-select from `Member A`, `Member B`, `Lead`): `Enabled` [US-03, D-04].
  - `Priority` (`C-03.3`, selectable options strictly `Low`, `Normal`, `High`, `Critical`; `Not Set` is display-only and cannot be selected to reset): `Enabled` [BR-05, BR-06, AC-04.1, AC-04.2, D-03].
  - `Status` (`C-03.4`): `Disabled` — Team Lead cannot change the status of an issue assigned to another member or an unassigned issue [AC-07.4, BR-08].
- **Team Lead (`current assignee`, e.g., Lead viewing `ISS-007`):**
  - All controls (`Assignee`, `Priority`, and valid `Status` transitions) are `Enabled` [US-03, BR-06, BR-08].

## 6. Status Transitions Rules
Valid status values are strictly `Open`, `In Progress`, `Resolved`, and `Closed` [BR-07, AC-07.2]. Only the current `Assignee` can trigger transitions [AC-07.4]:
1. `Open` -> `In Progress` (only valid outgoing option from `Open`) [BR-08.1].
2. `In Progress` -> `Resolved` (only valid outgoing option from `In Progress`) [BR-08.1].
3. `Resolved` -> `In Progress` (re-opening work) OR `Resolved` -> `Closed` [BR-08.1].
4. `Closed` -> No outgoing status transitions (`Status` control and `Apply` are `disabled`) [BR-08.1].
- Do not list the current status as a selectable transition target.
- `Closed` status only locks further status changes; it does NOT disable adding comments or changing priority [BR-08.1, US-06].

## 7. Important Issues Predicate
An issue appears in `S-02-B (Important Issues)` if and only if:
`Priority IN {High, Critical} AND Status != Closed` [BR-09, BR-09.1, AC-08.1..08.4].
- Issues with `Status = Resolved` and `Priority = High` or `Critical` are active (not yet closed) and MUST remain visible in `Important Issues` [BR-09.1].
- Issues with `Priority = Low`, `Normal`, or `Not Set` are excluded [AC-08.3].
- Issues with `Status = Closed` are always excluded regardless of priority [AC-08.4].

## 8. Required Empty, Validation, and Error Examples
- **Empty Repository (`S-02-C`):** Displays `"No issues registered yet"` when total issues equal zero [AC-02.5].
- **Create Modal Validation (`M-01`):** Show validation states for missing Title (`"Title is required"`) and missing Description (`"Description is required"`). Invalid submission keeps `M-01` open and creates no issue [AC-01.2, AC-01.3].
- **Comment Validation & Save Failure (`S-03` / `REF-01`):**
  - Ready state: empty input ready for typing.
  - Validation error: submitting an empty comment displays `"Comment text cannot be empty"` [AC-06.1].
  - Saved state: displays comment text, author, and timestamp [AC-06.2..06.4].
  - Save error state: displays error banner `"Failed to save comment. Please try again."` while ensuring the failed comment is NOT appended to the saved comments list [AC-06.5].

## 9. Navigation Intent
- Switching `All Issues` / `Important Issues` tabs toggles between `S-02-A` and `S-02-B` (`T-01`, `T-02`) [D-01].
- Clicking `Create Issue` opens overlay `M-01` (`T-03`); clicking `Cancel` closes `M-01` and restores the previous list state (`T-04`) [D-01, D-07].
- Valid `Create` submission transitions to `S-03` displaying the newly created issue with default `Open`, `Not Set`, and `Unassigned` values (`T-05`) [AC-01.4, AC-01.5, D-04].
- Clicking an issue `Title` in `S-02-A` or `S-02-B` opens its corresponding `S-03` (`T-07`) [AC-02.3].
- Clicking `Return to Issues List` on `S-03` returns the user to the previously active list tab (`S-02-A` or `S-02-B`) (`T-08`) [D-07].
- Clicking `Apply` for `Assignee`, `Priority`, or `Status`, or clicking `Add Comment`, updates the state in-place on `S-03` (`T-09`..`T-13`) [D-06].

## 10. Sample Data (Consistent Across All Frames)
Use these exact 8 records in `S-02-A` [D-08]:
- `ISS-001` | Title: `"Auth service timeout on peak load"` | Status: `Open` | Priority: `Critical` | Assignee: `Member A` -> **In Important (`S-02-B`)**
- `ISS-002` | Title: `"Broken pagination layout on desktop"` | Status: `In Progress` | Priority: `High` | Assignee: `Member B` -> **In Important (`S-02-B`)**
- `ISS-003` | Title: `"Memory leak in background worker"` | Status: `Resolved` | Priority: `High` | Assignee: `Member A` -> **In Important (`S-02-B`)** *(Canonical `S-03` item)*
- `ISS-004` | Title: `"Crash on empty configuration file"` | Status: `Closed` | Priority: `Critical` | Assignee: `Member B` -> Excluded from Important (`Closed`)
- `ISS-005` | Title: `"Update outdated README setup steps"` | Status: `Open` | Priority: `Normal` | Assignee: `Unassigned` -> Excluded from Important (`Normal`)
- `ISS-006` | Title: `"Alignment glitch in modal footer"` | Status: `Open` | Priority: `Not Set` | Assignee: `Unassigned` -> Excluded from Important (`Not Set`)
- `ISS-007` | Title: `"Database connection pool exhaustion"` | Status: `Resolved` | Priority: `Critical` | Assignee: `Lead` -> **In Important (`S-02-B`)**
- `ISS-008` | Title: `"Typo in validation helper message"` | Status: `In Progress` | Priority: `Low` | Assignee: `Member A` -> Excluded from Important (`Low`)

## 11. Exclusions
Strictly exclude: file attachments or upload dropzones (`US-09` Deferred), AI category or priority recommendations (`SR-10`, `SR-11` Future), issue deletion (`BR-10`), change history logs (`BR-11`), editing issue title/description after creation, search bars, column sorting, pagination, bulk selection checkboxes, multi-assignee selection, notifications, dashboards, and separate standalone screens for assignment, priority, status, or comments.

## 12. Acceptance Checklist & Completion Report
Before finishing generation, verify:
- [ ] Exactly 4 statuses (`Open`, `In Progress`, `Resolved`, `Closed`) and 4 selectable priorities (`Low`, `Normal`, `High`, `Critical`) + display-only `Not Set` are used.
- [ ] `S-02-B` contains strictly `ISS-001`, `ISS-002`, `ISS-003`, and `ISS-007`.
- [ ] On `S-03` (`Lead` viewing `ISS-003` assigned to `Member A`), `Assignee` and `Priority` are `enabled`, while `Status` is `disabled`.
- [ ] `REF-01` is placed outside product frames and covers all 4 role/assignee combinations, all 4 status transition states, and comment error states.
- [ ] In the same response, output a brief report listing created frames, reusable local components, any unrepresented rules, and verification boundaries (noting that database persistence, real authentication, server-side authorization, `< 1 s` response time, `HTTPS`, and backups cannot be proven by static wireframes).