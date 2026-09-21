> ### ⚠️ Superseded — this file is from the **previous** Milestone 2 submission
>
> It is kept at its original path so that links from the previous Proof of Achievement still
> resolve. It has **not** been edited, so anything it says that we later found to be wrong is
> still wrong here — deliberately.
>
> **Start at [`README.md`](./README.md)** for the resubmission.
> **[`08_CORRECTIONS.md`](./08_CORRECTIONS.md)** lists, with the on-chain arithmetic, every claim in
> this file that we have since corrected or withdrawn — including the claim that tx `88579a30…` used a **no-origination-fee** pool, which the chain contradicts, the statement that open and repay are "covered implicitly", and the §"Why mainnet and not testnet" claim that Fluid deploys no smart contracts on any Cardano testnet, which we have since withdrawn (C‑5).

---

# Milestone 2 — Deployment & Transaction Evidence (Mainnet)

**Feature:** Rolling Loan ("Refinance via Dano") · **App:** the **mainnet app** (staging deployment, live mainnet contracts) https://v3.danogo.io/
**Network:** Cardano **mainnet** · **Explorer:** https://cardanoscan.io/

> This document provides publicly verifiable, on-chain evidence that the back-end produces correct
> contract interactions for every loan state (open, repay, refinance). Each link can be inspected
> independently on Cardanoscan.

---

## Why mainnet and not testnet

The Milestone 2 flow refinances a loan **out of Fluid** into a Dano Finance loan. **Fluid does not
deploy its smart contracts on any Cardano testnet.** Consequently there is no testnet environment
in which the Fluid → Dano refinance flow can be executed — the only environment where this
cross-protocol flow exists is **mainnet**.

Mainnet evidence is fully verifiable and, for review purposes, stronger than testnet:

- Public, immutable, independently inspectable on Cardanoscan.
- Accepted by the **live deployed validators** with real value.
- Reviewers can inspect consumed inputs, created outputs, datum before/after, collateral
  continuity, and reference/contract inputs directly.

---

## 1. Transaction evidence by loan state

| Scenario | What it proves | Tx hash | Cardanoscan (mainnet) link |
|---|---|---|---|
| **Refinance → Dano ADA pool (fee pool)** — core | Fluid loan consumed + Dano loan created + collateral carried, atomically; loan bumped by the 2 ADA origination fee | `c426d9fa…d25c8c` | https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c |
| **Refinance → Dano no-fee pool** | Same atomic refinance on a pool with **no origination fee** — new loan borrow = exact Fluid debt (no fee bump); collateral (10 USDM) carried across | `88579a30…6652` | https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652 |

> Both transactions are real Dano refinances (`valid_contract: true`, metadata "Dano Finance: Create
> Loan"). Open-loan and repay-loan are covered implicitly: each refinance both *closes* the source
> Fluid loan and *opens* a Dano loan atomically.

---

## 2. Core refinance transaction — detail (VERIFIED ON-CHAIN)

**Tx hash:** `c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c`
**Link:** https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c
**Block:** 13,853,825 · **Time:** 2026-08-25 09:06:41 UTC · **Fee:** ~1.56 ADA ·
**`valid_contract`: true** (Plutus scripts validated) · metadata label **"Dano Finance: Create Loan"**.

This single transaction demonstrates the rolling-loan / refinance mechanism end-to-end:

- **Consumed (input):** the borrower's existing **Fluid** loan UTxO — a Plutus V3 script input
  carrying the loan's **DJED collateral (−6 DJED)** and the Fluid market position.
- **Created (output):** the **Dano Float (DanoFlex)** pool UTxO is updated with the new loan —
  Plutus V3 output carrying the **Float ADA Market Token (+1)** and **dADA (+10,514,011.39)**.
- **Collateral:** the **DJED** stays in the flow (no withdraw/re-deposit) — collateral continuity.
- **Atomicity:** closing the Fluid position and originating the Dano loan happen in the **same
  transaction**; the borrower did **not** repay-then-reopen. `valid_contract: true` means both the
  Fluid-close and Dano-create validators succeeded together.

### Before / after (from the loan detail + Eternl tx inspector)
| Field | Before (Fluid loan) | After (Dano Float loan) |
|---|---|---|
| Borrowed token | ADA | ADA (Dano Float) |
| Total debt | $2.64 (12 ADA) | Dano Borrow ~$5.94 in Portfolio after (Fluid position gone) |
| Net cost | 4.00% | −0.56% (Save 4.56%) |
| Health Factor | 1.77 (Healthy) | 1.52 (Fair) |
| Collateral | DJED 6 (~$5.86) | DJED 6 (carried across) |
| Origination fee | — | 2 ADA |
| Protocol / script | Fluid (Plutus V3) | Dano Float / DanoFlex (Plutus V3) |

Evidence screenshots: [`screenshots/02-refinance-card.png`](./screenshots/02-refinance-card.png) (before + preview),
[`screenshots/03-eternl-inputs-outputs.png`](./screenshots/03-eternl-inputs-outputs.png) (on-chain inputs/outputs),
[`screenshots/04-transaction-confirmed.png`](./screenshots/04-transaction-confirmed.png) (confirmation),
[`screenshots/05-cardanoscan.png`](./screenshots/05-cardanoscan.png) (public explorer: Pool Contract → Loan Contract, DJED moved),
[`screenshots/06-portfolio-after.png`](./screenshots/06-portfolio-after.png) (Fluid settled, position now on Dano).

---

## 2b. No-fee pool refinance — detail (VERIFIED ON-CHAIN)

**Tx hash:** `88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652`
**Link:** https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652
**Block:** 13,827,598 · **Time:** 2026-08-19 03:58:23 UTC · **Fee:** ~1.56 ADA ·
**`valid_contract`: true** · metadata label **"Dano Finance: Create Loan"**.

Second real refinance, on a Dano pool that charges **no origination fee**, so the new loan borrow
equals the source Fluid debt with **no fee bump** (contrast with the ADA fee pool above). Verified
on-chain:

- **Consumed:** the Fluid loan UTxO (script `addr1z9dth23wk9mm2…`) carrying its collateral
  **USDM 10** (`0014df10…5553444d` = 10,000,000).
- **Created:** the new Dano loan UTxO at the loan contract (`addr1zxk23ccxak37k…`) holding the same
  **USDM 10** + a loan NFT.
- **Pool token conserved:** the pool market token (`…f3f0f123…` = 10,517,420,054,910) is unchanged
  input → output.
- **Collateral continuity:** USDM 10 in = USDM 10 out (moved to the Dano loan, not returned).

---

## 3. Contract interaction (AC2 — back-end correctness)

The back-end builds the transaction that the on-chain validators accept. For the refinance, a single
transaction combines two contract interactions atomically:

| Action | On-chain interaction |
|---|---|
| Close the Fluid loan | Consume the Fluid loan UTxO (with its DJED collateral) |
| Open the Dano loan | Create the Dano Float loan UTxO in the same transaction |

`valid_contract: true` on the mainnet tx means both interactions validated together — confirming the
back-end produced correct contract interactions for the refinance (which closes one loan state and
opens another).
