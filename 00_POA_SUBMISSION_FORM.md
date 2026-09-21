# Proof of Achievement — Milestone 2 (Resubmission)

**Project** 1400107 — Rolling Loan
**Milestone** 2 — Feature Integration
**Submission** Resubmission following the *Not Approved* review
**Milestone page** https://milestones.projectcatalyst.io/projects/1400107/milestones/2

---

We are resubmitting the Milestone 2 evidence package for Feature Integration of the Rolling Loan
mechanism. This milestone integrates the rolling-loan / refinancing feature end-to-end into the
Danogo interface, so a borrower can view, open, repay, and refinance loans directly from the
front-end with their own wallet.

Evidence repository:

https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence

Naming note: in this proposal the feature is called "Rolling Loan"; in the delivered product and UI
it is named "Refinance via Dano". The two names refer to the same capability. Source specification:
`BorrowModify.Fluid.md §7.15`.

## What is new in this resubmission

**1. Each of the four journeys now has its own evidence and its own test result.** The previous
submission offered the refinance as evidence for open and repay as well — they were "covered
implicitly" by it. **The mechanism it described was right and has not changed:** a refinance is still
**one transaction, signed once**, which settles the Fluid debt and opens the Dano loan atomically,
with no separate repayment (section B). (Journey 3 is the one session where the first submit failed
and the tester signed again — for the same single transaction, which is the only one on chain; that
defect is published as D-1, with its cause traced to the wallet's stale collateral selection.) What was wrong was treating that as *evidence* for
the other two journeys — a settlement leg inside a refinance does not show a user opening a loan, or
repaying one, from the interface. Each of the four now carries its own evidence and its own
reported result (sections A and C): **open, repay
and refinance each have their own mainnet transactions**, and **view** — which is not a transaction
by nature — is carried by the live-app capture on both signing wallets, with every displayed figure
re-derived from the ledger. What links one journey to another is a token rather than an inference:
each Fluid loan's **position NFT held by the loan script** (policy `30f1095a…`) has exactly one mint
and one burn in its whole on-chain history — the mint is the Open transaction, the burn is the
Refinance or Repay that closed it. Open also mints two sibling tokens under other Fluid policies,
sharing the same asset name; those are not burned, and the pairing is not claimed for them
([`05` §2.2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#22-the-three-fluid-tokens-minted-at-open-and-which-one-the-pairing-uses)).

**2. The usability self-assessment is withdrawn, and replaced with evidence a third party can
check.** The row *"Tester completed the flow without external guidance — ✅ Yes"* in
`01_Integration_Test_Report_M2.md` §4 was our own opinion offered as evidence, and it is withdrawn.
What replaces it is checkable: **15 transactions signed from the front end and settled on Cardano
mainnet** (7 open, 5 refinance, 3 repay) from two wallets over eleven days; **four complete sessions
captured screen by screen**, on **three different wallet accounts — all Eternl**, two of them
separate installations (the versions recorded are v2.1.7.1 and v2.1.5.0; journey 4's was not
recorded), three of the sessions running all four journeys
on a single loan, with screen recordings and every wallet signing dialog (section C); and the
counterparty protocol's own repayment records (section B). **The third session was run by a second person**, on
their own Eternl account and their own Eternl installation, on a different version of the wallet.
They completed **all four journeys — open, view, refinance and repay — in one continuous 7 min 29 s
recording**, checking the result for themselves in their own wallet **and** on a public explorer
([`06` §3.2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#33-coverage-stability-and-findings)), and they hit something the delivery team had not: the refinance **failed to submit on the first
attempt** and needed a *Retry*. The session is published with that defect in it rather than around it
([`06` Journey 3](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) D-1).

What that session does **not** settle is usability. Their name, their relation to the delivery team,
the instructions they were given and the questions they asked during the session were not recorded,
so no conclusion about how intuitive the interface is can be drawn from it — and none is drawn,
here or anywhere else in this package. The criterion is scored on that basis, openly, in
[`07` §B.1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#b1-ac3-clause-by-clause).

A fifth session, three days later, was run through a **different wallet application — Vespr, not
Eternl** — and settled two transactions on preprod inside one 2 min 26 s recording: an open,
[`6f68ac98…eb38`](https://preprod.cardanoscan.io/transaction/6f68ac98039b84e48f65e3d0921b53e513c3ddeeb3f4f8d479f7c2f52976eb38),
and the refinance that closed it,
[`b2937327…7bf4`](https://preprod.cardanoscan.io/transaction/b2937327cc0661395029834652ad9f076eed677248f95f51fcdb9f76a2ab7bf4)
— same value flow as the Eternl sessions, **13 Plutus script executions, all valid**
([`06` Journey 5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr)).
It covers *open* and *refinance* only, with no screenshot set and no QC checks of its own, and is
offered as exactly that: evidence the integration is not Eternl-specific.

The fourth session, on a third Eternl account, was run large enough to force the **multi-pool** case — one
borrow that fills two Fluid pools and opens two loans, both then refinanced
([`06` Journey 4](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-4--one-borrow-two-pools-two-loans)).

**3. We disclose six errors we found in our own previous evidence** (section D), including the
transaction the previous submission cited for the no-fee case.

**Finding the feature.** *Refinance via Dano* is behind a rollout flag. On the **mainnet app**
(https://v3.danogo.io/) it is **off by default** — open **https://v3.danogo.io/?ff=fluid-refinance**
once and it stays on for that browser tab; on the **preprod app** (https://preprod.danogo.io) it
ships on. Both deployments expose the flag to the page at load time, so a reviewer can read it back
for themselves in the browser console — the exact call is in `07`. Only
the refinance entry point is gated — view, open and repay are not. Details:
[`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#where-the-app-is-and-how-to-reach-the-feature).

**Environments.** All settlement evidence is **Cardano mainnet**, 18–28 August 2026. The four
screen-by-screen walkthroughs in section C were run on **preprod** (https://preprod.danogo.io) on
18 September 2026 against Fluid's preprod contracts; they are interface evidence, not settlement
evidence. Every on-chain figure below was re-derived from the **public ledger** — the
**Koios** API for the mainnet transactions and for journeys 3 and 4, the public **Cardanoscan**
explorer for journeys 1 and 2 — and every repayment status from **Fluid's** own public API. None of
it comes from our backend.

## A. Feature integration

The rolling-loan feature is integrated across the wallet, front-end, back-end, and smart-contract
layers. From the **mainnet app** (staging deployment, live mainnet contracts, https://v3.danogo.io/) a
user can view, open, repay, and refinance a loan — refinancing (rolling)
a loan from Fluid into a Dano Finance (Dano Float) loan, closing the source loan and originating the
new Dano loan atomically, with collateral carried across and no manual repay-then-reopen.

This is enabled by the Dano Borrow Aggregator, which integrates external lending protocols (Fluid,
Liqwid, Surf); rolling a Fluid loan into a Dano loan is the first cross-protocol case delivered.

All transactions below were built by this application (each carries Danogo's metadata under label
674) and every Plutus script execution in every one of them returned `valid_contract = true`.

**VIEW** — no transaction by nature; the user reads their position. *My Account → Loans* was
captured from the mainnet app on both signing wallets, and every displayed borrowed amount re-derived
from the ledger (on-chain principal plus interest at the APR the same row displays). The number of
loans the app lists equals the number of Danogo Borrower NFTs each wallet holds on chain — 2 and 4.

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
burned again by the Refinance or Repay that closes it.

**REPAY** — 3 mainnet transactions in which the borrower settles a debt from their own funds, plus
the settlement leg inside each of the five refinances.

| Tx | Metadata 674 | What it does |
|---|---|---|
| [`17c23dde…559a`](https://cardanoscan.io/transaction/17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a) | *Repay Fluid Loan* | pays **USDCx 5.000011** from the borrower's own wallet, releases **ADA 50.000000** of collateral, burns the position NFT, creates no Dano loan. Closes the loan opened by `247e1218…` |
| [`ea823365…e04d`](https://cardanoscan.io/transaction/ea823365562e1eefec0cb3be614a54963ec128dcb3ef8430099d308902bfe04d) | *Repay Fluid Loan* | settlement output **2.011600 ₳**, releases **USDM 16.000000** of collateral. The loan it closes was opened in July 2026, before this milestone's window, so it appears here as repay evidence only and has no row in the Open table |
| [`77748bd9…22bc`](https://cardanoscan.io/transaction/77748bd9673991565c25671522fe70914e29098d7bd0f4c164cf4677585522bc) | **_Dano Finance: Repay Loan_** | Danogo's own repayment path on Danogo's own contracts: the borrower pays **USDM 6.096399**, split exactly between the pool (4.396378) and the fee address (1.700021), and the **Borrower NFT is burned**. Counterpart to `ee87712a…` three hours earlier — a complete open-and-repay cycle with no external protocol in the path |

**REFINANCE** — 5 mainnet transactions, each closing a Fluid loan and opening a Dano loan atomically.

| Tx | Date | Collateral carried | Borrowed | Liquidity source | Origination fee |
|---|---|---|---|---|---|
| [`88579a30…6652`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 08-19 | USDM 10.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`d240fab1…d84c`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 08-24 | SNEK 20,979 | ADA | Staking (fixed-term) | 0.969750 ₳ |
| [`1cf8f08b…4f10`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 08-25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`c426d9fa…25c8c`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 08-25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| [`0e26cc58…05b8`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 08-27 | DJED 10.000000 | **STRIKE** | Flexible Pool | **none** |

Two wallets, seven distinct days, four collateral assets, three borrowed assets, two Dano liquidity
sources, three origination-fee configurations.

Evidence:

- Integration test report: [`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md)
- Per-journey detail and resulting app state: [`04_USER_JOURNEYS_AND_APP_STATE.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md)
- Transaction ledger and value flow: [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md)
- **Mainnet app** (staging deployment, live mainnet contracts): https://v3.danogo.io/ — see *How to reach the feature* in [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#where-the-app-is-and-how-to-reach-the-feature) before looking for the button

## B. Fully functional feature — successful mainnet execution

The refinance is demonstrated through successful, publicly verifiable mainnet execution
transactions. Mainnet is where the Fluid → Dano path exists as a real market — Fluid's pools that
hold real liquidity and the collateral assets borrowers actually post are on mainnet — and it is
also the stronger evidence, because the transactions are public, immutable and independently
verifiable on Cardanoscan, on a ledger neither protocol controls.

Main rolling-loan execution transaction:

https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c

This transaction demonstrates the tested rolling-loan / refinancing flow:

- an existing loan UTxO is consumed from the Fluid smart contract (carrying the DJED collateral)
- a new loan UTxO is created in the Dano Float smart contract
- the old Fluid loan is settled atomically in the same transaction using the newly originated loan
- collateral continues into the new loan position
- the borrower does not need to manually repay, withdraw collateral, and open a new loan separately

A second transaction shows the same flow on a Dano pool with **no origination fee**, and with a
non-ADA borrowed asset — the pool disburses STRIKE 5.000560, the settlement receives STRIKE 5.000558,
and the transaction contains no leg to the Dano fee address at all:

https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8

*(The previous submission cited `88579a30…` for the no-fee case. That was incorrect — see section D,
correction C-1.)*

All five refinances are `valid_contract: true` (validators accepted them), with metadata label
"Dano Finance: Create Loan", and in each one the source Fluid position NFT is burned.

**The protocol that was owed the money confirms the repayments.** Fluid Tokens publishes each
borrower's loan history from its own indexer, with no credentials required:

```
GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>
```

Queried on the two borrower wallets it returns **seven** loan events — every one
`"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0`. Its
`loanUtxoId` field names the transaction that opened each loan and `finishingTxHash` names the
transaction this application built to close it, so Fluid's records and the Cardano ledger produce
the open-to-close pairing in section A independently of each other, and they agree. The token Fluid
names in its `nft` field is byte-for-byte the token our transaction burned, and every settlement
pays at least the total Fluid says was due.

Evidence:

- Mainnet transaction analysis (inputs/outputs, before/after): [`05_ONCHAIN_TRANSACTIONS_AND_REPAY.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md)
  and [`02_Mainnet_Transactions_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_Mainnet_Transactions_M2.md)
- Transaction screenshots: [`screenshots/03-eternl-inputs-outputs.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/03-eternl-inputs-outputs.png), [`04-transaction-confirmed.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/04-transaction-confirmed.png),
  [`05-cardanoscan.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/05-cardanoscan.png), [`06-portfolio-after.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/06-portfolio-after.png)
- Fluid's own borrower dashboard, the loans marked `REPAID`: [`screenshots/fluid-dashboard/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/fluid-dashboard)

## C. Test case and test result

The evidence has two layers. **Manual end-to-end journeys** that produced real signed transactions:
fifteen on **Cardano mainnet** (the settlement evidence), and four screen-by-screen walkthroughs on
**preprod** (the interface evidence) — the split is set out under *Environments* above. And an
**automated UI suite** over the Loan Details screen and the refinance surface, run against live
**mainnet** data — but in **no-sign** mode: the wallet's reads are real, `signTx` and `submitTx` are stubbed, so the suite shows
the surface and its arithmetic are right and never shows a transaction settling. What shows that is
the fifteen mainnet transactions in section A. The distinction is set out in
[`07` §C](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#c-what-the-automated-suite-does-and-does-not-prove).

Key verified results: the source Fluid loan UTxO is consumed and a new Dano Float loan UTxO created,
in **one** transaction with `valid_contract: true` and no separate repayment; the collateral is
carried across, never returned to the borrower; **on the Dano Flexible Pool** the origination fee is
max(0.1% of borrow, 2 ADA), financed by the loan with no upfront capital from the borrower — and zero
on a no-fee pool (`0e26cc58…`, which has no fee leg at all), while the one refinance drawing on the
fixed-term **staking** contract (`d240fab1…`) charges a different fee, 0.969750 ₳, which the borrower
pays out of pocket rather than capitalising (correction C-4); afterwards the Fluid position is settled
and a single Dano loan remains.

**Results per journey**, as the review asked. The columns keep two different things apart: what the
executed workflow produced and a third party can observe, and what an automated test asserts. The
automated suite now reaches all four journeys, and not one of the four is clean; every failure is
grouped by cause in [`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) C-6 rather than netted off.

| Journey | Executed workflow / observable result | Automated-test result | Evidence |
|---|---|---|---|
| **View** | Every figure in *My Account → Loans* re-derived from the ledger; the loan count equals the Borrower NFTs the wallet holds on chain (2 and 4) | **All 80 in-scope automated UI tests pass** against the live mainnet app — Loan Details overview, collateral list, health factor, APR, utilisation and display formats | [`04` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) |
| **Open** | 7 signed Open transactions on mainnet, each `valid_contract = true`; captured screen by screen in all four preprod walkthroughs, including the **multi-pool** case where one borrow opens two loans | **70 automated tests run, 15 fail** — the `[Create loan]` suite; most failures are the suite declaring its own Tier-2 limits ([C-6](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md)) | [`05` §1.1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) · [`06` §4.2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#42-verification-qc) |
| **Repay** | 3 signed mainnet repayments from the borrower's own funds, plus the settlement leg inside each of the 5 refinances; Fluid's indexer reports all 7 of its loans `loan_repaid`, `remainingDebt: 0`. Captured screen by screen on preprod **three times** — quote → wallet dialog → settled transaction — each closing the loan that walkthrough had just opened | **50 automated tests run, 12 fail** — the `[Repay]` suite; two failures are the same defect as OI-1, found independently ([C-6](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md)) | [`05` §5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) |
| **Refinance** | 5 signed mainnet refinances; **25 transaction QC checks** (TC-01 … TC-25), **24 of which hold**; 5 further refinances captured screen by screen on preprod | **All 7 in-scope automated UI tests pass** on mainnet with the feature flag on — the CTA renders, its label reads `Save 91.98% net cost`, the dearer branch reads `+1.30% net cost`, it sits last in the footer, and it opens the preview; plus the control that a Dano loan offers no refinance. **The previous submission's “8/8” is still withdrawn** — two of those eight had never reached their own assertion — correction [C-6](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md). **No-sign**: real wallet reads, `signTx`/`submitTx` stubbed | [`07` §B.2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#b2-ac4-what-is-automated-and-what-is-not) |

TC-24 is the one check that does not hold. It asserts that the transaction we cited as the zero-fee
example carries no fee leg; the transaction we cited was the wrong one (correction C-1). It is an
error in our own evidence, not a defect in the product.

### Four complete journeys, captured screen by screen

To show the click path itself rather than our description of it, four sessions were walked
end-to-end on preprod (https://preprod.danogo.io) on 18 September 2026, on **three different Eternl
accounts**, two of them separate installations — v2.1.7.1 for journeys 1 and 2, v2.1.5.0 for journey
3; the Eternl version used for journey 4 was not recorded. Each is captured in
full — every screen from the first click to the settled transaction, a
screen recording of the whole session, the wallet signing dialog for each signature, and the
explorer record of each transaction — then checked line by line against the chain:
**[`06_UAT_Reports_Four_Journeys_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md)**, **123 QC checks, 123 hold**.

| | [Journey 1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-1--borrow-25-ada-against-100-fusdm) | [Journey 2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-2--borrow-11-fusdm-against-100-ada) | [Journey 3](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-3--a-second-tester-on-a-second-wallet) | [Journey 4](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-4--one-borrow-two-pools-two-loans) |
|---|---|---|---|---|
| Wallet | team | the same | **a second tester, second wallet** | a third wallet |
| Borrowed / collateral | 25 ADA / 100 fUSDM | 11 fUSDM / 100 ADA | 11 fUSDM / 100 ADA | **905.004 ADA / 98,327.69 fUSDM → two loans** |
| Journeys | open · view · refinance · repay | the same, reversed direction | the same | open · view · refinance **×2**, no repay |
| Quoted **before** signing | fee 2 ADA · HF 7.27 → **1.21** · net cost 4.00% → 0.68% | fee 2 fUSDM · HF 3.63 → **1.87** · net cost 4.00% → −5.86% | fee 2 fUSDM · HF 3.63 → **1.87** · net cost 4.00% → −5.84% | fee 2 ADA on each · HF 197 → 32.3 / 35.5 · net cost 4.00% → 0.68% |
| The loan **after** signing | **27 ADA** · HF **1.21** · APR 3.21% · collateral 100 fUSDM | **13 fUSDM** · HF **1.87** · APR 3.07% · collateral 100 ADA | **13 fUSDM** · HF **1.87** · APR 3.10% · collateral 100 ADA | two Dano loans, collateral 2,172.978020 + 96,154.711980 fUSDM |
| Refinance | [`0bfa25db…`](https://preprod.cardanoscan.io/transaction/0bfa25db49a676c15645c25d1d8b35acf5630d1d9cb1d472d386430cb954ccfd) | [`78d434d5…`](https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6) | [`a04fe52e…`](https://preprod.cardanoscan.io/transaction/a04fe52e0545f546b71a866ed48b1a83aac271dbece74d21fb27e835d268d8f4) | [`6ab3bde0…`](https://preprod.cardanoscan.io/transaction/6ab3bde0739c1a53aacbbd6c43aacd848b3311a8958030be412c22ffe1c7ad43) · [`3cbb01a7…`](https://preprod.cardanoscan.io/transaction/3cbb01a7b9dce89f175a75174dff109d48c231f42b2059fc8950d2485f9694a8) |
| Repay, same interface | ✅ [`e134b85e…`](https://preprod.cardanoscan.io/transaction/e134b85eb5631391089598adefd8f06024307fc7d41ec4ac8d421276d8681416) — quoted 27.000474 ADA, paid 27.000473, **100 fUSDM released** | ✅ [`bae9a9c9…`](https://preprod.cardanoscan.io/transaction/bae9a9c9e0e4f63657d43cc21b4ab071e82d57919733ddb8908a958937c75b7d) — quoted 13.000218 fUSDM, paid 13.000218, **100 ADA released** | ✅ [`a4bebdb7…`](https://preprod.cardanoscan.io/transaction/a4bebdb7ca185ffc1ce4fc22873e9e3d9cefa80e0d1e0d8ad698e6b1813fbeed) — **100 ADA released**, Borrower NFT burned | ❌ not exercised |
| QC checks | 31 / 31 | 33 / 33 | 22 / 22 | 37 / 37 |

In every refinance the fee the interface quoted is the fee paid — **2.000000 to the fee address, five
times out of five** — the health factor quoted is the one the new loan carries, the resulting debt is
debt + fee, and the pool disbursed exactly settlement + fee. The collateral is the same collateral,
moved from the Fluid script to the Dano loan contract inside one transaction, never returned to the
borrower in between.

**Two sessions add something the others cannot.** *Journey 3* was run by **a second tester, on a
different wallet and a different wallet installation** — and **it failed once before it succeeded**:
after the first refinance signature the app showed *"Submit failed: Your wallet may not have finished
syncing…"*; the tester pressed **Retry**, signed again, and the refinance settled. Exactly one
refinance exists on chain. The failure, the retry and the recovery are all in the recording and are
reported as a defect rather than edited out. *Journey 4* was run large enough that one pool cannot
fill it: the Borrow Market warns *"This rate may fill several pools and open more than one loan"*,
and one signature then drains two Fluid pool UTxOs — **885.004000 ₳ and 20.000000 ₳, exactly the
905.004 offered as *Max*** — opening **two** loans, both later refinanced one at a time. Between the
two, the portfolio shows the split the chain shows, *Fluid · Supply* **$96,154.71** and
*Dano · Supply* **$2,172.98**, so refinancing one loan demonstrably leaves the other untouched.

All four record, as open items rather than passes, the two things this package has not reconciled:
the *Deposit* and *Fee* lines in the Borrow Market preview at open (**OI-1**) and which token the
refinance burns (**OI-2**) — [`06` Open items](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session).

These are still our own sessions. They establish the complete, uninterrupted click path of each
journey and the agreement between what the interface promised and what the chain recorded; they are
not evidence that an independent user finds the interface intuitive, and we do not present them as
that. The settlement evidence for the milestone remains the mainnet transactions in section A, which
anyone can verify without us.

Evidence:

- The four sessions in full, journey by journey and check by check: [`06_UAT_Reports_Four_Journeys_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md)
- Current integration-test results in one table: [`04` §1.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#15-current-integration-test-results)
- The report they come from, kept unedited as an archive: [`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md)
- Acceptance-criteria mapping for this resubmission: [`07_ACCEPTANCE_CRITERIA_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md) — the previous submission's checklist, [`03_Reviewer_Checklist_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/03_Reviewer_Checklist_M2.md), is kept unedited as an archive and several of its ✅ rest on claims since withdrawn
- Step-by-step screenshots: [`screenshots/01-loans-sheet.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/01-loans-sheet.png) … [`06-portfolio-after.png`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/06-portfolio-after.png)

## D. Clarification

This milestone builds on Milestone 1. Milestone 1 proved the rolling/refinancing mechanism at the
smart-contract level (between two Dano contracts). Milestone 2 applies it to the real cross-protocol
case (Fluid → Dano) and integrates it end-to-end into the product itself — the **mainnet app**
(staging deployment, live mainnet contracts) at https://v3.danogo.io/, and the **preprod app** at
https://preprod.danogo.io.

The evidence does not rely only on written claims: the delivered behavior is verified through public
blockchain data — the mainnet transactions show the source Fluid loan consumed, the Dano loan
created, and the collateral carried across, all in a single atomic transaction. All five refinanced
Fluid position NFTs now have a total supply of zero across Cardano — the repaid loans do not exist
anywhere on chain, which is why neither borrower's screen lists a Fluid loan.

**Demo video.** Four unedited screen recordings, one per journey — continuous takes, nothing cut,
each starting at the first click. Not a developer talking over the feature, but the interface being
operated. What each take does and does not reach is stated in its row:

| | Session | Length |
|---|---|---|
| 1 | [Open → View → Refinance](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/01-open-and-refinance-borrow-ADA-collateral-USDM.mp4) — borrow 25 ADA against 100 fUSDM (this session's *Repay* is captured as screenshots, not on the recording) | 5 min 21 s |
| 2 | [Open → View → Refinance](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/02-open-and-refinance-borrow-USDM-collateral-ADA.mp4) — the opposite direction, borrow 11 fUSDM against 100 ADA (*Repay* likewise in screenshots) | 3 min 33 s |
| 3 | [Open → View → Refinance → **Repay**](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/03-open-refinance-repay-borrow-USDM-collateral-ADA.mp4) — all four journeys in one unbroken take, run by a second tester on a second wallet and a different Eternl version | 7 min 29 s |
| 4 | [Open → View → Refinance ×2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/04-open-and-two-refinances-borrow-ADA-collateral-USDM.mp4) — one borrow of 905 ADA filling two pools and opening two loans | 3 min 50 s |

Every frame is cross-referenced step by step, with its transaction hash, in
[`06_UAT_Reports_Four_Journeys_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md),
so any figure visible on screen can be checked against the chain.

> The video submitted the first time, https://youtu.be/z07TxLJLC2w, **is withdrawn as evidence.** It
> was a developer walking through the feature — the first thing the reviewer rejected — and the four
> recordings above stand in its place. The link is kept here only as a record of what was submitted
> ([`07` §A](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#a-milestone-outputs)).

**Corrections to our own previous submission.** While re-deriving every figure from a public API for
this resubmission we found errors in our own evidence and disclose them unprompted. **No on-chain
figure in this package changes.** Full arithmetic in [`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md).

- **C-1 — the wrong transaction was cited as the zero-fee example.** On `88579a30…` the Dano
  Flexible Pool disburses 22.000857 ₳, of which 20.000851 ₳ settles the Fluid debt and exactly
  2.000000 ₳ goes to the fee address: the pool does charge a fee, capitalised into the new loan. The
  underlying claim is right, the transaction cited for it was wrong. The zero-fee example is
  `0e26cc58…`, which has no fee leg at all.
- **C-2 — the usability self-assessment is withdrawn** and reported as the sessions it recorded, not
  as a verdict. See *What is new*, point 2.
- **C-3 — a recurring marker token was described as a per-loan NFT.** Danogo mints two tokens per
  loan; the one that stays in the loan contract is a recurring marker (57 mints, 49 burns, the same
  token in three of the five refinances). The per-loan identity is the Borrower NFT paid to the
  borrower's wallet. Every "a distinct loan was created" claim is now made against the Borrower NFT.
- **C-4 — "the borrower contributes only the network fee" was over-stated.** It holds for the four
  Flexible Pool refinances. On `d240fab1…`, drawing on the fixed-term staking contract, the
  disbursement (15.015024 ₳) is smaller than settlement plus fee (15.969752 ₳) and the borrower funds
  the 0.954728 ₳ difference. That transaction still settles the debt in full and carries the
  collateral across; it simply does not demonstrate the "no capital needed" property.
- **C-5 — "Fluid has no Cardano testnet deployment" was stated too absolutely.** The walkthroughs in
  section C ran against Fluid's preprod contracts, so the absolute form of the claim is withdrawn.
  What holds is the reason the *settlement* evidence is on mainnet: that is where the Fluid → Dano
  path exists as a real market, and where a third party can verify it without us.
- **C-6 — "the 8 automated refinance tests pass 8 / 8" is withdrawn.** We re-ran them on
  21 September 2026: **0 of 8** pass on mainnet as the deployment ships, because the feature sits
  behind a flag that is off there; with the flag on, 5 of 8. Worse, all eight carry a `KNOWN-FAIL (finding, app-vs-spec)` annotation
  in their own source — they were written to record a divergence from the screen spec, not to pass,
  and we repeated the figure without reading them. What replaces it is measured, on mainnet, with
  the flag on: **all 87 Loan Details tests in scope have passed**, across three runs rather than one;
  **70 tests ran** over **open** and **50** over **repay**. Unscoped the last full run was 83 of 98.
  Full account, including the eleven tests left out of scope and why, in
  [`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md).

## Summary

This submission demonstrates the fully integrated Rolling Loan (Refinance via Dano) feature,
journey by journey:

- **View** — the borrower's loans in *My Account*, every displayed figure re-derived from the ledger
- **Open** — 7 mainnet Open transactions, plus four walkthroughs capturing the workflow screen by
  screen from the first click to the settled loan, one of them the multi-pool case where a single
  borrow opens two loans
- **Repay** — 3 mainnet Repay transactions as a direct user action, one of them on Danogo's own
  `Repay Loan` path, plus the settlement leg inside every refinance; the counterparty protocol's own
  records report seven loans repaid with nothing owing
- **Refinance** — 5 mainnet refinances, each consuming the Fluid loan UTxO, creating the Dano loan
  UTxO, carrying the collateral across and burning the source position NFT in one transaction;
  25 QC checks, 24 of which hold

Behind those four lines are **four complete sessions**, captured screen by screen on three Eternl
accounts, and **123 QC checks of which 123 hold** — every one re-derived from the public ledger or
from the counterparty protocol's own API
([`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md)).
The criterion-by-criterion mapping is
[`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md).

All 15 transactions were built by this application (metadata label 674) and every Plutus script
execution in them returned `valid_contract = true`, on a public ledger the reviewer can query
directly.

The core functionality demonstrated is an active Fluid loan state being consumed and a new Dano
Finance loan state being created in a single atomic refinancing transaction, integrated end-to-end
and executed successfully on Cardano mainnet.
