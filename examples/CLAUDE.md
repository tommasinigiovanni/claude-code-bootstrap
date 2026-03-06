# 3D Printer Booking System

FabLab booking system for 3D printers — slot visualization and reservation.

## Stack

- TypeScript 5.4
- Next.js 14 (App Router)
- PostgreSQL 16 + Prisma ORM
- TailwindCSS 3.4
- Auth.js (GitHub + Google OAuth)
- pnpm

## Commands

- `pnpm dev`: Start dev server (port 3000)
- `pnpm build`: Production build
- `pnpm test`: Run Vitest unit tests
- `pnpm test:e2e`: Run Playwright e2e tests
- `pnpm lint`: ESLint + Prettier check
- `pnpm db:push`: Push Prisma schema to DB
- `pnpm db:studio`: Open Prisma Studio

## Structure

- `/app`: Next.js App Router pages and layouts
- `/app/api`: API routes (REST)
- `/components`: Reusable UI components
- `/components/ui`: Base design system (shadcn/ui)
- `/lib`: Utilities, DB client, auth config
- `/prisma`: Database schema and migrations

## Code Style

- ES modules, named exports only
- Functional components with hooks, no class components
- Tailwind utility classes, no custom CSS files
- Server Components by default, `'use client'` only when needed
- All API responses use consistent `{ data, error }` shape

## Workflow

1. Create a feature branch from `main`
2. Implement with tests (Vitest for logic, Playwright for flows)
3. Run `pnpm lint && pnpm test && pnpm build` before committing
4. Conventional commits: `feat:`, `fix:`, `refactor:`, `test:`, `docs:`
5. Open PR, CI must pass, merge to `main`

## IMPORTANT

- NEVER modify `/prisma/migrations/` manually — use `pnpm db:push`
- NEVER commit `.env` — use `.env.example` as reference
- All booking times are stored and compared in UTC
- The `BookingSlot` model has a unique constraint on `[printerId, startTime]`
- Auth callbacks in `/lib/auth.ts` must not be modified without review

## References

- Product requirements: @ai/PRD.md
- Architecture and data model: @ai/ARCHITECTURE.md
- Coding rules and anti-patterns: @ai/AI_RULES.md
- Implementation plan: @ai/PLAN.md
