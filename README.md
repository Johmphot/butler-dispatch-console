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
sub-tabs, Group Check first) · **Butler Schedule** (one card per travel party) · **PSI** (condensed one-row-per-service
export view, with the CSV export button) · **Ops Guide** · **Read Me**.

## What's new (v1.20.0)

**Grouping now enforces the requirement's membership rule: one group holds one flight.** The
**Group** and **Merge groups** buttons compare Airport, Flight Type, Flight Number and Flight Date
across the selection, and the disabled button says which of them stopped it. Flight numbers are
compared normalised, so `BR 67`, `BR67` and `BR0067` count as one flight. Merge checks every member
of each selected group, not only the ticked rows, so two groups on different flights can no longer
be combined.

**Meeting Date is no longer compared** — deliberately. One departure's passengers met either side of
midnight share a flight date and belong in the same group; up to v1.19.0 the prototype had this
inverted, blocking on meeting date while ignoring the flight entirely.

⚠️ The requirement exempts **Custom Flights** from the flight-number match and only warns. This demo
does not model Custom Flights at all, so here a differing number always blocks.

## Previously (v1.19.0)

**The Group button opens a name box**, pre-filled with the suggested default
`<Partner> (<total pax> pax)` when every selected booking shares one partner, and left blank when
they do not. The pax count is frozen at that moment.

**Merge groups lets you choose which group survives.** The confirm dialog carries a survivor
dropdown, defaulting to the first selected booking's group, and states which reference and name
carry across; the name stays editable there. Absorbed references are retired and never re-minted.

## Previously (v1.18.0)

**The dispatch sheet now downloads as a real Excel workbook (`.xlsx`)** instead of CSV. It opens
straight in Excel with a bold, frozen header row and a filter already on, the task number as a
number, and wrap-text so multi-line cells (booking numbers, contact, passengers) show their line
breaks. It is built in the browser from the zip library the page already embeds for reading
uploads, so the file still runs fully offline with no new dependency.

## Previously (v1.17.0)

**Phase 1's bulk bar, on Group Check.** Booking Management's bulk actions are adapted onto this
screen so the behaviour can be tried before it's built: **Group**, **Ungroup**, **Rename group**,
**Merge groups** and the four-field **bulk group edit**. The bar floats over the bottom of the
window once something is selected. Tick bookings, or click a group to select all of it. The rules
are live too — a disabled **Group** button names the first blocking condition it hits (differing
meeting dates, differing airports, mixed arrivals and departures, a booking that isn't Confirmed or
Processing, one already in a group), and two warnings warn-and-allow: a partial-group selection, and
a meeting-time spread over 3 hours. No Undo, per the 2026-09-01 decision.

**`GroupBookingID` is now Option 1** — `G<Airport>-<6-digit running>`, e.g. `GP-000123`, selected by
Airport Operations on 2026-09-11. No date component, one continuous counter per airport, and the ID
is minted once and **never changes**. ⚠️ No service date can be read off it, unlike the superseded
format.

**Group Status split into `Round Type` and `Grouping Source`** (the 2026-09-09 decision). Round Type
is Group/Individual; Grouping Source records **origin** — Auto, Confirmed, Review, Manual, plus
Adjusted and Duplicate — and deliberately doesn't flip to Manual when staff edit an auto-formed
group. In the export, Round Type takes the old column position and Grouping Source is appended last.

**Group Check reads as rounds, not rows.** It's now the tab the Grouping page opens on. A round's
task number prints once with a `×N` count and its members are joined by a dotted rule; consecutive
groups alternate between two tints so neighbours don't merge; solo bookings carry no tint at all. A
suggestion carries **Group together** / **Separate bookings** once per round, and its member rows
carry no action at all until the cluster is decided.

**Also**: the Group column leads with the GroupName over the reference, on both Group Check and PSI;
`GroupName` is now its own export column beside `GroupBookingID`; and a bug is fixed where
**Separate bookings** dropped the suggestion from the queue but left the bookings grouped, stuck
reading Group / Review.

## Previously (v1.10.0)

The Review queue's dismiss action is relabelled **Separate bookings** (was "Not one party"); the
spec's own verb, *Dismiss*, still lives in the tooltip. On the PSI table you can now click
**anywhere in a clipped row** to expand it, not just the small Expand button — buttons, links and
text fields inside the row keep their own behavior, and a click that's really a text selection
won't collapse the row mid-read. Only rows with something hidden get the pointer cursor. The
gradient fade behind the "…more" marker is gone; it read as a smudge over the last line of text
rather than as a hint that more follows.

## Previously (v1.9.1)

A new **Ops Guide** page carries a step-by-step walkthrough for Airport Operations staff — loading
an export, choosing the grouping logic, clearing the Review queue, verifying in Group Check, then
producing the dispatch sheet — plus a badge glossary and a plain-language account of what the
matching engine keys off.

**Butler Schedule and PSI** got a field rework. **Contact** now shows contact name, contact email
and contact phone. **Pax Name** runs one line per passenger, formatted `Name | Nationality | Phone`,
with `-` standing in for a blank nationality or phone; nationality reads the real export's own
`Pax 1 Nationality` column, which the demo data leaves blank. The **Details** column is renamed
**Special Request**, which is the only thing it ever carried.

On the PSI table, Expand/Collapse was widened from the Special Request cell to the **whole row**, so
booking numbers, contact, passengers and special request open together behind one toggle. Each cell
that is actually cut off now carries its own **"… more"** marker, measured from the laid-out DOM
rather than guessed from text length, so it tracks the real column width and follows a resize.

## Previously (v1.5.0)

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
