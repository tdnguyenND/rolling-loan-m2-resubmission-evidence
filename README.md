# Milestone 2 — Proof of Achievement Resubmission

**Project Catalyst · Project 1400107 — Rolling Loan**
**Milestone 2 — Feature Integration** · resubmission after *Not Approved*
Milestone page → https://milestones.projectcatalyst.io/projects/1400107/milestones/2

The feature is called **Rolling Loan** in the proposal and **Refinance via Dano** in the product.
Same capability, two names.

---

## For the reviewer — read in this order

| # | Document | What it answers | Time |
|---|---|---|---|
| 1 | [`00_POA_SUBMISSION_FORM.md`](./00_POA_SUBMISSION_FORM.md) | The submission itself: the response to both objections, then Output 1–5 with their evidence | 8 min |
| 2 | [`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md) | **The four journeys through the Eternl-connected front end**, and the resulting loans in the app matched to the ledger | 9 min |
| 3 | [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | **Six mainnet transactions**, the value flow of each, and the repay journey in both of its forms | 12 min |
| 4 | [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) | Errors we found in **our own** previous submission, unprompted | 3 min |

In a hurry? Read the journey matrix in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl), then the
transactions in [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions).

The previous submission's own files — the integration test report, the transaction evidence and the
reviewer checklist — are kept **unedited** at their original paths, each with a banner pointing at
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md). The manual refinance journey step by step, and the
integration-test results, are in
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §1–§3.

---

## Where the evidence comes from

Every factual claim in this package comes from one of two public APIs, and **neither is ours**:

| Source | What it settles | Credentials |
|---|---|---|
| **Koios** `api.koios.rest` | the transactions, what they burned and minted, what they paid, and what the chain looks like now | none |
| **Fluid Tokens** `api.fluidtokens.com` | whether the lender considers each loan repaid | the public key its own web app sends |

No Danogo server, indexer or database appears anywhere in the figures below: the ledger data is
Cardano's, and the loan records are the lender's own.

---

## What the reviewer said, and where each objection is answered

> **1.** *"The fact that a developer has successfully demonstrated the application does not
> automatically establish that the interface is intuitive to independent users […] there is no
> evidence to show that users even completed a specific workflow without external guidance."*

Answered by **describing the testing that was done rather than defending the verdict we drew from
it**. Every journey was walked end-to-end through the connected wallet by our own testers, and six
of those sessions settled on mainnet; the package reports that as internal testing wherever it
appears — see [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑2 and
[`04` §1.4](./04_USER_JOURNEYS_AND_APP_STATE.md#14-the-scope-of-these-claims).

> **2.** *"…provide evidence demonstrating that the four approved user journeys (view, open, repay
> and refinance) were each successfully tested through the Eternl-connected front end, and provide
> the corresponding integration-test results."*

Answered per journey in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl) — a
matrix with two independent classes of evidence for each journey, plus the user path for each — and
evidenced by six mainnet transactions instead of two
([`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions)), the
post-settlement state in the application
([`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)),
and the counterparty protocol's own loan records
([`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-both-of-its-forms)).

Integration-test results are in the previous submission's
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) — §2 (25 transaction QC
checks) and §3 (8 automated UI tests, all passing). Those figures are our own reporting and are
named as such; the load-bearing evidence is the ledger and Fluid's own records.

"Repay" — the journey the previous submission covered "implicitly" — has its own section:
[`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-both-of-its-forms).

---

## The evidence in one screen

**Five signed Cardano mainnet refinances**, two wallets, nine days, all independently verifiable:

| Tx | Date (UTC) | Collateral carried | Borrowed | Liquidity source | Fee |
|---|---|---|---|---|---|
| [`88579a30…`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 2026‑08‑19 | USDM 10.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`d240fab1…`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 2026‑08‑24 | SNEK 20,979 | ADA | Staking (fixed-term) | 0.969750 ₳ |
| [`1cf8f08b…`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 2026‑08‑25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`c426d9fa…`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 2026‑08‑25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`0e26cc58…`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 2026‑08‑27 | DJED 10.000000 | **STRIKE** | Flexible Pool | **none** |

Verified for **every** one of them:

- every Plutus script execution returned `valid_contract = true` — the live validators of **both**
  protocols accepted the transaction
- the source **Fluid loan position NFT is burned** — Fluid's own validator permits that only when
  the loan is settled in full. **This is the repayment proof.**
- a new **Dano loan position NFT is minted** into the Dano Flexible Loan contract
- **collateral in = collateral out**, to the smallest unit — never returned to the borrower
- one transaction, one block — the close and the reopen cannot partially fail
- the borrower contributed only the Cardano network fee; the Dano pool funded the repayment
- metadata label 674 = *"Dano Finance: Create Loan"*

**The protocol that was owed the money confirms every repayment.** Fluid Tokens' public API,
queried for these two wallets over this period, returns **six** loan events — every one
`"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, and every one naming a
transaction *this application built*. The loan token Fluid names is the exact token each
transaction burned, and each transaction pays at least the total Fluid says was due —
[`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-both-of-its-forms) and
[`screenshots/fluid-dashboard/`](./screenshots/fluid-dashboard/).

**And one of those six is a standalone repayment.** `17c23dde…`, written by this application with
the metadata *"Dano Finance: Repay Fluid Loan"* — the borrower pays the debt from their **own**
wallet, the collateral is **released back to them**, and no Dano loan is created. It is the exact
mirror of the refinance case, so the repay journey is evidenced on mainnet in both of its forms —
[`05` §5.5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#55-the-standalone-repay-journey-also-on-mainnet).

**UI/ledger agreement, before signing.** Before signing `c426d9fa…`, the interface showed a Fluid
debt of 12 ADA and a fee of 2 ADA. On chain: settlement 12.000102 ADA, fee exactly 2.000000 ADA,
DJED 6.000000 carried across. The number the user was shown is the number that settled.

**UI/ledger agreement, after settling.** The two wallets' *My Account → Loans* screens, captured
from the live app, list the resulting Dano loans and no Fluid loans. Every row reconciles —
[`04` §2.4](./04_USER_JOURNEYS_AND_APP_STATE.md#24-row-by-row-the-displayed-amount-is-the-ledger-amount):

| Wallet | UI shows | Created by | Principal on chain | + accrual at the APR the row displays |
|---|---|---|---|---|
| W1 | 15.07 ADA @ 8.47% | `d240fab1…` | 15.015024 | 15.0736 → **15.07** ✅ |
| W1 | 22.04 ADA @ 3.07% | `88579a30…` | 22.000857 | 22.0413 → **22.04** ✅ |
| W2 | 14.01 ADA @ 3.07% | `c426d9fa…` | 14.000108 | 14.0185 → **14.01** ✅ |
| W2 | 13.01 ADA @ 3.07% | `1cf8f08b…` | 13.000010 | 13.0174 → **13.01** ✅ |
| W2 | 5.01 **STRIKE** @ 5.00% | `0e26cc58…` | 5.000560 | 5.0101 → **5.01** ✅ |

The wallets in those screenshots are the wallets that signed the transactions, the loan count in the
app equals the number of Danogo *Borrower NFTs* each wallet holds on chain (2 and 4), and all five
**Fluid position NFTs now have a total supply of zero** — the repaid loans do not exist anywhere on
Cardano.

---

## Package contents

```
00_POA_SUBMISSION_FORM.md            the text submitted to the Catalyst form, and the
                                     response to both objections
04_USER_JOURNEYS_AND_APP_STATE.md    the four journeys via Eternl, and the resulting
                                     loans in the app matched to the ledger
05_ONCHAIN_TRANSACTIONS_AND_REPAY.md six mainnet transactions, per-transaction value
                                     flow, the repay journey in both of its forms
08_CORRECTIONS.md                    corrections to our own previous submission

screenshots/
  screens/                           the app's My Account - Loans, one per wallet
  fluid-dashboard/                   Fluid's own borrower dashboard: the loans marked REPAID
  cardanoscan/                       Cardanoscan, one per refinance transaction
  *.png                              walkthrough captures of the manual refinance journey

previous submission, kept in place so its links still resolve, each with a banner:
  01_Integration_Test_Report_M2.md   the manual refinance journey, the transaction QC
                                     checks, and the automated integration-test results
  02_Mainnet_Transactions_M2.md      its two mainnet transactions
  03_Reviewer_Checklist_M2.md        its output / criterion / evidence mapping
```

### The previous submission is still here

Those three files are the evidence the reviewer read the first time. They are **unedited** — we did
not quietly fix them — and each carries a banner pointing at
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md), which lists every claim in them we have since corrected
or restated, with the on-chain arithmetic. Their walkthrough screenshots in
[`screenshots/`](./screenshots/) remain valid and are still referenced by this package.

---

## Status

| | |
|---|---|
| Per-journey evidence matrix | ✅ `04` §1 |
| On-chain evidence, independently verifiable | ✅ 6 transactions |
| Post-refinance state in the app, reconciled to the ledger | ✅ every displayed borrowed amount |
| Repay journey evidenced in both of its forms | ✅ `05` §5 |
| Repayment confirmed by the counterparty protocol's own records | ✅ 6/6 `loan_repaid` |
| Standalone repay on mainnet | ✅ `17c23dde…` |
| Corrections to the previous submission | ✅ |
| Integration-test figures | ✅ reported, and named as our own reporting |
| User testing | ✅ every journey end-to-end; 6 sessions settled on mainnet — internal testing |

On the last two rows. The application **is** covered by an automated suite, and the testing behind
the previous submission's usability row **did** take place — but both are our own reporting about
our own work, and this package rests its verifiable claims on what a third party can check without
us. The testing is reported as internal testing, tied to the mainnet transaction each session
produced, rather than offered as a usability verdict.

---

## Why mainnet and not testnet

Fluid does not deploy its smart contracts to any Cardano testnet. There is no test network on which
a Fluid → Dano refinance can be executed at all. Mainnet is not a shortcut here — it is the only
environment where this cross-protocol path exists, and it produces the stronger evidence, because
every transaction above is public, immutable, and verifiable by the reviewer without our cooperation.
