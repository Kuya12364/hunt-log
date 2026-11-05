# Alvin — unauthenticated data read

| | |
|---|---|
| **Target** | AlvinOS portal (portal.alvin.finance) |
| **Reported** | 2026-11-05 |
| **Severity** | Medium (self-assessed) |
| **Status** | Pending |
| **Channel** | Responsible disclosure (VDP, no bounty) |

## Summary

A data entity was readable without authentication. The response contained
operational records with location data, internal email addresses, and
free-text notes naming individuals.

The same application gated its user entity correctly, so the gap was a
missing authorization check on one entity rather than a broken auth system.

## Notes

The vendor's published `security@` address bounced, so the report went to the
`hello@` fallback their policy specifies. Disclosure window runs to roughly
2027-02-03.
