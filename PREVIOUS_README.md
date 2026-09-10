> ### ⚠️ Superseded — this file is from the **previous** Milestone 2 submission
>
> It is kept at its original path so that links from the previous Proof of Achievement still
> resolve. It has **not** been edited, so anything it says that we later found to be wrong is
> still wrong here — deliberately.
>
> **Start at [`README.md`](./README.md)** for the resubmission.
> **[`08_CORRECTIONS.md`](./08_CORRECTIONS.md)** lists, with the on-chain arithmetic, every claim in
> this file that we have since corrected or withdrawn.

---

# Milestone 2 Submission — Feature Integration

We are submitting the Milestone 2 evidence package for Feature Integration of the Rolling Loan
mechanism. This milestone integrates the rolling-loan / refinancing feature end-to-end into the
Danogo interface, so a borrower can view, open, repay, and refinance loans directly from the
front-end with their own wallet.

Evidence repository:

https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence

Naming note: in this proposal the feature is called "Rolling Loan"; in the delivered product and UI
it is named "Refinance via Dano". The two names refer to the same capability. Source specification:
`BorrowModify.Fluid.md §7.15`.

This submission focuses on demonstrating the fully integrated feature through a published integration
test report, a step-by-step user journey with screenshots, publicly verifiable mainnet execution
transactions, and a demo video.

## A. Feature integration

The rolling-loan feature is integrated across the wallet, front-end, back-end, and smart-contract
layers. From the live app a user can view, open, repay, and refinance a loan — refinancing (rolling)
a loan from Fluid into a Dano Finance (Dano Float) loan, closing the source loan and originating the
new Dano loan atomically, with collateral carried across and no manual repay-then-reopen.

This is enabled by the Dano Borrow Aggregator, which integrates external lending protocols (Fluid,
Liqwid, Surf); rolling a Fluid loan into a Dano loan is the first cross-protocol case delivered.

Evidence:

- Integration test report: [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md)
- Staging application (live mainnet contracts): https://v3.danogo.io/

## B. Fully functional feature — successful mainnet execution

The refinance is demonstrated through successful, publicly verifiable mainnet execution
transactions. Mainnet is used because Fluid does not deploy its smart contracts on any Cardano
testnet, so the Fluid → Dano flow can only be executed on mainnet — where the transactions are
public, immutable, and independently verifiable on Cardanoscan.

Main rolling-loan execution transaction:

https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c

This transaction demonstrates the tested rolling-loan / refinancing flow:

- an existing loan UTxO is consumed from the Fluid smart contract (carrying the DJED collateral)
- a new loan UTxO is created in the Dano Float smart contract
- the old Fluid loan is settled atomically in the same transaction using the newly originated loan
- collateral continues into the new loan position
- the borrower does not need to manually repay, withdraw collateral, and open a new loan separately

A second transaction shows the same flow on a Dano pool with no origination fee (borrow equals the
exact Fluid debt): https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652

Both transactions are `valid_contract: true` (validators accepted them), with metadata label
"Dano Finance: Create Loan".

Evidence:

- Mainnet transaction analysis (inputs/outputs, before/after): [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md)
- Transaction screenshots: [`screenshots/03-eternl-inputs-outputs.png`](./screenshots/03-eternl-inputs-outputs.png), [`04-transaction-confirmed.png`](./screenshots/04-transaction-confirmed.png),
  [`05-cardanoscan.png`](./screenshots/05-cardanoscan.png), [`06-portfolio-after.png`](./screenshots/06-portfolio-after.png)

## C. Test case and test result

The test evidence has two layers: a manual end-to-end journey (real signed transaction) and an
automated UI suite, both against live mainnet data. The manual journey maps each step to its
expected result, actual on-chain evidence, and pass/fail status.

Key verified results:

- source Fluid loan UTxO is consumed; a new Dano Float loan UTxO is created
- collateral (DJED) is carried across into the new loan — not returned to the borrower
- the refinance completes in one transaction, `valid_contract: true`, with no separate repayment
- for the ADA fee pool: origination fee = max(0.1% of borrow, 2 ADA) = 2 ADA, and the loan is bumped
  by that fee (12 ADA debt → 14 ADA borrow), financed by the loan with no upfront capital
- for a no-fee pool: the new loan borrow equals the exact Fluid debt (no fee bump)
- automated integration tests for the refinance UI: 8/8 pass on live data
- after the refinance, the Fluid position is settled and a single Dano loan remains

Evidence:

- Integration test report (manual journey + detailed QC checks + automated results):
  [`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md)
- Reviewer checklist: [`03_Reviewer_Checklist_M2.md`](./03_Reviewer_Checklist_M2.md)
- Step-by-step screenshots: [`screenshots/01-loans-sheet.png`](./screenshots/01-loans-sheet.png) … [`06-portfolio-after.png`](./screenshots/06-portfolio-after.png)

## D. Clarification

This milestone builds on Milestone 1. Milestone 1 proved the rolling/refinancing mechanism at the
smart-contract level (between two Dano contracts). Milestone 2 applies it to the real cross-protocol
case (Fluid → Dano) and integrates it end-to-end into the live product.

The evidence does not rely only on written claims: the delivered behavior is verified through public
blockchain data — the mainnet transactions show the source Fluid loan consumed, the Dano loan
created, and the collateral carried across, all in a single atomic transaction.

Demo video:

https://youtu.be/z07TxLJLC2w

## Summary

This submission demonstrates the fully integrated Rolling Loan (Refinance via Dano) feature through:

- an integrated front-end with view / open / repay / refinance loan actions
- completed back-end API integration building the on-chain transactions
- a manual end-to-end user journey with step-by-step screenshots
- an automated integration test suite (8/8 refinance cases passing on live data)
- two publicly verifiable mainnet execution transactions (fee pool and no-fee pool)
- source Fluid loan input UTxO consumed, target Dano loan output UTxO created, collateral carried
- a reviewer checklist and a demo video

The core functionality demonstrated is an active Fluid loan state being consumed and a new Dano
Finance loan state being created in a single atomic refinancing transaction, integrated end-to-end
and executed successfully on Cardano mainnet.
