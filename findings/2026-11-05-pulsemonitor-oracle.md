# PulseMonitor — resource existence oracle

| | |
|---|---|
| **Target** | PulseMonitor (pulsemonitor.io) |
| **Reported** | 2026-11-05 |
| **Severity** | Low (self-assessed) |
| **Status** | Pending |
| **Channel** | Responsible disclosure (VDP, no bounty) |

## Summary

An API endpoint distinguished between an existing and a non-existing resource
to an unauthenticated client, through the difference between a 401 and a 404
response.

No resource content was exposed. The impact is limited to confirming whether
a given identifier exists, and identifiers are random UUIDs, so blind
enumeration is not practical.

## Notes

Tested against two accounts I created myself. All requests were reads.
