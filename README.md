# WMS — Warehouse Management System

Warehouse management system modelled on a real single-warehouse logistics
centre: multi-level rack storage, forklift picking, aisle-bay-level addressing.

Status: work in progress. v1 is being built; see the scope sections below.

## Stack

Laravel (REST API) · React + TypeScript (client) · MySQL · Docker

## Domain model

The database schema is the result of a dedicated domain modelling phase.
ER diagram to be added here.

## Scope (v1)

- Inbound receiving
- Hard allocation: the system picks the unit, not the operator
- Picking missions addressed by LPN (license plate number)
- Break-pack downstream of the pick, with putaway of remainders
- Anomalies recorded by the supervisor
- Three roles: operator, supervisor, manager

## Out of scope (v1)

Deliberate exclusions, not missing work. The schema is prepared for the
first two.

- Split-case picking
- Packing and shipping
- Inventory and cycle counting
- Multi-order picking
- Picking zones and slotting: putaway uses the first compatible free location

## Getting started

Requires Docker and Docker Compose.

    cp .env.example .env
    docker compose up -d

The API runs on http://localhost:8000, the client on http://localhost:5173.

## Code style

PHP is formatted with Laravel Pint, the client with Prettier and linted with
ESLint. See `CONTRIBUTING.md` for the commands and when to run them.