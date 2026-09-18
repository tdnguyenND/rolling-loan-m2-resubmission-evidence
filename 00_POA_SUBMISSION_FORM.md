# Proof of Achievement — Milestone 2 (Resubmission)

**Project** 1400107 — Rolling Loan
**Milestone** 2 — Feature Integration
**Submission** Resubmission following the *Not Approved* review
**Milestone page** https://milestones.projectcatalyst.io/projects/1400107/milestones/2

---

We are submitting the Milestone 2 evidence package for Feature Integration of **Rolling Loan**
(called **Refinance via Dano** in the UI — the proposal's name and the product's name for the same
capability; source specification `BorrowModify.Fluid.md` §7.15). This milestone integrates the
refinance feature end-to-end into the Danogo interface, so a borrower can view, open, repay, and
refinance loans directly from the front-end with their own wallet.

Evidence repository:

https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence

---

## Which environment each piece of evidence comes from

The package uses three environments, and never interchangeably:

| | Environment | When |
|---|---|---|
| **Settlement evidence** — the transactions everything rests on | **Cardano mainnet** | 18–28 August 2026 |
| **Interface walkthroughs** — the two journeys captured screen by screen | **preprod** (https://preprod.danogo.io) | 18 September 2026 |
| **Historical test report** (`01`, `02`, `03` in the repository, kept unedited) | staging UI connected to **mainnet** | previous submission |

Where a document says "the live app", it means the front end the wallet was connected to in that
row — mainnet for the settlement evidence, preprod for the walkthroughs.

---

## Response to the previous review

**Objection 1 — a developer's demo is not evidence of an intuitive interface.**

The reviewer is right, and the previous submission is where that landed: the row *"Tester completed
the flow without external guidance — ✅ Yes"* in
[`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md) §4 was a self-assessment
offered as evidence, and this submission does not lean on it.

**We do not claim that these sessions prove the interface is intuitive to an independent user.**
That is the claim the reviewer rejected, and this submission does not make it.

What they do show is that the four workflows were completed through the Eternl-connected interface.
Three things carry that, each checkable on its own:

- **The sessions.** All four journeys were walked end-to-end through the connected front end, and
  **fifteen of those sessions settled on Cardano mainnet** — 7 Open, 5 Refinance, 3 Repay — from two
  separate wallets over eleven days.
- **The click path.** Section D adds **two complete journeys captured screen by screen** —
  *Open → View → Refinance → View*, in opposite asset directions: twenty screenshots, two screen
  recordings of the whole session, the Eternl signing dialog for every signature, and the
  Cardanoscan record of every transaction. In both, the fee, health factor and resulting debt the
  interface quoted **before** signing are the ones the chain holds afterwards.
- **Independent verification.** For all of it, this package provides the signed transactions, the
  resulting application state, and Fluid's own repayment records. The transaction and repayment
  records are publicly verifiable.

Each journey below carries its own user path and its own evidence.

**Objection 2 — per-journey evidence, and the integration-test results.**

The previous submission said open and repay were "covered implicitly" by the refinance. That was
wrong. Each of the four journeys is now evidenced by **its own mainnet transactions**, listed in
section A below, and what links one journey to another is a **token** rather than an inference:
each Fluid loan's position NFT has exactly one mint and one burn in its entire on-chain history —
the mint is the Open transaction, the burn is the Refinance or Repay that closed it. The
integration-test results the reviewer asked for are in
[`04` §1.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) —
25 transaction QC checks against the executed transaction, of which **24 hold** (TC‑24 does not, and
why is in [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) C‑1), and 8 automated UI tests on live mainnet data, all
passing. Those are our own reporting and are named as such.

This submission focuses on demonstrating the fully integrated feature through per-journey mainnet
evidence, a published integration test report, step-by-step user journeys with screenshots and
screen recordings, publicly verifiable mainnet execution transactions, and a demo video.

---

## A. Feature integration

The refinance feature is integrated across the wallet, front-end, back-end, and smart-contract
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
[`04_USER_JOURNEYS_AND_APP_STATE.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md) §2.

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

- Integration test report: [`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md)
- Per-journey detail and the app state:
  [`04_USER_JOURNEYS_AND_APP_STATE.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md)
- Transaction ledger and value flow:
  [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md)
- Application: https://preprod.danogo.io

---

## B. Fully functional feature — successful mainnet execution

The refinance is demonstrated through successful, publicly verifiable mainnet execution
transactions. Mainnet is where the Fluid → Dano path exists as a real market: Fluid's pools that
hold real liquidity, and the collateral assets borrowers actually post, are on mainnet, so a
refinance that settles a real debt is executed there. It is also the stronger evidence — the
transactions are public, immutable, and independently verifiable on Cardanoscan, on a ledger
neither protocol controls. Fluid's smart contracts on **preprod** were used for the interface
walkthroughs in section D; the settlement evidence in this submission is mainnet throughout. (The
previous submission put this more absolutely — see section F, C‑5.)

**Main refinance execution transaction:**

https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c

This transaction demonstrates the refinance flow:

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
section F.)*

All five refinance transactions are `valid_contract: true` (validators accepted them), with
metadata label *"Dano Finance: Create Loan"*, and in each one the **source Fluid position NFT is
burned**. In this package that burn is used as the on-chain indicator that the source Fluid loan was
settled — and Fluid's own API independently reports the same loans as repaid, which is the next
section.

### The protocol that was owed the money confirms the repayments

Fluid Tokens publishes each borrower's loan history from its own indexer, with no credentials
required:

```
GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>
```

Queried on the two borrower wallets it returns **seven** loan events — every one
`"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0`. Two of
its fields matter beyond the status: **`loanUtxoId` names the transaction that opened each loan**,
and **`finishingTxHash` names the transaction this application built to close it**. Fluid's records
and the Cardano ledger produce the Open-to-Close pairing in section A independently of each other,
and they agree. The token Fluid names in its `nft` field is
byte-for-byte the token our transaction burned, and every settlement pays at least the total Fluid
says was due.

**Evidence:**

- Mainnet transaction analysis (inputs/outputs, before/after):
  [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md), and the
  previous submission's [`02_Mainnet_Transactions_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_Mainnet_Transactions_M2.md)
- Transaction screenshots: [`03-eternl-inputs-outputs.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/03-eternl-inputs-outputs.png),
  [`04-transaction-confirmed.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/04-transaction-confirmed.png),
  [`05-cardanoscan.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/05-cardanoscan.png),
  [`06-portfolio-after.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/06-portfolio-after.png)
- Fluid borrower dashboard, the loans marked `REPAID`:
  [`screenshots/fluid-dashboard/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/fluid-dashboard)

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

**Results per journey, as requested by the reviewer.** Two different things are reported here, and
the columns keep them apart: what the executed workflow produced and a third party can observe, and
what an automated test asserts. Only the refinance surface carries an automated suite.

| Journey | Executed workflow / observable result | Automated-test result | Evidence |
|---|---|---|---|
| **View** | Every figure displayed in *My Account → Loans* re-derived from the ledger; the loan count equals the Borrower NFTs the wallet holds on chain (2 and 4) | — *no journey-specific automated test is reported in this package* | [`04` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) |
| **Open** | 7 signed **Open** transactions on mainnet, each `valid_contract = true`; the preprod walkthroughs capture the workflow screen by screen | — *no journey-specific automated test is reported in this package* | [`05` §1.1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) · §D |
| **Repay** | 3 signed mainnet repayments from the borrower's own funds, plus the settlement leg inside each of the 5 refinances; Fluid's own indexer reports all 7 of its loans `loan_repaid`, `remainingDebt: 0` | — *no journey-specific automated test is reported in this package* | [`05` §5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) |
| **Refinance** | 5 signed mainnet refinances; **25 transaction QC checks** (TC‑01 … TC‑25), **24 of which hold** | **8 automated UI tests** (FN‑I7, FN‑I9 … FN‑I14, FN‑J10) on live mainnet data, **8/8 pass** | [`05` §1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) · [`04` §1.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) |

The 25 QC checks and the 8 automated tests are restated as they stand today — including the one
check that no longer holds, TC‑24 — in
[`04` §1.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results),
with the step-by-step detail behind them in
[`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md) §2 and §3, which is
kept unedited as an archive. They are our own reporting about our own work, offered as such. The primary verifiable evidence in this submission is the
Cardano ledger and Fluid's own loan records, both of which the reviewer can query directly.

**Evidence:**

- Current integration-test results, in one table:
  [`04` §1.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results)
- The integration test report they come from (manual journey + detailed QC checks + automated
  results), kept unedited as an archive:
  [`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md)
- Reviewer checklist: [`03_Reviewer_Checklist_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/03_Reviewer_Checklist_M2.md)
- Step-by-step screenshots: [`01-loans-sheet.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/01-loans-sheet.png) … [`06-portfolio-after.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/06-portfolio-after.png)
- Two further journeys captured screen by screen, with screen recordings — section D:
  [`journey-1-borrow-ADA-collateral-USDM/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-1-borrow-ADA-collateral-USDM) ·
  [`journey-2-borrow-USDM-collateral-ADA/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-2-borrow-USDM-collateral-ADA) ·
  [`videos/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/videos)

---

## D. Interface walkthroughs — two complete journeys, captured screen by screen

Objection 1 asked for evidence that a user can complete a specific workflow in the interface.
Two further journeys were therefore walked end-to-end on the preprod deployment
(https://preprod.danogo.io), tested against the Fluid smart contracts on preprod, on
**18 September 2026**, from one Eternl-connected wallet (`addr_test1qr4…f5g494`). Both run
**Open → View → Refinance → View** in opposite asset directions, and each is captured in full:
**ten screenshots covering every screen from the first click to the settled loan, a screen
recording of the whole session, the Eternl signing dialog for both signatures, and the Cardanoscan
record of both transactions.** Nothing between the first click and the settled loan is omitted.

| | Journey 1 | Journey 2 |
|---|---|---|
| Borrowed from Fluid | **25 ADA** | **11 fUSDM** |
| Collateral locked | **100 fUSDM** | **100 ADA** |
| Open signed at | 04:51:28 — *Dano Finance: Borrow from Fluid* | 04:59:12 — *Dano Finance: Borrow from Fluid* |
| Refinance signed at | 04:54:27 — *Dano Finance: Create Loan* | 05:01:04 — *Dano Finance: Create Loan* |
| Refinance transaction | [`0bfa25db…ccfd`](https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd) · block 5189867 | [`78d434d5…95d6`](https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6) · block 5189885 |
| Quoted in the app **before** signing | fee **2 ADA** · HF 7.27 `Healthy` → **1.21** `Fair` · net cost 4.00% → 0.68% | fee **2 fUSDM** · HF 3.63 `Healthy` → **1.87** `Healthy` · net cost 4.00% → −5.86% |
| The loan **after** signing | **Dano Finance · 27 ADA** · HF **1.21** · APR 3.21% · collateral 100 fUSDM | **Dano Finance · 13 fUSDM** · HF **1.87** · APR 3.07% · collateral 100 ADA |
| Screen recording | [`01-open-and-refinance-borrow-ADA-collateral-USDM.webm`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/01-open-and-refinance-borrow-ADA-collateral-USDM.webm) | [`02-open-and-refinance-borrow-USDM-collateral-ADA.webm`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/02-open-and-refinance-borrow-USDM-collateral-ADA.webm) |
| Screenshots | [`journey-1-borrow-ADA-collateral-USDM/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-1-borrow-ADA-collateral-USDM) | [`journey-2-borrow-USDM-collateral-ADA/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-2-borrow-USDM-collateral-ADA) |

**27 ADA = 25 debt + 2 fee. 13 fUSDM = 11 debt + 2 fee.** In both journeys the health factor the
interface showed before the user signed — 1.21 and 1.87 — is the health factor the new loan carries
afterwards, to two decimals, and the fee it quoted is the fee that was capitalised. The APR drops to
the rate of the Dano pool the quote named (3.21% and 3.07%), and the collateral is the same
collateral: 100 fUSDM and 100 ADA, unchanged, never returned to the borrower in between.

### D.1 Every screen, in the order the user sees it

Each caption says what the screenshot shows on screen. What the corresponding transaction does
on chain is in D.2.

| Step | What the screenshot shows | Journey 1 | Journey 2 |
|---|---|---|---|
| 1 | **Borrow Market** — the pools offered for the chosen collateral, the amounts entered, and the quoted *Loan Impact*, all before anything is signed | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/01-borrow-market-create-loan.png) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/01-borrow-market-create-loan.png) |
| 2 | **Eternl, opening signature** — the wallet's own view of one transaction: memo *"Dano Finance: Borrow from Fluid"*, the collateral leaving the wallet, the position token minted | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/02-eternl-sign-open-from-fluid.png) | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/02-eternl-sign-open-from-fluid.png) |
| 3 | **Portfolio** — the position after the Open: a *Fluid · Borrow* row and a *Fluid · Supply* row for the collateral | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/03-portfolio-after-open.png) | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/03-portfolio-after-open.png) |
| 4 | **My Account → Loans** *(the View journey)* — the loan as the app lists it: borrowed amount, collateral, APR, health factor | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/04-my-account-loans.png) | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/04-my-account-loans.png) |
| 5 | **Refinance via Dano preview** — the quoted fee, the health factor before → after, and the collateral, shown before signing | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/05-loan-details-refinance-quote.png) | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/05-loan-details-refinance-quote.png) |
| 6 | **Eternl, refinance signature** — the signature request for one transaction: memo *"Dano Finance: Create Loan"*, tagged Mint and Burn, transaction id visible before signing | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/06-eternl-sign-refinance.png) | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/06-eternl-sign-refinance.png) |
| 7 | **Loan Details, reopened** *(View)* — the resulting loan: protocol now *Dano Finance*, debt, APR, health factor, same collateral | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/09-loan-details-after-refinance.png) | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/07-loan-details-after-refinance.png) |
| 8 | **Portfolio** — the *Fluid · Borrow* row replaced by a *Dano · Borrow* row | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/10-portfolio-dano-borrow-27-ADA.png) | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/10-portfolio-after-both-refinances.png) |
| 9 | **Eternl → Transactions** — the wallet's own record of both transactions, with memos, block number and confirmations | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/07-eternl-transaction-list.png) | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/08-eternl-transaction-list.png) |
| 10 | **Cardanoscan** — the refinance transaction as a third party sees it | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-1-borrow-ADA-collateral-USDM/08-cardanoscan-refinance-utxos.png) | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/09-cardanoscan-refinance-overview.png) |

Steps 1–3 are the **Open** journey, step 4 is **View**, steps 5–6 are **Refinance**, and steps 7–8
are **View** again, on the loan the refinance created. The *Repay* action sits on the same
*Loan Details* screen the walkthrough opens twice (steps 5 and 7); the wallet's transaction list at
step 9 also shows three *"Dano Finance: Repay Loan"* transactions signed from the same wallet
minutes earlier. Journey 2's step 8 capture also lists a second, unrefinanced Fluid loan of
2 fUSDM, which this session did not touch.

### D.2 What each refinance transaction does on chain

**Journey 1 — [`0bfa25db…ccfd`](https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd)** · 3 inputs → 6 outputs · 7 contracts ·
3 mints & burns · 1 metadata entry · 14 reference inputs · network fee t₳1.56 · block 5189867

- input `#0` is the **Fluid loan UTxO**, carrying the **fUSDM 100.000000** collateral
- output `#3` pays that same **fUSDM 100.000000** to the Dano loan contract — the collateral is
  carried across, not returned to the borrower
- a **25.000003 ₳** output settles the Fluid debt, to `addr_test1qr9ew225…s2n6em`
- a separate **2.000000 ₳** output pays the origination fee the interface had quoted
- the transaction is one transaction, in one block, tagged **Mint** and **Burn**, and its metadata
  reads *"Dano Finance: Create Loan"*

**Journey 2 — [`78d434d5…95d6`](https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6)** · 3 inputs → 6 outputs · 7 contracts ·
3 mints & burns · 1 metadata entry · 14 reference inputs · fee 1.58428 ₳ · block 5189885 ·
epoch 314 / slot 10864 · 5.5 KB · ex-units 3.7M mem · 1.2B steps (21.35% / 12.37% of budget)

- **100.0 ₳** leaves the Fluid script address and **100.0 ₳** arrives at the Dano contract — one
  token is burned on the way out and one is minted on the way in
- the Dano pool disburses **fUSDM 13.000007**, of which **fUSDM 11.000001** settles the Fluid debt
  (the same `addr_test1qr9ew225…s2n6em` that received the settlement in Journey 1) and exactly
  **fUSDM 2.000000** goes to the fee address — the 11 + 2 = 13 the interface quoted
- the borrower's own outlay is the network fee; the pool funds the repayment

In Journey 2 the token Eternl shows being **minted** by the open transaction
(`2d3883b3…`, step 2) is the token it shows being **burned** by the refinance (step 6): the same
position token, one mint and one burn, opened and closed by the two transactions of the same
session.

### D.3 What this adds, and what it does not claim

- **It closes the capture gap.** The *open* journey now has its own interface walkthrough including
  its Eternl signing dialog — previously it was carried by its mainnet transactions and the
  resulting app state ([`04` §1.4](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#14-the-scope-of-these-claims), point 2). Open, View and Refinance are now
  captured screen by screen, twice, in both asset directions.
- **The whole session is recorded, not just its end points.** The two screen recordings show the
  clicks, the typing, the waiting and the wallet pop-ups between the screenshots, so the click path
  can be followed without our narration.
- **The number shown is the number that settled** — again, and now on a loan opened minutes earlier
  in the same session: the quoted fee, health factor and resulting debt match the loan the chain
  holds afterwards.
- **What it does not claim.** These are our own sessions; they are not evidence that an independent
  user finds the interface intuitive, and we do not present them as that. What they establish is the
  complete, uninterrupted click path of each journey, and the agreement between what the interface
  promised and what the chain recorded. The settlement evidence for the milestone remains the
  mainnet transactions in section A, which anyone can verify without us.

---

## E. Clarification

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

## F. Correction to our own previous submission

While re-deriving every figure from a public API for this resubmission, we found errors in our own
evidence and are disclosing them unprompted. The full arithmetic for each is in
[`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md).

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

**C-5 — "Fluid has no Cardano testnet deployment" was stated too absolutely.** The previous
submission gave that as the reason mainnet was used. The interface walkthroughs in section D were
tested against the Fluid smart contracts on **preprod**, so the absolute form of the claim does not
hold and is withdrawn. What does hold is the reason the *settlement* evidence is on mainnet: that is
where the Fluid → Dano path exists as a real market, and where the transactions are verifiable by a
third party without us. No on-chain figure in this package changes.

---

## Summary

This submission demonstrates the fully integrated **Refinance via Dano** feature,
journey by journey:

- **View** — the borrower's loans in *My Account*, every displayed figure re-derived from the
  ledger.
- **Open** — **7 mainnet Open transactions**, and two walkthroughs that capture the workflow screen by
  screen from the first click to the settled loan (section D).
- **Repay** — **3 mainnet Repay transactions made as a direct user action**, one of them on
  Danogo's own `Repay Loan` path, plus the settlement leg inside every Refinance — the *Repay* step
  performed inside a *Refinance*, in the same transaction; the counterparty protocol's own
  records report **seven** loans repaid with nothing owing.
- **Refinance** — **5 mainnet refinances**, each consuming the Fluid loan UTxO, creating the Dano
  loan UTxO, carrying the collateral across and burning the source position NFT in one transaction;
  **25 QC checks** (24 hold) and **8/8 automated UI tests** on the refinance surface.

All 15 transactions were built by this application (metadata label 674), and every Plutus script
execution in them returned `valid_contract = true`. The detailed evidence map is in
[`04` §1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl).

The core functionality demonstrated is an active Fluid loan state being consumed and a new Dano
Finance loan state being created in a single atomic refinancing transaction, integrated end-to-end
and executed successfully on Cardano mainnet.
