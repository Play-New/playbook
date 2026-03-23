# Performance Audit Guide

Full performance checklist for the review command. Init mode establishes baseline targets and tooling. Review mode measures against those targets.

## Init Mode

When `.superskills/report.md` has no Performance Budget section, establish the baseline.

**Derive targets from the product's context.** Not every product has the same performance needs:
- A visual SaaS used daily needs tight Core Web Vitals (LCP, CLS, INP) because users notice lag on pages they visit hundreds of times.
- A CLI tool cares about execution time and output speed, not web vitals.
- A conversational product cares about response latency — how fast the agent or workflow returns an answer.
- A background workflow cares about throughput and cost, not user-facing latency.

For products with visual surfaces, use Google's Core Web Vitals thresholds as starting points (LCP < 2.5s, CLS < 0.1, INP < 200ms) and set bundle and API response targets based on the product's complexity. For non-visual products, define equivalent targets: CLI execution time, agent response time, workflow throughput.

Configure the appropriate measurement tools for the stack. Write the budget to `.superskills/report.md` Performance Budget section.

## Review Mode

When targets exist, measure against them.

### Bundle Analysis

- Run the framework build command and check output sizes
- Identify largest dependencies (anything >50KB gzipped deserves scrutiny)
- Missing dynamic imports for heavy libraries (chart libs, editors, syntax highlighters)?
- Tree-shaking working? Check for barrel file imports that pull entire packages
- Duplicate dependencies in the bundle (two versions of the same library)?
- CSS bundle: unused styles, duplicate utility classes?

### Core Web Vitals

**LCP (Largest Contentful Paint, target < 2.5s):**
- Largest content paint sources: hero images, large text blocks, above-fold components
- Images without framework optimization (next/image, nuxt/image, etc.)?
- Fonts without framework font loading (next/font, etc.)?
- Render-blocking resources in the critical path?
- Server response time (TTFB) contributing to slow LCP?

**INP (Interaction to Next Paint, target < 200ms):**
- Main thread blocking: heavy synchronous operations during interaction?
- Long tasks (>50ms) triggered by user input?
- State updates that cause large re-renders?
- Third-party scripts blocking the main thread?

**CLS (Cumulative Layout Shift, target < 0.1):**
- Images without explicit dimensions (width/height or aspect-ratio)?
- Dynamic content injected above the fold after initial render?
- Fonts causing layout shift on swap?
- Ads, embeds, or iframes without reserved space?

### Database Performance

- N+1 query patterns (queries in loops, fetching related data one row at a time)
- Missing indexes on columns used in WHERE, ORDER BY, or JOIN clauses
- Over-fetching (SELECT * instead of specific columns)
- Under-batching (multiple single inserts instead of batch operations)
- Unindexed queries on user-facing paths (these block on page load)
- Large result sets without pagination or cursor-based fetching

### API Cost Analysis

- Count LLM call sites across the codebase
- Estimate tokens per call (input + output)
- Estimate $/month at projected volume
- Caching opportunities for repeated calls with identical or similar inputs
- Model selection: are expensive models used where cheaper ones would suffice?
- Streaming responses where appropriate (avoid holding connections open for full generation)?

## Stack-Adaptive Checks

Detect from package.json. Apply checks for each detected tool. For tools not listed below, identify their performance surface and apply equivalent checks.

### Database (Supabase, Prisma, Drizzle, etc.)

- Over-fetching? Select specific columns, not all.
- Batch operations instead of single inserts in loops?
- Indexes on filtered/sorted columns?
- Edge/serverless functions for compute-heavy queries?
- Connection pooling configured for serverless environments?

### Hosting (Vercel, Netlify, etc.)

- Static/incremental regeneration for semi-static pages?
- Edge runtime for latency-sensitive routes?
- Image optimization via platform tools?
- Font optimization via platform tools?
- CDN caching headers set correctly?

### Workflow engine (Inngest, Trigger.dev, etc.)

- Sleep/wait instead of polling loops?
- Batching for high-frequency events?
- Concurrency limits set to avoid overwhelming downstream services?
- Fan-out patterns instead of sequential processing?

### Framework (Next.js, Remix, Nuxt, etc.)

- Dynamic imports for heavy client components?
- Server-first rendering, client components only when needed?
- Streaming for slow data sources?
- Route-level static/dynamic configuration?
- Prefetching for likely navigation targets?
- Suspense boundaries around slow components?

## Output Format

| Category | Issue | Impact | Fix | Estimated Savings |
|----------|-------|--------|-----|-------------------|
| bundle/cwv/database/api-costs/caching | description | quantified | what to change | time or money saved |
