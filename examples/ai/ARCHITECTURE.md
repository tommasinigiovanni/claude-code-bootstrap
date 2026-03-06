# Architecture — 3D Printer Booking System

## Overview

Server-rendered Next.js 14 application with App Router. PostgreSQL for persistence via Prisma ORM. Auth.js for authentication. Deployed on Vercel with Neon PostgreSQL.

## Tech Stack

- **Runtime:** Node.js 20 LTS
- **Framework:** Next.js 14.2 (App Router, Server Components)
- **Language:** TypeScript 5.4 (strict mode)
- **Database:** PostgreSQL 16 (Neon serverless)
- **ORM:** Prisma 5.x
- **Auth:** Auth.js 5 (GitHub + Google providers)
- **UI:** TailwindCSS 3.4 + shadcn/ui
- **Testing:** Vitest (unit) + Playwright (e2e)
- **Package Manager:** pnpm 9.x

## Folder Structure

```
├── app/
│   ├── layout.tsx              # Root layout with auth provider
│   ├── page.tsx                # Landing / redirect to calendar
│   ├── calendar/
│   │   └── page.tsx            # Main calendar view
│   ├── bookings/
│   │   └── page.tsx            # User's bookings list
│   ├── admin/
│   │   ├── layout.tsx          # Admin guard
│   │   ├── printers/page.tsx   # Printer management
│   │   └── bookings/page.tsx   # All bookings overview
│   └── api/
│       ├── bookings/route.ts   # CRUD bookings
│       ├── printers/route.ts   # CRUD printers
│       └── auth/[...nextauth]/ # Auth.js handlers
├── components/
│   ├── ui/                     # shadcn/ui base components
│   ├── calendar-grid.tsx       # Weekly slot grid
│   ├── booking-modal.tsx       # Booking creation dialog
│   ├── printer-card.tsx        # Printer info card
│   └── nav.tsx                 # Navigation bar
├── lib/
│   ├── db.ts                   # Prisma client singleton
│   ├── auth.ts                 # Auth.js config
│   ├── slots.ts                # Slot calculation logic
│   └── utils.ts                # Shared utilities
├── prisma/
│   └── schema.prisma           # Database schema
├── tests/
│   ├── unit/                   # Vitest unit tests
│   └── e2e/                    # Playwright e2e tests
└── public/                     # Static assets
```

## Data Model

```
User
  id          String   @id @default(cuid())
  email       String   @unique
  name        String?
  role        Role     @default(MEMBER)  // MEMBER | ADMIN
  bookings    Booking[]

Printer
  id          String   @id @default(cuid())
  name        String                      // "Prusa MK4 #1"
  status      PrinterStatus @default(ACTIVE)  // ACTIVE | MAINTENANCE | RETIRED
  bookings    Booking[]

Booking
  id          String   @id @default(cuid())
  userId      String
  printerId   String
  startTime   DateTime
  endTime     DateTime
  status      BookingStatus @default(CONFIRMED)  // CONFIRMED | CANCELLED
  createdAt   DateTime @default(now())

  @@unique([printerId, startTime])        // Prevents double-booking at DB level
```

## API Design

| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/bookings?printerId=X&week=Y` | Get bookings for a printer/week |
| POST | `/api/bookings` | Create a booking |
| DELETE | `/api/bookings/[id]` | Cancel a booking |
| GET | `/api/printers` | List all printers |
| POST | `/api/printers` | Add a printer (admin) |
| PATCH | `/api/printers/[id]` | Update printer status (admin) |

## Main Flows

**Booking flow:** User opens calendar → selects printer tab → clicks empty slot → confirms time block (1h/2h/4h) → booking created → confirmation shown + slot turns occupied.

**Cancellation flow:** User opens "My Bookings" → clicks cancel on future booking (>2h from now) → confirmation dialog → booking status set to CANCELLED → slot freed.

**Admin flow:** Admin opens admin panel → can view all bookings, add/remove printers, set printer to maintenance (blocks all future slots).

## Architectural Decisions

- **Server Components default:** reduces client JS bundle. Only interactive components (calendar grid, modals) use `'use client'`.
- **DB-level unique constraint** on `[printerId, startTime]` as the ultimate double-booking prevention, not just application logic.
- **UTC storage:** all times stored in UTC, converted to local timezone only in the UI layer.
- **No real-time:** polling every 30s on calendar view is sufficient for a 4-printer FabLab. WebSockets would be overengineering.

## Security

- All API routes check session via Auth.js `auth()` helper
- Admin routes additionally check `user.role === 'ADMIN'`
- Booking cancellation checks ownership (`booking.userId === session.user.id`)
- No raw SQL — all queries through Prisma (prevents SQL injection)

## Performance Considerations

- Calendar view pre-fetches current + next week data
- Prisma query uses indexed `printerId + startTime` for slot lookups
- Static pages (landing, about) are ISR-cached
