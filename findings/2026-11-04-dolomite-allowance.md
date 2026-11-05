# Dolomite — stale token allowance

| | |
|---|---|
| **Target** | Dolomite (GLP vault integration) |
| **Reported** | 2026-11-04 |
| **Severity** | Low (self-assessed) |
| **Status** | Pending — vendor disputes it as a known issue |
| **Channel** | Direct vendor disclosure |

## Summary

A transfer signal path left a token allowance behind instead of clearing it.
The exposed balance belongs to the caller's own rewards, and the caller is
restricted to the vault owner, which is why I rated it Low rather than higher.

## Notes

The vendor replied that this is a known issue but offered no reference — no
advisory ID, issue number, commit, or audit finding name. I pushed back and
asked for the disclosure reference, since their program scope excludes
anything previously disclosed and an unverifiable "known" claim cannot be
checked.

Their own test suite exercises the code path three times and asserts only the
rejection case, with no assertion on the allowance. That absence is consistent
with the behaviour being unnoticed rather than acknowledged.

The report has not been withdrawn.
