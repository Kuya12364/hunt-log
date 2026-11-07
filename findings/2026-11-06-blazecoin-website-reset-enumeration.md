# Blazecoin website — account enumeration via password reset

| | |
|---|---|
| **Target** | blazecoin.co.uk (`POST /Account/ResetPassword`) |
| **Reported** | 2026-11-06 |
| **Severity** | Low (self-assessed, confirmed by vendor) |
| **Status** | **Resolved** — deployed 2026-11-07 |
| **Channel** | Direct vendor disclosure, BitcoinTalk bounty thread |

## Summary

Completing a password reset answered differently for an address with no
account than for one with an account. The endpoint was unauthenticated, so
anyone could enumerate which addresses were registered.

## Why it was a finding

The reset flow is the kind of endpoint that gets built to be helpful. It tells
you whether the code you entered was right, whether it had expired, and whether
the address you typed exists. Each of those is a separate oracle, and together
they answer a question the product never meant to answer: which addresses have
accounts here.

An unauthenticated enumeration oracle on a wallet platform is a targeting
primitive. It does not expose funds and it does not expose account data, but it
turns a list of addresses into a list of users.

## Outcome

Fixed on the website, deployed 2026-11-07. An unknown address and a wrong or
expired code now get the same answer, and the endpoint is rate-limited.

## Conduct

Testing was limited to accounts I created myself, plus the reset flow against
them. Nothing was run against other users' accounts.

The vendor noted afterwards that lockouts on their admin address had been
observed during testing, and asked that future testing stay on self-created
accounts. That is a fair point and it is now the standing rule for this target.

## Scope note

The BitcoinTalk bounty thread lists "the website and its services ... account
security" as in scope. The site's own terms said security research was welcome
against the researcher's own wallets and nodes, not against the platform's
accounts or infrastructure without permission, and there was no published
bounty page on the site itself.

Both reports stated the scope reference explicitly and offered to withdraw if
the operator considered the terms to restrict the testing. The operator
accepted both reports, so the conflict resolved in the researcher's favour, but
the ambiguity was real and worth flagging in the original submission rather
than assuming.

---

*Galih — aswdah1213@gmail.com — @Galih121343*
