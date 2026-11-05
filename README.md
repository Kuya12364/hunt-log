# hunt-log

Public record of paid and resolved security research.

This repo lists findings a vendor has **accepted, fixed, and paid for**.
Anything still pending, still unfixed, or still inside a disclosure window
stays off this list. Publishing an unpatched vulnerability puts users at
risk, so it does not go here.

---

## `01 // IDENTITY`

> **I break web apps, APIs, and smart contracts with permission, and I report
> what I find with the evidence attached.**

**Galih** — independent security researcher. Bug bounty and coordinated
disclosure work across web/API surfaces and on-chain protocols.

I work in two lanes. The first is application security: access control,
authentication and session flows, authorization boundaries, and API surface
behaviour. The second is smart contract security, where I read deployed
bytecode and source, build a fork, and prove a claim against real chain state
before I report it.

What I care about most in a report is that it survives triage. I rate my own
findings conservatively, I state the limitations of my own exploit, and when a
finding does not hold up I withdraw it myself rather than let a vendor find
out first.

`WEB / API SECURITY` · `ACCESS CONTROL` · `SMART CONTRACTS` · `COORDINATED DISCLOSURE`

### Operating context

| | |
|---|---|
| Base | Indonesia |
| Role | Independent security researcher |
| Lanes | Web / API · EVM · Solana · Canton |
| Programs | Direct vendor disclosure, VDP, and bounty programs |
| Open to | Security research collaborations and contract work |

---

## `02 // ARSENAL`

**Application security**

Reconnaissance through certificate transparency and DNS rather than blind
scanning. JavaScript bundle analysis to map an API surface before touching it.
Authorization testing across entity and object boundaries, with a second
account I control to prove cross-tenant reach. Session and authentication flow
review.

**Smart contract security**

Foundry for fork testing against real deployed state, so a claim is verified
against the chain rather than argued from source. Source review across
Solidity, Rust, and Daml. On-chain state reading to establish what a contract
actually holds before claiming what an attacker could reach.

**Tooling**

`curl` · `Python` · `Foundry` / `forge` · `Burp Suite` · `imapfilter` · `git`

**Reporting**

Every report ships with a reproduction, a redacted evidence file, and a
self-assessed severity with the reasoning written out. I run an overclaim scan
over my own drafts before sending, because a report that inflates its impact
gets downgraded and costs credibility on the next one.

---

## `03 // HUNT LOG`

Paid and resolved findings only.

| # | Target | Severity | Date | Status |
|---|---|---|---|---|
| 1 | Noves / Palladium | Medium | 2026-10-16 | **Paid — $250** |

**Total paid: $250**

Other findings I have reported are not listed here, because they are still
open with their vendors. They get added when the vendor closes them and agrees
to disclosure.

---

## `04 // RESEARCH RULES`

- Authorized programs and in-scope assets only. I read the policy before I
  probe, and I stop when it says stop.
- Minimal probing. Enough to confirm the issue, not enough to map everything.
- My own test accounts for cross-tenant checks. I do not touch another
  customer's data beyond what the vulnerability itself exposes.
- Read-only where I can. When I probe a write path, I use empty or
  non-matching payloads and verify the record count is unchanged afterwards.
- Data I pull during testing gets deleted once the finding is written up.
  Reports carry redacted evidence, not raw dumps.
- No public disclosure before the vendor has had a fair chance to fix it.

---

## `05 // CONTACT`

Galih — aswdah1213@gmail.com — Telegram [@Galih1214](https://t.me/Galih1214)
