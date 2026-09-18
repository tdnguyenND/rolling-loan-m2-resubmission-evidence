# Milestone 2 — Proof of Achievement Resubmission

**Project Catalyst · Project 1400107 — Rolling Loan**
**Milestone 2 — Feature Integration** · resubmission after *Not Approved*
Milestone page → https://milestones.projectcatalyst.io/projects/1400107/milestones/2

**Rolling Loan** (called **Refinance via Dano** in the UI) — the proposal's name and the product's
name for the same capability. This package uses *Refinance via Dano*, or simply *refinance*.

---

### Which environment each piece of evidence comes from

The package uses three environments, and never interchangeably:

| | Environment | When |
|---|---|---|
| **Settlement evidence** — the transactions everything rests on | **Cardano mainnet** | 18–28 August 2026 |
| **Interface walkthroughs** — the two journeys captured screen by screen | **preprod** (https://preprod.danogo.io) | 18 September 2026 |
| **Historical test report** (`01`, `02`, `03`, kept unedited) | staging UI connected to **mainnet** | previous submission |

Where a document says "the live app", it means the front end the wallet was connected to in that
row — mainnet for the settlement evidence, preprod for the walkthroughs.

---

## For the reviewer — read in this order

| # | Document | What it answers | Time |
|---|---|---|---|
| 1 | [`00_POA_SUBMISSION_FORM.md`](./00_POA_SUBMISSION_FORM.md) | The submission itself: the response to both objections, then Output 1–5 with their evidence | 8 min |
| 2 | [`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md) | **The four journeys through the Eternl-connected front end**, and the resulting loans in the app matched to the ledger | 9 min |
| 3 | [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | **Fifteen mainnet transactions** grouped by journey, the value flow of each, and the repayment evidence — two direct-repay flows, plus repayment within refinance | 14 min |
| 4 | [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) | Errors we found in **our own** previous submission, unprompted | 3 min |

In a hurry? Read the journey matrix in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl), then the
transactions in [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions).

The integration-test results, as they stand today, are in
[`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) — one table,
including the one check that no longer holds. Read that rather than the old report.

The previous submission's own files — the integration test report, the transaction evidence and the
reviewer checklist — are kept **unedited** at their original paths as an **archive**, each with a
banner pointing at [`08_CORRECTIONS.md`](./08_CORRECTIONS.md).
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) still holds the step-by-step
detail behind those results, and is linked from `04` §1.5 where it is still good.

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
and **3 Repay** ([`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance)), the
post-settlement state in the application
([`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)),
and the counterparty protocol's own loan records
([`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance)).

Integration-test results are in
[`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) as they stand
today — 25 transaction QC checks of which **24 hold**, and 8 automated UI tests, all passing — with
the step-by-step detail in the previous submission's
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §2–§3, kept unedited as an
archive. Those figures are our own reporting and are named as such; the primary verifiable evidence is
the ledger and Fluid's own records.

"Repay" — the journey the previous submission covered "implicitly" — has its own section:
[`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance).

---

## The evidence in one screen

| Journey | In the interface | On the ledger |
|---|---|---|
| **View** | *My Account → Loans* on both signing wallets — [`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) | every displayed figure re-derived from the chain — [`04` §2.4](./04_USER_JOURNEYS_AND_APP_STATE.md#24-row-by-row-the-displayed-amount-is-the-ledger-amount) |
| **Open** | two journeys captured screen by screen, first click to settled loan — [`00` §D](./00_POA_SUBMISSION_FORM.md) | **7 mainnet Open transactions** — [`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) |
| **Repay** | the *Repay* action in the loan sheet — [`04` §1.3](./04_USER_JOURNEYS_AND_APP_STATE.md#13-the-user-path-for-each-journey) | **3 direct mainnet Repay transactions**, plus the settlement leg inside every Refinance (the *Repay* step performed in the same transaction); Fluid's own API reports all seven loans repaid — [`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) |
| **Refinance** | refinance card → Eternl dialog → confirmation → portfolio after — [`01` §1](./01_Integration_Test_Report_M2.md) | **5 mainnet refinances** — collateral in = out, source position NFT burned, Borrower NFT minted — [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) |

Three things hold across all of it:

- **Fifteen signed Cardano mainnet transactions**, two wallets, 18–28 August 2026 — every one built
  by this application (metadata label 674), every Plutus script execution `valid_contract = true`.
- **The pairing is the ledger's, not ours.** Each Fluid loan's position NFT has exactly one mint and
  one burn in its whole history: the mint is the Open transaction, the burn is the transaction that
  closed it. Fluid's own API independently reports the same loans as repaid, naming both hashes.
- **The interface quoted what settled.** Before signing `c426d9fa…` it showed a Fluid debt of 12 ADA
  and a fee of 2 ADA; on chain the settlement is 12.000102 ADA, the fee exactly 2.000000 ADA, and
  DJED 6.000000 is carried across.

The transaction-by-transaction detail is in
[`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md); the journey-by-journey detail, including the
integration-test results as they stand today, is in
[`04` §1](./04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl).

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
  journey-1-borrow-ADA-collateral-USDM/    open + view + refinance, screen by screen
  journey-2-borrow-USDM-collateral-ADA/    the same, in the opposite asset direction
  *.png                              walkthrough captures of the manual refinance journey

videos/                              screen recordings of the two journeys above,
                                     first click to settled loan (`00` §D)

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
| Repay journey evidenced — two direct-repay flows, plus repayment within refinance | ✅ `05` §5 |
| Repayment confirmed by the counterparty protocol's own records | ✅ 7/7 `loan_repaid` |
| Repayment from the borrower's own funds, on mainnet | ✅ `17c23dde…` · `ea823365…` · `77748bd9…` |
| Corrections to our own previous submission | ✅ 5, four of them unprompted |
| Integration-test figures | ✅ reported, and named as our own reporting |
| Testing sessions, per journey, through the Eternl-connected front end | ✅ 15 settled on mainnet, 2 wallets, 11 days — `04` §1.3 |
| Interface walkthrough per journey | ✅ refinance end-to-end including the Eternl signing dialog; **open captured end-to-end** in the two preprod walkthroughs — `00` §D; repay via its own transactions and the resulting app state — `04` §1 |

On the last two rows. The application **is** covered by an automated suite, and every journey was
walked end-to-end through the connected front end during this milestone — fifteen of those sessions
settled on mainnet. The package reports those sessions and rests its verifiable claims on what a
third party can check without us: the ledger and the counterparty protocol's records. The refinance
journey is captured end-to-end, including the Eternl signing dialog, and so is the **open** journey
— in the two walkthroughs added for this resubmission, which run *Open → View → Refinance → View*
on preprod with a screenshot of every screen and a screen recording of the whole session
([`00`](./00_POA_SUBMISSION_FORM.md) §D). For **repay** there is no end-to-end recording: its entry
point is visible in those walkthroughs, and the journey's own transactions and the resulting app
state carry the record. A recorded repay can be added on request.

---

## Why the settlement evidence is on mainnet

Mainnet is where the Fluid → Dano path exists as a real market: that is where Fluid's pools hold
real liquidity and where the collateral assets borrowers actually post live, so a refinance that
settles a real debt is executed there. Mainnet is not a shortcut here — it is the environment this
cross-protocol path lives in, and it produces the stronger evidence, because every transaction above
is public and immutable, and the transaction and repayment records are publicly verifiable.

Fluid's smart contracts on **preprod** were used for the two interface walkthroughs added to this
resubmission — open and refinance, screen by screen, with preprod Cardanoscan links
([`00` §D](./00_POA_SUBMISSION_FORM.md#d-interface-walkthroughs--two-complete-journeys-captured-screen-by-screen)).
The settlement evidence in this package is mainnet throughout. The previous submission stated more
absolutely that Fluid has no testnet deployment at all; that overstatement is withdrawn and recorded
in [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑5.
