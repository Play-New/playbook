# RecipeBox — Build Plan

The plan is the tests. Written from the vision conversation, before any product code.

## Vision Summary

A home cook takes a photo of their grocery receipt on WhatsApp. Within a minute, they get back recipe suggestions matched to what they just bought, adjusted for their dietary preferences. The conversation is warm, specific, helpful — like a knowledgeable friend, not a search engine. The web archive is for browsing saved recipes later, not for primary interaction. The product lives where the user already is: their phone, their messaging app.

## What We Would NOT Build

- Recipe creation UI. Users don't create recipes — the system matches them.
- Complex filtering on the web. Three ways to browse: recent, collections, search. That's it.
- Admin panel. Solo developer product. No roles, no multi-tenancy.
- Real-time updates on the web. The web is an archive. Static read.
- Notification settings per channel. WhatsApp is primary, email is weekly. Fixed.

## Test Suite

Tests installed: vitest (integration for agent/workflow/enrichment), Playwright (e2e for web archive).

### Conversational Tests (vitest — agent interaction)

```
// Core flow — the reason the product exists
test('user sends receipt photo, receives recipe suggestions within 60 seconds')
test('recipe suggestions match ingredients from the receipt, not random recipes')
test('suggestions respect dietary preferences without the user repeating them')

// Agent personality
test('agent response leads with the recipe name and match score, not with explanation')
test('agent uses warm, specific language — "Thai basil chicken works with 8 of your 10 ingredients" not "Based on your input, I found matches"')
test('when user asks to substitute, agent responds with the alternative and the adjusted recipe in one message')
test('agent asks clarification with a default — "Spicy ok? I will assume yes" not "What spice level do you prefer?"')

// Edge cases the user will notice
test('when receipt photo is blurry, agent says what it could read and asks about the rest — no silent failure')
test('when no good recipe matches, agent suggests what to add — not "no results found"')
test('agent never asks "is there anything else?" — the conversation ends when the user stops talking')

// Delivery
test('weekly email summary groups saved recipes by cuisine, most recent first')
test('email and WhatsApp use the same recipe names and terminology')
```

### E2E Tests (Playwright — web archive)

```
// Web archive — secondary surface
test('recipe archive shows saved recipes as cards with the availability badge')
test('availability badge shows ingredient match — "8/10 ingredients" — the signature')
test('search finds recipes by name, ingredient, or cuisine')
test('recipe detail shows full recipe with substitution notes from the agent conversation')

// Character
test('body text is 18px — kitchen reading distance')
test('single column layout, no multi-column grids')
test('recipe cards feel warm and generous — photos are large, text is readable')

// Settings
test('settings shows dietary preferences, connected accounts, and prompt inspector')
test('prompt inspector shows what the agent is instructed to do, in readable language')
```

### Integration Tests (vitest — enrichment/inference)

```
// Enrichment
test('receipt OCR extracts ingredient names and quantities from a photo')
test('pantry photo recognition identifies items on a shelf')

// Inference
test('recipe matching scores recipes against current pantry, sorted by match percentage')
test('matching penalizes missing key ingredients more than missing garnishes')

// Graduation marker
test('recipe parser handles the standard formats without LLM call — reserved for unusual layouts')
```

## Build Order (derived from test dependencies)

1. Schema + auth (users, recipes, ingredients, pantry_items, collections)
2. Enrichment (WhatsApp webhook, OCR, pantry recognition — unblocks everything)
3. Inference (recipe matching workflow — unblocks agent tests)
4. Agent (tools first, then orchestration, prompts in dedicated files)
5. Delivery (WhatsApp handling via agent, email weekly summary)
6. Web archive (layout, recipe cards with availability badge, search, settings)

## Key Design Decisions

- The availability badge ("8/10 ingredients") is built with the recipe component, not added later. It's the signature.
- 18px body text. Kitchen reading distance. From the type scale, embedded from the first component.
- No multi-column layouts. Recipe reading is linear.
- The agent's tone matches the design direction: warm, specific, helpful. Not terse, not chatty.
- Prompts live in `/prompts/`, each with a comment explaining its purpose. Settings UI shows a readable version.
- WhatsApp is the primary surface. The web archive exists because message history is not a good recipe book.
