# Data Model & Access Control

## Conceptual data model

### Core entities
- **User**: A person with an account. Tracks role progression (subscriber → purchaser → member).
- **AnonymousVisitor**: Not stored as a first-class entity; may be represented by a temporary session until they subscribe.
- **EmailSubscription**: Lead magnet and newsletter subscriptions tied to an email address and (optionally) a User.
- **ContentItem**: MDX-based content entries (essays, excerpts, Extras).
- **Book**: Memoir volume metadata (title, slug, release state).
- **ContentBook**: Join table between ContentItem and Book for cross-book Extras.
- **Purchase**: One-time transaction for Extras content.
- **Subscription**: Future membership subscriptions (Stripe).
- **Entitlement**: Access grants tied to a User (or email) that specify which content tiers are accessible.

### Key relationships
- User **1—n** EmailSubscription (optional linkage by email).
- User **1—n** Purchase.
- User **1—n** Subscription.
- User **1—n** Entitlement.
- ContentItem **n—n** Book via ContentBook.
- Purchase **n—n** ContentItem via PurchaseItem (for multi-item Extras bundles).

## Access-control logic (server-side only)
- Access decisions are derived from **entitlements**, purchases, and subscription status on the server.
- The client never decides access; the server checks the ContentItem access level against the User’s entitlements and active subscription.
- Upgrades are additive: when a user purchases or subscribes, new entitlements are granted without removing previous ones.

## Access tiers
- **PUBLIC**: Always accessible.
- **LEAD_MAGNET**: Requires EmailSubscription.
- **PURCHASE**: Requires Purchase entitlement for the specific ContentItem or bundle.
- **MEMBER**: Requires active Subscription.

## Prisma schema (conceptual)

```prisma
enum AccessTier {
  PUBLIC
  LEAD_MAGNET
  PURCHASE
  MEMBER
}

enum SubscriptionStatus {
  ACTIVE
  PAST_DUE
  CANCELED
  TRIALING
}

enum PurchaseStatus {
  PAID
  REFUNDED
}

model User {
  id              String           @id @default(cuid())
  email           String           @unique
  createdAt       DateTime         @default(now())
  updatedAt       DateTime         @updatedAt
  subscriptions   Subscription[]
  purchases       Purchase[]
  entitlements    Entitlement[]
  emailSubs       EmailSubscription[]
}

model EmailSubscription {
  id              String    @id @default(cuid())
  email           String    @unique
  userId          String?
  leadMagnetSlug  String?
  createdAt       DateTime  @default(now())
  user            User?     @relation(fields: [userId], references: [id])
}

model Book {
  id          String        @id @default(cuid())
  title       String
  slug        String        @unique
  createdAt   DateTime      @default(now())
  content     ContentBook[]
}

model ContentItem {
  id          String        @id @default(cuid())
  slug        String        @unique
  title       String
  accessTier  AccessTier    @default(PUBLIC)
  mdxPath     String
  createdAt   DateTime      @default(now())
  updatedAt   DateTime      @updatedAt
  books       ContentBook[]
  purchaseItems PurchaseItem[]
}

model ContentBook {
  contentId   String
  bookId      String
  content     ContentItem  @relation(fields: [contentId], references: [id])
  book        Book         @relation(fields: [bookId], references: [id])
  @@id([contentId, bookId])
}

model Purchase {
  id          String        @id @default(cuid())
  userId      String
  status      PurchaseStatus
  stripeId    String        @unique
  createdAt   DateTime      @default(now())
  user        User          @relation(fields: [userId], references: [id])
  items       PurchaseItem[]
}

model PurchaseItem {
  purchaseId  String
  contentId   String
  purchase    Purchase     @relation(fields: [purchaseId], references: [id])
  content     ContentItem  @relation(fields: [contentId], references: [id])
  @@id([purchaseId, contentId])
}

model Subscription {
  id          String            @id @default(cuid())
  userId      String
  status      SubscriptionStatus
  stripeId    String            @unique
  currentPeriodEnd DateTime
  createdAt   DateTime          @default(now())
  user        User              @relation(fields: [userId], references: [id])
}

model Entitlement {
  id          String     @id @default(cuid())
  userId      String
  accessTier  AccessTier
  bookId      String?
  contentId   String?
  createdAt   DateTime   @default(now())
  user        User       @relation(fields: [userId], references: [id])
  book        Book?      @relation(fields: [bookId], references: [id])
  content     ContentItem? @relation(fields: [contentId], references: [id])
}
```
