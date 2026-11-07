# Alvin — unauthenticated read access to the Location entity

| | |
|---|---|
| **Target** | portal.alvin.finance (AlvinOS portal) |
| **Reported** | 2026-11-05 |
| **Severity** | Medium (self-assessed) |
| **Status** | **Resolved** |
| **Channel** | Direct vendor disclosure, under their published responsible-disclosure policy |

## Summary

The `Location` entity on the AlvinOS portal could be listed and read one
record at a time by anyone, with no cookie, no bearer token and no account.
The response held operational travel records, GPS coordinates, internal email
addresses, and free-text notes that named people and described business
journeys.

## Why it was a finding

The portal itself is configured `public_without_login`, so the website is
reachable without a session. That covers the site, not the entity API. Three
things showed the entity layer was supposed to be gated.

The `User` entity on the same app rejected the identical request with a 401,
so a gate existed in that codebase and someone had applied it to at least one
entity. `Location` was not covered by it.

The front-end bundle never called `entities/Location`. The entities the SPA
actually used were `LighthouseAccessInvite`, `LighthouseLoginActivity`,
`Organization`, `OrganizationMembership`, `Person` and `UserPageVisit`. No
public feature depended on `Location` being readable.

Writes on `Location` were permission-checked: `POST` and `PUT` both returned
403, while `GET` returned 200 with data. A design that meant reads to be
public would not 403 the writes.

All 28 records carried `is_sample: false`, so none of it was demo data.

## What was exposed

28 records. 24 carried GPS coordinates, 17 carried free-text notes. Fields
included `country`, `city`, `formatted_address` at street level, `latitude`,
`longitude`, `iata_code`, `icao_code`, `notes`, `created_by`, `created_date`,
`geocoding_status`, `google_place_id`, `verification_status`, `location_type`
and `timezone`.

The `notes` field was the sensitive one. It recorded a traveller's confirmed
stay at a named hotel on a named business journey on a specific date, and
described coordinates as operational data for a "founder-authorized"
inspection journey. `created_by` exposed internal email addresses, including
one at `alvin.finance`.

The realistic harm was reconnaissance. Someone learned where named people
stayed, when, and on what business, plus an internal address to go after. For
a company whose product is travel and delegation infrastructure for
institutional clients, that was a privacy exposure to the people in the data,
not just to Alvin.

I called this Medium rather than High because there was no authentication
bypass, no write access, and the dataset was 28 records rather than a full
customer export. At least 20 other entities on the same app also answered 200
unauthenticated, but every one of them was empty, so the impact sat on
`Location` alone. If any of those later held data, the same missing gate would
expose it and the severity goes up.

## Outcome

Reported on 2026-11-05. The vendor reviewed it, confirmed the issue, and
implemented remediation. They also corrected the delivery issue with their
security reporting address, which had caused the first send to land at the
wrong inbox.

I stopped testing as soon as the behaviour was confirmed.

## Conduct

Every request was a `GET`, plus `POST`/`PUT`/`DELETE` probes with empty or
non-matching bodies to work out whether writes were protected. No record was
created, modified or deleted; the count was 28 before and after every probe.

No customer data was accessed beyond what the unauthenticated endpoint itself
returned. The downloaded records were deleted after analysis and only the
redacted evidence file was attached. No automated scanning, fuzzing or bulk
enumeration was run.

## Notes

The vendor runs a responsible-disclosure policy rather than a bounty program,
so this one closed without a payment. It is published here because the issue
is resolved and the vendor's disclosure terms allow it.

---

*Galih — aswdah1213@gmail.com — @Galih121343*
