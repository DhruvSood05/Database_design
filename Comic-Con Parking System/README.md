# Comic-Con Parking System Database Design

This folder contains the database design for a Comic-Con parking management system. The schema models vehicles, parking spots, reserved parking, tickets, parking sessions, and payment records for a multi-zone, multi-level venue.

## Scope

The design is intended to support parking for:

- Bikes
- Cars
- SUVs
- Cabs
- EVs
- Other vehicle types

It also accounts for reserved parking needs for:

- Exhibitors
- Creators
- VIP guests
- Staff members
- EV charging vehicles

## Main Entities

### Vehicles

Stores vehicle details such as vehicle number, category, and timestamps.

### Parking Spots

Stores each parking spot with its status, zone, level, and timestamps.

### Reserved Parking

Maps reserved spots to a reservation category for priority parking use.

### Parking Tickets

Links a vehicle to a parking spot when parking is assigned.

### Parking Sessions

Tracks entry and exit times for each parking ticket.

### Payment Records

Stores payment details for each parking ticket, including mode, status, and amount.

## Relationships

- One vehicle can have one or more parking tickets over time.
- One parking spot can be associated with tickets and may also be marked as reserved.
- One parking ticket can have one parking session.
- One parking ticket can have one payment record.

## Design Notes

- Parking spots are separated by zone and level to support large venue layouts.
- Reserved parking is kept as a separate table so special access rules can be managed independently.
- Payment amount can be calculated from the parking session duration.
- Timestamps are included on all major entities for auditing and tracking.

## Files

- [schema.md](schema.md): Source schema notes and table definitions.

## Status

This is a conceptual database design and can be extended later with indexes, constraints, triggers, or additional operational tables if needed.
