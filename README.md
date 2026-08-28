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

Top-level pages: **Grouping** (booking import/window, plus Timeline/Group Check/Summary/Review
sub-tabs) · **Butler Schedule** (one card per travel party) · **PSI** (condensed one-row-per-service
export view, with the CSV export button) · **Read Me**.

## What's new (v1.5.0)

The **Review** queue got three changes. Buttons are now labelled with the action they perform —
*Group together* instead of Confirm, *Not one party* instead of Dismiss — since the screen hosts
two different queues that each had their own meaning of "Confirm"/"Dismiss"; the original verb
names live in the tooltips. Suggested Duplicates gained a second action, **Group together**, which
clears the duplicate flag *and* groups the bookings in one step — for the case where they're
genuinely separate travellers who happen to have been booked identically. And every booking number
is now clickable, opening a **detail modal** with the full record: the contact fields the matcher
keys off, flight and passenger detail, notes, and the run's own grouping state including the
evidence string explaining why the engine decided what it did.

## Previously (v1.4.0)

Fixed the **Assign butler** button (and Butler Schedule's Save/Email-sent controls, and PSI's
Confirm/Dismiss/Mark Individual): the shared click-handling logic was still scoped to the old
results panel from before Butler Schedule and PSI became their own top-level pages, so their
buttons silently did nothing. **Email sent** is now a checkbox instead of a toggle button.
Gate/Baggage Belt/Check-in Counter are confirmed sourced from a new airport-ops-data-feed
integration in the real system — still shown as a dimmed "—" here since this demo's data doesn't
carry them.

**Butler Schedule** (v1.3.0) shows one card per travel party, following a Backoffice booking-card
mockup. Group cards show a booking-count badge and a staff-editable group name; every card
quantity is already the party-level total, not one member's figures. Each card also has a
per-field icon row confirmed against separate Arrival/Departure mockups (Gate, Passengers,
Checked/Oversized Baggage, and Pets on both; Baggage Belt, Buggy, and Flower on Arrival only;
Check-in Counter and Lounge on Departure only) — only Passengers and Buggy are real in this demo's
data, the rest show a dimmed "—" since the sample GG19/AOT export has no columns for them.

Butler Schedule and PSI were also promoted from tabs nested under a single page to their own
top-level pages, and that page was renamed from "Grouping & PSI" to plain **Grouping**.

## Source

This is a build artifact filed in a separate private knowledge-base repo, where the full
requirement it illustrates is documented. This repo exists solely to host the demo via GitHub
Pages; it has no other history or content.
