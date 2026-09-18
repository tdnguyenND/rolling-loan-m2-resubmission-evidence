# On-Chain Transaction Evidence, and the Repay Journey

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"
**Network** Cardano mainnet · **Explorer** https://cardanoscan.io/
**Independent data source used for every figure below** Koios public API (`api.koios.rest`) — not
our own indexer, not our own backend. Repayment status additionally from Fluid Tokens' own public
API (§5.3).

**§1** is the five refinance transactions, and **§1.1** the seven transactions that **opened** the
loans they close — the *open* journey, evidenced on its own rather than inferred. **§2–§4** are the
invariants that hold across the refinances, the value flow of each, and what the five together cover
that one would not. **§5** is the *repay* journey, which the previous submission covered only
implicitly: three repayments made as a **direct user action** from the borrower's own funds and,
as a second line of evidence, the settlement leg carried inside each of the five refinances — all on
mainnet and confirmed by the protocol that was owed the money. The settlement legs are not extra
transactions: they are part of the refinances already counted in §1.

The front-end half of the evidence is in [`04_USER_JOURNEYS_AND_APP_STATE.md`](./04_USER_JOURNEYS_AND_APP_STATE.md).

**Five terms, once:**

- **UTxO** — an on-chain transaction output: a parcel of value sitting at an address, spendable once.
- **Mint / burn** — creating or destroying a token. A burn of `−1` destroys that token permanently.
- **Position NFT** — the token that identifies one **Fluid** loan. **Borrower NFT** — the token that
  identifies one **Dano** loan and is held by the borrower.
- **`valid_contract = true`** — the transaction was accepted by the contract's own on-chain
  validator (the Plutus script). A transaction whose validator rejected it cannot produce these
  outputs.
- **min-UTxO** — the small amount of ADA every output must carry by protocol rule. It is a deposit
  held by the output, not a fee paid to anyone.
- **Settlement leg** — the part of a *Refinance* transaction that pays off the source loan: the
  *Repay* step, performed inside the *Refinance* rather than as a separate user action.

---

## Why the settlement evidence is on mainnet

Mainnet is where the Fluid → Dano path exists as a real market — Fluid's pools with real liquidity,
and the collateral assets borrowers actually post — so a refinance that settles a real debt is
executed there. Mainnet is not a shortcut here; it happens to be the stronger evidence, because
every transaction below is public and immutable on a ledger neither protocol controls.

Fluid's smart contracts on **preprod** were used for the interface walkthroughs in
[`00` §C](./00_POA_SUBMISSION_FORM.md#four-complete-journeys-captured-screen-by-screen);
every transaction in *this* file is a mainnet transaction. The previous submission stated more
absolutely that no testnet Fluid deployment exists — see
[`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑5.

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

Two borrower wallets signed these five transactions: **W1** (TX‑01, TX‑02) and **W2** (TX‑03,
TX‑04, TX‑05). The wallets as shown connected in the application are in
[`04` §2.2](./04_USER_JOURNEYS_AND_APP_STATE.md#22-the-wallets-in-the-screenshots-are-the-wallets-that-signed);
their full on-chain addresses, and why each wallet produces a distinct Fluid loan script address,
are in the [appendix](#appendix--the-addresses-behind-the-labels).

Screenshots of each transaction on Cardanoscan are in
[`screenshots/cardanoscan/`](./screenshots/cardanoscan/) (timestamps in those screenshots render in
UTC+7 — the table above is normalised to UTC).

### 1.1 The transactions that opened these loans

Each Fluid loan closed above was itself **opened through this application**, in its own mainnet
transaction carrying the metadata *"Dano Finance: Borrow from Fluid"*. These are the *open* journey,
evidenced on its own rather than inferred from the refinance.

| # | Tx hash | Block | UTC | Signer | Collateral locked | Closed by |
|---|---|---|---|---|---|---|
| O‑01 | [`917bdfe1…b289`](https://cardanoscan.io/transaction/917bdfe19763fac39a2a154a817f8297958e7f4fb12cf7047a7ee8661e61b289) | 13,824,065 | 2026‑08‑18 08:40:24 | W1 | USDM 10.000000 | **TX‑01** |
| O‑02 | [`247e1218…8e4e`](https://cardanoscan.io/transaction/247e121881a03f483556bc2339a1d6be9a52cd37299dc97c57f3a1dc46368e4e) | 13,848,736 | 2026‑08‑24 04:01:53 | W1 | ADA 50.000000 | **`17c23dde…`** (§5.5) |
| O‑03 | [`a3ed946c…bf6c`](https://cardanoscan.io/transaction/a3ed946c165fc9de0e48fe318a5764ca6149e779818b043c01740b7ba60dbf6c) | 13,848,826 | 2026‑08‑24 04:35:13 | W1 | SNEK 20,979 | **TX‑02** |
| O‑04 | [`7a6caf51…945d`](https://cardanoscan.io/transaction/7a6caf51f61f1a7ca635f74a1b8df629c48550e20a61d869798fa4b91d3e945d) | 13,852,635 | 2026‑08‑25 02:30:36 | W2 | DJED 6.000000 | **TX‑03** |
| O‑05 | [`4901277c…77d7`](https://cardanoscan.io/transaction/4901277c80a06dd3d891eaccb90fd91b0e07037809bd99dd1477c9c4fd6777d7) | 13,853,465 | 2026‑08‑25 07:15:05 | W2 | DJED 6.000000 | **TX‑04** |
| O‑06 | [`9b3aa00c…36ee`](https://cardanoscan.io/transaction/9b3aa00c0c13093fe2ef9489f3fa6b3877195af45d8f35935a0ea3a2340736ee) | 13,856,913 | 2026‑08‑26 02:31:41 | W2 | DJED 10.000000 | **TX‑05** |
| O‑07 | [`ee87712a…7bca`](https://cardanoscan.io/transaction/ee87712aef570de2e0ac8616935f30949f57d20942c2ddbd8d25a94bfce97bca) | 13,865,727 | 2026‑08‑28 04:11:20 | W1 | ADA 35.000000 | **`77748bd9…`** (§5.6) |

O‑01 … O‑06 open a loan on **Fluid** through the Danogo interface; **O‑07 opens a loan on Dano
directly**, with no external protocol in the path — it mints Borrower NFT
`asset1dtp0ke55rxvcazn545ytvfnt3kguyn8udxn3a2` to W1 and disburses USDM 6.096263. All seven returned
`valid_contract = true` on every script execution.

**The pairing is a fact about the ledger, not a claim in this document.** Each Fluid position NFT
**held by the loan script** — policy `30f1095a…`, and only that policy (§2.2) — has exactly **one
mint and one burn** in its entire on-chain history: the mint is the Open transaction, the burn is
the Refinance or Repay that closed it. It can be checked directly:

```
GET https://api.koios.rest/api/v1/asset_txs
      ?_asset_policy=30f1095a8a2acb68bb0ffa193e18e004b6dd3e12b5d9c2375a1d5c41
      &_asset_name=<asset name from the burn record>&_history=true
```

Fluid's own records produce the same pairing independently — its `loanUtxoId` field names the
opening transaction (§5.3).

---

## 2. Facts that hold for all five transactions

Each of these was read directly from Koios `tx_info`; none is a UI claim.

| # | Invariant | Verified value | What it proves |
|---|---|---|---|
| INV‑1 | Every Plutus script execution succeeded | `valid_contract = true` on **all** script witnesses (7, 12, 7, 7, 7 respectively) | The live deployed validators of *both* protocols accepted the transaction. Nothing was simulated. |
| INV‑2 | Transaction metadata label 674 | `{"674": {"msg": ["Dano Finance: Create Loan"]}}` | The transaction was produced by the Danogo back-end tx-builder, not hand-crafted. |
| INV‑3 | The source **Fluid loan UTxO is consumed** | A script input at the Fluid loan address is spent in every tx | The existing debt position is closed, not left open. |
| INV‑4 | The source **Fluid loan position NFT is burned** (`−1`) | TX‑01 `asset128rrfu48…`, TX‑02 `asset12nvvpmrv…`, TX‑03 `asset1zxnfclhy…`, TX‑04 `asset16hjjsk7m…`, TX‑05 `asset1hc8hc7kt…` | **This is the repayment proof.** A Fluid loan's identity token can only be burned when the loan is settled in full. After the transaction the loan does not exist on-chain. |
| INV‑5 | A **distinct Borrower NFT is minted to the signing wallet** (`+1`) | TX‑01 `asset1cf3ey4y9…`, TX‑02 `asset18980lwqk…`, TX‑03 `asset198w3w26r…`, TX‑04 `asset19435naka…`, TX‑05 `asset1gnw4grx8…` — five different tokens, each `supply 1 · 1 mint · 0 burns` | A new Dano loan is originated in the same transaction, and the borrower holds its title. **This is the per-loan identity** — see §2.1. |
| INV‑6 | **Collateral continuity is exact** | in = out, to the smallest unit, in all five (see §3) | The collateral is never returned to the borrower and never re-deposited by them. It moves protocol-to-protocol inside one transaction. |
| INV‑7 | **Atomicity** | Single tx hash, single block, one balanced input/output set | Close-and-reopen cannot partially fail. There is no window in which the user is unhedged or double-borrowed. |
| INV‑8 | **No borrower top-up** — TX‑01, TX‑03, TX‑04, TX‑05 | The borrower's own wallet contributes only ADA for the network fee and min-UTxO; the settlement leg is funded by the Dano pool | The user does not need capital on hand to repay their Fluid loan. **TX‑02 is the exception** — it draws on the fixed-term staking contract, where the fee is not capitalised and the borrower funds 0.954728 ₳ of it; see §3, TX‑02 and [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑4. |

### 2.1 Two Danogo tokens, and which one identifies a loan

Danogo mints **two** tokens under policy `aca8e306eda3eb6c25a838bebac37d929c216aab13c8d463fca5a08d`
when a Dano loan is created, and they are not the same kind of object. Live figures from Koios
`asset_info`:

| | Where it goes | Supply | Mints | Burns | What it is |
|---|---|---|---|---|---|
| `asset1pr26rn8r…` | stays in `LOAN` | 8 | **57** | **49** | a **recurring marker** for loan UTxOs in that contract |
| `asset1hwst3ac0…` | stays in `LOAN` | 8 | **32** | **24** | same, for the staking product |
| `asset1nghq6njh…` | stays in `LOAN` | 1 | **17** | **16** | same, for the STRIKE pool |
| `asset1cf3ey4y9…` · `asset18980lwqk…` · `asset198w3w26r…` · `asset19435naka…` · `asset1gnw4grx8…` | **the borrower's wallet** | 1 each | **1** each | **0** each | the **Borrower NFT** — the borrower's unique title to that loan |

The marker token recurs across many loans — `asset1pr26rn8r…` is the *same* token in TX‑01, TX‑03
and TX‑04 — so it cannot evidence that a distinct loan was created. The **Borrower NFT** can, and
Danogo's own CIP‑25 metadata names it *"Borrower NFT (Flexible Pool Lending)"*. Every claim of the
form "a distinct loan was created" in this package is made against the Borrower NFT. Our previous
submission did not draw this distinction; see [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑3.

### 2.2 The three Fluid tokens minted at Open, and which one the pairing uses

Opening a Fluid loan mints **three** tokens that all carry the **same asset name** — the loan's
identifier — under three different policies. Mainnet open `917bdfe1…` (O‑01, the loan closed by
TX‑01) shows the shape:

| Policy | Where it goes | Burned when the loan closes? |
|---|---|---|
| `30f1095a…` | the **Fluid loan script** `addr1z9dth23…` | ✅ **yes** — TX‑01 burns it `−1` |
| `bcd713bb…` | the loan's **settlement address** `addr1q9mt6pcx…` (the `SETTLE` label in §3) | ❌ no |
| `eadc69a5…` | the **borrower's wallet** `addr1q8009gf2…` | ❌ no |

Only the first is the position NFT the Open ↔ Close pairing is built on, and it is the one the Koios
query above names. The other two survive the loan: the borrower's wallet keeps a token for a
position that no longer exists. That is harmless — each sits in its own min-UTxO — but it means
**"the token minted at open is the token burned at close" is true of the script-held token and false
of the wallet-held one**, and the difference is only visible if the policy is read as well as the
name. Both preprod sessions that looked at this see the same three policies — [`06`
OI‑2](./06_UAT_Reports_Four_Journeys_M2.md#open-items-common-to-more-than-one-session).

Note this is a **Fluid** policy set, distinct from the two **Danogo** tokens in §2.1.

---

---

## 3. Per-transaction value flow

Net deltas, from Koios. Five labels do the work, and every full address behind them is in the
[appendix](#appendix--the-addresses-behind-the-labels):

| Label | What it is |
|---|---|
| `FLUID` | the Fluid loan script address — where the loan being closed lives |
| `POOL` | the Dano pool that funds the new loan — Flexible Pool, or `STAKING` for the fixed-term contract |
| `LOAN` | the Dano loan contract — where the new loan lands, carrying the collateral |
| `FEE` | the Dano origination-fee address |
| `SETTLE` | the Fluid loan's settlement recipient — a distinct address per loan |

### TX‑01 — `88579a30…6652` · USDM collateral, ADA borrow, fee pool

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.288610 ₳ · **−1 Fluid loan NFT** `asset128rrfu48…` · **−10.000000 USDM** |
| `POOL` | −22.000857 ₳ (loan disbursement) |
| `SETTLE` `addr1q9mt6pcx…` | **+20.000851 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ (Dano origination fee) |
| `LOAN` (out) | +5.000000 ₳ (min-UTxO) · **+1 loan-contract marker** `asset1pr26rn8r…` (recurring — §2.1) · **+10.000000 USDM** |
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
| `LOAN` (out) | +5.000000 ₳ · **+1 loan-contract marker** `asset1hwst3ac0…` (recurring — §2.1) · **+20,979 SNEK** |
| Pool accounting | dADA pool token burned −14,723,905; auxiliary market token −14,602,941 |
| Borrower wallet | −6.374582 ₳ net · +1 Borrower NFT `asset18980lwqk…` (minted) |

12 Plutus scripts executed, all `valid_contract = true` — the most complex of the five.

**TX‑02 is the one refinance where the borrower contributes more than the network fee.** The
staking contract disburses 15.015024 ₳ against a settlement plus fee of
`15.000002 + 0.969750 = 15.969752 ₳`; the borrower funds the **0.954728 ₳** difference, which is why
W1's net outflow here (−6.374582 ₳) is larger than on the Flexible Pool transactions. The fee is
therefore **not** capitalised into the loan on this product. TX‑02 still settles the Fluid debt in
full and still carries the collateral across; what it does not demonstrate is the "no capital
needed" property. See [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑4.

### TX‑03 — `1cf8f08b…4f10` · DJED collateral, ADA borrow, fee pool

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.318780 ₳ · **−1 Fluid loan NFT** `asset1zxnfclhy…` · **−6.000000 DJED** |
| `POOL` | −13.000010 ₳ |
| `SETTLE` `addr1qxe843xq…` | **+11.000005 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 loan-contract marker** `asset1pr26rn8r…` (recurring — §2.1) · **+6.000000 DJED** |
| Borrower wallet | −4.233088 ₳ net · **+1 Borrower NFT** `asset198w3w26r…` (minted) |

Gross-up check: `11.000005 + 2.000000 = 13.000005`, against a pool disbursement of `13.000010` — a
5-lovelace min-UTxO residual. ✅ Same capitalised fee as TX‑01.

### TX‑04 — `c426d9fa…25c8c` · DJED collateral, ADA borrow, fee pool — the UI-matched transaction

| Leg | Delta |
|---|---|
| `FLUID` (in) | −2.318780 ₳ · **−1 Fluid loan NFT** `asset16hjjsk7m…` · **−6.000000 DJED** |
| `POOL` | −14.000108 ₳ |
| `SETTLE` `addr1qyh4agxd…` | **+12.000102 ₳ — the Fluid debt, settled** |
| `FEE` | +2.000000 ₳ |
| `LOAN` (out) | +5.000000 ₳ · **+1 loan-contract marker** `asset1pr26rn8r…` (recurring — §2.1) · **+6.000000 DJED** |
| Borrower wallet | −4.236974 ₳ net · **+1 Borrower NFT** `asset19435naka…` (minted) |

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
| `LOAN` (out) | +5.000000 ₳ · **+1 loan-contract marker** `asset1nghq6njh…` (recurring — §2.1) · **+10.000000 DJED** |
| Borrower wallet | −5.849254 ₳ net · **+1 Borrower NFT** `asset1gnw4grx8…` (minted) |

This transaction proves two things the other four do not: the flow is **not ADA-specific**, and a
pool with **no origination fee** produces a borrow equal to the debt with no gross-up. **It is the
zero-fee example** — the previous submission cited `88579a30…` for this, which the chain
contradicts; see [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑1.

---

## 4. Coverage summary

### 4.1 Within the five refinances — why five, not one

| Dimension | Values exercised |
|---|---|
| Signing wallet | 2 (W1, W2) |
| Calendar days | 5 distinct occasions across 9 days (19 → 27 Aug 2026) |
| Collateral asset | USDM · SNEK · DJED |
| Borrowed asset | ADA · STRIKE |
| Dano liquidity source | Flexible Pool · Staking (fixed-term) contract |
| Origination fee configuration | 2.000000 ₳ flat-minimum · 0.969750 ₳ · zero |
| Plutus script executions per tx | 7 · 12 · 7 · 7 · 7 — **all valid** |
| Distinct Fluid loans closed | 5 (five different Fluid position NFTs burned, each `1 mint / 1 burn` in its whole history) |
| Distinct Dano loans opened | 5 (five different **Borrower NFTs** minted — §2.1) |

*The two rows above are scoped to the five refinance transactions in §1. Package-wide totals are in
§4.2.*

### 4.2 Across the whole package

| | |
|---|---|
| Mainnet transactions built by this application | **15** — 7 *open* (§1.1) · 5 *refinance* (§1) · 3 *repay* (§5.5, §5.6) |
| Journeys with transactions of their own | **3** — *view* has none by nature, and is evidenced in [`04` §2](./04_USER_JOURNEYS_AND_APP_STATE.md#2-the-post-refinance-state-in-the-app-reconciled-to-the-ledger) |
| Calendar span | 18 → 28 August 2026 · **7 distinct days** |
| Signing wallets | 2 |
| Collateral assets | USDM · SNEK · DJED · ADA |
| Borrowed assets | ADA · STRIKE · USDM |
| Distinct **Fluid** loans closed | **7** — 5 by refinance, 2 by standalone repay (§5.5) |
| Distinct **Dano** loans opened | **6** — 5 by refinance, 1 directly (O‑07, §1.1) |
| Distinct **Dano** loans repaid on Danogo's own path | **1** — §5.6 |
| Plutus script executions | **all `valid_contract = true`**, in all 15 |
| Metadata label 674 present | all 15 — *Borrow from Fluid* ×6 · *Create Loan* ×6 · *Repay Fluid Loan* ×2 · *Repay Loan* ×1 |

---

## 5. Evidence for repayment: two direct-repay flows, and repayment within a refinance

The approved **Repay** journey is the direct user action — **R‑A** and **R‑B** below, where the
borrower opens the sheet and settles the debt from their own funds. **R‑C** is not a second repay
journey and is not counted as one: it is included to show that a refinance also settles the source
debt in full, on chain, in the same transaction.

The previous submission covered **Repay** only implicitly, through the refinance. All three rows
below executed on Cardano mainnet during this milestone.

| Form | Where it lives in the product | How the user reaches it | Evidence |
|---|---|---|---|
| **R‑A — Repay an external loan** | `BorrowModify` sheet, `Repay` action | Portfolio / My Account → Loans → *Manage* → **Repay** | **Two live mainnet repayments**, `17c23dde…` and `ea823365…`, debt paid from the borrower's own funds and collateral released — §5.5. Implemented for all four supported protocols, each with its own rules. |
| **R‑B — Repay a Dano loan** | the same sheet, for a loan held on Dano | *Manage* → **Repay** | **One live mainnet repayment**, `77748bd9…`, metadata **_"Dano Finance: Repay Loan"_** — Danogo's own repayment path, burning the borrower's own title to the loan — §5.6. |
| **R‑C — Repay as the settlement leg of a refinance** | The refinance transaction itself | Loan Details → **Refinance via Dano** → *Confirm* | Five mainnet transactions in which a Fluid loan is repaid in full and its position token burned — §1, §2 (INV‑4). **Fluid's own dashboard marks all five `REPAID`** — §5.3. |

### 5.1 Why the refinance settles the source debt in the same transaction

A conventional refinance is three user actions and three risks:

```
  1. User finds capital to repay Fluid          →  needs money they do not have
  2. User repays Fluid, withdraws collateral    →  collateral sits unlevered in the wallet
  3. User deposits collateral to Dano, borrows  →  price may have moved; step 3 may fail
```

Between steps the user is exposed. The refinance collapses all three into **one Cardano
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
the Dano loan failed to open.

For the **Flexible Pool** route, the refinance combines the repayment and the new borrowing in one
transaction: the user does not separately withdraw and re-deposit the collateral, and needs no funds
of their own beyond the network fee, because the new loan is sized to cover the outstanding Fluid
debt plus its own origination fee. **TX‑02 is the exception** — it draws on a different product, the
fixed-term staking contract, where the fee is not capitalised and the borrower contributes
0.954728 ₳; see §3, TX‑02 and [`08_CORRECTIONS.md`](./08_CORRECTIONS.md) C‑4.

The approved specification describes the same thing, and calls the leg *"Fluid repay"* because that
is what it is — `docs/screens/LendBorrow/BorrowModify.Fluid.md` §7.15.

### 5.2 The on-chain proof that a repayment occurred

Cardano gives a protocol-level signal, and it is present in **all five** transactions: **the Fluid
loan's position NFT is burned.** Fluid mints a unique loan-position token when a loan is opened, and
that token is the loan's on-chain identity; once it is burned the loan does not exist on chain — it
cannot be queried, serviced, liquidated, or repaid again.

This package uses that burn as the on-chain indicator that the source loan was settled. Two things
corroborate it, and neither is our word: each of these transactions pays a **settlement leg** of at
least the amount Fluid says was owed (the table below), and **Fluid's own public API and dashboard
report the same five loans as repaid**, with `remainingDebt: 0` — §5.3.

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

Queried for the two borrower wallets it returns **seven** events — three for W1, four for W2. Every
one is `"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, `"penaltyPaid": 0` —
and every one names a transaction this application built in its `finishingTxHash` field, and the
transaction that **opened** the loan in its `loanUtxoId` field.

| Fluid's `loanUtxoId` → our label | Its `finishingTxHash` | Transaction metadata | Journey |
|---|---|---|---|
| `917bdfe1…#1` → **O‑01** | `88579a30…146652` | *Dano Finance: Create Loan* | refinance |
| `247e1218…#1` → **O‑02** | `17c23dde…e2559a` | ***Dano Finance: Repay Fluid Loan*** | **repay** — §5.5 |
| `a3ed946c…#1` → **O‑03** | `d240fab1…dad84c` | *Dano Finance: Create Loan* | refinance |
| `38c7802d…#1` → *(opened July 2026)* | `ea823365…2bfe04d` | ***Dano Finance: Repay Fluid Loan*** | **repay** — §5.5 |
| `7a6caf51…#1` → **O‑04** | `1cf8f08b…549f10` | *Dano Finance: Create Loan* | refinance |
| `4901277c…#1` → **O‑05** | `c426d9fa…d25c8c` | *Dano Finance: Create Loan* | refinance |
| `9b3aa00c…#1` → **O‑06** | `0e26cc58…8b05b8` | *Dano Finance: Create Loan* | refinance |

**Fluid names our opening transaction.** For six of the seven loans, `loanUtxoId` is exactly the
transaction listed in §1.1. Fluid's records and the Cardano ledger produce the Open-to-Close pairing
independently of each other, and they agree. The seventh loan was opened
in July 2026, before this milestone's window, and we do not claim its opening transaction.

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
| `ea823365…` | 2.011598 ADA | 2.011600 ADA | +2 |
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

**The user did not fund the repayment — on the four Flexible Pool refinances.** In TX‑01, TX‑03,
TX‑04 and TX‑05 the settlement leg is paid by the Dano pool, not by the borrower's wallet; the
borrower's net ADA change is the network fee and min-UTxO movement only. On TX‑04 the pool disbursed
14.000108 ADA, of which 12.000102 ADA settled the Fluid debt and exactly 2.000000 ADA paid the
origination fee, while the borrower contributed 1.555760 ADA of network fee and nothing else.
The claim this establishes: a borrower can settle a Fluid debt without holding the funds to settle
it, because the new Dano loan funds the settlement in the same transaction.
**TX‑02 is the exception** and is stated as such in §3 — it draws on the fixed-term staking
contract, where the fee is not capitalised and the borrower funds 0.954728 ₳ of it.

**The repayment is still true today.** All five Fluid position NFTs currently have a **total supply
of zero** across Cardano — they exist in no wallet and no UTxO. Correspondingly, when either
borrower opens the application, **no Fluid loan is listed** —
[`04_USER_JOURNEYS_AND_APP_STATE.md` §2.5](./04_USER_JOURNEYS_AND_APP_STATE.md#25-what-is-absent-from-the-screenshots-and-why-that-is-the-point).

### 5.5 Repaying an external loan, on mainnet: two transactions

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
owing.

**A second external repayment, on the other wallet.**

> **`ea823365562e1eefec0cb3be614a54963ec128dcb3ef8430099d308902bfe04d`**
> 2026‑08‑25 02:22:30 UTC · wallet **W2** · four Plutus scripts, all `valid_contract = true`
> Metadata 674: **`"Dano Finance: Repay Fluid Loan"`**
> [Cardanoscan](https://cardanoscan.io/transaction/ea823365562e1eefec0cb3be614a54963ec128dcb3ef8430099d308902bfe04d)

| | |
|---|---|
| Settlement output to the lender | **2.011600 ₳** |
| Fluid position NFT | `asset10j4utwxpjyh6ujtgv42l82tnrkg3psw88w09my` **burned** |
| Collateral | **USDM 16.000000 released to the borrower** |
| Dano loan created | **none** |
| Fluid's own record | `loan_repaid`, `remainingDebt: 0`, `totalPaid: 2.011598` |

Its loan was opened in **July 2026**, outside this milestone's window, so we do not claim its
opening transaction as *open*-journey evidence. It is included because it is a real repayment
executed by one of these wallets through this application inside the milestone period, settling in
a different asset and releasing a different collateral than `17c23dde…`.

### 5.6 Repaying a Dano loan — Danogo's own repayment path

The two transactions above settle loans held on an **external** protocol. This one settles a loan
held on **Dano**, through Danogo's own `Repay Loan` path, with no external protocol in the
transaction at all.

> **`77748bd9673991565c25671522fe70914e29098d7bd0f4c164cf4677585522bc`**
> 2026‑08‑28 07:04:38 UTC · wallet **W1** · five Plutus scripts, all `valid_contract = true`
> Metadata 674: **`"Dano Finance: Repay Loan"`**
> [Cardanoscan](https://cardanoscan.io/transaction/77748bd9673991565c25671522fe70914e29098d7bd0f4c164cf4677585522bc)

| | |
|---|---|
| Consumed | the Dano loan UTxO that **O‑07** created (§1.1) |
| Paid by the borrower | **USDM 6.096399**, from W1's own wallet |
| Where it went | `POOL` +USDM 4.396378 · `FEE` +USDM 1.700021 — **sum 6.096399**, exactly what W1 paid |
| **Borrower NFT** `asset1dtp0ke55rxvcazn545ytvfnt3kguyn8udxn3a2` | **burned** — the borrower's title to the loan is destroyed |
| Loan-contract marker `asset1x87wysuy3ah2e0r056y6pqnq385vklsfm2my52` | **burned** |
| Returned to W1 | 2.551703 ₳ |
| Dano loan created | **none** |

**O‑07 → `77748bd9…` is a complete loan lifecycle on Danogo's own contracts** — opened and repaid
through the connected interface, by the same wallet, within three hours, both transactions carrying
Danogo metadata and both accepted by the live validators. It is the only transaction pair in this
package that exercises Danogo's own `Repay Loan` path, and it is why *repay* is no longer evidenced
solely as a leg of something else.

---

The repay journey is therefore evidenced on mainnet by **three repayments made as a direct user
action**, from the borrower's own funds, and — as a second line of evidence, not five more
transactions — by the **five settlement legs carried inside the five refinances of §1**. Each is
verified against the Cardano ledger, and the seven Fluid loans additionally against the counterparty
protocol's own records.

---

---

## Appendix — the addresses behind the labels

Kept here so the tables above stay readable. Every address can be opened directly on Cardanoscan.

**The two signing wallets.** Each is identified in §1 by the **Fluid loan script address** its stake
credential produces: the script portion is protocol-owned and identical for both, the delegation
portion is the borrower's — which is why the two differ while the payment credential is the same.

| | Fluid loan script address | Transactions |
|---|---|---|
| **W1** | `addr1z9dth23wk9mm2ars073kzl35xc5463wh090qsarz822sfkk7lqahdkjjknfuxdj9kevvyqmlu3zyx3x547dqw2pevx0sewx5g2` | TX‑01, TX‑02 |
| **W2** | `addr1z9dth23wk9mm2ars073kzl35xc5463wh090qsarz822sfk39xk00fdnqnawyvkcs43kmt7hv4uqwetw9yd6lkjl5vxhs279pxf` | TX‑03, TX‑04, TX‑05 |

The base (wallet) addresses, as shown connected in the application, are in
[`04` §2.2](./04_USER_JOURNEYS_AND_APP_STATE.md#22-the-wallets-in-the-screenshots-are-the-wallets-that-signed).

**The contracts in the value-flow tables (§3).**

| Label | Address |
|---|---|
| `POOL` — Dano Flexible Pool | `addr1wx2degj2ru0uctl4rnvs7vh5l608smvxrgkm7lf8txxjd6qs43szs` |
| `STAKING` — Dano Staking (fixed-term) | `addr1xxt4n07cnlafzefqvne69mmxmnzu2t9gtd27jw9d9yvc7u5htxla38l6j9jjqe8n5thkdhx9c5k2sk64ayu262ge3aequnmfak` |
| `LOAN` — Dano Flexible Loan contract | `addr1zxk23ccxak37kmp94qutawkr0kffcgt24vfu34rrljj6pr…` |
| `FEE` — Dano origination-fee address | `addr1qywadgaxcnh993zpzl5kfs806nqe7jyxp4e8unjpll5quymw2aappdz98nah303sy0dc3p83x4hewv5z5c44q2sfqgqqdnjkku` |
| `FLUID` — Fluid loan script address | per wallet, in the table above |
| `SETTLE` — the Fluid loan's settlement recipient | a distinct address per loan; each one is named in that loan's row in §3 |
