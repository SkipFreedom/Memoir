# High Mountains, Deep Oceans — Platform Plan

## Understanding
This platform is a long-term literary home for a memoir series, not a marketing site. The experience should feel calm, trustworthy, and archival, with a minimalist aesthetic and a focus on performance, SEO, and longevity over short-term growth tactics. The platform must support:

- Email capture via a free literary lead magnet.
- Paid “Extras” content (deleted scenes, deep dives).
- A membership program with ongoing content.
- Book sales and pre-orders.
- Future expansion into speaking and coaching.

## Recommended tech stack
**Frontend**
- **Next.js (App Router) + TypeScript**: server-rendered performance, SEO, and a durable content architecture.
- **Tailwind CSS + CSS variables**: minimalist theme control and rapid iteration.
- **MDX** for longform essays, excerpts, and archival posts.

**Backend & content**
- **PostgreSQL** for membership data, content gating, and purchase history.
- **Prisma** for schema evolution and query safety.
- **Auth.js** for secure, flexible authentication.
- **Stripe** for payments (one-time Extras purchases and memberships).
- **Resend** or **Postmark** for transactional email.

**Hosting & infrastructure**
- **Vercel** (or Fly.io if deeper server control is needed).
- **S3-compatible storage** for media and archival files.
- **Plausible** or **Fathom** for privacy-respecting analytics.

**Editorial tooling**
- **Sanity** or **Payload** if you want a visual CMS; otherwise MDX in Git for maximum durability.

## High-level system architecture
1. **Public reading layer**
   - SSR for main essays, excerpts, and lead magnet pages.
   - Fast, accessible typography and longform readability.

2. **Membership layer**
   - Auth + Stripe subscription.
   - Content gating for Extras and member-only essays.
   - Account area for downloads, access history, and profile.

3. **Commerce layer**
   - Book sales and pre-orders (one-time Stripe checkouts).
   - Purchase ledger stored in Postgres.

4. **Editorial layer**
   - Versioned MDX content or CMS-backed longform entries.
   - Searchable archive with tags, themes, and timelines.

5. **Email & lead magnet**
   - Lead magnet delivery via email automation.
   - Segmentation for readers, members, and buyers.

## Phased build plan
### Phase 1 — MVP (8–12 weeks)
- Landing page with lead magnet download flow.
- Email list and lead magnet automation.
- Basic blog/essay publishing.
- Minimal brand system: typography, palette, layout.

### Phase 2 — Commerce + Extras (6–10 weeks)
- Stripe checkout for Extras (one-time purchases).
- Content gating for paid Extras.
- User accounts and purchase history.

### Phase 3 — Membership (6–12 weeks)
- Subscription tier(s).
- Members-only archive and ongoing content stream.
- Community-touch features (comments or letter-to-author). 

### Phase 4 — Archive & expansion (ongoing)
- Full archive explorer (themes, timeline, cross-links).
- Speaking/coaching funnels and inquiry forms.
- Deep SEO enhancements and long-term preservation tooling.
