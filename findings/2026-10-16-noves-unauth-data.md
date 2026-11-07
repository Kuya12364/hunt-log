# Noves / Palladium — unauthenticated account data access

| | |
|---|---|
| **Target** | Noves data app (noves.canton.palladiumlabs.org) |
| **Reported** | 2026-10-16 |
| **Severity** | Medium (self-assessed) |
| **Status** | **Resolved** |
| **Channel** | Direct vendor disclosure |

## Summary

Account data was reachable without authentication. Reported with full
reproduction detail, including exact requests and raw responses with
timestamps.

## Outcome

The vendor acknowledged the report and confirmed it was resolved. They
explained that they do not run a bounty program and do not normally expect
unsolicited reports, then researched typical rates for a Medium and offered a
goodwill payment, which they sent via PayPal.

I took it. It was offered as a goodwill payment rather than a bounty, and the
report was accepted on its merits.

## Notes

Six leaked account records were exposed by the flaw. I offered to help clean
them up and to re-test once the fix shipped. I downloaded nothing beyond what
the unauthenticated endpoint itself returned, and deleted what I had pulled
after analysis.

The vendor has resolved the issue, which is why this one is published. The
other findings I have reported are not, because they are still open.
