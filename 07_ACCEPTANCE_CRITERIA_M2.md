# Milestone 2 — Acceptance Criteria, mapped to this resubmission

This is the mapping for the **resubmission**. It replaces
[`03_Reviewer_Checklist_M2.md`](./03_Reviewer_Checklist_M2.md), which is the previous submission's
mapping and is kept unedited as an archive — several of its ✅ rest on claims we have since withdrawn
([`08_CORRECTIONS.md`](./08_CORRECTIONS.md)).

Every row below points at evidence a third party can check without us. Where a criterion is met with
open items still attached, the row says which ones and links them; **§E** lists everything this
package leaves open, including the one thing it cannot evidence at all.

"Rolling Loan" = the **Refinance via Dano** feature.

**Where the app is.** Mainnet: https://v3.danogo.io/ · preprod: https://preprod.danogo.io. All
fifteen settlement transactions were signed from the mainnet app between 18 and 28 August 2026; the
four screen-by-screen walkthroughs were run on preprod on 18 September 2026.

---

## A. Milestone Outputs

| Output | Satisfied by | Status |
|---|---|---|
| Integrated front-end with loan actions (view, open, repay, refinance) | **15 mainnet transactions** signed from the app — 7 open, 5 refinance, 3 repay — [`05` §1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions), [§1.1](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans), [§5](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) · all four actions captured screen by screen in [`06`](./06_UAT_Reports_Four_Journeys_M2.md) | ✅ |
| Completed connection to back-end API endpoints | every quoted figure re-derived from the ledger and found to match — [`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger); Fluid's own API independently reports the same loans repaid — [`05` §5.3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) | ✅ |
| Ready-to-test user journeys | four complete sessions on three Eternl accounts, each with a screen recording, every signing dialog and every explorer record — [`06`](./06_UAT_Reports_Four_Journeys_M2.md), **123 QC checks, 123 hold** | ✅ |
| Demo video | **four unedited screen recordings of complete workflows**, first click to settled transaction, one per journey (3 min 50 s – 7 min 29 s) — [`videos/`](./videos/). Every frame is cross-referenced step by step in [`06`](./06_UAT_Reports_Four_Journeys_M2.md), so any figure on screen can be checked against the chain | ✅ |

> **On the demo video.** The link in the previous submission, https://youtu.be/z07TxLJLC2w, was a
> developer walking through the feature — the first thing the reviewer rejected. **It is withdrawn as
> evidence.** What stands in its place is the four session recordings above: unedited, full-length,
> and showing the interface being operated rather than described.

## B. Acceptance Criteria

| # | Criterion | What we can show | Status |
|---|---|---|---|
| **AC1** | View / open / repay / refinance via **Eternl** wallet | all four exercised through Eternl: 15 mainnet transactions from 2 wallets, plus 4 preprod sessions on 3 Eternl accounts and 2 Eternl versions (v2.1.7.1, v2.1.5.0) — [`06` At a glance](./06_UAT_Reports_Four_Journeys_M2.md#at-a-glance). **View** produces no transaction by nature; its evidence is the app's own screens reconciled to the ledger | ✅ |
| **AC2** | Back-end correct contract interactions for all loan states | every Plutus script execution in all 15 transactions returned `valid_contract = true`; inputs, outputs, mints and burns re-derived from the public **Koios** API — [`05` §2](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions), [§3](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow) | ✅ |
| **AC3** | Interface **stable** — no crash, no data mismatch | **No crash, no hang and no lost state in any of the four sessions**, across 123 QC checks. On the settled loan every figure the interface displays reconciles to the ledger — fee, collateral, health factor and resulting debt, to the lovelace, in all five refinances and all three repayments ([`06`](./06_UAT_Reports_Four_Journeys_M2.md)). Two items are tracked as open rather than as failures: journey 3's first submit needed a *Retry* and recovered, with exactly one transaction on chain ([`06` D-1](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet)), and the *Deposit* / *Fee* lines in the pre-signature preview are still unreconciled ([`06` OI-1](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session)) — see §E | ✅ |
| **AC4** | Major journeys covered by integration tests | All four journeys are covered. **Refinance:** 8 automated UI tests, **8/8 pass** (no-sign — §C), plus **25 transaction QC checks**, 24 of which hold. **View / open / repay:** covered end to end by the four executed sessions and **123 manual QC checks, 123 holding**, each check re-derived from the ledger or the counterparty's own API ([`06`](./06_UAT_Reports_Four_Journeys_M2.md)). Journey-specific *automated* results for view / open / repay are not reported yet — the automated suite covers the refinance surface | ✅ |

## C. What the automated suite does and does not prove

This is stated up front because the distinction matters and is easy to miss.

The eight tests (FN-I7, FN-I9 … FN-I14, FN-J10) run in the harness's **Tier 2, "connected,
no-sign"** mode:

- **Real, live data.** A real Cardano **mainnet** wallet is bridged into the page over CIP-30.
  `getBalance`, `getUtxos` and the address calls are served from real chain state via Blockfrost, so
  the app loads real positions and computes its quotes from them. It is not a mock wallet.
- **No signing, no broadcast.** `signTx` and `submitTx` are **stubbed** — `signTx` returns a fixed
  witness and `submitTx` returns a placeholder hash. Nothing is signed. Nothing reaches the chain.

So the suite evidences that **the refinance surface renders correctly from live data, that the
figures it quotes are computed correctly, and that the app reaches the point of asking for a
signature**. It does **not** evidence that a transaction signs, submits and settles.

That last part is evidenced by something stronger: the **15 mainnet transactions**, each signed by
hand through Eternl and each independently verifiable by anyone, with no test harness in the path —
[`00` §A](./00_POA_SUBMISSION_FORM.md#a-feature-integration).

## D. Evidence of Completion

| # | Evidence item | Artifact | Status |
|---|---|---|---|
| 1 | Published integration test report (successful user journeys) | current results: [`04` §1.5](./04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results) · the four sessions check by check: [`06`](./06_UAT_Reports_Four_Journeys_M2.md) · previous submission's report kept as an archive: [`01`](./01_Integration_Test_Report_M2.md) | ✅ |
| 2 | Demo video of workflows | four unedited session recordings, [`videos/`](./videos/) | ✅ |
| 3 | Links to deployments showing correct contract interactions | [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md) and, with the full value flow, [`05`](./05_ONCHAIN_TRANSACTIONS_AND_REPAY.md) | ✅ |

## E. What is open, stated here rather than left to be found

| | Where |
|---|---|
| Is the interface intuitive to an independent user — **not evidenced**, and not claimed anywhere in this package. The previous submission's self-assessment is withdrawn | [`08` C-2](./08_CORRECTIONS.md) |
| The *Deposit* / *Fee* preview lines at open reconcile to nothing on chain — root cause not established | [`06` OI-1](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session) |
| The Fluid borrower NFT is left in the wallet after the position is gone — intended or not, unconfirmed | [`06` OI-2](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session) |
| Journey 3's submit failure (D-1) — root cause not established | [`06` §3.3](./06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) |
| TC-24 does not hold — our own citation error, not a product defect | [`08` C-1](./08_CORRECTIONS.md) |
| Only one wallet brand (Eternl) is exercised | AC1 above |
| No journey-specific automated tests for view / open / repay | AC4 above |
