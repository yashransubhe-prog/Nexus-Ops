# NEXUS OPS 2.0 — Product Experience Vision

> **Status: proposed design and implementation plan, not a claim of completed functionality.**  
> **Public showcase only.** Application source, backend configuration, secrets, customer data, and business logic are intentionally not published in this repository.

**Product identity:** NEXUS OPS · *The Industrial Command Center*  
**Core promise:** *See the work. Assign the right people. Close the loop.*

---

## What the next experience should feel like

A premium, reliable industrial operations app designed for managers, supervisors, field engineers, maintenance technicians, and on-site operators. Dense in real capability, **light in cognitive load**. A user should be able to identify the most urgent work and take their next meaningful action within a few seconds.

**Design inspirations:** best-in-class industrial maintenance workflows (work orders, assets, mobile evidence), the clarity of enterprise task-management products, and purposeful Material motion. This is an original design direction, not a copy of another product.

### Visual direction

| Token | Direction |
|---|---|
| Primary background | Deep graphite / midnight navy, **#0A111A** |
| Elevated surfaces | **#141F2B**, **#1C2937** |
| Accent | Precision teal, **#40D8CC** |
| Caution | Safety amber, **#F5B84B** |
| Critical | Muted alert red, **#EE6467** |
| Typography | Clean, legible type, with tabular numerals for metrics |
| Layout | Clear 8-point spacing system, readable industrial-density cards |
| Modes | Accessible light and dark themes; follow system preferences |
| Scale | Phones first; optimized responsive panels for tablets and larger screens |

Motion is functional: state feedback, continuity, navigation hierarchy, and progress. Avoid excessive blur, decorative particles, repeated splashes, looping loaders, and unreadable glass effects.

## Launch & first-run experience

### 1. Fast brand entrance

- Animate the **original Nexus Dynamics / Nexus Ops logo**, using a locally bundled asset and native Flutter rendering.
- A subtle node-to-node line reveal suggests connected operations; wordmark emerges with a short, controlled transition.
- Do not access Firebase, a CDN, image API, or any network service to display the animation.
- Target a **sub-1.5-second visual sequence**; skip or shorten it when the app is already initialized, or when reduced motion is requested.
- Never use an artificial delay to conceal slow loading. Show cached application content while remote work happens asynchronously.

### 2. First-install feature tour

- A **three-to-four-card carousel** with first-party, locally bundled artwork.
- Each card communicates one real workflow: **Mission Control**, **Connected Assets**, **Shift Pulse**, and **ProofPack**.
- Use smooth transitions, swipe gestures, page progress, Skip/Next/Get Started, accessible text, and an option to replay the tour in Settings.
- Track completion locally with a versioned flag and appropriate per-user handling; never repeat on every login.
- Do not present illustrative artwork as a screenshot of a feature that is not yet working.

## Main navigation

**Home** · **Tasks** · **Assets** · **Operations** · **Insights** (visible destinations depend on role). Persistent search, clear notifications, profile/settings, and a context-aware create action.

### Operations Home (command center)

1. Greeting, user role, active site, connectivity/sync status.
2. High-priority **Action Deck**: overdue work, incidents needing review, jobs requiring approval.
3. Real, calculated KPIs: open work orders, due today, overdue tasks, assets with active faults.
4. **Operations Pulse** timeline: recent changes from actual persisted events.
5. Quick actions: New Task, Report Issue, Scan Asset, Start Inspection.
6. Themed, restrained entry motion; empty states tell users how to create their first real record.

### Mission Control: tasks and work orders

Preserve existing task-assigner behavior and data. Upgrade around these workflows:

- Create/edit/assign/reassign work with responsible person, priority, deadline, location, asset, description, and attachments.
- Record state transitions: Open → Assigned → In Progress → Blocked → Pending Review → Completed/Rejected.
- Each transition includes a real actor, timestamp, and persisted activity entry.
- Search, filter, sort, saved views, overdue badges, due-soon reminders.
- Attach before/after photos; add checklist confirmations, labor time, notes and signatures where appropriate.
- Manager/supervisor approval rules enforced by backend security policy, not hidden buttons alone.
- On tablet: drag-and-drop dispatch view if an equally functional non-drag accessible alternative exists.

### Asset Passport (high-value differentiator)

A QR-linked equipment record: asset name and ID, physical location, maintenance history, current status, open tasks, inspection records, and approved manuals. Scan locally on supported devices and open a **real database record**. Clearly handle unknown codes and permission errors.

### Shift Pulse

A practical handover workspace: outgoing notes, unresolved work, safety concerns, equipment exceptions, incoming owner, acknowledgement and timestamps. Generate a summary **deterministically from real persisted events**, without depending on a cloud AI request.

### ProofPack

Close-out package assembled from existing work-order records: work completed, assignee, timestamps, before/after images, checklist, sign-off, follow-up items and optional PDF export. Reflect actual data and missing evidence honestly.

### Other industrial features — phased

- Preventive maintenance recurring schedules with generated work orders.
- Incident and near-miss reporting with attachments and escalation rules.
- Digital inspection checklists with readings, pass/fail rules, versioned templates.
- Parts and consumables stock movement linked to asset/work order.
- Transparent severity scoring based on **configurable rules**, never market it as trained AI.
- Meaningful performance trends and exports based on valid data; no fictional OEE or machine telemetry.
- For real hardware/IoT integration: require an actual connector and data provenance before showing 'live' metrics.

## Functional and architectural principles

**Protect existing code.** Begin with a full audit of the actual source and Firebase data models. Reuse working authentication, role assignment, storage, and navigation. Preserve data and provide backward-compatible migrations.

**Fast, offline-capable UI.** Bundle brand assets and onboarding artwork inside the app. Use cached/local data first; when online synchronization exists, run it in the background. Firestore's mobile offline support can be used where compatible, but design for real conflict handling and clearly label unsynchronized edits. Sensitive records require appropriate access control, secure storage decisions, and logout handling.

**Real buttons only.** Every action must navigate to a real route or perform a verifiable CRUD operation with validations, loading/error/success states, real persistence, and tests. Disable and explain unavailable features rather than pretending.

**Security.** Enforce authorization at the data layer; isolate company/site data; audit who changed what; keep credentials in secure development and deployment configuration, never in public Git history.

**Rendering.** Prefer Flutter native implicit/explicit animations, lazy lists, precached local imagery, bounded animated subtrees, and profile-mode performance verification. Respect OS reduced-motion and dynamic text.

## Suggested delivery order

| Phase | Scope | Release gate |
|---|---|---|
| **P0 — Audit** | Inspect source, record existing screens, dependencies, Firebase models and critical flows | No functional regression |
| **P1 — Foundation** | Design tokens, typography, navigation, fast local splash, one-time tour | Tour is one-time and fully offline |
| **P2 — Core UX** | Command Center + existing task assigner, refined detail pages | End-to-end create → assign → complete succeeds |
| **P3 — Sellable features** | Asset Passport, Shift Pulse, ProofPack | Each workflow works with persisted, real records |
| **P4 — Industry expansion** | Preventive maintenance, inspections, incidents, inventory | Role, offline, sync and error cases tested |
| **P5 — Quality** | Accessibility, performance, integration testing, APK release validation | No blocking failures or dead actions |

## Non-negotiable acceptance criteria

- ✅ No lost existing app features, user records, or task assignments.
- ✅ No network dependency for logo reveal, introductory carousel, or bundled images.
- ✅ No dead buttons, duplicated fake metrics, placeholder charts, or false success messages.
- ✅ No publicly uploaded source code, secrets, service credentials, or proprietary customer data.
- ✅ Automated tests cover key task states and role restrictions.
- ✅ Release/profile builds measured for jank, startup, and image-memory behavior.
- ✅ Workflows handle loading, empty data, validation, offline, errors, and permission denial.
- ✅ The public GitHub showcase clearly labels planned features versus verified implementations.

---

### References that informed this direction

- [Flutter offline-first architecture](https://docs.flutter.dev/app-architecture/design-patterns/offline-first)
- [Flutter performance best practices](https://docs.flutter.dev/perf/best-practices)
- [Flutter adaptive and responsive design](https://docs.flutter.dev/ui/adaptive-responsive)
- [Cloud Firestore offline persistence](https://firebase.google.com/docs/firestore/manage-data/enable-offline)
- [UpKeep mobile CMMS](https://upkeep.com/product/mobile-cmms/)
- [UpKeep work order management](https://upkeep.com/product/work-order-management)

**This is an implementation blueprint, not an announcement that Nexus Ops 2.0 has shipped.**
