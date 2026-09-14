> ### ⚠️ Superseded — this file is from the **previous** Milestone 2 submission
>
> It is kept at its original path so that links from the previous Proof of Achievement still
> resolve. It has **not** been edited, so anything it says that we later found to be wrong is
> still wrong here — deliberately.
>
> **Start at [`README.md`](./README.md)** for the resubmission.
> **[`08_CORRECTIONS.md`](./08_CORRECTIONS.md)** lists, with the on-chain arithmetic, every claim in
> this file that we have since corrected or withdrawn — including the usability row *"Tester completed the flow without external guidance ✅ Yes"*, which is **restated as internal testing** rather than as a verdict on usability. The resubmission also no longer offers test-suite figures as evidence at all; it rests on the Cardano ledger and on the records of the protocol whose loans were settled.

---

# Milestone 2 — Integration Test Report

**Feature:** Rolling Loan ("Refinance via Dano") — refinance a loan from **Fluid** into a
**Dano Finance (Dano Float)** loan.
**Environment:** staging front-end https://v3.danogo.io/, running against **live Cardano mainnet**
smart contracts. Signed with the **Eternl** wallet.
**Date:** 2026-08-25.

> **Why mainnet, not testnet.** Fluid does not provide a testnet deployment of its smart contracts,
> so the Fluid → Dano refinance flow can only be executed on mainnet — where all evidence below was
> produced and is publicly verifiable on Cardanoscan. (Public production golive on the main app is
> the next step; the staging app already uses the real mainnet contracts.)

---

## 1. Successful refinance — manual end-to-end journey (via Eternl)

A full refinance was executed end-to-end on the live app with the Eternl wallet — a real signed and
submitted transaction, settlement confirmed on-chain (`valid_contract: true`), tx `c426d9fa…d25c8c`.
Each step has a screenshot in `screenshots/`.

| # | Step | Expected | Actual result | Screenshot |
|---|---|---|---|---|
| 1 | Open the ADA loan (**ADA · Loans**) → **Manage** | Loan row with a "Dano save …% net cost" hint | ADA loan $2.6 (12 ADA), collateral $5.9, APR 4.00%, **"Dano save 4.56% net cost"**, HF 1.77 Healthy | [`01-loans-sheet.png`](./screenshots/01-loans-sheet.png) |
| 2 | **Loan Details** → expand **Refinance via Dano**, then **Confirm** & sign in Eternl | Net cost before→after, HF before→after, fee, "Same loan, Same collateral" | **Net cost (Fluid→Dano) 4.00% → −0.56%** (Save 4.56%), **HF 1.77 Healthy → 1.52 Fair**, **Fee 2 ADA**, "Same loan, Same collateral", collateral DJED 6 | [`02-refinance-card.png`](./screenshots/02-refinance-card.png) |
| 3 | Eternl **Inputs/Outputs** detail (before signing) | Fluid loan UTxO consumed; Dano Float pool updated | Label **Dano Finance: Create Loan**; inputs spend **DJED −6** (Fluid loan) + Float ADA Market Token/dADA; outputs create **Float ADA Market Token +1 + dADA** (Dano Float loan) and return **DJED +6** | [`03-eternl-inputs-outputs.png`](./screenshots/03-eternl-inputs-outputs.png) |
| 4 | **Transaction confirmed** | Success + tx hash | "Transaction confirmed" `c426d9fa4b…d25c8c` | [`04-transaction-confirmed.png`](./screenshots/04-transaction-confirmed.png) |
| 5 | Verify on **Cardanoscan** | Public on-chain proof of the routing | tx `c426d9fa…`; **From: Dano Finance Flexible Pool Contract** (dADA out/in) + the Fluid loan address (**DJED −6**); **To: Dano Finance Flexible Loan Contract** (**DJED +6**) — one block, 3 in → 6 out | [`05-cardanoscan.png`](./screenshots/05-cardanoscan.png) |
| 6 | **Portfolio after** refinance | Fluid loan gone; position now on Dano | Fluid Borrow no longer listed; a single **Dano Borrow −27 (−$5.94)** remains — the loan is settled and refinanced to Dano | [`06-portfolio-after.png`](./screenshots/06-portfolio-after.png) |

**Refinance tx:** https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c
· on-chain analysis → [`02_Mainnet_Transactions_M2.md`](./02_Mainnet_Transactions_M2.md).

A real Fluid loan was refinanced into a Dano Float loan in **one signed, on-chain-confirmed
transaction** — the Fluid loan is repaid and closed, a new Dano loan is opened, and the collateral
(DJED) is moved across, all atomically (no separate repayment, no upfront capital). This single
journey also demonstrates **view** (steps 1–2) and **open** (the Dano loan is originated in the
same tx), and the **Portfolio after** state confirms the Fluid position is settled.

---

## 2. Detailed transaction verification (QC) — inputs, outputs, assets, signing

Granular QC checks on the executed refinance transaction
`c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c`. Each check has an oracle and is
backed by a screenshot and/or public on-chain data (Cardanoscan / Blockfrost). All checks **PASS**.
Two real refinance transactions are referenced: a **fee pool** (tx `c426d9fa…`, the ADA pool
charging a 2 ADA origination fee) and a **no-fee pool** (tx `88579a30…`).

### 2.1 Preview values (UI correctness)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-01 | Savings label = net-cost delta | 4.00% − (−0.56%) = **4.56%** → "Save 4.56% net cost" | "Dano save 4.56% net cost" / "Save 4.56% net cost" | `01`, `02` | ✅ |
| TC-02 | Net cost before → after | source 4.00% → Dano −0.56% | 4.00% → −0.56% shown | `02` | ✅ |
| TC-03 | Health factor before → after | 1.77 (Healthy) → 1.52 (Fair), ≥ pool minimum | 1.77 Healthy → 1.52 Fair | `02` | ✅ |
| TC-04 | Origination fee shown | 2 ADA | Fee 2 ADA | `02` | ✅ |
| TC-05 | "Same loan, Same collateral" note | shown (collateral unchanged, no extra funds) | shown | `02` | ✅ |

### 2.2 Signing & submission
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-06 | Confirm builds tx + opens Eternl prompt | one tx, label "Dano Finance: Create Loan" | Eternl shows 1 tx, "Dano Finance: Create Loan" | `03` | ✅ |
| TC-07 | Transaction **signed & submitted** successfully | no error → "Transaction confirmed" + hash | "Transaction confirmed" `c426d9fa…d25c8c` | `04` | ✅ |
| TC-08 | Returned hash resolves on explorer | Cardanoscan shows the same tx | tx `c426d9fa…` present on Cardanoscan | `05` | ✅ |
| TC-09 | Smart-contract validation on-chain | `valid_contract = true` | `valid_contract: true` (Blockfrost) | Blockfrost | ✅ |

### 2.3 Inputs consumed (source side)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-10 | Fluid loan UTxO consumed with its collateral | input spends **DJED −6** from the Fluid loan address | DJED −6 spent from the Fluid loan (Plutus V3) input | `03`, `05` | ✅ |
| TC-11 | Dano Float pool UTxO consumed | input spends Float ADA Market Token −1 + **dADA −10,514,011.394364** | Pool Contract input: dADA −10,514,011.394364, Float ADA Market Token −1 | `03`, `05` | ✅ |
| TC-12 | No borrower top-up capital | Fluid debt repaid from the new Dano loan, not from user funds (only fee/min-UTxO from wallet) | wallet input contributes ADA for fee/min-UTxO only; no collateral top-up | `03`, `05` | ✅ |

### 2.4 Outputs created (target side)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-13 | New Dano loan UTxO created | output at **Dano Finance Flexible Loan Contract** holds ADA + **DJED +6** + a loan NFT | To Loan Contract: ADA 5.0, **DJED +6**, loan NFT `08d5a1c…398 +1` | `05` | ✅ |
| TC-14 | Dano Float pool updated | pool output returns Float ADA Market Token +1 + **dADA +10,514,011.394364** | Pool output: Float ADA Market Token +1, dADA +10,514,011.394364 | `03`, `05` | ✅ |
| TC-15 | Borrowed asset unchanged | borrowed token stays **ADA** (no asset swap) | ADA loan before and after | `02`, `06` | ✅ |

### 2.5 Asset conservation & correctness (input ↔ output)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-16 | **Collateral continuity** | DJED in (6) = DJED out to Dano loan (6); collateral **not** returned to borrower | DJED −6 (in) = DJED +6 (out to Loan Contract) | `03`, `05` | ✅ |
| TC-17 | **Pool token conservation** | dADA sent = dADA received; Float ADA Market Token −1 = +1 | dADA −10,514,011.394364 = +10,514,011.394364; market token −1 = +1 | `05` | ✅ |
| TC-18 | **Atomicity** | one transaction, one block, inputs balance outputs | single tx, one block (13,853,825); inputs balanced to outputs | `05`, Blockfrost | ✅ |
| TC-19 | Network fee reasonable | small network fee only | fee ~1.56 ADA (1,555,760 lovelace) | `05`, Blockfrost | ✅ |

### 2.6 Post-state (Portfolio after)
| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-20 | Fluid position settled | Fluid Borrow no longer listed in Portfolio | Fluid Borrow absent after refinance | `06` | ✅ |
| TC-21 | Dano position present | a Dano Borrow position exists (refinanced) | Dano Borrow −27 (−$5.94) | `06` | ✅ |

### 2.7 Fee calculation (origination fee & network fee)

Two fees apply: (a) the **Dano pool loan-origination fee**, financed into the new loan; (b) the
**Cardano network fee**, paid from the wallet. The origination fee is **per-pool config** — the ADA
pool charges **0.1% of the borrowed amount, minimum 2 ADA**; some pools charge **0**. TC-22/23/25
use the fee pool (tx `c426d9fa…`); TC-24 uses the no-fee pool (tx `88579a30…`).

| TC | Check | Expected (oracle) | Actual (evidence) | Source | Result |
|---|---|---|---|---|---|
| TC-22 | Origination fee rate/min (ADA pool) | fee = max(0.1% × borrow, 2 ADA) = max(0.1% × 12, 2) = **2 ADA** | card shows **Fee 2 ADA** | `02` | ✅ |
| TC-23 | Loan bumped to cover the fee, financed by the loan (no out-of-pocket) | new Dano borrow = Fluid debt + origination fee = 12 + 2 = **14 ADA**; borrower adds no extra funds ("no upfront capital") | Portfolio after: Dano Borrow **−27** = existing Dano 13 + new 14 (27 × $0.2198 = $5.94); no wallet top-up for the fee — loan bumped instead | `02`, `03`, `06` | ✅ |
| TC-24 | No-origination-fee pool → borrow = exact Fluid debt | for a pool with 0 origination fee, new loan borrow = the Fluid debt exactly (no +fee bump) | separate real refinance to a **no-fee pool** — tx `88579a30…` (`valid_contract: true`), collateral (10 USDM) carried across, no origination-fee bump | tx `88579a30…` | ✅ |
| TC-25 | Network fee separate from origination fee | network fee ~1.56 ADA, distinct from the 2 ADA origination fee | tx fee 1,555,760 lovelace (~1.56 ADA) vs origination 2 ADA | `05`, Blockfrost | ✅ |

---

## 3. Refinance UI — automated integration tests (all passing)

The refinance surface is covered by 8 automated integration tests (Tier-2, connected wallet) in
`borrow-detail-connected.spec.ts`, run against **live mainnet data** on 2026-08-25 — **all 8 pass**
(dashboard `reports/playwright/250826-1553/`):

| Case | Verifies | Result |
|---|---|---|
| FN-I7 | Refinance via Dano CTA renders for an eligible Fluid loan | ✅ PASS |
| FN-I9 | Label shows the real saving when Dano is cheaper — **"Save 4.56% net cost"** | ✅ PASS |
| FN-I10 / FN-I11 | Label shows "+{delta} net cost" when Dano is dearer, never "Save +…" | ✅ PASS |
| FN-I12 | Label always carries digits + % + "net cost" | ✅ PASS |
| FN-I13 | Collapsed card renders below the secondary actions, positive accent tone | ✅ PASS |
| FN-I14 | Refinance action occupies the last footer slot | ✅ PASS |
| FN-J10 | Tapping the CTA expands the refinance preview in place | ✅ PASS |

These verify the front-end integration (CTA, net-cost / health-factor preview, fee, placement)
against real data. On-chain settlement is proven separately by the manual journey (§1) and the
mainnet transaction.

---

## 4. Stability & usability

| Check | Result |
|---|---|
| No crashes during the refinance journey (no app crash / white screen) | ✅ No crash |
| No data mismatch (UI values match the on-chain transaction) | ✅ Consistent (net cost, HF, collateral, fee) |
| Tester completed the flow without external guidance | ✅ Yes |
| Eternl signing worked across the flow | ✅ Sign + submit succeeded, tx confirmed |

---

## 5. Conclusion

The rolling-loan / Refinance-via-Dano feature is integrated end-to-end (front-end → back-end APIs →
on-chain contracts). A real Fluid loan was refinanced into a Dano Float loan successfully on Cardano
mainnet, the automated UI suite passes (8/8), and the interface was stable and intuitive throughout.
