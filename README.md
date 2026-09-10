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
| 1 | [`01_REVIEWER_RESPONSE.md`](./01_REVIEWER_RESPONSE.md) | Point-by-point answer to both objections, and what we withdrew | 6 min |
| 2 | [`02_USER_JOURNEY_EVIDENCE_MATRIX.md`](./02_USER_JOURNEY_EVIDENCE_MATRIX.md) | **The four journeys × two classes of evidence, in one table** | 4 min |
| 3 | [`03_REPAY_JOURNEY_EXPLAINED.md`](./03_REPAY_JOURNEY_EXPLAINED.md) | Where "repay" lives in a rolling-loan product, and its on-chain proof | 5 min |
| 4 | [`04_ONCHAIN_TRANSACTION_LEDGER.md`](./04_ONCHAIN_TRANSACTION_LEDGER.md) | Five mainnet transactions, and the value flow of each | 8 min |
| 5 | [`06_POST_STATE_UI_RECONCILIATION.md`](./06_POST_STATE_UI_RECONCILIATION.md) | **The resulting loans, in the app — every displayed number matched to the ledger** | 5 min |
| 6 | [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) | Errors we found in **our own** previous submission, unprompted | 3 min |
| — | [`00_POA_SUBMISSION_FORM.md`](./00_POA_SUBMISSION_FORM.md) | The text submitted to the Catalyst form | — |

In a hurry? Read **#2**, then the ledger in **#4**.

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

Answered in [`01_REVIEWER_RESPONSE.md`](./01_REVIEWER_RESPONSE.md) §Objection 1 — by **withdrawing
the claim rather than defending it**, and without substituting another. This submission does not
assert that the interface has been shown to be intuitive to independent users, and nothing in it
should be read that way.

> **2.** *"…provide evidence demonstrating that the four approved user journeys (view, open, repay
> and refinance) were each successfully tested through the Eternl-connected front end, and provide
> the corresponding integration-test results."*

Answered per journey — [`02_USER_JOURNEY_EVIDENCE_MATRIX.md`](./02_USER_JOURNEY_EVIDENCE_MATRIX.md)
— and evidenced by the expanded ledger in
[`04_ONCHAIN_TRANSACTION_LEDGER.md`](./04_ONCHAIN_TRANSACTION_LEDGER.md) (five transactions, not
two), the post-settlement state in the application
([`06_POST_STATE_UI_RECONCILIATION.md`](./06_POST_STATE_UI_RECONCILIATION.md)), and the
counterparty protocol's own loan records.

On the second half of that request: **we are not submitting test-suite figures.** The previous
submission's *"8/8 refinance tests pass"* was true and was the wrong kind of thing to lead with —
our own count, of our own tests, about our own work. What replaces it is evidence we cannot
influence: the Cardano ledger, and Fluid Tokens' records of the loans we settled.

The "repay" journey has a dedicated annex because it is the one most likely to be judged missing:
[`03_REPAY_JOURNEY_EXPLAINED.md`](./03_REPAY_JOURNEY_EXPLAINED.md).

---

## The evidence in one screen

**Five signed Cardano mainnet transactions**, two wallets, nine days, all independently verifiable:

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
transaction burned, and each transaction pays at least the total Fluid says was due.
See [Annex C](./03_REPAY_JOURNEY_EXPLAINED.md) §3 and
[`screenshots/fluid-dashboard/`](./screenshots/fluid-dashboard/).

**And one of those six is a standalone repayment.** `17c23dde…`, written by this application with
the metadata *"Dano Finance: Repay Fluid Loan"* — the borrower pays the debt from their **own**
wallet, the collateral is **released back to them**, and no Dano loan is created. It is the exact
mirror of the refinance case, so the repay journey is evidenced on mainnet in both of its forms —
[Annex C](./03_REPAY_JOURNEY_EXPLAINED.md) §6.

**UI/ledger agreement, before signing.** Before signing `c426d9fa…`, the interface showed a Fluid
debt of 12 ADA and a fee of 2 ADA. On chain: settlement 12.000102 ADA, fee exactly 2.000000 ADA,
DJED 6.000000 carried across. The number the user was shown is the number that settled.

**UI/ledger agreement, after settling.** The two wallets' *My Account → Loans* screens, captured
from the live app, list the resulting Dano loans and no Fluid loans. Every row reconciles —
[Annex F](./06_POST_STATE_UI_RECONCILIATION.md):

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
00_POA_SUBMISSION_FORM.md          the text submitted to the Catalyst form
01_REVIEWER_RESPONSE.md            point-by-point response to both objections
02_USER_JOURNEY_EVIDENCE_MATRIX.md four journeys x four evidence classes
03_REPAY_JOURNEY_EXPLAINED.md      the repay journey, and its on-chain proof
04_ONCHAIN_TRANSACTION_LEDGER.md   five mainnet transactions, verified
06_POST_STATE_UI_RECONCILIATION.md the resulting loans in the app, matched to the ledger
08_CORRECTIONS.md                  corrections to our own previous submission

screenshots/
  screens/                         the app's My Account - Loans, one per wallet
  fluid-dashboard/                 Fluid's own borrower dashboard: the loans marked REPAID
  cardanoscan/                     Cardanoscan, one per refinance transaction
  *.png                            walkthrough captures from the previous submission

previous submission, kept in place so its links still resolve, each with a banner:
  01_Integration_Test_Report_M2.md
  02_Mainnet_Transactions_M2.md
  03_Reviewer_Checklist_M2.md
  PREVIOUS_README.md
  screenshots/*.png                its walkthrough captures (kept in place)
```

### The previous submission is still here

The four files above are the evidence the reviewer read the first time. They are **unedited** — we
did not quietly fix them — and each carries a banner pointing at
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md), which lists every claim in them we have since corrected
or withdrawn, with the on-chain arithmetic. Their walkthrough screenshots in
[`screenshots/`](./screenshots/) remain valid and are still referenced by this package.

---

## Status

| | |
|---|---|
| Per-journey evidence matrix | ✅ |
| On-chain ledger, independently verifiable | ✅ 6 transactions |
| Post-refinance state in the app, reconciled to the ledger | ✅ every displayed amount |
| Repay journey explained and proven on-chain | ✅ |
| Repayment confirmed by the counterparty protocol's own records | ✅ 6/6 `loan_repaid` |
| Standalone repay on mainnet | ✅ `17c23dde…` |
| Corrections to the previous submission | ✅ |
| Integration-test figures | — **not submitted as evidence** |
| Independent user testing | — **not claimed in this submission** |

On the last two rows. The application **is** covered by an automated suite, and the previous
submission's usability row **was** written in good faith — but both are our own reporting about
our own work, and this package deliberately rests only on what a third party can check without us.
The usability claim is withdrawn and not replaced. If Milestone 2 cannot be approved without
independent user-testing evidence, this package does not meet that bar, and we would rather be
told so plainly — see [`01_REVIEWER_RESPONSE.md`](./01_REVIEWER_RESPONSE.md), Objection 1.

---

## Why mainnet and not testnet

Fluid does not deploy its smart contracts to any Cardano testnet. There is no test network on which
a Fluid → Dano refinance can be executed at all. Mainnet is not a shortcut here — it is the only
environment where this cross-protocol path exists, and it produces the stronger evidence, because
every transaction above is public, immutable, and verifiable by the reviewer without our cooperation.
