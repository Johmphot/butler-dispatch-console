# Butler Dispatch Console

**Live demo:** https://johmphot.github.io/butler-dispatch-console/

Interactive, single-file HTML/JS prototype comparing two ways of grouping split travel-party
bookings for SAWASDEE Pass's Personal Butler service:

- **GG19 skill (original)** — the shipped `butler-booking-grouping` skill's 5-tier heuristic
  detector (split codes, hidden split codes, multi-pax rows, note cross-references, phone/name
  matching).
- **APS matching engine (new)** — the proposed Group Booking Reference requirement's matching
  engine: primary-signal clustering, 5-field corroboration, a stricter Contact Email + Pax 1 Phone
  auto-group bar, Duplicate Detection, and a staff-action layer (Mark/Unmark Individual, Merge
  Groups, Link/Unlink Connecting Itinerary, and a combined Review queue for grouping suggestions
  and duplicates).

Runs entirely client-side — no build step, no server, no dependencies fetched at runtime (fonts
and the xlsx-unzip library are embedded inline). Upload a `.xlsx` booking export, paste raw CSV,
or click **Load sample export** to try it with demo data.

Tabs: **Timeline** (Gantt-style overview) · **Butler Schedule** (one card per travel party — badge
+ editable group name, contact/pax info, and Assigned/Email Sent status dots) · **PSI** (condensed
one-row-per-service export view) · **Group Check** (full matching detail + Merge/Link actions) ·
**Summary** (butler/status breakdowns) · **Review** (grouping-suggestion and duplicate queue,
APS engine only).

## What's new (v1.1.0)

Added the **Butler Schedule** tab: a per-round card view following a Backoffice booking-card
mockup. Group cards show a booking-count badge and a staff-editable group name; every card
quantity (pax, suggested butler count) is already the party-level total, not one member's figures.
Assigned/Email Sent are demo-only toggle buttons — the prototype has no real butler-assignment
state, so they just show what the two confirmed status dots look like independently.

## Source

This is a build artifact filed in a separate private knowledge-base repo, where the full
requirement it illustrates is documented. This repo exists solely to host the demo via GitHub
Pages; it has no other history or content.
