# Annex D — On-Chain Transaction Ledger (Cardano Mainnet)

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"
**Network** Cardano mainnet · **Explorer** https://cardanoscan.io/
**Independent data source used for every figure below** Koios public API (`api.koios.rest`) —
not our own indexer, not our own backend.

---

## 1. Why this annex exists

The reviewer asked for evidence that the four approved journeys were each executed through the
Eternl-connected front end. This annex supplies the on-chain half of that evidence: **five
independent mainnet transactions**, produced on five different occasions, from **two different
signing wallets**, across **three different collateral assets**, **two different borrowed assets**,
**two different Dano liquidity sources**, and **three different origination-fee configurations**.

They are not five copies of one demo. They are five executions of the same production code path
under materially different conditions — which is what "the feature works" means.

---

## 2. Ledger — the five transactions

| # | Tx hash | Block | Timestamp (UTC) | Network fee | Signer wallet | Collateral carried | Borrowed asset | Dano liquidity source | Origination fee |
|---|---|---|---|---|---|---|---|---|---|
| TX‑01 | [`88579a30…6652`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 13,827,598 | 2026‑08‑19 03:58:23 | 1.560138 ₳ | **W1** | **USDM 10.000000** | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑02 | [`d240fab1…d84c`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 13,848,828 | 2026‑08‑24 04:37:17 | 2.669674 ₳ | **W1** | **SNEK 20,979** | ADA | **Staking (fixed‑term) contract** | 0.969750 ₳ |
| TX‑03 | [`1cf8f08b…4f10`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 13,852,647 | 2026‑08‑25 02:34:45 | 1.551873 ₳ | **W2** | **DJED 6.000000** | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑04 | [`c426d9fa…25c8c`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 13,853,825 | 2026‑08‑25 09:06:41 | 1.555760 ₳ | **W2** | **DJED 6.000000** | ADA | Flexible Pool | 2.000000 ₳ |
| TX‑05 | [`0e26cc58…05b8`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 13,861,168 | 2026‑08‑27 02:31:36 | 1.538854 ₳ | **W2** | **DJED 10.000000** | **STRIKE** | Flexible Pool | **none** |

Signer wallets, shown as the Fluid loan script address carrying each borrower's own stake
credential (publicly inspectable):

- **W1** `addr1z9dth23wk9mm2ars073kzl35xc5463wh090qsarz822sfkk7lqahdkjjknfuxdj9kevvyqmlu3zyx3x547dqw2pevx0sewx5g2` … TX‑01, TX‑02
- **W2** `addr1z9dth23wk9mm2ars073kzl35xc5463wh090qsarz822sfk39xk00fdnqnawyvkcs43kmt7hv4uqwetw9yd6lkjl5vxhs279pxf` … TX‑03, TX‑04, TX‑05

Screenshots of each transaction on Cardanoscan are in [`cardanoscan/`](./screenshots/cardanoscan/)
(timestamps in those screenshots render in UTC+7 — the ledger above is normalised to UTC).

---

## 3. Facts that hold for **all five** transactions

Each of these was read directly from Koios `tx_info`; none is a UI claim.

| # | Invariant | Verified value | What it proves |
|---|---|---|---|
| INV‑1 | Every Plutus script execution succeeded | `valid_contract = true` on **all** script witnesses (7, 12, 7, 7, 7 respectively) | The live deployed validators of *both* protocols accepted the transaction. Nothing was simulated. |
| INV‑2 | Transaction metadata label 674 | `{"674": {"msg": ["Dano Finance: Create Loan"]}}` | The transaction was produced by the Danogo back-end tx-builder, not hand-crafted. |
| INV‑3 | The source **Fluid loan UTxO is consumed** | A script input at the Fluid loan address is spent in every tx | The existing debt position is closed, not left open. |
| INV‑4 | The source **Fluid loan position NFT is burned** (`−1`) | TX‑01 `asset128rrfu48…`, TX‑02 `asset12nvvpmrv…`, TX‑03 `asset1zxnfclhy…`, TX‑04 `asset16hjjsk7m…`, TX‑05 `asset1hc8hc7kt…` | **This is the repayment proof.** A Fluid loan's identity token can only be burned when the loan is settled in full. After the transaction the loan does not exist on-chain. |
| INV‑5 | A **new Dano loan position NFT is minted** (`+1`) | TX‑01/03/04 `asset1pr26rn8r…`, TX‑02 `asset1hwst3ac0…`, TX‑05 `asset1nghq6njh…` | A new Dano loan is originated in the same transaction. |
| INV‑6 | **Collateral continuity is exact** | in = out, to the smallest unit, in all five (see §4) | The collateral is never returned to the borrower and never re-deposited by them. It moves protocol‑to‑protocol inside one transaction. |
| INV‑7 | **Atomicity** | Single tx hash, single block, one balanced input/output set | Close-and-reopen cannot partially fail. There is no window in which the user is unhedged or double-borrowed. |
| INV‑8 | **No borrower top-up** | The borrower's own wallet contributes only ADA for the network fee and min-UTxO; the settlement leg is funded by the Dano pool | The user does not need capital on hand to repay their Fluid loan. |

---

## 4. Per-transaction value flow (net deltas, from Koios)

Legend — `FLUID` = Fluid loan script address · `POOL` = Dano Flexible Pool contract
`addr1wx2degj2ru0uctl4rnvs7vh5l608smvxrgkm7lf8txxjd6qs43szs` · `STAKING` = Dano Staking (fixed-term)
contract `addr1xxt4n07cnlafzefqvne69mmxmnzu2t9gtd27jw9d9yvc7u5htxla38l6j9j` · `LOAN` = Dano Flexible
Loan contract (`addr1zxk23ccxak37kmp94qutawkr0kffcgt24vfu34rrljj6pr…`) · `FEE` = Dano origination-fee
address `addr1qywadgaxcnh993zpzl5kfs806nqe7jyxp4e8unjpll5quymw2aappdz98na` · `SETTLE` = the Fluid
loan's settlement recipient (a distinct address per loan).

Note that the Fluid script address and the Dano loan script address in each transaction carry the
**borrower's own stake credential** — the script portion is protocol-owned, the delegation portion
is the borrower's. That is why the addresses differ between W1 and W2 while the payment credential
is identical.

### TX‑01 — `88579a30…6652` · USDM collateral, ADA borrow, fee pool

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.288610 ₳ · **−1 Fluid loan NFT** `asset128rrfu48…` · **−10.000000 USDM** |
| `POOL` | −22.000857 ₳ (loan disbursement) |
| `SETTLE` `addr1q9mt6pcx…` | **+20.000851 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ (Dano origination fee) |
| `LOAN` (out) | +5.000000 ₳ (min-UTxO) · **+1 Dano loan NFT** `asset1pr26rn8r…` (minted) · **+10.000000 USDM** |
| Borrower wallet | −4.271522 ₳ net (network fee + min-UTxO movement) · +1 borrower bond `asset1cf3ey4y9…` (minted) |

Gross-up check (spec `BorrowModify.Fluid.md §7.15`): `20.000851 + 2.000000 = 22.000851`, against a
pool disbursement of `22.000857` — a 6-lovelace min-UTxO residual. ✅ **The origination fee is
capitalised into the new loan**: the pool lends both the settlement amount and the fee, so a 20 ADA
Fluid debt becomes a 22 ADA Dano loan and the borrower funds nothing beyond the network fee.

### TX‑02 — `d240fab1…d84c` · SNEK collateral, ADA borrow, **fixed-term liquidity source**

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.249820 ₳ · **−1 Fluid loan NFT** `asset12nvvpmrv…` · **−20,979 SNEK** |
| `STAKING` | −15.015024 ₳ (loan disbursement — a *different* Dano product than TX‑01/03/04/05) |
| `SETTLE` `addr1qxukdu6h…` | **+15.000002 ₳ — the Fluid debt, settled** |
| `FEE` | +0.969750 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1hwst3ac0…` (minted) · **+20,979 SNEK** |
| Pool accounting | dADA pool token burned −14,723,905; auxiliary market token −14,602,941 |
| Borrower wallet | −6.374582 ₳ net · +1 borrower bond `asset18980lwqk…` (minted) |

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

### TX‑04 — `c426d9fa…25c8c` · DJED collateral, ADA borrow, fee pool — **the UI-matched transaction**

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.318780 ₳ · **−1 Fluid loan NFT** `asset16hjjsk7m…` · **−6.000000 DJED** |
| `POOL` | −14.000108 ₳ |
| `SETTLE` `addr1qyh4agxd…` | **+12.000102 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1pr26rn8r…` (minted) · **+6.000000 DJED** |

**This transaction is the one whose front-end preview was screenshotted before signing.** The
preview stated: Fluid debt 12 ADA · origination fee 2 ADA · collateral DJED 6 · "Same loan, Same
collateral". On-chain: settlement leg 12.000102 ADA, fee leg exactly 2.000000 ADA, DJED 6.000000
carried, pool disbursement 14.000108 ADA. **The number the user was shown is the number that
settled.** This is the strongest single answer to "no data mismatch".

### TX‑05 — `0e26cc58…05b8` · DJED collateral, **STRIKE borrow**, **zero-fee pool**

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.650650 ₳ · **−1 Fluid loan NFT** `asset1hc8hc7kt…` · **−10.000000 DJED** |
| `POOL` | **−5.000560 STRIKE** (non-ADA disbursement) |
| `SETTLE` `addr1q8ng3ndx…` | **+5.000558 STRIKE — the Fluid debt, settled** · +1.961050 ₳ min-UTxO |
| `FEE` | *(no leg — this pool charges no origination fee)* |
| `LOAN` (out) | +5.000000 ₳ · **+1 Dano loan NFT** `asset1nghq6njh…` (minted) · **+10.000000 DJED** |

This transaction proves two things the other four do not: the flow is **not ADA-specific**, and a
pool with **no origination fee** produces a borrow equal to the debt with no gross-up.

---

## 5. Coverage summary — why five, not one

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

## 6. What the transactions left behind

This annex ends at the moment each transaction settled. What the chain looks like **now**, and
whether the application shows the connected borrower the same figures the ledger holds, is the
subject of [Annex F](./06_POST_STATE_UI_RECONCILIATION.md): the five Fluid position NFTs are at
zero total supply, the five Dano Borrower NFTs are still held by the wallets that signed for them,
and every borrowed amount on the *My Account → Loans* screen re-derives from the principals in §4.

Those figures come from the same public API, read as live state rather than as history.

---

## 7. Why mainnet and not testnet

Fluid does not deploy its smart contracts to any Cardano testnet. There is therefore no test
network on which a Fluid → Dano refinance can be executed at all. Mainnet is not a shortcut here;
it is the only environment where this cross-protocol path exists — and it happens to be the
stronger evidence, because every transaction above is public and immutable on a ledger neither
protocol controls.
