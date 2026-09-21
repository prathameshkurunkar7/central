# Team network identity

## Purpose

Each Team has one permanent numeric tenant ID for Atlas network isolation. The Team name remains the customer-facing label.

## Allocation

Central assigns the ID when it inserts a Team. It replaces any caller-supplied value. IDs range from 1 to 4294967295. Zero belongs to infrastructure.

The Framework naming series `TENANT-ID` serializes allocation in the Team transaction. A rolled-back allocation can be reused. A committed allocation is never decremented, including after Team deletion.

Schema setup creates the series before concurrent customer requests. It advances past preserved IDs without lowering the existing counter. Gaps are valid.

The field uses Int with length 20 to store the full range. The controller adds the database uniqueness constraint after legacy zero values are filled.

## Changes and access

The tenant ID is read-only in Desk. Saving a Team cannot change its tenant ID, including as an operator. Renaming the Team label preserves it.

Existing Team permissions still apply to reads and writes. A tenant ID does not grant authority. Regional callers must resolve authorization through Central IAM.

## Migration

Central is pre-1.0, with no site to carry a tenant ID forward. A fresh site (see
[MIGRATION.md](../../../../MIGRATION.md)) creates the empty series and the uniqueness
constraint at schema setup, and assigns every Team a tenant ID at insert time. There is
nothing to backfill.

## Validation

Run `central.tests.test_team_tenant_id` through Pilot. Tests cover caller input, immutability, database uniqueness, full-range storage, and concurrent allocation.

Run the existing IAM tests to check that Team capability rules still apply. No capability or permission bypass is added by tenant allocation.
