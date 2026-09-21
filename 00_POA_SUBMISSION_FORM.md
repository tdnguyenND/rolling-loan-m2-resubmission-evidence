# Proof of Achievement — Milestone 2 (Resubmission)

**Project** 1400107 — Rolling Loan
**Milestone** 2 — Feature Integration
**Submission** Resubmission following the *Not Approved* review
**Milestone page** https://milestones.projectcatalyst.io/projects/1400107/milestones/2
**Evidence repository** https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence

---

> **How to use this file.** It is organised as Output → Acceptance criteria → Evidence, the shape the
> Catalyst Proof of Achievement form requires. Every claim links to a public artifact: the Cardano
> ledger, the counterparty protocol's own API, or a file in the repository above. Independent
> usability testing is **not** claimed — see *What this submission does and does not contain*.
>
> **Naming.** "Rolling Loan" is the proposal's name for the feature the product ships as
> **Refinance via Dano**. The two names mean the same capability
> (`docs/screens/LendBorrow/BorrowModify.Fluid.md` §7.15).
>
> **Before looking for the button.** On the **mainnet app** (https://v3.danogo.io/) the refinance
> entry point sits behind a rollout flag and is **off by default** — open
> **https://v3.danogo.io/?ff=fluid-refinance** once and it persists for that browser tab. On the
> **preprod app** (https://preprod.danogo.io) it ships on. *View, open and repay are not gated.*
> How to read the flag back in the browser console:
> [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/07_ACCEPTANCE_CRITERIA_M2.md#where-the-app-is-and-how-to-reach-the-feature).

---

## Opening statement

We accept the previous review in full. Both objections described real gaps.

1. **Each journey is now evidenced on its own**, not inferred from another. Open, repay and
   refinance each have their own mainnet transactions — **15 in total** (7 open, 5 refinance,
   3 repay), from two wallets over eleven days. View, which is not a transaction by nature, is
   carried by the app's own screens reconciled to the ledger.
2. **The independent-usability claim is withdrawn**, and nothing is substituted for it. What replaces
   it is checkable by a third party: five recorded end-to-end sessions and the public ledger.
3. **Every on-chain figure is re-derived from public sources** — the **Koios** API and the
   **Cardanoscan** explorer for the ledger, **Fluid Tokens' own API** for the repayment status. None
   comes from our backend.
4. **Six errors in our own previous evidence are disclosed unprompted** (Output 7). No on-chain
   figure changes as a result.

The previous submission's files are kept unedited in the same repository, each pointing at the
corrections log. We did not quietly fix them.

**Environments.** All settlement evidence is **Cardano mainnet**, 18–28 August 2026. The
screen-by-screen walkthroughs were run on **preprod** on 18 and 21 September 2026 against Fluid's
preprod contracts; they are interface evidence, not settlement evidence.

---

## At a glance

| Journey | Result | Main evidence |
|---|---|---|
| **View** | Both borrower wallets' loan screens captured from the live app; every displayed borrowed amount re-derived from the ledger, and the loan count equals the Borrower NFTs each wallet holds on chain (2 and 4) | [`04` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) |
| **Open** | **7 mainnet transactions** — 6 opening a Fluid loan through the Danogo interface, 1 opening a Dano loan directly | [`05` §1.1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans) |
| **Repay** | **3 mainnet repayments as a direct user action** from the borrower's own funds, plus the settlement of the Fluid debt **inside each of the 5 refinance transactions**. Fluid's own indexer reports **7** loans `repaid`, `remainingDebt: 0` | [`05` §5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-three-direct-repayments-and-repayment-within-a-refinance) |
| **Refinance** | **5 mainnet transactions**, each repaying the Fluid loan and opening the Dano loan **in one and the same transaction** | [`05` §1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) |

---

## Acceptance criteria, and the evidence offered for each

The criteria are quoted as written. Each row lists **only what the linked artifacts show**, so the
reviewer can check every entry against a public source rather than against our assessment of it.

| # | Criterion | Evidence offered | Where |
|---|---|---|---|
| **AC1** | View / open / repay / refinance via **Eternl** wallet | **15 mainnet transactions** signed through Eternl from two wallets — 7 open, 5 refinance, 3 repay — plus four preprod sessions on three Eternl accounts and two Eternl versions. *Open* and *refinance* were also signed in **Vespr** | Outputs 1–4 |
| **AC2** | Back-end performs correct contract interactions for all loan states | Every Plutus script execution in all 15 transactions returned `valid_contract = true`; inputs, outputs, mints and burns re-derived from the public **Koios** API | Outputs 2, 3, 5 |
| **AC3** | *"Integration must be stable, intuitive, and free from blocking UI/UX issues across supported wallets"* | **Stability**: no crash, no hang and no lost state in any of the five recorded sessions; the four that carry a QC checklist hold **123 of 123 checks**, each re-derived from the public ledger. **Wallets exercised**: Eternl on all four journeys, Vespr on *open* and *refinance* | Output 4 |
| **AC4** | Major journeys covered by integration tests | Each of the four journeys executed end to end and verified check by check against the public ledger: **123 session QC checks** and **25 transaction QC checks**, each published with the figure it asserts and the source it was re-derived from | Output 6 |

**Scope of this submission.** It claims only what the linked artifacts show. Where an artifact does
not support a claim, the claim is not made — including claims the previous submission made and we
have since withdrawn (Output 7).

---

## Terms used in this document

| Term | Meaning |
|---|---|
| **One transaction (1 TX)** | A single Cardano transaction hash, in a single block, signed once. If any part fails, none of it is applied. |
| **Refinance (rolling)** | In **one transaction**: the Fluid debt is repaid in full and the new Dano loan is opened, with the collateral moving straight from one protocol to the other. It is **not** a repay transaction followed by an open transaction. |
| **Fluid position NFT** | A Fluid loan's on-chain identity token, held by the Fluid loan script (policy `30f1095a…`). Fluid's validator permits its burn only on full settlement, so the burn is the repayment proof. |
| **Borrower NFT** | The token Danogo mints to the borrower's own wallet when a Dano loan opens — the per-loan identity. The app lists the loans whose Borrower NFT the connected wallet holds. |
| **Settlement leg** | The output inside the refinance transaction that pays off the Fluid debt, funded by the incoming Dano loan — not by the borrower. |
| **Origination fee** | The fee a Dano pool charges when a loan is created. On the Flexible Pool it is added to the new loan amount, so the borrower does not provide it separately. |

---

## Output 1 — Integrated front end: the four journeys, live on mainnet

**Output:** From the **mainnet app** (staging deployment, live mainnet contracts,
https://v3.danogo.io/) a wallet-connected borrower can **view**, **open**, **repay** and
**refinance** loans against live Cardano mainnet contracts. This is delivered by the Dano Borrow
Aggregator, which integrates external lending protocols; rolling a **Fluid** loan into a **Dano
Finance (Dano Float)** loan is the first cross-protocol case shipped. The app requests signatures
through the borrower's wallet over CIP-30 and does not custody private keys.

**Result: 15 mainnet transactions signed from the front end**, 18–28 August 2026, two wallets.

| Journey | Mainnet transactions | Metadata (label 674) |
|---|---|---|
| **Open** | **7** — table below | *Dano Finance: Borrow from Fluid* (6), *Dano Finance: Create Loan* (1) |
| **Repay** | **3** — Output 3 | *Dano Finance: Repay Fluid Loan* (2), *Dano Finance: Repay Loan* (1) |
| **Refinance** | **5** — Output 2 | *Dano Finance: Create Loan* |
| **View** | none by nature — Output 5 | — |

**The 7 Open transactions**, each opening a loan the borrower later refinanced or repaid:

| Tx | Date (UTC) | Collateral locked | Closed later by |
|---|---|---|---|
| [`917bdfe1…b289`](https://cardanoscan.io/transaction/917bdfe19763fac39a2a154a817f8297958e7f4fb12cf7047a7ee8661e61b289) | 2026‑08‑18 | USDM 10.000000 | refinance TX‑01 |
| [`247e1218…8e4e`](https://cardanoscan.io/transaction/247e121881a03f483556bc2339a1d6be9a52cd37299dc97c57f3a1dc46368e4e) | 2026‑08‑24 | ADA 50.000000 | repay `17c23dde…` |
| [`a3ed946c…bf6c`](https://cardanoscan.io/transaction/a3ed946c165fc9de0e48fe318a5764ca6149e779818b043c01740b7ba60dbf6c) | 2026‑08‑24 | SNEK 20,979 | refinance TX‑02 |
| [`7a6caf51…945d`](https://cardanoscan.io/transaction/7a6caf51f61f1a7ca635f74a1b8df629c48550e20a61d869798fa4b91d3e945d) | 2026‑08‑25 | DJED 6.000000 | refinance TX‑03 |
| [`4901277c…77d7`](https://cardanoscan.io/transaction/4901277c80a06dd3d891eaccb90fd91b0e07037809bd99dd1477c9c4fd6777d7) | 2026‑08‑25 | DJED 6.000000 | refinance TX‑04 |
| [`9b3aa00c…36ee`](https://cardanoscan.io/transaction/9b3aa00c0c13093fe2ef9489f3fa6b3877195af45d8f35935a0ea3a2340736ee) | 2026‑08‑26 | DJED 10.000000 | refinance TX‑05 |
| [`ee87712a…7bca`](https://cardanoscan.io/transaction/ee87712aef570de2e0ac8616935f30949f57d20942c2ddbd8d25a94bfce97bca) | 2026‑08‑28 | ADA 35.000000 — opens a **Dano** loan directly, no external protocol | repay `77748bd9…` |

The pairing is a fact about the ledger, not a claim in this document: each Fluid loan's position NFT
held by the loan script (policy `30f1095a…`) has exactly **one mint and one burn** in its entire
on-chain history — the mint is the Open transaction, the burn is the Refinance or Repay that closed
it. Fluid's own records produce the same pairing independently.

**Acceptance criteria:**
1. All four journeys are reachable and completable in the live application with a wallet connected.
2. Each journey is evidenced separately — not inferred from another journey.
3. Every transaction is built by this application and accepted by the live deployed validators.

**Evidence:**
- Open, transaction by transaction, with the transaction that later closed each loan:
  [`05` §1.1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans)
- Repay, in both of its forms: Output 3 below
- Refinance, the milestone's core deliverable: Output 2 below
- Per-journey user path and evidence class:
  [`04` §1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#1-the-four-approved-user-journeys-via-eternl)
- All 15 carry Danogo metadata under label 674, and **every Plutus script execution in every one of
  them returned `valid_contract = true`** —
  [`05` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions)

---

## Output 2 — The rolling refinance: repay Fluid and open the Dano loan in **one transaction**

**Output:** The refinance is **a single Cardano transaction, signed once, settled in one block**. In
that one transaction the Fluid debt is repaid in full and the new Dano loan is opened, with the
collateral carried across. There is **no separate repayment transaction and no separate open
transaction**, and no intermediate state in which the Fluid loan is repaid but the Dano loan failed
to open.

```
 ┌───────────────────── ONE transaction · one block · one signature ─────────────────────┐
 │  INPUT   Fluid loan UTxO   — carries the collateral and the Fluid position NFT        │
 │  INPUT   Dano pool UTxO    — supplies the new borrow                                  │
 │  OUTPUT  settlement leg    → Fluid debt paid in full            ◀── THE REPAYMENT     │
 │  OUTPUT  Dano loan UTxO    → new loan, same collateral          ◀── THE NEW LOAN      │
 │  MINT    Fluid position NFT −1  (the Fluid loan ceases to exist)                      │
 │  MINT    Borrower NFT      +1  (the borrower's title to the new Dano loan)            │
 └───────────────────────────────────────────────────────────────────────────────────────┘
```

It replaces three user actions and the exposure between them — find capital, repay and withdraw
collateral, re-deposit and borrow again. The borrower does none of that.

**Result: 5 mainnet refinance transactions**, five occasions, two signing wallets, three collateral
assets, two borrowed assets, two Dano liquidity sources, three origination-fee configurations.

| # | Tx | Date (UTC) | Collateral carried | Borrowed | Liquidity source | Origination fee |
|---|---|---|---|---|---|---|
| TX‑01 | [`88579a30…6652`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 2026‑08‑19 | USDM 10.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑02 | [`d240fab1…d84c`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 2026‑08‑24 | SNEK 20,979 | ADA | Staking (fixed-term) | 0.969750 ₳ |
| TX‑03 | [`1cf8f08b…4f10`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 2026‑08‑25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑04 | [`c426d9fa…25c8c`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 2026‑08‑25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑05 | [`0e26cc58…05b8`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 2026‑08‑27 | DJED 10.000000 | **STRIKE** | Flexible Pool | **none** |

**Acceptance criteria:**
1. **One transaction.** The repayment of the Fluid loan and the origination of the Dano loan occur in
   the same transaction hash and the same block, under one signature.
2. Every transaction is accepted by the live deployed validators of **both** protocols.
3. The source Fluid loan UTxO is consumed and its position NFT burned — full settlement.
4. Collateral is carried across exactly, never returned to the borrower in between.
5. On a Flexible Pool the borrower provides no repayment funds beyond the network fee and the
   minimum-ADA amounts Cardano requires each output to hold.

**Evidence:**
- Why the settlement happens inside the same transaction, with the input/output shape:
  [`05` §5.1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#51-why-the-refinance-settles-the-source-debt-in-the-same-transaction)
- Per-transaction value flow, every figure sourced to a public API:
  [`05` §3](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow)
  · summary: [`02_Mainnet_Transactions_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_Mainnet_Transactions_M2.md)
- Cardanoscan screenshot of each of the five:
  [`screenshots/cardanoscan/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/cardanoscan)
- Checks that hold for **all five** ([`05` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions)):

  | Check | Result |
  |---|---|
  | Every Plutus script execution succeeded (`valid_contract = true`) | ✅ |
  | Metadata label 674 reads *"Dano Finance: Create Loan"* | ✅ |
  | The source Fluid loan UTxO is consumed | ✅ |
  | The Fluid position NFT is burned — the Fluid loan is settled in full | ✅ |
  | A distinct **Borrower NFT** is minted to the signing wallet — the new Dano loan exists and the borrower holds its title | ✅ |
  | Collateral in = collateral out, to the smallest unit | ✅ |
  | **One transaction hash, one block** | ✅ |
  | Borrower contributes no repayment capital beyond network fee and min-UTxO | ✅ on the four Flexible Pool transactions; **TX‑02 excepted** — the fixed-term staking route does not capitalise the fee and the borrower funds 0.954728 ₳ of it ([`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) C‑4) |

- **The zero-fee case** is `0e26cc58…`: the pool disburses STRIKE 5.000560, the settlement receives
  STRIKE 5.000558, and the transaction has no leg to the fee address at all. *(The previous
  submission cited `88579a30…` for this; that was wrong — Output 7, C‑1.)*
- **The interface and the ledger agree at the point of signing.** Before signing TX‑04 the app showed
  a Fluid debt of 12 ADA and a fee of 2 ADA; on chain the settlement leg is 12.000102 ADA, the fee leg
  is exactly 2.000000 ADA, and DJED 6.000000 was carried across —
  [`05` TX‑04](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#3-per-transaction-value-flow)

---

## Output 3 — The repay journey, in both of its forms

**Output:** Repay exists in two distinct forms, and both are evidenced on mainnet.

| Form | What it is | Mainnet evidence |
|---|---|---|
| **A — Repay as a direct user action** | The borrower opens *Manage → Repay* and settles the debt **from their own funds**; the collateral is **released back to them** and no new loan is created | **3 transactions**: [`17c23dde…`](https://cardanoscan.io/transaction/17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a) (pays USDCx 5.000011, releases ADA 50.000000) · [`ea823365…`](https://cardanoscan.io/transaction/ea823365562e1eefec0cb3be614a54963ec128dcb3ef8430099d308902bfe04d) (settles 2.011600 ₳, releases USDM 16.000000) · [`77748bd9…`](https://cardanoscan.io/transaction/77748bd9673991565c25671522fe70914e29098d7bd0f4c164cf4677585522bc) — **Danogo's own `Repay Loan` path**, USDM 6.096399 split pool 4.396378 / fee 1.700021, Borrower NFT burned |
| **B — Repay inside the refinance, same transaction** | The Fluid debt is settled **by the Dano pool, in the very transaction that opens the new Dano loan** — not a second transaction and not a second user action | The settlement leg of each of the **5** refinances in Output 2 |

Form B is **not** counted as a second repay journey. It is stated here because it is what makes the
rolling loan one transaction rather than two.

`ee87712a…` (open, 2026-08-28) and `77748bd9…` (repay, three hours later) are a complete open-and-repay
lifecycle on Danogo's own contracts, with no external protocol in the path.

**Acceptance criteria:**
1. **Full settlement** — evidenced by the burn of the Fluid position NFT, which Fluid's own validator
   permits only on full settlement.
2. **Post-settlement state** — the loan is gone from the chain and from what the app shows, and the
   protocol that was owed the money reports it as repaid.
3. In form B the borrower provides no repayment funds beyond the network fee and min-ADA amounts.

**Evidence:**
- **The counterparty protocol confirms the repayments.** Fluid Tokens publishes each borrower's loan
  history from its own indexer, no credentials required:
  `GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>`. Queried on the two
  borrower wallets it returns **seven** loan events, every one `"action": "loan_repaid"`,
  `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0` —
  [`05` §5.3](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#53-fluids-own-records-say-the-loans-are-repaid)
- **The unmodified API responses**, byte for byte, with fetch time and SHA-256 of each file:
  [`fluid-api/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/fluid-api)
- **Fluid's record and the ledger agree independently**: Fluid's `loanUtxoId` names the transaction
  that opened each loan and `finishingTxHash` the transaction this app built to close it; the token
  Fluid names is byte-for-byte the token our transaction burned; and every settlement pays at least
  the total Fluid says was due —
  [`05` §5.2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#52-the-on-chain-proof-that-a-repayment-occurred)
- The three direct repayments in full:
  [`05` §5.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#55-repaying-an-external-loan-on-mainnet-two-transactions)
  and [`05` §5.6](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#56-repaying-a-dano-loan--danogos-own-repayment-path)
- **The settlement holds after the fact.** All five refinanced Fluid position NFTs now have a total
  supply of zero across Cardano — the repaid loans do not exist anywhere on chain, which is why
  neither borrower's screen lists a Fluid loan —
  [`04` §2.5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#25-what-is-absent-from-the-screenshots-and-why-that-is-the-point)
- Fluid's own borrower dashboard, the loans marked `REPAID`:
  [`screenshots/fluid-dashboard/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/fluid-dashboard)

---

## Output 4 — Ready-to-test user journeys: five recorded end-to-end sessions

**Output:** Five recorded sessions, each an unedited continuous take from the first click to the
settled transaction, with every wallet signing dialog and every explorer record. Four ran on preprod
on 18 September 2026 through **Eternl**, on three different accounts, two of them separate
installations (v2.1.7.1 and v2.1.5.0; journey 4's version was not recorded). A fifth, on
21 September 2026, ran the **same account as journey 3 through a second wallet brand, Vespr** — a
different wallet application, not a different tester.

| | Journeys covered | Recording | Length | QC |
|---|---|---|---|---|
| **1** | open · view · refinance · repay — borrow 25 ADA against 100 fUSDM | [`01.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/01-open-and-refinance-borrow-ADA-collateral-USDM.mp4) *(this session's repay is captured as screenshots, not on the recording)* | 5 min 21 s | 31 / 31 |
| **2** | the same, in the opposite asset direction — borrow 11 fUSDM against 100 ADA | [`02.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/02-open-and-refinance-borrow-USDM-collateral-ADA.mp4) *(repay likewise in screenshots)* | 3 min 33 s | 33 / 33 |
| **3** | **all four journeys in one unbroken take**, run by a second tester on a second wallet and a different Eternl version | [`03.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/03-open-refinance-repay-borrow-USDM-collateral-ADA.mp4) | 7 min 29 s | 22 / 22 |
| **4** | open · view · refinance ×2 — **one borrow of 905.004 ADA fills two pools and opens two loans**, each refinanced separately | [`04.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/04-open-and-two-refinances-borrow-ADA-collateral-USDM.mp4) | 4 min 03 s | 37 / 37 |
| **5** | open · refinance — **through Vespr, not Eternl** | [`05.mp4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/05-refinance-via-vespr-second-wallet-brand.mp4) | 2 min 25 s | no QC set of its own |

**Acceptance criteria:**
1. The complete click path of each journey is visible, uninterrupted, not described.
2. What the interface quotes **before** signing is what the chain records **after** it.
3. The refinance settles as **one transaction** in each session.

**Evidence:**
- The sessions check by check — **123 QC checks across journeys 1–4, 123 hold**:
  [`06_UAT_Reports_Four_Journeys_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md)
  ([At a glance](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#at-a-glance))
- In every refinance the quoted fee is the fee paid — **2.000000 to the fee address, five times out
  of five** — the quoted health factor is the one the new loan carries, the resulting debt is
  debt + fee, and the pool disbursed exactly settlement + fee, with the collateral moving from the
  Fluid script to the Dano loan contract **inside one transaction**.
- The three preprod repayments, each closing the loan that same walkthrough had just opened:
  [`e134b85e…`](https://preprod.cardanoscan.io/transaction/e134b85eb5631391089598adefd8f06024307fc7d41ec4ac8d421276d8681416) (quoted 27.000474 ADA, paid 27.000473, 100 fUSDM released) ·
  [`bae9a9c9…`](https://preprod.cardanoscan.io/transaction/bae9a9c9e0e4f63657d43cc21b4ab071e82d57919733ddb8908a958937c75b7d) (quoted 13.000218 fUSDM, paid 13.000218, 100 ADA released) ·
  [`a4bebdb7…`](https://preprod.cardanoscan.io/transaction/a4bebdb7ca185ffc1ce4fc22873e9e3d9cefa80e0d1e0d8ad698e6b1813fbeed) (100 ADA released, Borrower NFT burned)
- **What the journey-3 recording also shows.** The first refinance submit failed after the
  signature; the tester pressed **Retry** in the app, signed again, and it settled. The ledger
  carries **exactly one transaction** for that refinance and no funds were lost. Cause established:
  the ledger rejected the first submission with `ScriptsNotPaidUTxO` because the wallet offered a
  collateral UTxO its own cache had not refreshed — [`06` D‑1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions)
- **A second wallet brand.** The Vespr session settled an open,
  [`6f68ac98…eb38`](https://preprod.cardanoscan.io/transaction/6f68ac98039b84e48f65e3d0921b53e513c3ddeeb3f4f8d479f7c2f52976eb38),
  and the refinance that closed it,
  [`b2937327…7bf4`](https://preprod.cardanoscan.io/transaction/b2937327cc0661395029834652ad9f076eed677248f95f51fcdb9f76a2ab7bf4)
  — 13 Plutus script executions across the two, all valid; 200.000000 ₳ of collateral carried across
  to the lovelace; fee exactly 2 fUSDM. It covers *open* and *refinance* only, with no screenshot set
  and no QC checks —
  [`06` Journey 5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#journey-5--open--refinance-through-a-second-wallet-brand-vespr)
- Step-by-step screenshot sets, one folder per journey:
  [`journey-1`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-1-borrow-ADA-collateral-USDM) ·
  [`journey-2`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-2-borrow-USDM-collateral-ADA) ·
  [`journey-3`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-3-borrow-USDM-collateral-ADA) ·
  [`journey-4`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-4-borrow-ADA-collateral-USDM)
  · the mainnet refinance walkthrough: [`screenshots/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots)

> **The demo video submitted the first time, https://youtu.be/z07TxLJLC2w, is withdrawn as evidence.**
> It was a developer walking through the feature — the first thing the reviewer rejected. The five
> recordings above replace it: unedited, full length, the interface being operated rather than
> described. The link is kept only as a record of what was submitted.

---

## Output 5 — The resulting state in the application matches the ledger

**Output:** The *view* journey. Both signing wallets' *My Account → Loans* screens were captured from
the live mainnet app, and every displayed borrowed amount equals the on-chain principal plus interest
accrued at the APR the same row displays.

| Wallet | App shows | Created by | Principal on chain | Principal + accrual |
|---|---|---|---|---|
| W1 | 15.07 ADA @ 8.47% | `d240fab1…` | 15.015024 ADA | 15.0736 → **15.07** |
| W1 | 22.04 ADA @ 3.07% | `88579a30…` | 22.000857 ADA | 22.0413 → **22.04** |
| W2 | 14.01 ADA @ 3.07% | `c426d9fa…` | 14.000108 ADA | 14.0185 → **14.01** |
| W2 | 13.01 ADA @ 3.07% | `1cf8f08b…` | 13.000010 ADA | 13.0174 → **13.01** |
| W2 | 5.01 **STRIKE** @ 5.00% | `0e26cc58…` | 5.000560 STRIKE | 5.0101 → **5.01** |

**Acceptance criteria:**
1. The wallets shown connected in the app are the wallets that signed the transactions.
2. Every displayed borrowed amount is re-derivable from public chain data.
3. The number of loans the app lists is independently countable on chain.
4. The settled Fluid loans are absent from the interface, because they are absent from the chain.

**Evidence:**
- Row-by-row reconciliation, with the screenshots:
  [`04` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger)
  · wallet screens: [`screenshots/screens/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/screens)
- **The loan counts match the chain.** The app lists **2** loans for W1 and **4** for W2 — exactly the
  number of Borrower NFTs each wallet holds on chain. W2's fourth is a USDA loan from October 2025,
  outside this milestone, named only so the count adds up:
  [`04` §2.3–2.4](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_USER_JOURNEYS_AND_APP_STATE.md#23-how-the-application-knows-which-loans-to-show)
- The figures are live reads from the public **Koios** API — the principals are fixed, the accrued
  interest grows as the loans age.

---

## Output 6 — Integration test results

**Output:** Each of the four journeys was executed end to end and verified check by check. Every
check below is published with the figure it asserts and the public source that figure was re-derived
from, so a reviewer can repeat it without us. Failing checks are published as failures.

| What was tested | Result | Detail |
|---|---|---|
| **Four end-to-end sessions on preprod** — each covering open, view and refinance; three of them repay as well | **123 QC checks across the four sessions, 123 hold** | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md) |
| **The executed mainnet refinance, transaction by transaction** (TC‑01 … TC‑25) | **24 of 25 hold.** TC‑24 does not: it asserts that `88579a30…` carries no origination-fee leg, and that is the wrong transaction to cite for the zero-fee case (Output 7, C‑1). It is an error in our own evidence, not a product defect | [`01` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md) · [`08` C‑1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) |
| **All 15 mainnet transactions**, against the public ledger | **every Plutus script execution returned `valid_contract = true`**; inputs, outputs, mints and burns re-derived from Koios | [`05` §2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#2-facts-that-hold-for-all-five-transactions) |
| **Repayment status, from the counterparty protocol** | **7 of 7** Fluid loans reported `loan_repaid`, `remainingDebt: 0`, `penaltyPaid: 0` | [`05` §5.3](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#53-fluids-own-records-say-the-loans-are-repaid) · [`fluid-api/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/fluid-api) |
| **What the interface quoted before signing, against what settled** | fee, collateral, health factor and resulting debt match to the smallest unit, in all five preprod refinances and all three preprod repayments | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md) |

**Acceptance criteria:**
1. Every check names the figure it asserts and the public source it was re-derived from.
2. Checks that do not hold are published as such, with the reason.
3. No result is reported that a third party cannot reproduce from the artifacts in this repository.

**Evidence:**
- The four sessions, check by check:
  [`06_UAT_Reports_Four_Journeys_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md)
- Transaction-level QC against the executed refinance:
  [`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md) §2
- Known issues found during the sessions, with their established root causes — the *Deposit* / *Fee*
  preview lines that reconcile to nothing on chain (OI‑1), the Fluid borrower token left in the
  wallet after the position is gone (OI‑2), and the journey‑3 submit failure (D‑1):
  [`06` Root causes](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#root-causes-established-after-the-sessions)
  · [`06` OI‑1 / OI‑2](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session)
- The previous submission's report and checklist, kept **unedited** as archives:
  [`01_Integration_Test_Report_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/01_Integration_Test_Report_M2.md) ·
  [`03_Reviewer_Checklist_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/03_Reviewer_Checklist_M2.md)

---

## Output 7 — Corrections to our own previous submission

**Output:** While re-deriving every figure from public APIs for this resubmission we found errors in
our own evidence and disclose them unprompted. **No on-chain figure in this package changes.**

- **C‑1 — the wrong transaction was cited as the zero-fee example.** On `88579a30…` the pool
  disburses 22.000857 ₳, of which 20.000851 ₳ settles the Fluid debt and exactly 2.000000 ₳ goes to
  the fee address. The underlying claim was right; the transaction cited for it was wrong. The
  zero-fee example is `0e26cc58…`.
- **C‑2 — the usability self-assessment is withdrawn**, and reported as the sessions it recorded
  rather than as a verdict.
- **C‑3 — a recurring marker token was described as a per-loan NFT.** Danogo mints two tokens per
  loan; the one that stays in the loan contract recurs across many loans. The per-loan identity is
  the **Borrower NFT** paid to the borrower's wallet, and every "a distinct loan was created" claim
  is now made against it.
- **C‑4 — "the borrower contributes only the network fee" was over-stated.** It holds for the four
  Flexible Pool refinances. On `d240fab1…`, drawing on the fixed-term staking contract, the borrower
  funds 0.954728 ₳ of the fee. That transaction still settles the debt in full and carries the
  collateral across; it simply does not demonstrate the "no capital needed" property.
- **C‑5 — "Fluid has no Cardano testnet deployment" was stated too absolutely.** The walkthroughs ran
  against Fluid's preprod contracts. What holds is the reason the *settlement* evidence is on
  mainnet: that is where the Fluid → Dano path exists as a real market.
- **C‑6 — "the 8 automated refinance tests pass 8 / 8" is withdrawn.** Two of those eight never
  reached their own assertion, and all eight carry a `KNOWN-FAIL (finding, app-vs-spec)` annotation
  in their own source — they were written to record a divergence from the screen spec, not to pass,
  and we republished the figure without reading them. **No automated-test pass rate is claimed in
  its place.** The integration-test evidence this submission does offer is Output 6, all of it
  re-derivable from public sources.

**Acceptance criteria:** every figure in this submission is drawn from public data rather than from
our own reporting, and errors we find are disclosed rather than left for a reviewer to find.

**Evidence:**
- Full on-chain arithmetic for each correction:
  [`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md)
- The previous submission's transaction evidence, kept unedited at its original path with a banner
  pointing at that log:
  [`02_Mainnet_Transactions_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_Mainnet_Transactions_M2.md)

---

## What this submission does and does not contain

| | |
|---|---|
| Functional and repayment-status claims | supported by public **Koios** / **Cardanoscan** data and **Fluid Tokens'** own API |
| Interface evidence — screenshots, five session recordings | linked in full under Outputs 4 and 5, and named as our own captures |
| Integration-test results | every check published with the figure it asserts and the public source it was re-derived from — Output 6 |
| Anything not listed above | **not claimed.** Where an artifact does not support a claim, the claim is not made |

The core functionality demonstrated is an active **Fluid** loan state being consumed and a new **Dano
Finance** loan state being created **in a single atomic transaction** — the Fluid debt repaid and the
Dano loan opened together, integrated end-to-end into the product and executed successfully on Cardano
mainnet, on a public ledger the reviewer can query without us.
