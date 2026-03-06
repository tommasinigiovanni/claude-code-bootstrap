# AI Rules — 3D Printer Booking System

## Code Style

- TypeScript strict mode, no `any` types — use `unknown` + type guards
- ES modules with named exports: `export function X` not `export default`
- Destructure imports: `import { auth } from '@/lib/auth'`
- Prefer `const` over `let`, never `var`
- Functional components with hooks, no class components
- Server Components by default, add `'use client'` only for interactivity

## Testing

- **Unit tests (Vitest):** for pure logic in `/lib` — slot calculation, validation, utils
- **E2E tests (Playwright):** for user flows — booking, cancellation, admin actions
- **Convention:** test files next to source: `slots.ts` → `slots.test.ts`
- **Before committing:** run `pnpm test` — all tests must pass
- **New features:** write at least one happy-path test and one error-case test

## Git & Commits

- **Branch naming:** `feat/booking-modal`, `fix/double-booking`, `refactor/slot-logic`
- **Conventional commits:** `feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`
- **Commit scope:** one logical change per commit, not a dump of 15 files
- **Never commit directly to `main`** — always use feature branches

## Naming Conventions

- **Files:** kebab-case (`booking-modal.tsx`, `slot-utils.ts`)
- **Components:** PascalCase (`BookingModal`, `CalendarGrid`)
- **Functions/variables:** camelCase (`getAvailableSlots`, `isSlotAvailable`)
- **DB models:** PascalCase (`Booking`, `Printer`)
- **API routes:** kebab-case folders (`/api/bookings/`)
- **Env vars:** SCREAMING_SNAKE_CASE (`DATABASE_URL`, `AUTH_SECRET`)

## Error Handling

- API routes return `{ data: T }` on success, `{ error: string }` on failure
- Use appropriate HTTP status codes (400, 401, 403, 404, 409, 500)
- Never expose stack traces or internal errors to the client
- Log errors server-side with context (userId, action, input)
- Booking conflicts return 409 with a clear message

## Logging

- Use `console.error` for errors with context object
- Use `console.info` for significant actions (booking created, printer status changed)
- Never log sensitive data (tokens, passwords, full user objects)

## Protected Files (NEVER modify without confirmation)

- `prisma/migrations/` — managed by Prisma, never edit manually
- `lib/auth.ts` — auth config, changes can lock out users
- `.env.example` — source of truth for env var documentation
- `middleware.ts` — route protection logic

## Anti-Patterns

- ❌ Don't use `new Date()` for comparisons — use the `utcNow()` helper from `lib/utils`
- ❌ Don't fetch all bookings then filter client-side — always filter in the DB query
- ❌ Don't hardcode printer count or operating hours — read from DB/config
- ❌ Don't add client-side booking validation without matching server-side validation
- ❌ Don't use `prisma.booking.create` without checking slot availability first

## Verification Commands

```bash
pnpm lint          # ESLint + Prettier
pnpm test          # Vitest unit tests
pnpm test:e2e      # Playwright e2e tests
pnpm build         # Production build (catches type errors)
pnpm db:push       # Sync Prisma schema to DB
```
