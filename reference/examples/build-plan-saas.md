# FleetPulse — Build Plan

The plan is the tests. Written from the vision conversation, before any product code.

## Vision Summary

A fleet manager glances at one screen and knows if anything needs attention. Vehicles sorted by urgency. The attention count in the header is the heartbeat of the product. Dense, operational, calm. WhatsApp for drivers to report, email for daily summaries. The web surface shows what messages cannot: maps, trends, timelines.

## What We Would NOT Build

- Onboarding wizard. Fleet managers are set up by admin. One config, done.
- Dashboard builder. The attention count and fleet map are fixed. They work because the design chose them.
- Export page. A button on Reports, not a screen.
- Notification preferences page. Thresholds and channels live in Settings. One place.

## Test Suite

Tests installed: Playwright (e2e for visual flows), vitest (integration for enrichment/inference).

### E2E Tests (Playwright)

```
// Fleet Overview — the primary screen
test('fleet overview shows attention count in the header, visible without scrolling')
test('vehicles sorted by urgency — most critical first, not alphabetical')
test('clicking a vehicle navigates to detail with history and current status in one view')
test('when no vehicles need attention, the overview says "all clear" — no empty table')
test('attention count updates within 60 seconds of a new alert')

// Signature — attention count
test('attention count is amber when vehicles need attention, neutral when all clear')
test('attention count is visible on every page, not just the overview')

// Density
test('table rows are compact — the manager sees 20+ vehicles without scrolling')
test('fleet map shows vehicle positions with status indicators, not generic pins')

// Settings
test('settings page has polling frequency, alert thresholds, notification channels — nothing else')

// Responsive
test('fleet overview is usable on a tablet in landscape — the field use case')
```

### Integration Tests (vitest)

```
// Enrichment
test('telematics polling normalizes GPS, fuel, speed into vehicle state')
test('email receipt parser extracts maintenance records from forwarded shop emails')
test('WhatsApp media intake processes driver photos and attaches to vehicle record')

// Inference
test('fuel baseline flags vehicles deviating more than 15% from their historical average')
test('maintenance prediction identifies vehicles approaching service threshold')
test('idle detection flags vehicles stationary for more than configured threshold')

// Interpretation
test('insight includes comparison to fleet average, not just the raw number')
test('insight includes 4-week trend direction')
test('recommended action is specific — "schedule maintenance" not "review vehicle"')

// Delivery
test('WhatsApp alert sent only during business hours, queued otherwise')
test('daily email summary groups by urgency, most critical first — same order as the overview')
test('email and WhatsApp use the same terminology for vehicle status')
```

## Build Order (derived from test dependencies)

Infrastructure → intelligence → surfaces. The tests define what each piece must do.

1. Schema + auth (unblocks everything)
2. Enrichment connectors (telematics, email, WhatsApp — unblocks inference tests)
3. Inference analyzers (fuel, maintenance, idle — unblocks interpretation tests)
4. Interpretation formatting (unblocks delivery and visual tests)
5. Delivery (WhatsApp alerts, email summary)
6. Visual surfaces (overview with attention count first, then detail, then the rest)
7. Settings

## Key Design Decisions

- The attention count appears before any other surface is built. It's the signature.
- Table rows are 36px, not 48px. Dense product. The design system says so.
- No card-heavy dashboard. The overview's focal point is the attention count + map.
- Amber for "needs attention", not red. Red is reserved for emergencies.
- WhatsApp and email use the same voice as the web surface. Same product, different channel.
