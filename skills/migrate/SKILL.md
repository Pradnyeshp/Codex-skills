---
name: migrate
description: Write a safe, reversible database schema or data migration following the project's migration tool and conventions
---

# Migrate

Create a correct, reversible database migration that the team can apply and roll back with confidence.

## Steps

1. **Understand the change** — get the exact schema or data change wanted (new table/column, index, backfill, type change, drop). Ask if unclear
2. **Detect the migration tool** — look for the framework in use: Alembic (Python), Prisma/Knex/TypeORM/Sequelize (JS/TS), Rails/ActiveRecord, Flyway/Liquibase, `golang-migrate`, Django migrations. Match its file layout and naming
3. **Read a recent migration** to mirror conventions (numbering/timestamps, up/down structure, transaction handling)
4. **Write the `up`** — the forward change, ordered safely (create before backfill before constraint)
5. **Write the `down`** — the exact inverse so the migration is reversible. If truly irreversible (e.g. dropped data), say so explicitly
6. **Plan for safety on large/production tables** — avoid long locks: add indexes concurrently where supported, backfill in batches, make column adds nullable-then-populate-then-constrain
7. **Verify** — run the migration up and down against a dev/test database and confirm the schema and data are as expected

## Rules

- Every migration must be **reversible** unless it destroys data — and then call that out loudly
- Separate schema changes from large data backfills when possible; long-running backfills shouldn't hold a schema lock
- Never edit an already-applied/committed migration — add a new one
- Be explicit about locking and downtime risk on big tables; recommend the safe multi-step pattern
- Don't run migrations against production; prepare them and let the user deploy
- Keep migrations idempotent-friendly and test both directions before finishing
