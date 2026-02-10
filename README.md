# Memoir

Custom author platform for the literary memoir series **High Mountains, Deep Oceans** by Todd Forney.

## Planning
- [Platform plan](docs/platform-plan.md)

## Non-Goals

This platform will deliberately avoid:
- Aggressive growth hacks or manipulative funnel mechanics
- Ad-based monetization
- Social-media-first content strategies
- Dependency on proprietary no-code platforms
- Short-lived trends over durable systems

## Editorial Model (MDX vs CMS)

Editorial content will default to **MDX stored in Git** for durability, versioning, and long-term preservation.

A headless CMS (e.g. Sanity or Payload) will only be introduced if non-technical editorial workflows become necessary. The platform should not depend on a CMS by default, and CMS adoption must not compromise narrative integrity or archival clarity.

## Narrative Integrity

All features must preserve narrative continuity and emotional pacing.

No system should fragment the reader’s experience, introduce unnecessary urgency, or prioritize conversion over trust.

If a feature introduces noise, distraction, or pressure, it should be rejected or redesigned.

## System Boundaries

The README is the source of truth for what belongs where in this platform. Boundaries are strict to protect the reading experience and long-term durability.

### Frontend (rendering, layout, reading experience)
- Longform reading and archive navigation.
- Lead magnet landing experience and on-site copy.
- Member area presentation (what a reader sees after access is granted).
- Performance, accessibility, and SEO-related rendering decisions.

### Backend (auth, payments, access control, data)
- Authentication and session management.
- Payments for Extras and membership tiers.
- Access control and content gating.
- Transactional email delivery for the lead magnet.
- Purchase history and membership state.

### Content-only (MDX, editorial files)
- Essays, excerpts, and archival entries stored as MDX in Git.
- Narrative sequencing, tags, and editorial metadata.
- Long-term preservation of the literary corpus.

### Explicitly not in Phase 1
- Membership subscriptions and member-only archive.
- Paid Extras commerce and purchase history.
- Archive explorer features beyond basic navigation (themes, timeline, cross-links).
- Speaking/coaching funnels and inquiry workflows.

## Phase 1 Scope

### Phase 1 inclusions
- Lead magnet landing page and delivery flow.
- Email list capture and basic automation.
- Basic essay publishing and reading experience.
- Minimal brand system (typography, palette, layout).

### Phase 1 exclusions
- Paid Extras commerce and gating.
- Membership tiers and subscriber-only content.
- Advanced archive explorer capabilities.
- Speaking/coaching expansion pathways.
