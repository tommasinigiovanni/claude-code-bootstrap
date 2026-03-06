# Implementation Plan — 3D Printer Booking System

## Phase 1: Project Setup — 30 min

### Objective
Scaffold Next.js project with all dependencies, Prisma schema, and base configuration.

### Files to Create
- `package.json`, `tsconfig.json`, `next.config.ts`
- `prisma/schema.prisma`
- `lib/db.ts` (Prisma client)
- `app/layout.tsx` (root layout)
- `app/page.tsx` (landing page)
- `.env.example`
- `.gitignore`

### Steps
- [ ] Initialize Next.js 14 with TypeScript and pnpm
- [ ] Install dependencies: Prisma, Auth.js, TailwindCSS, shadcn/ui
- [ ] Create Prisma schema with User, Printer, Booking models
- [ ] Configure Tailwind and shadcn/ui
- [ ] Create root layout with basic nav placeholder
- [ ] Push schema to database

### Verification
```bash
pnpm dev              # Dev server starts on :3000
pnpm db:push          # Schema syncs without errors
pnpm build            # Clean build, no type errors
```

---

## Phase 2: Authentication — 45 min

### Objective
Working auth flow with GitHub and Google OAuth providers.

### Files to Create
- `lib/auth.ts` (Auth.js config)
- `app/api/auth/[...nextauth]/route.ts`
- `middleware.ts` (route protection)
- `components/nav.tsx` (with sign in/out)

### Steps
- [ ] Configure Auth.js with GitHub + Google providers
- [ ] Create auth API route
- [ ] Add middleware to protect `/calendar`, `/bookings`, `/admin`
- [ ] Build nav component with session-aware sign in/out button
- [ ] Test login flow with both providers

### Verification
```bash
pnpm dev              # Navigate to /calendar → redirects to sign-in
                      # Sign in with GitHub → redirected to /calendar
                      # Nav shows user name and sign-out button
```

---

## Phase 3: Printer Management (Admin) — 45 min

### Objective
Admin can add, edit, and change status of printers.

### Files to Create
- `app/api/printers/route.ts` (GET, POST)
- `app/api/printers/[id]/route.ts` (PATCH)
- `app/admin/layout.tsx` (admin role guard)
- `app/admin/printers/page.tsx`
- `components/printer-card.tsx`
- `tests/unit/printers.test.ts`

### Steps
- [ ] Create API routes for printer CRUD
- [ ] Add admin role check in admin layout
- [ ] Build printer management page with add/edit/status toggle
- [ ] Write unit tests for printer API validation
- [ ] Seed database with 4 test printers

### Verification
```bash
pnpm test             # Printer API tests pass
pnpm dev              # /admin/printers shows 4 printers
                      # Can add a new printer
                      # Can set printer to maintenance
```

---

## Phase 4: Calendar & Slot Logic — 1h

### Objective
Users can view a weekly calendar grid showing available and booked slots per printer.

### Files to Create
- `lib/slots.ts` (slot calculation logic)
- `app/calendar/page.tsx`
- `components/calendar-grid.tsx`
- `app/api/bookings/route.ts` (GET)
- `tests/unit/slots.test.ts`

### Steps
- [ ] Implement slot calculation logic (operating hours → 1h blocks → filter booked)
- [ ] Write thorough unit tests for slot logic (edge cases: timezone, boundaries)
- [ ] Create GET /api/bookings endpoint with printer + week filters
- [ ] Build calendar grid component (printer tabs, weekly view, slot cells)
- [ ] Color-code slots: available (green), booked (gray), mine (blue)

### Verification
```bash
pnpm test             # Slot calculation tests pass (≥8 test cases)
pnpm dev              # /calendar shows weekly grid with 4 printer tabs
                      # Empty slots show as clickable green cells
```

---

## Phase 5: Booking Flow — 1h

### Objective
Users can book a slot, see their bookings, and cancel future bookings.

### Files to Create
- `app/api/bookings/route.ts` (POST — add to existing)
- `app/api/bookings/[id]/route.ts` (DELETE)
- `components/booking-modal.tsx`
- `app/bookings/page.tsx` (my bookings)
- `tests/unit/bookings.test.ts`
- `tests/e2e/booking-flow.spec.ts`

### Steps
- [ ] Implement POST /api/bookings with availability check + unique constraint
- [ ] Implement DELETE /api/bookings/[id] with ownership check + 2h rule
- [ ] Build booking modal (select duration: 1h/2h/4h → confirm)
- [ ] Build "My Bookings" page with cancel button
- [ ] Write unit tests for booking validation (conflicts, permissions, time rules)
- [ ] Write e2e test: open calendar → book slot → see in my bookings → cancel

### Verification
```bash
pnpm test             # All booking tests pass
pnpm test:e2e         # Booking flow e2e passes
pnpm dev              # Full flow works: book → appears in "My Bookings" → cancel → slot freed
```

---

## Phase 6: Admin Overview & Polish — 45 min

### Objective
Admin can view all bookings. UI polish. Production readiness.

### Files to Create
- `app/admin/bookings/page.tsx`
- Final CSS adjustments
- `tests/e2e/admin-flow.spec.ts`

### Steps
- [ ] Build admin bookings overview (filterable by printer, date, user)
- [ ] Add admin ability to cancel any booking (override)
- [ ] Responsive design check (mobile calendar view)
- [ ] Loading states and error handling for all pages
- [ ] Write admin e2e test
- [ ] Final `pnpm build` and Lighthouse check

### Verification
```bash
pnpm test             # All tests pass
pnpm test:e2e         # All e2e tests pass (booking + admin)
pnpm build            # Clean production build
pnpm lint             # No lint errors
```
