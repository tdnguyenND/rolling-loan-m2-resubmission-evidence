# On-Chain Transaction Evidence, and the Repay Journey

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"
**Network** Cardano mainnet · **Explorer** https://cardanoscan.io/
**Independent data source used for every figure below** Koios public API (`api.koios.rest`) — not
our own indexer, not our own backend. Repayment status additionally from Fluid Tokens' own public
API (§5.3).

**§1–§4** are the five refinance transactions: the ledger, the invariants that hold across all of
them, the value flow of each, and what the five together cover that one would not. **§5** is the
*repay* journey, which the previous submission covered only implicitly — delivered in two forms,
evidenced six times on mainnet, and confirmed by the protocol that was owed the money.

The front-end half of the evidence is in [`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md).

---

## Why mainnet and not testnet

Fluid does not deploy its smart contracts to any Cardano testnet. There is therefore no test network
on which a Fluid → Dano refinance can be executed at all. Mainnet is not a shortcut here; it is the
only environment where this cross-protocol path exists — and it happens to be the stronger evidence,
because every transaction below is public and immutable on a ledger neither protocol controls.

---

## 1. The five refinance transactions

Five independent mainnet transactions, produced on five different occasions, from **two different
signing wallets**, across **three different collateral assets**, **two different borrowed assets**,
**two different Dano liquidity sources**, and **three different origination-fee configurations**.
They are five executions of the same production code path under materially different conditions.

| # | Tx hash | Block | Timestamp (UTC) | Network fee | Signer wallet | Collateral carried | Borrowed asset | Dano liquidity source | Origination fee |
|---|---|---|---|---|---|---|---|---|---|
| TX‑01 | [`88579a30…6652`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 13,827,598 | 2026‑08‑19 03:58:23 | 1.560138 ₳ | **W1** | **USDM 10.000000** | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑02 | [`d240fab1…d84c`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 13,848,828 | 2026‑08‑24 04:37:17 | 2.669674 ₳ | **W1** | **SNEK 20,979** | ADA | **Staking (fixed‑term) contract** | 0.969750 ₳ |
| TX‑03 | [`1cf8f08b…4f10`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 13,852,647 | 2026‑08‑25 02:34:45 | 1.551873 ₳ | **W2** | **DJED 6.000000** | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑04 | [`c426d9fa…25c8c`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 13,853,825 | 2026‑08‑25 09:06:41 | 1.555760 ₳ | **W2** | **DJED 6.000000** | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑05 | [`0e26cc58…05b8`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 13,861,168 | 2026‑08‑27 02:31:36 | 1.538854 ₳ | **W2** | **DJED 10.000000** | **STRIKE** | Flexible Pool | **none** |

The two borrower wallets, identified here by the **Fluid loan script address** each one's stake
credential produces — the script portion is protocol-owned, the delegation portion is the
borrower's, which is why the two differ while the payment credential is identical:

- **W1** `addr1z9dth23wk9mm2ars073kzl35xc5463wh090qsarz822sfkk7lqahdkjjknfuxdj9kevvyqmlu3zyx3x547dqw2pevx0sewx5g2` … TX‑01, TX‑02
- **W2** `addr1z9dth23wk9mm2ars073kzl35xc5463wh090qsarz822sfk39xk00fdnqnawyvkcs43kmt7hv4uqwetw9yd6lkjl5vxhs279pxf` … TX‑03, TX‑04, TX‑05

Their base (wallet) addresses, as shown connected in the application, are in
[`04_USER_JOURNEYS_AND_APP_STATE.md` §2.2](./04_USER_JOURNEYS_AND_APP_STATE.md#22-the-wallets-in-the-screenshots-are-the-wallets-that-signed).

Screenshots of each transaction on Cardanoscan are in
[`screenshots/cardanoscan/`](./screenshots/cardanoscan/) (timestamps in those screenshots render in
UTC+7 — the table above is normalised to UTC).

---

## 2. Facts that hold for all five transactions

Each of these was read directly from Koios `tx_info`; none is a UI claim.

| # | Invariant | Verified value | What it proves |
|---|---|---|---|
| INV‑1 | Every Plutus script execution succeeded | `valid_contract = true` on **all** script witnesses (7, 12, 7, 7, 7 respectively) | The live deployed validators of *both* protocols accepted the transaction. Nothing was simulated. |
| INV‑2 | Transaction metadata label 674 | `{"674": {"msg": ["Dano Finance: Create Loan"]}}` | The transaction was produced by the Danogo back-end tx-builder, not hand-crafted. |
| INV‑3 | The source **Fluid loan UTxO is consumed** | A script input at the Fluid loan address is spent in every tx | The existing debt position is closed, not left open. |
| INV‑4 | The source **Fluid loan position NFT is burned** (`−1`) | TX‑01 `asset128rrfu48…`, TX‑02 `asset12nvvpmrv…`, TX‑03 `asset1zxnfclhy…`, TX‑04 `asset16hjjsk7m…`, TX‑05 `asset1hc8hc7kt…` | **This is the repayment proof.** A Fluid loan's identity token can only be burned when the loan is settled in full. After the transaction the loan does not exist on-chain. |
| INV‑5 | A **new Dano loan position NFT is minted** (`+1`) | TX‑01/03/04 `asset1pr26rn8r…`, TX‑02 `asset1hwst3ac0…`, TX‑05 `asset1nghq6njh…` | A new Dano loan is originated in the same transaction. |
| INV‑6 | **Collateral continuity is exact** | in = out, to the smallest unit, in all five (see §3) | The collateral is never returned to the borrower and never re-deposited by them. It moves protocol-to-protocol inside one transaction. |
| INV‑7 | **Atomicity** | Single tx hash, single block, one balanced input/output set | Close-and-reopen cannot partially fail. There is no window in which the user is unhedged or double-borrowed. |
| INV‑8 | **No borrower top-up** | The borrower's own wallet contributes only ADA for the network fee and min-UTxO; the settlement leg is funded by the Dano pool | The user does not need capital on hand to repay their Fluid loan. |

---

## 3. Per-transaction value flow

Net deltas, from Koios. Legend — `FLUID` = Fluid loan script address · `POOL` = Dano Flexible Pool
contract `addr1wx2degj2ru0uctl4rnvs7vh5l608smvxrgkm7lf8txxjd6qs43szs` · `STAKING` = Dano Staking
(fixed-term) contract `addr1xxt4n07cnlafzefqvne69mmxmnzu2t9gtd27jw9d9yvc7u5htxla38l6j9j` ·
`LOAN` = Dano Flexible Loan contract (`addr1zxk23ccxak37kmp94qutawkr0kffcgt24vfu34rrljj6pr…`) ·
`FEE` = Dano origination-fee address
`addr1qywadgaxcnh993zpzl5kfs806nqe7jyxp4e8unjpll5quymw2aappdz98na` · `SETTLE` = the Fluid loan's
settlement recipient (a distinct address per loan).

### TX‑01 — `88579a30…6652` · USDM collateral, ADA borrow, fee pool

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.288610 ₳ · **−1 Fluid loan NFT** `asset128rrfu48…` · **−10.000000 USDM** |
| `POOL` | −22.000857 ₳ (loan disbursement) |
| `SETTLE` `addr1q9mt6pcx…` | **+20.000851 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ (Dano origination fee) |
| `LOAN` (out) | +5.000000 ₳ (min-UTxO) · **+1 Dano loan NFT** `asset1pr26rn8r…` (minted) · **+10.000000 USDM** |
| Borrower wallet | −4.271522 ₳ net (network fee + min-UTxO movement) · +1 Borrower NFT `asset1cf3ey4y9…` (minted) |

Gross-up check (spec `BorrowModify.Fluid.md §7.15`): `20.000851 + 2.000000 = 22.000851`, against a
pool disbursement of `22.000857` — a 6-lovelace min-UTxO residual. ✅ **The origination fee is
capitalised into the new loan**: the pool lends both the settlement amount and the fee, so a 20 ADA
Fluid debt becomes a 22 ADA Dano loan and the borrower funds nothing beyond the network fee.

### TX‑02 — `d240fab1…d84c` · SNEK collateral, ADA borrow, fixed-term liquidity source

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.249820 ₳ · **−1 Fluid loan NFT** `asset12nvvpmrv…` · **−20,979 SNEK** |
| `STAKING` | −15.015024 ₳ (loan disbursement — a *different* Dano product than TX‑01/03/04/05) |
| `SETTLE` `addr1qxukdu6h…` | **+15.000002 ₳ — the Fluid debt, settled** |
| `FEE` | +0.969750 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1hwst3ac0…` (minted) · **+20,979 SNEK** |
| Pool accounting | dADA pool token burned −14,723,905; auxiliary market token −14,602,941 |
| Borrower wallet | −6.374582 ₳ net · +1 Borrower NFT `asset18980lwqk…` (minted) |

12 Plutus scripts executed, all `valid_contract = true` — the most complex of the five.

### TX‑03 — `1cf8f08b…4f10` · DJED collateral, ADA borrow, fee pool

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.318780 ₳ · **−1 Fluid loan NFT** `asset1zxnfclhy…` · **−6.000000 DJED** |
| `POOL` | −13.000010 ₳ |
| `SETTLE` `addr1qxe843xq…` | **+11.000005 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1pr26rn8r…` (minted) · **+6.000000 DJED** |

Gross-up check: `11.000005 + 2.000000 = 13.000005`, against a pool disbursement of `13.000010` — a
5-lovelace min-UTxO residual. ✅ Same capitalised fee as TX‑01.

### TX‑04 — `c426d9fa…25c8c` · DJED collateral, ADA borrow, fee pool — the UI-matched transaction

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.318780 ₳ · **−1 Fluid loan NFT** `asset16hjjsk7m…` · **−6.000000 DJED** |
| `POOL` | −14.000108 ₳ |
| `SETTLE` `addr1qyh4agxd…` | **+12.000102 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1pr26rn8r…` (minted) · **+6.000000 DJED** |

**This transaction is the one whose front-end preview was screenshotted before signing**
([`01_Integration_Test_Report_M2.md`](./01_Integration_Test_Report_M2.md) §1).
The preview stated: Fluid debt 12 ADA · origination fee 2 ADA · collateral DJED 6 · "Same loan, Same
collateral". On-chain: settlement leg 12.000102 ADA, fee leg exactly 2.000000 ADA, DJED 6.000000
carried, pool disbursement 14.000108 ADA. **The number the user was shown is the number that
settled.** This is the strongest single answer to "no data mismatch".

Before / after, as the loan detail and the Eternl tx inspector showed it:

| Field | Before (Fluid loan) | After (Dano Float loan) |
|---|---|---|
| Borrowed token | ADA | ADA (Dano Float) |
| Total debt | $2.64 (12 ADA) | Dano Borrow ~$5.94 in Portfolio after (Fluid position gone) |
| Net cost | 4.00% | −0.56% (Save 4.56%) |
| Health Factor | 1.77 (Healthy) | 1.52 (Fair) |
| Collateral | DJED 6 (~$5.86) | DJED 6 (carried across) |
| Origination fee | — | 2 ADA |
| Protocol / script | Fluid (Plutus V3) | Dano Float / DanoFlex (Plutus V3) |

Evidence screenshots: [`02-refinance-card.png`](./screenshots/02-refinance-card.png) (before +
preview), [`03-eternl-inputs-outputs.png`](./screenshots/03-eternl-inputs-outputs.png) (on-chain
inputs/outputs), [`04-transaction-confirmed.png`](./screenshots/04-transaction-confirmed.png)
(confirmation), [`05-cardanoscan.png`](./screenshots/05-cardanoscan.png) (public explorer: Pool
Contract → Loan Contract, DJED moved),
[`06-portfolio-after.png`](./screenshots/06-portfolio-after.png) (Fluid settled, position now on
Dano).

### TX‑05 — `0e26cc58…05b8` · DJED collateral, STRIKE borrow, zero-fee pool

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.650650 ₳ · **−1 Fluid loan NFT** `asset1hc8hc7kt…` · **−10.000000 DJED** |
| `POOL` | **−5.000560 STRIKE** (non-ADA disbursement) |
| `SETTLE` `addr1q8ng3ndx…` | **+5.000558 STRIKE — the Fluid debt, settled** · +1.961050 ₳ min-UTxO |
| `FEE` | *(no leg — this pool charges no origination fee)* |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1nghq6njh…` (minted) · **+10.000000 DJED** |

This transaction proves two things the other four do not: the flow is **not ADA-specific**, and a
pool with **no origination fee** produces a borrow equal to the debt with no gross-up. **It is the
zero-fee example** — the previous submission cited `88579a30…` for this, which the chain
contradicts; see [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑1.

---

## 4. Coverage summary — why five, not one

| Dimension | Values exercised |
|---|---|
| Signing wallet | 2 (W1, W2) |
| Calendar days | 5 distinct occasions across 9 days (19 → 27 Aug 2026) |
| Collateral asset | USDM · SNEK · DJED |
| Borrowed asset | ADA · STRIKE |
| Dano liquidity source | Flexible Pool · Staking (fixed-term) contract |
| Origination fee configuration | 2.000000 ₳ flat-minimum · 0.969750 ₳ · zero |
| Plutus script executions per tx | 7 · 12 · 7 · 7 · 7 — **all valid** |
| Distinct Fluid loans closed | 5 (five different loan-position NFTs burned) |
| Distinct Dano loans opened | 5 (five different loan-position NFTs minted) |

---

## 5. The repay journey, in both of its forms

The previous submission said open and repay were "covered implicitly" by the refinance. That was
the reviewer's second objection, and it was fair. Repay is delivered twice, in two different places,
and both are evidenced.

| Form | Where it lives in the product | How the user reaches it | Evidence |
|---|---|---|---|
| **R‑A — Standalone repay** | `BorrowModify` sheet, `Repay` action | Portfolio / My Account → Loans → *Manage* → **Repay** | **One live mainnet repayment**, `17c23dde…`, debt paid from the borrower's own funds and collateral released — §5.5. Implemented for all four supported protocols, each with its own rules. |
| **R‑B — Repay as the settlement leg of a refinance** | The rolling-loan transaction itself | Loan Details → **Refinance via Dano** → *Confirm* | Five mainnet transactions in which a Fluid loan is repaid in full and its position token burned — §1, §2 (INV‑4). **Fluid's own dashboard marks all five `REPAID`** — §5.3. |

### 5.1 Why the rolling-loan model makes repay an atomic leg

A conventional refinance is three user actions and three risks:

```
  1. User finds capital to repay Fluid          →  needs money they do not have
  2. User repays Fluid, withdraws collateral    →  collateral sits unlevered in the wallet
  3. User deposits collateral to Dano, borrows  →  price may have moved; step 3 may fail
```

Between steps the user is exposed. The rolling-loan design collapses all three into **one Cardano
transaction**:

```
  ┌──────────────────────── one transaction, one block ────────────────────────┐
  │                                                                            │
  │  INPUT   Fluid loan UTxO  ──── carries the collateral + the position NFT   │
  │  INPUT   Dano pool UTxO   ──── supplies the new borrow                     │
  │                                                                            │
  │  OUTPUT  settlement leg   ──→  Fluid debt paid in full  ◀── THE REPAYMENT  │
  │  OUTPUT  Dano loan UTxO   ──→  new loan, same collateral                   │
  │  MINT    Fluid position NFT  −1   ◀── the loan ceases to exist             │
  │  MINT    Dano  position NFT  +1                                            │
  │                                                                            │
  └────────────────────────────────────────────────────────────────────────────┘
```

Either everything happens or nothing does. There is no state in which the Fluid loan is repaid but
the Dano loan failed to open. This is what the approved specification says, in its own language —
from `docs/screens/LendBorrow/BorrowModify.Fluid.md` §7.15, excerpted verbatim:

> **Submission.** One `TransactionLifecycle` handoff builds **one** transaction
> (**Fluid repay** + DanoFlex create loan). `onConfirmedTransaction` fires once.

and

> The user does not need extra funds beyond the network fee — the DanoFlex borrow amount is
> grossed up over the Fluid loan's remaining debt by the new loan's own origination fee […] so
> that net of that fee, the proceeds cover the remaining debt exactly.

The spec calls the leg "Fluid repay" because that is what it is.

### 5.2 The on-chain proof that a repayment occurred

Cardano gives an unambiguous, protocol-level signal, and it is present in **all five** transactions:
**the Fluid loan's position NFT is burned.** Fluid mints a unique loan-position token when a loan is
opened; that token is the loan's on-chain identity, and Fluid's validator permits it to be burned
**only when the loan is settled**. Once burned, the loan does not exist — it cannot be queried,
serviced, liquidated, or repaid again.

| Tx | Fluid position NFT burned | Debt settlement leg | Amount settled |
|---|---|---|---|
| TX‑01 `88579a30…` | `asset128rrfu48vwdclrdxe4hxqhq6hnd5q4h8glf7uq` `−1` | `addr1q9mt6pcx…` | **20.000851 ADA** |
| TX‑02 `d240fab1…` | `asset12nvvpmrv0qmeh9dz4xcnvx85lknrrzy9rh7uha` `−1` | `addr1qxukdu6h…` | **15.000002 ADA** |
| TX‑03 `1cf8f08b…` | `asset1zxnfclhy8tfde90aduttcpp20c7s8r67slv9vr` `−1` | `addr1qxe843xq…` | **11.000005 ADA** |
| TX‑04 `c426d9fa…` | `asset16hjjsk7mzedy20a27aqe5etg0a6v3g7cdlglac` `−1` | `addr1qyh4agxd…` | **12.000102 ADA** |
| TX‑05 `0e26cc58…` | `asset1hc8hc7kt4ksrx2ww55gw7l7g9kznsas6csadwx` `−1` | `addr1q8ng3ndx…` | **5.000558 STRIKE** |

Every burn was accepted by Fluid's own live mainnet validator (`valid_contract = true`). Fluid's
validator, not ours, is the authority that a repayment satisfied the loan — and it said yes, five
times. A partial repayment could not burn the token: the loan would still have to exist. Five burns
are five full settlements.

### 5.3 Fluid's own records say the loans are repaid

Everything else here is our reading of the chain. This is not: it is what **Fluid Tokens**, the
protocol whose loans were settled, records about them. Fluid is a separate company, a separate
protocol, and the counterparty whose money was owed.

Fluid publishes a borrower's loan history from its own indexer:

```
GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>&from=…&to=…
```

Queried for the two borrower wallets over this milestone's period it returns **six** events. Every
one is `"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0` —
and every one names a transaction this application built in its `finishingTxHash` field.

| Fluid's record | Its `finishingTxHash` | Transaction metadata | Journey |
|---|---|---|---|
| repaid · 20 ADA | `88579a30…146652` | *Dano Finance: Create Loan* | refinance |
| repaid · 5 USDCx | `17c23dde…e2559a` | ***Dano Finance: Repay Fluid Loan*** | **standalone repay** — §5.5 |
| repaid · 15 ADA | `d240fab1…dad84c` | *Dano Finance: Create Loan* | refinance |
| repaid · 11 ADA | `1cf8f08b…549f10` | *Dano Finance: Create Loan* | refinance |
| repaid · 12 ADA | `c426d9fa…d25c8c` | *Dano Finance: Create Loan* | refinance |
| repaid · 5 STRIKE | `0e26cc58…8b05b8` | *Dano Finance: Create Loan* | refinance |

**The reconciliation.** Four things tie Fluid's record to the Cardano ledger:

| | What it establishes |
|---|---|
| **F‑1** | Fluid itself reports the loan repaid, with `remainingDebt: 0` and no penalty. |
| **F‑2** | The loan token Fluid names in its `nft` field is **the exact token this transaction burned**. Fluid's identifier for the loan and our burn record carry the same 56-hex policy id and the same 56-hex asset name — this is what makes "Fluid's record" and "our transaction" provably the same loan. |
| **F‑3** | The transaction pays **at least** the total Fluid says was due. In every case the settlement output exceeds Fluid's own debt figure by 3–5 of the smallest unit — the transaction rounds up so the validator's check cannot fail. It never underpays. |
| **F‑4** | Which journey the transaction is, decided **from the chain**: a refinance consumes the Fluid loan *and* mints a Dano loan in the same transaction; the standalone repay mints no Dano loan and releases the collateral back to the borrower. |

Fluid's exact figures against the ledger:

| Tx | Fluid's `totalPaid` | Settlement output on chain | Excess |
|---|---|---|---|
| `88579a30…` | 20.000848 ADA | 20.000851 ADA | +3 |
| `17c23dde…` | 5.000007 USDCx | 5.000011 USDCx | +4 |
| `d240fab1…` | 14.999998 ADA | 15.000002 ADA | +4 |
| `1cf8f08b…` | 11.000000 ADA | 11.000005 ADA | +5 |
| `c426d9fa…` | 12.000098 ADA | 12.000102 ADA | +4 |
| `0e26cc58…` | 5.000555 STRIKE | 5.000558 STRIKE | +3 |

*(Fluid's `interestPaid` on `d240fab1…` is **−2** — a negative two-lovelace interest figure from
Fluid's own accounting, which is why its dashboard renders "−0.000 ADA". We reproduce it as Fluid
reports it rather than tidying it.)*

**The same thing, as a borrower sees it** — the five refinance loans in Fluid's own borrower
dashboard at `app.fluidtokens.com/dashboard`:

| Screenshot | Fluid shows | Its "Finishing TX" |
|---|---|---|
| [`repaid-01`](./screenshots/fluid-dashboard/repaid-01-88579a30.png) | **LOAN REPAID** · 20 ADA · Total Paid 20.001 ADA | `88579a30…146652` |
| [`repaid-02`](./screenshots/fluid-dashboard/repaid-02-d240fab1.png) | **LOAN REPAID** · 15 ADA · Total Paid 15.000 ADA | `d240fab1…dad84c` |
| [`repaid-03`](./screenshots/fluid-dashboard/repaid-03-1cf8f08b.png) | **LOAN REPAID** · 11 ADA · Total Paid 11.000 ADA | `1cf8f08b…549f10` |
| [`repaid-04`](./screenshots/fluid-dashboard/repaid-04-c426d9fa.png) | **LOAN REPAID** · 12 ADA · Total Paid 12.000 ADA | `c426d9fa…d25c8c` |
| [`repaid-05`](./screenshots/fluid-dashboard/repaid-05-0e26cc58.png) | **LOAN REPAID** · 5 STRIKE · Total Paid 5.001 STRIKE | `0e26cc58…8b05b8` |

The dashboard rounds `totalPaid` to three decimals and shows Fluid's event time — which the API
gives as 33–61 seconds after the transaction's block — in the browser's local zone, UTC+7 in these
captures. `0e26cc58…` settled in a block at `02:31:36Z`; Fluid timestamps its event at `02:32:18Z`;
the dashboard displays `09:32`. These are full-window captures, address bar included.

**One edit, disclosed:** a 25-pixel horizontal band of browser chrome — the bookmarks bar — was
removed from these five, because it showed the names of unrelated internal tools. Nothing else was
altered: the address bar, the page, and every figure on it are as captured.

Why this matters: **Fluid is the party that was owed the money.** It marks a loan repaid when its
own indexer sees its own validator accept the settlement. A loan transferred, rolled internally, or
liquidated would not appear as `loan_repaid` with `remainingDebt: 0` on the lender's side. And F‑2
makes Fluid's identifier for the loan and our burn record provably the same object, so a reviewer
does not have to believe either party.

### 5.4 Two further properties the evidence settles

**The user did not fund the repayment.** In every refinance the settlement leg is paid by the Dano
pool, not by the borrower's wallet; the borrower's net ADA change is the network fee and min-UTxO
movement only. On TX‑04 the pool disbursed 14.000108 ADA, of which 12.000102 ADA settled the Fluid
debt and exactly 2.000000 ADA paid the origination fee, while the borrower contributed 1.555760 ADA
of network fee and nothing else. **This is the whole point of the product**: you can repay a loan
you do not have the money to repay.

**The repayment is still true today.** All five Fluid position NFTs currently have a **total supply
of zero** across Cardano — they exist in no wallet and no UTxO. Correspondingly, when either
borrower opens the application, **no Fluid loan is listed** —
[`04_USER_JOURNEYS_AND_APP_STATE.md` §2.5](./04_USER_JOURNEYS_AND_APP_STATE.md#25-what-is-absent-from-the-screenshots-and-why-that-is-the-point).

### 5.5 The standalone repay journey, also on mainnet

> **`17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a`**
> 2026‑08‑24 04:16:26 UTC · wallet **W1** · four Plutus scripts, all `valid_contract = true`
> Metadata 674: **`"Dano Finance: Repay Fluid Loan"`** — written by this application
> [Cardanoscan](https://cardanoscan.io/transaction/17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a)

What the transaction does, from the chain:

| | |
|---|---|
| Fluid loan UTxO consumed | 50.000000 ADA of collateral + the position NFT |
| Debt paid | **USDCx 5.000011**, from the **borrower's own wallet** |
| Fluid position NFT | `asset14n98plzqqlsd9y4k6hh6p7zjzhmqrwnwvgt5gv` **burned** |
| Collateral | **returned to the borrower** — net **+47.497186 ADA** after the 0.546074 ADA network fee |
| Dano loan created | **none** |
| Fluid's own record | `loan_repaid`, `status: repaid`, `remainingDebt: 0`, `totalPaid: 5.000007` |

It is the exact mirror of the refinance case, and the contrast is the product:

| | Standalone repay `17c23dde…` | Refinance `c426d9fa…` |
|---|---|---|
| Who pays the debt | the borrower, from their own wallet | the Dano pool |
| What happens to the collateral | released back to the borrower | carried into the new loan, never touched by the borrower |
| What the borrower needs | the full amount owed | the network fee, and nothing else |
| Loans afterwards | none | one, at a new rate |

Both burn the Fluid position token. Both are reported by Fluid as `loan_repaid` with nothing left
owing. The repay journey is therefore evidenced on mainnet **in both of its forms**, six times in
total, each verified against the counterparty protocol's own records.

---
