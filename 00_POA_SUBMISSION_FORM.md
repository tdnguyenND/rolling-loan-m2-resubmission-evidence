# Proof of Achievement — Milestone 2 (Resubmission)

**Project** 1400107 — Rolling Loan
**Milestone** 2 — Feature Integration
**Submission** Resubmission following the *Not Approved* review
**Milestone page** https://milestones.projectcatalyst.io/projects/1400107/milestones/2

---

We are submitting the Milestone 2 evidence package for Feature Integration of the Rolling Loan
mechanism. This milestone integrates the rolling-loan / refinancing feature end-to-end into the
Danogo interface, so a borrower can view, open, repay, and refinance loans directly from the
front-end with their own wallet.

Evidence repository:

https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence

Naming note: in this proposal the feature is called "Rolling Loan"; in the delivered product and UI
it is named "Refinance via Dano". The two names refer to the same capability. Source specification:
`BorrowModify.Fluid.md` §7.15.

---

## Response to the previous review

**Objection 1 — a developer's demo is not evidence of an intuitive interface.**

The reviewer is right, and the previous submission is where that landed: the row *"Tester completed
the flow without external guidance — ✅ Yes"* in
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §4 was a self-assessment
offered as evidence, and this submission does not lean on it.

What the testing sessions themselves record is kept, and is reported per journey rather than as a
verdict: all four journeys were walked end-to-end through the Eternl-connected front end, and
**fifteen of those sessions settled on Cardano mainnet** — 7 Open, 5 Refinance, 3 Repay — from two
separate wallets over eleven days. Each journey below carries its own user path and its own
evidence, and the load-bearing evidence is public and third-party-checkable: the Cardano ledger,
and the records of the protocol whose loans were settled. A reviewer can check all of it without
our cooperation.

**Objection 2 — per-journey evidence, and the integration-test results.**

The previous submission said open and repay were "covered implicitly" by the refinance. That was
wrong. Each of the four journeys is now evidenced by **its own mainnet transactions**, listed in
section A below, and what links one journey to another is a **token** rather than an inference:
each Fluid loan's position NFT has exactly one mint and one burn in its entire on-chain history —
the mint is the Open transaction, the burn is the Refinance or Repay that closed it. The
integration-test results the reviewer asked for are in
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §2 (25 transaction QC
checks against the executed transaction) and §3 (8 automated UI tests on live mainnet data, all
passing); those are our own reporting and are named as such.

This submission focuses on demonstrating the fully integrated feature through per-journey mainnet
evidence, a published integration test report, a step-by-step user journey with screenshots,
publicly verifiable mainnet execution transactions, and a demo video.

---

## A. Feature integration

The rolling-loan feature is integrated across the wallet, front-end, back-end, and smart-contract
layers. From the live app a user can view, open, repay, and refinance a loan — refinancing
(rolling) a loan from Fluid into a Dano Finance (Dano Float) loan, closing the source loan and
originating the new Dano loan atomically, with collateral carried across and no manual
repay-then-reopen.

This is enabled by the Dano Borrow Aggregator, which integrates external lending protocols (Fluid,
Liqwid, Surf); rolling a Fluid loan into a Dano loan is the first cross-protocol case delivered.

### Each of the four approved journeys, with its own transactions

All of the transactions below were built by this application — each carries Danogo's own metadata
under label 674 — and every Plutus script execution in every one of them returned
`valid_contract = true`.

**VIEW** — no transaction by nature; the user reads their position.

Evidence: *My Account → Loans* captured from the live app on **both** signing wallets, with every
displayed borrowed amount re-derived from the Cardano ledger (on-chain principal plus interest at
the APR the same row displays). The number of loans the app lists equals the number of Danogo
**Borrower NFTs** each wallet holds on chain — **2** and **4** — a count held on the ledger, not in
our database. See
[`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md) §2.

**OPEN** — 7 mainnet transactions. Metadata *"Dano Finance: Borrow from Fluid"* (6) and
*"Dano Finance: Create Loan"* (1).

| Tx | Date (UTC) | Collateral locked |
|---|---|---|
| [`917bdfe1…b289`](https://cardanoscan.io/transaction/917bdfe19763fac39a2a154a817f8297958e7f4fb12cf7047a7ee8661e61b289) | 2026-08-18 | USDM 10.000000 |
| [`247e1218…8e4e`](https://cardanoscan.io/transaction/247e121881a03f483556bc2339a1d6be9a52cd37299dc97c57f3a1dc46368e4e) | 2026-08-24 | ADA 50.000000 |
| [`a3ed946c…bf6c`](https://cardanoscan.io/transaction/a3ed946c165fc9de0e48fe318a5764ca6149e779818b043c01740b7ba60dbf6c) | 2026-08-24 | SNEK 20,979 |
| [`7a6caf51…945d`](https://cardanoscan.io/transaction/7a6caf51f61f1a7ca635f74a1b8df629c48550e20a61d869798fa4b91d3e945d) | 2026-08-25 | DJED 6.000000 |
| [`4901277c…77d7`](https://cardanoscan.io/transaction/4901277c80a06dd3d891eaccb90fd91b0e07037809bd99dd1477c9c4fd6777d7) | 2026-08-25 | DJED 6.000000 |
| [`9b3aa00c…36ee`](https://cardanoscan.io/transaction/9b3aa00c0c13093fe2ef9489f3fa6b3877195af45d8f35935a0ea3a2340736ee) | 2026-08-26 | DJED 10.000000 |
| [`ee87712a…7bca`](https://cardanoscan.io/transaction/ee87712aef570de2e0ac8616935f30949f57d20942c2ddbd8d25a94bfce97bca) | 2026-08-28 | ADA 35.000000 — opens a **Dano** loan directly, no external protocol |

Each of the first six locks the collateral and mints that loan's Fluid position NFT — the token
that is burned again by the Refinance or Repay transaction that closes it.

**REPAY** — 3 mainnet transactions in which the borrower settles a debt from their **own** funds,
plus the five settlement legs inside the refinances.

| Tx | Metadata 674 | What it does |
|---|---|---|
| [`17c23dde…559a`](https://cardanoscan.io/transaction/17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a) | *Repay Fluid Loan* | pays **USDCx 5.000011** from the borrower's own wallet; releases **ADA 50.000000** of collateral back to them; burns the Fluid position NFT; creates no Dano loan. Closes the loan opened by `247e1218…` |
| [`ea823365…e04d`](https://cardanoscan.io/transaction/ea823365562e1eefec0cb3be614a54963ec128dcb3ef8430099d308902bfe04d) | *Repay Fluid Loan* | settlement output **2.011600 ₳**; releases **USDM 16.000000** of collateral |
| [`77748bd9…22bc`](https://cardanoscan.io/transaction/77748bd9673991565c25671522fe70914e29098d7bd0f4c164cf4677585522bc) | **_Dano Finance: Repay Loan_** | Danogo's **own** repayment path, on Danogo's own contracts. The borrower pays **USDM 6.096399** from their own wallet; it splits exactly between the pool (4.396378) and the fee address (1.700021); the **Borrower NFT is burned**. It is the counterpart to `ee87712a…` above, which opened that loan three hours earlier — a complete open-and-repay cycle with no external protocol in the path |

**REFINANCE** — 5 mainnet transactions, each closing a Fluid loan and opening a Dano loan
atomically.

| Tx | Date | Collateral carried | Borrowed | Liquidity source | Origination fee |
|---|---|---|---|---|---|
| [`88579a30…6652`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 08-19 | USDM 10.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`d240fab1…d84c`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 08-24 | SNEK 20,979 | ADA | Staking (fixed-term) | 0.969750 ₳ |
| [`1cf8f08b…4f10`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 08-25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`c426d9fa…25c8c`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 08-25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`0e26cc58…05b8`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 08-27 | DJED 10.000000 | **STRIKE** | Flexible Pool | **none** |

Two wallets, seven distinct days, four collateral assets, three borrowed assets, two Dano liquidity
sources, three origination-fee configurations.

**Evidence:**

- Integration test report: [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md)
- Per-journey detail and the app state:
  [`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md)
- Transaction ledger and value flow:
  [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md)
- Staging application (live mainnet contracts): https://v3.danogo.io/

---

## B. Fully functional feature — successful mainnet execution

The refinance is demonstrated through successful, publicly verifiable mainnet execution
transactions. Mainnet is used because Fluid does not deploy its smart contracts on any Cardano
testnet, so the Fluid → Dano flow can only be executed on mainnet — where the transactions are
public, immutable, and independently verifiable on Cardanoscan.

**Main rolling-loan execution transaction:**

https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c

This transaction demonstrates the tested rolling-loan / refinancing flow:

- an existing loan UTxO is consumed from the Fluid smart contract (carrying the DJED collateral)
- a new loan UTxO is created in the Dano Float smart contract
- the old Fluid loan is settled atomically in the same transaction using the newly originated loan
- collateral continues into the new loan position
- the borrower does not need to manually repay, withdraw collateral, and open a new loan separately

**A second transaction shows the same flow against a Dano pool that charges no origination fee**,
and with a non-ADA borrowed asset — the pool disburses STRIKE 5.000560, the settlement receives
STRIKE 5.000558, and the transaction contains **no leg to the Dano fee address at all**:

https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8

*(The previous submission cited `88579a30…` as the no-fee example. That was incorrect — see
section E.)*

All five refinance transactions are `valid_contract: true` (validators accepted them), with
metadata label *"Dano Finance: Create Loan"*, and in each one the **source Fluid position NFT is
burned** — a burn that Fluid's own validator permits only when the loan is settled in full.

### The protocol that was owed the money confirms the repayments

Fluid Tokens publishes each borrower's loan history from its own indexer, with no credentials
required:

```
GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>
```

Queried on the two borrower wallets it returns **seven** loan events — every one
`"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0`. Two of
its fields matter beyond the status: **`loanUtxoId` names the transaction that opened each loan**,
and **`finishingTxHash` names the transaction this application built to close it**. The
Open-to-Close pairing in section A is therefore not our construction — Fluid's records and the
Cardano ledger produce it independently, and they agree. The token Fluid names in its `nft` field is
byte-for-byte the token our transaction burned, and every settlement pays at least the total Fluid
says was due.

**Evidence:**

- Mainnet transaction analysis (inputs/outputs, before/after):
  [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md), and the
  previous submission's [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md)
- Transaction screenshots: `screenshots/03-eternl-inputs-outputs.png`,
  `04-transaction-confirmed.png`, `05-cardanoscan.png`, `06-portfolio-after.png`
- Fluid borrower dashboard, the loans marked `REPAID`:
  [`screenshots/fluid-dashboard/`](./screenshots/fluid-dashboard/)

---

## C. Test case and test result

The test evidence has two layers: a manual end-to-end journey (real signed transaction) and an
automated UI suite, both against live mainnet data. The manual journey maps each step to its
expected result, actual on-chain evidence, and pass/fail status.

**Key verified results:**

- source Fluid loan UTxO is consumed; a new Dano Float loan UTxO is created
- collateral (DJED) is carried across into the new loan — not returned to the borrower
- the refinance completes in one transaction, `valid_contract: true`, with no separate repayment
- for the ADA fee pool: origination fee = `max(0.1% of borrow, 2 ADA)` = 2 ADA, and the loan is
  bumped by that fee (12 ADA debt → 14 ADA borrow), financed by the loan with no upfront capital
- for a no-fee pool: the new loan borrow equals the exact Fluid debt (no fee bump) — evidenced by
  `0e26cc58…`, which has no fee leg at all
- automated integration tests for the refinance UI: 8/8 pass on live data
- after the refinance, the Fluid position is settled and a single Dano loan remains

**Integration-test results, as requested by the reviewer — per journey:**

| Journey | Test result | Where |
|---|---|---|
| **View** | Every figure displayed in *My Account → Loans* re-derived from the ledger; the loan count equals the Borrower NFTs the wallet holds on chain (2 and 4) | [`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) |
| **Open** | 7 signed mainnet originations, each `valid_contract = true` | [`05` §1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) |
| **Repay** | 3 signed mainnet repayments plus 5 settlement legs inside the refinances; Fluid's own indexer reports all 7 of its loans `loan_repaid`, `remainingDebt: 0` | [`05` §5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-the-repay-journey-in-all-three-of-its-forms) |
| **Refinance** | 5 signed mainnet refinances; 25 transaction QC checks (TC‑01 … TC‑25); 8 automated UI tests (FN-I7, FN-I9 … FN-I14, FN-J10) on live mainnet data, all passing | [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) · [`01` §2–§3](./01_Integration_Test_Report_M2.md) |

The 25 QC checks and the 8 automated tests are documented in
[`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §2 and §3 — our own
reporting about our own work, offered as such. The load-bearing evidence in this submission is the
Cardano ledger and Fluid's own loan records, both of which the reviewer can query directly.

**Evidence:**

- Integration test report (manual journey + detailed QC checks + automated results):
  [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md)
- Reviewer checklist: [`03_Reviewer_Checklist_M2.md`](./03_Reviewer_Checklist_M2.md)
- Step-by-step screenshots: `screenshots/01-loans-sheet.png` … `06-portfolio-after.png`

---

## D. Clarification

This milestone builds on Milestone 1. Milestone 1 proved the rolling/refinancing mechanism at the
smart-contract level (between two Dano contracts). Milestone 2 applies it to the real cross-protocol
case (Fluid → Dano) and integrates it end-to-end into the live product.

The evidence does not rely only on written claims: the delivered behavior is verified through public
blockchain data — the mainnet transactions show the source Fluid loan consumed, the Dano loan
created, and the collateral carried across, all in a single atomic transaction. Every figure in this
submission was re-derived from the public **Koios** API (`api.koios.rest`) rather than from our own
backend, and the repayment statuses from **Fluid's** own public API.

The state the transactions left behind is also shown in the application: both signing wallets'
*My Account → Loans* screens were captured from the live app, and every displayed borrowed amount
equals the on-chain principal plus interest accrued at the APR that same row displays. All five
refinanced Fluid position NFTs now have a **total supply of zero** across Cardano — the repaid loans
do not exist anywhere on chain, which is why neither borrower's screen lists a Fluid loan.

**Demo video:**

https://youtu.be/z07TxLJLC2w

---

## E. Correction to our own previous submission

While re-deriving every figure from a public API for this resubmission, we found errors in our own
evidence and are disclosing them unprompted. The full arithmetic for each is in
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md).

**C-1 — the wrong transaction was cited as the zero-fee example.** We cited `88579a30…` as a
zero-origination-fee example, with the new borrow equal to the Fluid debt. The chain contradicts
that. On `88579a30…` the Dano Flexible Pool disburses **22.000857 ₳**, of which **20.000851 ₳**
settles the Fluid debt and exactly **2.000000 ₳** goes to the Dano origination-fee address. The pool
charges a fee, and the fee is capitalised into the new loan: a 20 ADA debt became a 22 ADA Dano
loan. That is by design and it is why the borrower needs no capital of their own — but it makes
`88579a30…` the wrong transaction to cite for the no-fee case. The underlying claim is correct; the
transaction cited for it was wrong. The zero-fee example is `0e26cc58…`, which has no leg to the fee
address at all, and which is also a better example because it uses a non-ADA borrowed asset.

**C-2 — the usability self-assessment is reported as the sessions it recorded, not as a verdict.**
See *Response to the previous review*, Objection 1.

**C-3 — a recurring marker token was described as a per-loan NFT.** Danogo mints two tokens per
loan. The one that stays in the loan contract is a **recurring marker** — `asset1pr26rn8r…` has 57
mints and 49 burns, and is the *same* token in three of the five refinances. The per-loan identity
is the **Borrower NFT** paid to the borrower's wallet (one mint, no burn while the loan is open).
Every "a distinct loan was created" claim is now made against the Borrower NFT.

**C-4 — "the borrower contributes only the network fee" was over-stated.** It holds for the four
Flexible Pool refinances. On `d240fab1…`, which draws on the fixed-term staking contract, the
disbursement (15.015024 ₳) is smaller than the settlement plus fee (15.969752 ₳) and the borrower
funds the **0.954728 ₳** difference. That transaction still settles the Fluid debt in full and still
carries the collateral across; it simply does not demonstrate the "no capital needed" property.

---

## Summary

This submission demonstrates the fully integrated Rolling Loan (Refinance via Dano) feature through:

- an integrated front-end with view / open / repay / refinance loan actions, **each journey
  evidenced by its own mainnet transactions** rather than inferred from another journey
- completed back-end API integration building the on-chain transactions
- **15 mainnet transactions** built by this application — 7 Open, 5 Refinance, 3 Repay — every
  Plutus script execution returning `valid_contract = true`
- a manual end-to-end user journey with step-by-step screenshots
- an automated integration test suite (8/8 refinance cases passing on live data) and 25 transaction
  QC checks
- the repay journey evidenced in all three of its forms: on Danogo's own contracts, on an external
  protocol, and as the settlement leg of a rolling loan
- source Fluid loan input UTxO consumed, target Dano loan output UTxO created, collateral carried
- **seven repayments confirmed by the counterparty protocol's own public records**, with nothing
  owing
- the resulting loans shown in the application and reconciled row by row against the ledger
- a reviewer checklist and a demo video

The core functionality demonstrated is an active Fluid loan state being consumed and a new Dano
Finance loan state being created in a single atomic refinancing transaction, integrated end-to-end
and executed successfully on Cardano mainnet.
