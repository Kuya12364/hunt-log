# Bulk Trade — conditional orders inert (WITHDRAWN)

| | |
|---|---|
| **Target** | Bulk Trade (Solana perps DEX, testnet) |
| **Reported** | 2026-11-02 |
| **Severity** | Medium, as filed |
| **Status** | **Withdrawn — false positive** |
| **Channel** | Direct vendor disclosure |

## Summary

I reported that trigger and on-fill conditional orders were stored with a
zero size and never executed their nested action.

## Why it was withdrawn

It did not hold up, and I retracted it myself.

My original test nested a market order inside the trigger. The testnet order
book was empty, so the market order had no counterparty and filled zero. The
position staying open was documented behaviour, not a defect — the vendor's
own changelog lists a trigger-failed status for exactly that case.

Two further misreadings on my side:

1. A zero value on the trigger root was read as a dropped size field. The
   struct has no root size field at all; the size lives in the nested action,
   and the internal representation is signed.
2. I never actually observed the nested action failing to execute. I had only
   the absence of a side effect under conditions where that side effect could
   not occur.

The decisive test nested a limit order instead. A resting limit order is
directly observable and needs no counterparty, and it behaved correctly.

## Notes

Kept in this log deliberately. A tracker that only lists wins is not a
tracker, and the failure mode here — reading a missing side effect as a bug
when the conditions could not produce it — is worth recording.
