# Milestone 2 — User Acceptance Test Report · Journey 2

**Feature:** Rolling Loan ("Refinance via Dano") — **open**, **view**, **refinance** and **repay** a
loan from the Danogo front end with the user's own wallet.
**Journey:** borrow **11 fUSDM** from Fluid against **100 ADA** collateral, refinance it into a Dano
Finance loan, then **repay that Dano loan in full and unlock the collateral** — *open → view →
refinance → view → repay*, all four approved journeys on one loan. This is Journey 1's asset direction reversed, run on the same day from the same
wallet: [`06_UAT_Report_Journey_1_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md).
**Environment:** front end https://preprod.danogo.io, running against **Cardano preprod** and the
**Fluid preprod** smart contracts. Signed with the **Eternl** wallet (v2.1.7.1), account *1kang (#0)*,
address `addr_test1qr4rlljcxqj8dwyf076u3g3lh3p879mvkr9qdhu9kmquqejlawjwdzr7ul3pseee3j05kalkqhququxqqum4j83w4n8qf5g494`.
**Date:** 2026-09-18. Open 04:59:12 and refinance 05:01:04 (wallet clock; the explorer renders that
settlement at 10:01:04 AM in its own timezone); the repay settled later the same day, in block
5,190,611 at 2:39:18 PM (explorer clock).
**Screen recording of the whole session:** [`02-open-and-refinance-borrow-USDM-collateral-ADA.webm`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/videos/02-open-and-refinance-borrow-USDM-collateral-ADA.webm)
**Screenshots:** [`screenshots/journey-2-borrow-USDM-collateral-ADA/`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/tree/main/screenshots/journey-2-borrow-USDM-collateral-ADA)

> **What this session is, and what it is not.** It was run by the delivery team, not by an
> independent tester, and is **not** offered as evidence that the interface is intuitive to someone
> who has never seen it — that is the claim the review rejected
> ([`08_CORRECTIONS.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/08_CORRECTIONS.md) C-2). What it establishes is the complete click path
> of each journey and the agreement between what the interface quoted before the signature and what
> the chain recorded after it. The milestone's settlement evidence is the fifteen **mainnet**
> transactions in [`00`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/00_POA_SUBMISSION_FORM.md) §A.

---

## 1. The journey, step by step

*Open* is steps 1–3, *View* is step 4, *Refinance* is steps 5–6, *View* again is steps 7 and 10, and
*Repay* is steps 11–13, on the loan the refinance created.

| # | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|
| 1 | **Borrow Market** — choose ADA as collateral, pick a pool, enter the amounts | pools listed with their rates; a *Loan Impact* preview before anything is signed | collateral **ADA**, balance 1,163.58; **fUSDM** market, Fluid Flexible **4.00%** (67% LTV) selected; **You Borrow 11 fUSDM** ($11.00), **Collateral 100 ADA** ($30.50); *Loan Impact* — Current Borrow APR **4.00%**, 2.04%/month, New Collateral $30.50, **New Health Factor 3.63 Healthy**, Deposit 5 ADA, Fee 5 fUSDM ($5.00) | [`01`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/01-borrow-market-create-loan.png) |
| 2 | **Create Loan** → sign in Eternl | one transaction, memo *"Dano Finance: Borrow from Fluid"*, collateral leaving, position token minted | Eternl: 1 transaction, tags **Sent TADA · Contract · Mint · Memo**, memo **Dano Finance: Borrow from Fluid**, wallet net **−t₳102.05**, token `2d3883b3…70b67d8a` **+1**, **fUSDM +11,000,000** (the full 11 fUSDM borrowed); the app behind the dialog shows *"Waiting for wallet signature…"* | [`02`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/02-eternl-sign-open-from-fluid.png) |
| 3 | **Portfolio** after the open | the new Fluid position appears | Total value **$1,342.30**; **Fluid · Supply 110 ADA ($33.55)** — the 100 just locked plus 10 already supplied; Journey 1's **Dano · Borrow −27** still listed | [`03`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/03-portfolio-after-open.png) |
| 4 | **My Account → Loans** *(the View journey)* | one row per open loan | *fUSDM · Loans*, two rows: **11 fUSDM ($11.0)**, collateral $30.5 (ADA), APR **4.00%**, badge **"Dano save 9.86% net cost"**, **HF 3.63 · Healthy** — and a second, unrelated **2 fUSDM** loan, HF 1.99, which this session does not touch | [`04`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/04-my-account-loans.png) |
| 5 | **Loan Details → Refinance via Dano** | the quote, in full, before signing | Loan Details (Fluid): Total Debt **$11.00 / 11 fUSDM**, HF **3.63 Healthy**, APR **4.00%**, collateral **ADA 100 ($30.50)**. Card: **Save 9.86% net cost**; Net cost **4.00% → −5.86%**; Health Factor **3.63 Healthy → 1.87 Healthy**; ✓ *Same loan, Same collateral*; *"You don't need extra funds to close your Fluid loan."*; **Fee 2 fUSDM** | [`05`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/05-loan-details-refinance-quote.png) |
| 6 | **Confirm Refinance via Dano** → sign in Eternl | **one** transaction, memo *"Dano Finance: Create Loan"* | Eternl: 1 transaction, tags **Sent TADA · Contract · Mint · Burn · Memo**, memo **Dano Finance: Create Loan**; inputs include the Fluid loan script `addr_test1zpyg4esucs…n47ypv` at **−t₳100** carrying token `2d3883b3…` −1; **6 outputs**, among them **+t₳1.96 and fUSDM +11,000,001** to `addr_test1qr9ew225…s2n6em` | [`06`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/06-eternl-sign-refinance.png) |
| 7 | **Loan Details, reopened** *(View)* | the loan is now a Dano loan, same collateral | **Dano Finance** · Total Debt **$13.00 / 13 fUSDM** · HF **1.87 Healthy** · APR **3.07%** · Collateral Backing $30.50 · utilisation 3% · collateral **ADA 100** · actions *Repay Loan · Modify Collateral · Increase Loan* | [`07`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/07-loan-details-after-refinance.png) |
| 8 | **Eternl → Transactions** — the wallet's own record | both transactions, with memos and amounts | 09/18/2026 **04:59:12** *Dano Finance: Borrow from Fluid* (−t₳102.05) and **05:01:04** *Dano Finance: Create Loan* (−t₳4.7, token `7b8a46d8…` +1, fUSDM +6 = 0.000006 fUSDM of change); Journey 1's two transactions are in the same list | [`08`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/08-eternl-transaction-list.png) |
| 9 | **Cardanoscan** — the refinance as a third party sees it | the collateral moved, the debt settled, the fee paid | **3 inputs → 6 outputs**, 7 contracts, **3 mints & burns**, 1 metadata entry, 3 withdrawals, 14 reference inputs. **ADA Sent −100.0** from the Fluid side, **ADA Received 100.0** on the Dano side; **Tokens Sent `8dcf151…cf6` −1**, **Tokens Received `904770e…4c5` +1**; the pool sends **fUSDM −13,000,007**; **fUSDM +11,000,001** to the lender and **fUSDM +2,000,000** to the fee address; borrower net **−4.70041 ₳**, of which fee **1.58428 ₳**. Block **5,189,885**, index 3 of 3, epoch 314 / slot 10,864, 5.5 KB (34.4% of the limit), ex-units 3.7M mem · 1.2B steps (21.35% / 12.37% of budget), 1 signatory | [`09`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/09-cardanoscan-refinance-overview.png) |
| 10 | **Portfolio** | the refinanced Fluid loan is replaced by a Dano loan | Total value **$1,338.87**; **fUSDM · Dano Borrow −13 (−$13.00)**; **fUSDM · Dano Supply 100** (Journey 1's collateral); the only remaining **Fluid · Borrow is −2**, the untouched second loan | [`10`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/10-portfolio-after-both-refinances.png) |
| 11 | **Loan Details → Repay Loan** *(the Repay journey)* | the amount pre-fills to the full debt; the preview says what repaying will do | *Repay Loan · Dano Finance* — *"Fully repay your loan to unlock all collateral… Minimum amount to repay is 10.001 fUSDM."* **You Repay 13.000218 fUSDM** ($13.00), available 139.29; collateral **ADA 100** ($30.50); *Loan Impact* — Current Debt **13 fUSDM** → New Debt **↘ Repaid**, collateral **No change**, health factor **1.87 Healthy → --** | [`11`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/11-repay-quote-dano-13-fUSDM.png) |
| 12 | **Confirm** → sign in Eternl | one transaction, memo *"Dano Finance: Repay Loan"*, the debt leaving and the collateral returning | Eternl: 1 transaction, tags **Sent Tokens · Contract · Burn · Memo**, memo **Dano Finance: Repay Loan**, wallet net **+t₳97.58**, **fUSDM −13,000,218** paid from the wallet, token `7b8a46d8…5d469b70` **−1** — the same token this wallet received from the refinance at step 8 | [`12`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/12-eternl-sign-repay.png) |
| 13 | **Cardanoscan** — the repayment as a third party sees it | the loan UTxO consumed, the collateral released, the debt paid | tx `bae9a9c9…5b7d`, **block 5,190,611**, epoch 314 / slot 27,558, **3 in → 3 out**, fee **1.271961 ₳**, confirmed within 54 secs, **2 mints & burns**, metadata **label 674 · CIP-20** `{"msg": ["Dano Finance: Repay Loan"]}`. The loan contract `addr_test1zzx7xn…56t24n` sends **−100.0 ₳** and its loan token `904770e…4c5` **−1**; the pool receives **fUSDM +12,999,665** and the fee address **fUSDM +553**; the borrower pays **fUSDM −13,000,218**, burns `3e8aa6c…74d` **−1** and receives **+97.577269 ₳** | [`13`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/screenshots/journey-2-borrow-USDM-collateral-ADA/13-cardanoscan-repay-metadata.png) |

**Refinance transaction:** https://preprod.cardanoscan.io/transaction/78d434d5d3028e2f8025f9ad06ad65849cee4dcbd89d6abd206334baaa2495d6

**Repay transaction:** https://preprod.cardanoscan.io/transaction/bae9a9c9e0e4f63657d43cc21b4ab071e82d57919733ddb8908a958937c75b7d

One loan, followed from origination to settlement: opened on Fluid at 04:59:12, refinanced into Dano
at 05:01:04, and repaid in full later the same day — the loan UTxO the refinance created consumed,
its loan token burned, and all 100 ADA of collateral released.

---

## 2. Detailed verification (QC) — what was quoted against what settled

Sections 2.1–2.6 check the refinance `78d434d5…95d6`; section 2.7 checks the repayment
`bae9a9c9…5b7d` that closed the same loan. Each row is backed by a screenshot from this session,
with the on-chain column taken from Cardanoscan's own rendering.

### 2.1 Preview values (UI correctness)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-01 | Savings label = net-cost delta | 4.00% − (−5.86%) = **9.86%** → "Save 9.86% net cost" | "Dano save 9.86% net cost" on the row; "Save 9.86% net cost" on the card | `04`, `05` | ✅ |
| UAT2-TC-02 | Net cost before → after | 4.00% → −5.86% (the Dano position earns more than it costs) | 4.00% → −5.86% | `05` | ✅ |
| UAT2-TC-03 | Health factor before → after | 3.63 Healthy → 1.87 Healthy | 3.63 Healthy → 1.87 Healthy | `05` | ✅ |
| UAT2-TC-04 | Origination fee shown, in the borrowed asset | 2 fUSDM | Fee 2 fUSDM | `05` | ✅ |
| UAT2-TC-05 | "Same loan, Same collateral" and the no-extra-funds note | both shown | both shown | `05` | ✅ |

### 2.2 Signing & submission
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-06 | Confirm builds **one** transaction and opens the wallet | 1 transaction, memo "Dano Finance: Create Loan" | Eternl shows 1 transaction, memo *Dano Finance: Create Loan*, tagged Mint **and** Burn | `06` | ✅ |
| UAT2-TC-07 | The transaction in the wallet history is the one on the explorer | same memo, same amounts, same moment | wallet: 05:01:04 *Create Loan* −t₳4.7 · explorer: `78d434d5…95d6`, borrower net −4.70041 ₳ | `08`, `09` | ✅ |
| UAT2-TC-08 | Included in a block, accepted by the validators | one block, script budget within limits | **block 5,189,885**, index 3 of 3, ex-units 21.35% / 12.37% of budget | `09` | ✅ |
| UAT2-TC-09 | Built by this application | Danogo metadata present | 1 metadata entry, memo *Dano Finance: Create Loan* | `06`, `09` | ✅ |

### 2.3 Inputs consumed (source side)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-10 | The Fluid loan UTxO is consumed with its collateral | 100 ADA leaves the Fluid script | Plutus V3 input `addr_test1zpyg4esucs…n47ypv` **−t₳100**; **ADA Sent −100.0** | `06`, `09` | ✅ |
| UAT2-TC-11 | The Dano pool funds the transaction | the pool disburses the borrowed asset | `addr_test1wpuxsu…5z3psh` sends **fUSDM −13,000,007** | `09` | ✅ |
| UAT2-TC-12 | The Fluid debt is **not** paid from the borrower's own funds | the settlement is funded by the pool | the borrower's fUSDM balance **increases** by 0.000006 in this transaction; their only ADA outlay is **4.70041 ₳**, of which **1.58428 ₳** is the network fee, and no leg of the transaction pays Dano in ADA | `08`, `09` | ✅ |

### 2.4 Outputs created (target side)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-13 | A new Dano loan UTxO is created holding the collateral | 100 ADA arrives at the Dano side with a new loan token | **ADA Received 100.0**, **Tokens Received `904770e…4c5` +1** | `09` | ✅ |
| UAT2-TC-14 | The Fluid debt is settled in the same transaction | the lender receives the full debt | **fUSDM +11,000,001** to `addr_test1qr9ew225…s2n6em` | `06`, `09` | ✅ |
| UAT2-TC-15 | The origination fee is a separate leg, in the borrowed asset | exactly 2.000000 fUSDM to the fee address | **fUSDM +2,000,000** | `09` | ✅ |
| UAT2-TC-16 | The source position token leaves the Fluid script and a new one is minted | one token out, one token in | **`8dcf151…cf6` −1** out, **`904770e…4c5` +1** in, **3 mints & burns** in the transaction | `09` | ✅ |

### 2.5 Conservation & "the number shown is the number that settled"
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-17 | **Collateral continuity** | ADA in = ADA out to the Dano loan; not returned to the borrower | 100.0 ₳ out of Fluid = 100.0 ₳ into Dano | `09` | ✅ |
| UAT2-TC-18 | **Atomicity** | one transaction, one block, 3 in → 6 out | 3 → 6, block 5,189,885, total output 1,164.948262 ₳ | `09` | ✅ |
| UAT2-TC-19 | The pool disbursed exactly what the loan is for | disbursement = settlement + fee (+ dust) | **13.000007 = 11.000001 + 2.000000 + 0.000006** fUSDM | `09` | ✅ |
| UAT2-TC-20 | Quoted fee = fee paid | 2 fUSDM quoted → 2.000000 fUSDM on chain | 2 fUSDM (`05`) = +2,000,000 (`09`) | `05`, `09` | ✅ |
| UAT2-TC-21 | Quoted health factor = health factor after | 1.87 Healthy → 1.87 Healthy | 1.87 Healthy quoted, 1.87 Healthy on the settled loan | `05`, `07` | ✅ |
| UAT2-TC-22 | Resulting debt = debt + fee | 11 + 2 = **13 fUSDM** | 13 fUSDM in Loan Details and in Portfolio | `07`, `10` | ✅ |
| UAT2-TC-23 | Network fee separate from the origination fee | ADA network fee vs fUSDM origination fee | 1.58428 ₳ network (281 lovelace/byte) vs 2.000000 fUSDM origination | `09` | ✅ |

### 2.6 Post-state
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-24 | The refinanced Fluid position is settled | it no longer appears as a Fluid borrow | after: **fUSDM · Dano Borrow −13**; no Fluid borrow of 11 remains | `10` | ✅ |
| UAT2-TC-25 | Only the selected loan is touched | the second, unrelated 2 fUSDM Fluid loan is untouched | it is still listed, still **Fluid · Borrow −2** | `04`, `10` | ✅ |
| UAT2-TC-26 | The APR moves to the rate the quote named | the fUSDM pool's 3.07% | Loan Details after: **APR 3.07%**, the rate the Borrow Market listed for that pool | `01`, `07` | ✅ |
| UAT2-TC-27 | The collateral is the same collateral | 100 ADA, unchanged, never returned in between | 100 ADA on the Dano loan; no ADA returned to the wallet in the transaction | `07`, `09` | ✅ |

### 2.7 The repayment that closed the loan
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| UAT2-TC-28 | Quoted repay amount = amount actually paid | 13.000218 fUSDM quoted → the same, split between pool and fee address | wallet pays **fUSDM −13,000,218**; pool **+12,999,665** + fee address **+553** = **13,000,218** exactly | `11`, `12`, `13` | ✅ |
| UAT2-TC-29 | Full repayment releases **all** the collateral | 100 ADA returns to the borrower, less the transaction's own costs | loan contract sends **−100.0 ₳**; borrower receives **+97.577269 ₳** = 100 − 1.271961 network fee − 1.150770 to the fee address | `13` | ✅ |
| UAT2-TC-30 | The UTxO consumed is the one the refinance created | same address, same loan token the refinance minted | `addr_test1zzx7xn…56t24n` with token **`904770e…4c5`**, the token that arrived there in `78d434d5…` | `09`, `13` | ✅ |
| UAT2-TC-31 | The borrower's title to the loan is burned | the token the refinance paid to the wallet is destroyed | Eternl shows **`7b8a46d8…5d469b70` −1**, the token it showed +1 at step 8; Cardanoscan shows the borrower sending **`3e8aa6c…74d` −1**, the token they received in the refinance; **2 mints & burns** | `08`, `09`, `12`, `13` | ✅ |
| UAT2-TC-32 | Built by this application | metadata label 674 naming the action | label **674 · CIP-20** — `{"msg": ["Dano Finance: Repay Loan"]}` | `13` | ✅ |
| UAT2-TC-33 | Accepted and settled | one block, inputs balance outputs | **block 5,190,611**, 3 in → 3 out, total output 1,137.307193 ₳, confirmed within 54 secs | `13` | ✅ |

**33 checks, 33 hold** — 27 on the refinance, 6 on the repayment.

*Not captured:* no screenshot of the portfolio **after** the repayment, so this report does not show
the app's post-repay state; what it shows is the quote, the signature, and the settled transaction.

---

## 3. What this session covers, and what it does not

| Journey | Covered here | How |
|---|---|---|
| **Open** | ✅ end to end | steps 1–3 |
| **View** | ✅ end to end | step 4 (two loans listed) and steps 7, 10 |
| **Refinance** | ✅ end to end | steps 5–10, with the transaction on a public explorer |
| **Repay** | ✅ end to end, on the loan this session created | steps 11–13: the *Repay Loan* quote, the wallet dialog burning the loan token, and the settled transaction on a public explorer. Journey 1's loan was repaid the same way — [`06_UAT_Report_Journey_1_M2.md`](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md) §1 steps 11–13. The repay journey's **mainnet** evidence is in [`05` §5](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance) |

---

## 4. Stability, and two open items

| Check | Result |
|---|---|
| No crash, white screen or unrecoverable state during the session | ✅ none |
| Every value the interface displayed matched the chain — §2.5 | ✅ fee, health factor, debt, collateral and disbursement all match |
| Wallet signing worked on every signature | ✅ all three signed and submitted, all three confirmed |
| Whether an independent user finds this interface intuitive | **not tested — see the note at the top of this report** |

**F-1 · Open item — the *Fee* line in the Borrow Market preview is not reconciled.** At step 1 the
*Loan Impact* block quoted **Deposit 5 ADA** and **Fee 5 fUSDM ($5.00)**. The wallet then received
**fUSDM +11,000,000** — the full 11 fUSDM borrowed, with no fUSDM fee deducted — and the debt the app
showed afterwards was **11 fUSDM**, not 16. So whatever those two lines describe, it is not an amount
deducted at open or capitalised into this loan. Recorded as an open item, not as a pass; the same
line is queried in [Journey 1](https://github.com/tdnguyenND/rolling-loan-m2-resubmission-evidence/blob/main/06_UAT_Report_Journey_1_M2.md) F-1.

**F-2 · Open item — which token is burned.** The position token minted to the wallet at open,
`2d3883b3…70b67d8a`, appears in the refinance **as an input from the Fluid script (−1) and again in
an output back to the borrower's wallet (+1)** (`06`). The tokens Cardanoscan shows moving across
the refinance are **`8dcf151…cf6` out** and **`904770e…4c5` in** (`09`), with 3 mints & burns in the
transaction. This session's captures therefore do **not** support the statement that the token minted
at open is the token burned by the refinance; that statement should be checked against the
transaction's *Mints & Burns* tab before it is relied on anywhere.

---

## 5. Conclusion

**All four approved journeys were completed end to end on a single loan**, in the reverse asset
direction to Journey 1, through the Eternl-connected front end, screen by screen. The refinance settled in one transaction on preprod: 100 ADA of collateral moved from the
Fluid script to the Dano loan contract, the pool disbursed 13.000007 fUSDM, of which 11.000001
settled the Fluid debt and exactly 2.000000 went to the fee address — the same 2 fUSDM fee, the same
1.87 health factor and the same 13 fUSDM of debt the interface had quoted before the signature, and
the second, unrelated Fluid loan was left untouched. The repayment then consumed exactly that loan
UTxO, burned both the loan token and the borrower's title to it, paid 13.000218 fUSDM against the
13.000218 quoted — to the unit — and released all 100 ADA of collateral. Thirty-three QC checks
hold; two items (F-1, F-2) are recorded as unreconciled rather than as passes. No usability verdict is drawn from this
session.
