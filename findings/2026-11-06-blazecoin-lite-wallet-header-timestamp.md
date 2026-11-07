# Blazecoin Lite Wallet — header verification does not bound block timestamps

| | |
|---|---|
| **Target** | Blazecoin Lite Wallet (`BlazecoinWallet.Lite.Core`) |
| **Reported** | 2026-11-06 |
| **Severity** | Medium (self-assessed, confirmed by vendor) |
| **Status** | **Resolved** — fixed in Lite Wallet 1.3.4 |
| **Channel** | Direct vendor disclosure, BitcoinTalk bounty thread |

## Summary

The lite client verifies a forward run of block headers in Era 3 (after the
Phoenix-413 fork) by recomputing the expected nBits bit-for-bit from the anchor
and the parent header's timestamp. That timestamp comes from the gateway and is
never bounds-checked anywhere in the lite client.

A repo-wide search across `BlazecoinWallet.Lite.Core` for any timestamp bound
returned zero hits: no `MedianTimePast`, no `MaxTimeOffset`, no `MAX_FUTURE`,
no "future time", no "too far in future".

The node enforces the standard rules. The lite client does not, so the
difficulty schedule it verifies can be driven to `powLimit` by a gateway that
simply claims a future timestamp.

## Why it was a finding

The design comment at `HeaderChainSync.cs:70-73` states that forging a chain
that passes all four checks "requires real mining work bounded by the retarget
rule". That is the property that was missing.

The retarget rule responds to the parent timestamp, so a header claiming to be
far in the future drives the expected target to `powLimit`, the easiest target
the chain permits.

Measured against the real shipped checkpoint (height 4,400,000, bits
`0x1c315235`), at eval height 4,400,002:

| Parent timestamp used | Expected nBits | Scrypt work per header |
|---|---|---|
| honest (checkpoint's time) | `0x1c3168cc` | 2^34.4 |
| forged, +1 day | `0x1d18408f` | 2^27.4 |
| forged, +2 days | `0x1e0be76d` | 2^20.4 |
| forged, +1 week | `0x1e0fffff` | 2^20.0 (== powLimit) |
| forged, +1 year | `0x1e0fffff` | 2^20.0 (== powLimit) |

The saving per header is about 2^14.4, roughly 21,000 fewer scrypt hashes. I
want to be precise about this because it is easy to overstate: this is not a
10,000x or 128x saving. Each forged header still needs a genuine scrypt solve,
at powLimit rather than at the honest target, and the chain only advances one
header at a time.

I verified the arithmetic by independently porting `PhoenixNextBits` to Python
and checking it against all 45 vectors in
`test/phoenix413/phoenix413_vectors.json`, zero mismatches. So the C# is
bit-exact and this is a missing consensus rule, not a porting bug.

## What it was not

This was not a theft of funds, and I said so in the report because it is the
first thing a vendor asks.

The balance path still holds. A coin only reaches Spendable if the funding
transaction hashes to its claimed txid, pays the wallet's own script, and is
marked Verified. A gateway cannot invent money.

What broke was the maturity clamp. `EffectiveConfirmations` returns
`Math.Min(gatewayConfirmations, verifiedTipHeight - blockHeight + 1)`. With an
inflated verified tip, that clamp stops limiting the gateway's confirmation
count, so a gateway could present an immature coinbase as past its
30-confirmation maturity. The wallet would build and sign a transaction the
network rejects. The user sees a failed send with no explanation.

I rated this Medium. It needs a gateway the attacker controls, which is unusual
conditions, but it has a real user-visible effect.

## Vendor's reading

The vendor confirmed it and read it as reaching further than the maturity check
I described: on a device that syncs headers, cheaply forged future-dated headers
would have been the wallet's verified chain. That is a broader impact than I
claimed, and it is the vendor's own assessment rather than mine.

## Outcome

Fixed in **Lite Wallet 1.3.4**, released 2026-11-07 for Android and web.

A header dated more than two hours ahead of the device's clock is now refused
before its proof-of-work is looked at. The vendor did not take the bound I
suggested, which was within two hours of the parent block, because the real
chain can go longer than that without a block and the rule would then reject
legitimate headers. Their bound is against the device clock instead, which is
the better fix.

## Conduct

All testing was against the published source. Nothing was run against the live
network, the public gateways, or any node I did not own. The PoC script only
exercises the published difficulty function and does not touch the network.

No public disclosure was made before the fix shipped.

---

*Galih — aswdah1213@gmail.com — @Galih121343*
