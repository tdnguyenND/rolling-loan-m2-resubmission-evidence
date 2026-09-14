# Proof of Achievement — Milestone 2 (Resubmission)

**Project** 1400107 — Rolling Loan
**Milestone** 2 — Feature Integration
**Submission** Resubmission following the *Not Approved* review
**Milestone page** https://milestones.projectcatalyst.io/projects/1400107/milestones/2
**Evidence repository** https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence

---

> **How to use this file.** This document is organised as Output, Acceptance criteria and Evidence,
> the shape the Catalyst Proof of Achievement form requires. Each claim links to a public artifact.
> Each journey was walked end-to-end through the connected wallet by our own testers; that is
> presented here as internal testing — see the Opening statement.

---

## Opening statement

We accept the previous review in full. Both objections described real gaps.

The previous submission reported its testing as a verdict on usability, which is not what that
testing established. The testing itself is real and repeatable: each journey was walked end-to-end
through the connected wallet, and the open, repay and refinance sessions settled on mainnet six
times, from two wallets, over nine days. This resubmission presents that as internal testing and
pairs it with functional and on-chain evidence a reviewer can check without us.

Three things changed:

1. **The usability verdict is restated as the testing that produced it** — internal walkthroughs
   of each journey, six of them traceable to a mainnet transaction.
2. **Evidence is organised by journey** — each of the four is evidenced on its own, rather than
   inferred from another journey.
3. **On-chain figures were rechecked against Koios**, a public Cardano API, instead of our own
   backend; one correction is disclosed in Output 5.

The previous submission's files are kept, unedited, in the same repository, each carrying a banner
that points at the corrections log. We did not quietly fix them.

---

## At a glance

| Journey | Result | Main evidence |
|---|---|---|
| **View** | Both borrower wallets' loan screens captured from the live app; every displayed borrowed amount matches the ledger | [Post-state reconciliation](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_POST_STATE_UI_RECONCILIATION.md) |
| **Open** | Five Dano loans opened on mainnet, one in each refinance transaction | [Transaction ledger](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_ONCHAIN_TRANSACTION_LEDGER.md) |
| **Repay** | Six Fluid loans repaid in full — five as the settlement leg of a refinance, one standalone; Fluid reports all six as repaid | [Repay journey annex](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/03_REPAY_JOURNEY_EXPLAINED.md) |
| **Refinance** | Five mainnet transactions, each closing a Fluid loan and opening a Dano loan in one transaction | [Transaction ledger](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_ONCHAIN_TRANSACTION_LEDGER.md) |

Per-journey user paths and evidence classes:
[Journey matrix](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_USER_JOURNEY_EVIDENCE_MATRIX.md).

---

## Terms used in this document

| Term | Meaning |
|---|---|
| **Atomic** | The old loan closes and the new loan opens in the same transaction. If either step fails, neither is completed. |
| **Fluid position NFT** | A Fluid loan's on-chain identity token. When the loan is fully settled, this token is burned. |
| **Dano loan position NFT** | The token minted into the Dano loan contract when the new loan is created. Its existence is what makes the new loan a position on chain. |
| **Borrower NFT** | The token Danogo mints to the borrower's own wallet when a Dano loan opens — distinct from the position NFT above. The application lists the loans whose Borrower NFT the connected wallet holds. |
| **Origination fee** | The fee a Dano pool charges when a loan is created. Where a pool charges it, it is added to the new loan amount, so the borrower does not provide it separately. |
| **Settlement leg** | The output in a refinance transaction that pays off the Fluid debt, funded by the incoming Dano loan. |

---

## Output 1 — Integrated front end with the four approved loan journeys, on the Eternl-connected app

**Output:** The live application at https://v2.dano.finance/ lets an Eternl-connected borrower
**view**, **open**, **repay** and **refinance** loans against live Cardano mainnet contracts. In a
refinance ("Refinance via Dano"), the application closes a Fluid loan and opens a Dano loan in one
transaction. That journey is the milestone's core deliverable: it rolls a loan out of the external
Fluid protocol into a Dano Finance loan.

*Note: the app uses the borrower's Eternl wallet to request signatures and does not custody private
keys.*

**Acceptance criteria:**
1. All four journeys are reachable and completable in the live application with Eternl connected.
2. Each journey is evidenced separately — not inferred from another journey.
3. The refinance is atomic: the old loan closes and the new loan opens in the same transaction, the
   collateral carries across, and the borrower provides no repayment funds beyond the network fee
   and required minimum-ADA output amounts — the minimum ADA that Cardano requires each transaction
   output to hold.
4. What the application displays to the connected user is what the Cardano ledger holds.

**Evidence:**
- Live application — https://v2.dano.finance/
- Demo video — https://youtu.be/z07TxLJLC2w
- [Journey matrix](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_USER_JOURNEY_EVIDENCE_MATRIX.md)
  — the exact user path and evidence for each journey
- [Post-state reconciliation](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_POST_STATE_UI_RECONCILIATION.md)
  — the *view* journey, captured on two mainnet wallets, every displayed figure matched to the ledger
- [Walkthrough captures](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots)
  — loans sheet, refinance card, the Eternl signing dialog with inputs and outputs, confirmation
- The *refinance* journey, executed five times on mainnet — Output 3 below

---

## Output 2 — The "repay" journey, in both of its forms

**Output:** Repay has two forms: **(1)** standalone repayment from the loan-management screen,
implemented for each supported lending protocol with its own rules (see the
[journey matrix](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_USER_JOURNEY_EVIDENCE_MATRIX.md)),
and **(2)** repayment as part of a rolling refinance. In the rolling flow, the new Dano loan settles
the Fluid debt in the same transaction. The borrower does not need to repay the Fluid debt
separately before opening the Dano loan.

**Result:** Six Fluid loans were repaid in full on Cardano mainnet through the Eternl-connected
application — five as the settlement leg of the five refinance transactions detailed in Output 3
below, one standalone. Fluid's public loan-history API records six repayments for the two borrower
wallets, and each record reports `status: repaid` and `remainingDebt: 0`.

**Acceptance criteria:**
1. **Full settlement.** Each Fluid loan is settled in full, evidenced by the burn of its Fluid
   position NFT — a burn Fluid's own validator permits only on full settlement.
2. **Borrower funding.** In the rolling form the borrower provides no repayment funds beyond the
   network fee and required minimum-ADA output amounts.
3. **Post-settlement state.** After settlement the loan is gone from the chain and from what the
   application shows the user, and the protocol that was owed the money reports it as repaid.

**Evidence:**
- **The protocol that was owed the money reports these loans as repaid.** Fluid Tokens publishes a
  borrower's loan history from its own indexer (`api.fluidtokens.com/wallet-lending-history`).
  Queried for the two borrower wallets over this milestone's period, it returns six events, every
  one `"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0`.
  Each record identifies a transaction whose on-chain metadata reads *"Dano Finance: …"*. Fluid is a
  separate protocol and a separate company, and its repayment status is independently queryable from
  its public API.
- **That record reconciles to the ledger on four points**, per loan:
  - **Fluid's status** — the loan is reported repaid, with nothing owing and no penalty.
  - **Matching NFT** — the loan token Fluid names is the exact token the transaction burned.
  - **Payment amount** — the transaction pays at least what Fluid says was due, every time.
  - **Transaction type** — whether it was a refinance or a standalone repay follows from what the
    transaction does on chain: a refinance opens a Dano loan in the same transaction, a standalone
    repay does not.

  The per-loan table, the burn records and the arithmetic are in the
  [repay journey annex](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/03_REPAY_JOURNEY_EXPLAINED.md).
- **The Fluid position was closed in each refinance.** The Fluid position NFT was burned in all five
  transactions — in plain terms, the Fluid loan ceased to exist — and Fluid's API independently
  reports the same loans as repaid with zero remaining debt.
- **The settlement holds after the fact, not only inside the transaction.** At the time of the
  Koios query documented in the
  [post-state reconciliation](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_POST_STATE_UI_RECONCILIATION.md)
  (2026‑09‑10), all five Fluid position NFTs had a total supply of zero on Cardano, and neither
  borrower's *My Account* screen listed a Fluid loan.
- **The standalone repay flow, on mainnet.**
  [`17c23dde…e2559a`](https://cardanoscan.io/transaction/17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a)
  — 2026‑08‑24. Its transaction metadata reads *"Dano Finance: Repay Fluid Loan"*. It is the mirror
  of the refinance case: the borrower pays the debt (USDCx 5.000011) from their **own** wallet, the
  50.000000 ADA of collateral is **released back to them**, the Fluid position NFT is burned, and no
  Dano loan is created.
- [Fluid borrower-dashboard screenshots](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/fluid-dashboard),
  as a borrower sees them — the loans marked `REPAID`

---

## Output 3 — Successful mainnet execution: five independently verifiable transactions

**Output:** Five Cardano **mainnet** transactions completed the refinance flow across two wallets,
on five occasions from 19 to 27 August 2026. Each closes a Fluid loan and opens a Dano loan
atomically. Fluid has no testnet deployment, so this cross-protocol flow can only run on mainnet.

| # | Tx hash | Date (UTC) | Collateral carried | Borrowed | Liquidity source | Origination fee |
|---|---|---|---|---|---|---|
| 1 | `88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652` | 2026‑08‑19 | USDM 10.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| 2 | `d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c` | 2026‑08‑24 | SNEK 20,979 | ADA | Staking (fixed-term) | 0.969750 ₳ |
| 3 | `1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10` | 2026‑08‑25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| 4 | `c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c` | 2026‑08‑25 | DJED 6.000000 | ADA | Flexible Pool | 2.000000 ₳ |
| 5 | `0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8` | 2026‑08‑27 | DJED 10.000000 | **STRIKE** | Flexible Pool | **none** |

The transactions cover three collateral assets, two borrowed assets, two Dano liquidity sources and
three origination-fee configurations.

**Acceptance criteria:**
1. Every transaction is accepted by the live deployed validators of **both** protocols.
2. Every transaction consumes the source Fluid loan UTxO and creates the Dano loan UTxO atomically.
3. Collateral is carried across exactly, never returned to the borrower.
4. Every figure is derived from public chain data, not from our own backend.

**Evidence:**
- [Transaction ledger](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/04_ONCHAIN_TRANSACTION_LEDGER.md)
  — per-transaction value flow, with the public-API source of every figure
- [Cardanoscan screenshots](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/cardanoscan),
  one per transaction
- On Cardanoscan, transaction by transaction:
  - [`88579a30…`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652)
  - [`d240fab1…`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c)
  - [`1cf8f08b…`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10)
  - [`c426d9fa…`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c)
  - [`0e26cc58…`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8)
- Checks that hold for **all five** transactions:

  | Check | Result |
  |---|---|
  | All contract validations succeeded (`valid_contract = true` on every Plutus script execution) | ✅ |
  | Transaction metadata label 674 reads *"Dano Finance: Create Loan"* | ✅ |
  | The source Fluid loan UTxO is consumed | ✅ |
  | The Fluid position NFT is burned — the Fluid loan closed in full | ✅ |
  | A Dano loan position NFT is minted — the new loan opened | ✅ |
  | Collateral in = collateral out, to the smallest unit | ✅ |
  | One transaction, one block | ✅ |

- **The interface and the ledger agree at the point of signing.** Before signing transaction 4, the
  application showed a Fluid debt of 12 ADA and a fee of 2 ADA. On chain, the settlement leg is
  12.000102 ADA, the fee leg is exactly 2.000000 ADA, and DJED 6.000000 was carried across. The
  number the user was shown is the number that settled.

---

## Output 4 — The resulting state in the application matches the ledger

**Output:** This output shows the post-transaction state in the borrower-facing application and
compares the displayed loan amounts with the ledger. Both signing wallets' *My Account → Loans*
screens were captured from the live application; every displayed borrowed amount equals the
on-chain principal plus interest accrued at the APR the same row displays.

| Wallet | Application shows | Created by | Principal on chain | Principal + accrual |
|---|---|---|---|---|
| W1 | 15.07 ADA @ 8.47% | `d240fab1…` | 15.015024 ADA | 15.0736 → **15.07** |
| W1 | 22.04 ADA @ 3.07% | `88579a30…` | 22.000857 ADA | 22.0413 → **22.04** |
| W2 | 14.01 ADA @ 3.07% | `c426d9fa…` | 14.000108 ADA | 14.0185 → **14.01** |
| W2 | 13.01 ADA @ 3.07% | `1cf8f08b…` | 13.000010 ADA | 13.0174 → **13.01** |
| W2 | 5.01 **STRIKE** @ 5.00% | `0e26cc58…` | 5.000560 STRIKE | 5.0101 → **5.01** |

**Acceptance criteria:**
1. The wallets shown connected in the application are the wallets that signed the transactions.
2. Every displayed borrowed amount in the table is re-derivable from public chain data.
3. The number of loans the application lists is independently countable on chain.
4. The settled Fluid loans are absent from the interface, because they are absent from the chain.

**Evidence:**
- [Post-state reconciliation](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_POST_STATE_UI_RECONCILIATION.md)
  — the row-by-row reconciliation above, screenshots included
- [Application screenshots](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/screens),
  one per wallet
- **The loan counts match the chain.** The application shows **2** loans for W1 and **4** for W2.
  Those counts equal the Borrower NFTs each wallet holds on chain; the post-state annex explains the
  NFT mapping.
- The figures are **live** reads from the public Koios API: the principals are fixed, and the
  accrued interest grows as the loans age.

---

## Output 5 — Correction to our own previous submission

**Output:** While rechecking the transactions against a public API for this resubmission, we found
an error in our previously submitted evidence and are correcting it unprompted.

We previously cited transaction `88579a30…` as a zero-origination-fee example. That was incorrect:
it includes a 2.000000 ADA origination fee. The correct zero-fee example is transaction
`0e26cc58…`, which has no fee leg at all.

For fee-charging pools, the origination fee is added to the new loan amount. The borrower does not
provide that fee separately. For example, a 20 ADA Fluid debt plus a 2 ADA fee becomes a 22 ADA Dano
loan. The 20 ADA figure we previously described as the new borrow was actually the settlement
amount. On `88579a30…` the pool disbursed 22.000857 ADA in total: 20.000851 ADA settled the Fluid
debt and 2.000000 ADA paid the fee.

**Acceptance criteria:** every figure in this submission is drawn from public data rather than from
our own reporting, and errors we find are disclosed rather than left for a reviewer to find.

**Evidence:**
- [Corrections log](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md)
  — the on-chain arithmetic for each correction
- [The previous submission's transaction evidence](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/02_Mainnet_Transactions_M2.md),
  kept unedited at its original path with a banner pointing at that log

---

## What this submission does and does not contain

| | |
|---|---|
| Functional and repayment-status claims | ✅ supported by Koios and Fluid public data |
| UI screenshots and demo video | ✅ linked separately, under Outputs 1 and 4 |
| User testing | ✅ every journey walked end-to-end by our own testers; 6 sessions settled on mainnet — internal testing |

The functional on-chain and repayment-status claims above are supported by public data: the Cardano
ledger read from the Koios API, and Fluid Tokens' own loan records read from Fluid's API. The
interface evidence is of a different kind — screenshots and a video captured by us — and is linked
as such.
