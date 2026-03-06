# PRD — 3D Printer Booking System

## Vision

A web application that lets FabLab members view available 3D printer time slots and book them instantly, replacing the current spreadsheet-based system.

## Target User

FabLab members (makers, students, hobbyists) who need to reserve 3D printer time. Secondary: FabLab admins who manage printers and approve bookings.

## Problem

The current booking process is a shared Google Sheet. Double-bookings happen weekly. There's no visibility on printer availability without opening the spreadsheet. No-shows waste printer time because there's no reminder system.

## Core Features (MVP)

- View available slots on a calendar (weekly view per printer)
- Book a slot (1h, 2h, or 4h blocks)
- Cancel a booking (up to 2h before start)
- User auth via GitHub/Google (existing FabLab accounts)
- Admin: add/remove printers, view all bookings, manual override

## What This is NOT (Out of Scope)

- Payment processing (the FabLab uses a separate system for billing)
- Print file upload or slicing (users bring files on USB)
- Real-time printer monitoring or IoT integration
- Mobile native app (responsive web is sufficient)
- Multi-location support (single FabLab only for MVP)

## Success Metrics

- Zero double-bookings (hard constraint via DB unique constraint)
- 80% of members using the system within 1 month of launch
- < 3 second page load on calendar view

## Constraints and Assumptions

- FabLab has 4 printers, may scale to 8
- Operating hours: Mon-Sat, 9:00-21:00
- All users have GitHub or Google accounts
- PostgreSQL available via existing FabLab server
- Must work on latest Chrome, Firefox, Safari
