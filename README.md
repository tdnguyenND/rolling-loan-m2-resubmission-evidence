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
| 3 | [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | **Fifteen mainnet transactions** grouped by journey, the value flow of each, and the repay journey in all three of its forms | 14 min |
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

Taken on board. The previous submission's row *"Tester completed the flow without external guidance
✅ Yes"* was a self-assessment, and it is not carried as evidence here. What the testing sessions
record is kept, and is reported per journey: all four journeys were walked end-to-end through the
Eternl-connected front end, and **fifteen of those sessions settled on Cardano mainnet** — 7 Open,
5 Refinance, 3 Repay — from two separate wallets over eleven days. Everything below leads with
evidence that can be checked without us — the Cardano ledger, and the counterparty protocol's own
loan records — with the interface captures and signing dialogs itemised beside it. See
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl) and
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑2.

> **2.** *"…provide evidence demonstrating that the four approved user journeys (view, open, repay
> and refinance) were each successfully tested through the Eternl-connected front end, and provide
> the corresponding integration-test results."*

Answered per journey in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl) — a
matrix with two separate classes of evidence for each journey, plus the user path for each — and
evidenced by **fifteen** mainnet transactions instead of two: **7 Open** ([`05`
§1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans)),
**5 Refinance** ([`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions))
and **3 Repay** ([`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-all-three-of-its-forms)), the
post-settlement state in the application
([`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)),
and the counterparty protocol's own loan records
([`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-all-three-of-its-forms)).

Integration-test results are in the previous submission's
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) — §2 (25 transaction QC
checks) and §3 (8 automated UI tests, all passing). Those figures are our own reporting and are
named as such; the load-bearing evidence is the ledger and Fluid's own records.

"Repay" — the journey the previous submission covered "implicitly" — has its own section:
[`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-all-three-of-its-forms).

---

## The evidence in one screen

**Fifteen signed Cardano mainnet transactions**, two wallets, 18–28 August 2026, grouped by the
journey that produced them — **7 Open**, **5 Refinance**, **3 Repay** — every one built by this
application (metadata label 674) and every Plutus script execution `valid_contract = true`. Each
Fluid loan's position NFT has exactly one mint and one burn in its whole history, so the Open
transaction and the transaction that closed it are paired **by the ledger**, not by this document.

The five refinances:

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
- a **distinct Borrower NFT is minted to the signing wallet** — five different tokens, each with one mint and no burn; this is the per-loan identity ([`05` §2.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#21-two-danogo-tokens-and-which-one-identifies-a-loan))
- **collateral in = collateral out**, to the smallest unit — never returned to the borrower
- one transaction, one block — the close and the reopen cannot partially fail
- the borrower contributed only the Cardano network fee and min-UTxO movement, and the Dano pool funded the repayment — on the four Flexible Pool refinances; `d240fab1…` draws on the fixed-term staking contract and is the exception ([`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑4)
- metadata label 674 = *"Dano Finance: Create Loan"*

**The protocol that was owed the money confirms every repayment.** Fluid Tokens' public API,
queried for these two wallets, returns **seven** loan events — every one
`"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, and every one naming both the
transaction that **opened** the loan (`loanUtxoId`) and the transaction *this application built* to
close it (`finishingTxHash`). The loan token Fluid names is the exact token each transaction burned,
and each transaction pays at least the total Fluid says was due —
[`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-all-three-of-its-forms) and
[`screenshots/fluid-dashboard/`](./screenshots/fluid-dashboard/).

**Repay is evidenced in all three of its forms, on mainnet.** Two transactions settle an **external**
loan from the borrower's own funds — `17c23dde…` and `ea823365…`, metadata *"Dano Finance: Repay
Fluid Loan"*: the borrower pays the debt from their own wallet, the collateral is **released back to
them**, and no Dano loan is created. A third, `77748bd9…`, settles a **Dano** loan on Danogo's own
`Repay Loan` path and **burns the borrower's own title to it**; it is the counterpart to
`ee87712a…`, which opened that loan three hours earlier — a complete open-and-repay cycle with no
external protocol in the path. And five more repayments occur as the settlement leg of the
refinances above —
[`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-all-three-of-its-forms).

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
05_ONCHAIN_TRANSACTIONS_AND_REPAY.md fifteen mainnet transactions grouped by journey,
                                     per-transaction value flow, the repay journey in
                                     all three of its forms
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
or withdrawn, with the on-chain arithmetic. Their walkthrough screenshots in
[`screenshots/`](./screenshots/) remain valid and are still referenced by this package.

---

## Status

| | |
|---|---|
| Per-journey evidence matrix, each journey with its own transactions | ✅ `04` §1 |
| On-chain evidence, independently verifiable | ✅ 15 transactions |
| Post-refinance state in the app, reconciled to the ledger | ✅ every displayed borrowed amount |
| Repay journey evidenced in all three of its forms | ✅ `05` §5 |
| Repayment confirmed by the counterparty protocol's own records | ✅ 7/7 `loan_repaid` |
| Repayment from the borrower's own funds, on mainnet | ✅ `17c23dde…` · `ea823365…` · `77748bd9…` |
| Corrections to our own previous submission | ✅ 4, three of them unprompted |
| Integration-test figures | ✅ reported, and named as our own reporting |
| Testing sessions, per journey, through the Eternl-connected front end | ✅ 15 settled on mainnet, 2 wallets, 11 days — `04` §1.3 |
| Interface walkthrough per journey | ✅ refinance end-to-end including the Eternl signing dialog; open and repay via their own transactions and the resulting app state — `04` §1 |

On the last two rows. The application **is** covered by an automated suite, and every journey was
walked end-to-end through the connected front end during this milestone — fifteen of those sessions
settled on mainnet. The package reports those sessions and rests its verifiable claims on what a
third party can check without us: the ledger and the counterparty protocol's records. The refinance
journey is captured end-to-end, including the Eternl signing dialog; for open and repay, the
journey's own transactions and the resulting app state carry the record, and the sheet captures can
be added on request.

---

## Why mainnet and not testnet

Fluid does not deploy its smart contracts to any Cardano testnet. There is no test network on which
a Fluid → Dano refinance can be executed at all. Mainnet is not a shortcut here — it is the only
environment where this cross-protocol path exists, and it produces the stronger evidence, because
every transaction above is public, immutable, and verifiable by the reviewer without our cooperation.
