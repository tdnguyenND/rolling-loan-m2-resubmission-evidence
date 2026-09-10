# Annex F — The Post-Refinance State, Reconciled Between the Front End and the Ledger

**Project** 1400107 · **Milestone 2**

[Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md) proves what the five transactions **did**. This annex
proves what they **left behind**, and that the numbers the application shows a user today are the
numbers the Cardano ledger actually holds.

It exists because the reviewer's objection is ultimately about the front end, not the chain. A
transaction hash shows that a validator accepted something. It does not show that the borrower can
then open the app, see their loan, and recognise it as theirs. That is what the two screenshots
below establish, and every figure in them is re-derived from the public chain and checked
mechanically.

The figures on **Fluid's** borrower dashboard, where the settled loans appear as `REPAID`, are
reconciled separately in [Annex C](./03_REPAY_JOURNEY_EXPLAINED.md) §3.

Everything below is read from the public **Koios API**, as **live** state rather than as history —
so the loans age: the principal is fixed, the accrued interest grows.

---

## 1. The two screenshots

Captured on **2026‑09‑10**, from the deployed application at `v2.dano.finance/my-account`, with
each wallet connected through the CIP‑30 browser extension. Both are unedited full-window
captures — address bar included, nothing removed.

| Screenshot | Wallet shown in the app | Loans listed |
|---|---|---|
| [`screenshots/screens/my-account-loans-W1-addr1q80xx.png`](./screenshots/screens/my-account-loans-W1-addr1q80xx.png) | `addr1q80…edcr` | **2** |
| [`screenshots/screens/my-account-loans-W2-addr1q8ex.png`](./screenshots/screens/my-account-loans-W2-addr1q8ex.png) | `addr1q8e…4rfq` | **4** |

![My Account — wallet W1, two loans](./screenshots/screens/my-account-loans-W1-addr1q80xx.png)

![My Account — wallet W2, four loans](./screenshots/screens/my-account-loans-W2-addr1q8ex.png)

---

## 2. The wallets in the screenshots are the wallets that signed the transactions

This is the link that makes the rest of the annex meaningful, so it is stated first and in full.
The truncated addresses in the app's header expand to exactly the two addresses that appear as
signers in the five mainnet transactions:

| | Address, in full | Signed |
|---|---|---|
| **W1** | `addr1q8009gf2f66x5nnk3xd7f3kagn3avqtyhk5uf4zhnejmjrw7lqahdkjjknfuxdj9kevvyqmlu3zyx3x547dqw2pevx0scdedcr` | TX‑01, TX‑02 |
| **W2** | `addr1q8epy5jaharr0857d0lcwhlyg7tnarlajcml084mlhv92h39xk00fdnqnawyvkcs43kmt7hv4uqwetw9yd6lkjl5vxhs4p4rfq` | TX‑03, TX‑04, TX‑05 |

Both appear as signers in the transactions themselves, and both are visible on Cardanoscan against
the transactions in Annex D.

---

## 3. How the application knows which loans to show

Each refinance transaction mints **two** tokens under the Danogo policy
`aca8e306eda3eb6c25a838bebac37d929c216aab13c8d463fca5a08d`:

- one that stays in the **Dano loan contract**, identifying the loan UTxO, and
- one that is paid **to the borrower's own wallet**.

The second is the borrower's title to the loan. Danogo's own CIP‑25 metadata under the same policy
names it, verbatim:

> **`"name": "Borrower NFT (Flexible Pool Lending)"`**
> **`"description": "An NFT representing the loan from the Danogo Flexible Pool Lending"`**

*(read back from the chain with `/asset_history` — see §7)*

So the "Loans" list is not a report the backend composes at will. It is the set of Borrower NFTs the
connected wallet holds. That makes the list independently countable by anyone, without asking us.

**And it counts:**

| Wallet | Borrower NFTs held on chain, now | "Loans" badge in the screenshot | |
|---|---|---|---|
| W1 | 2 | **2** | ✅ |
| W2 | 4 | **4** | ✅ |

---

## 4. Row-by-row reconciliation: the displayed amount is the ledger amount

Every loan row the application renders is matched below to the transaction that created it. The
principal is what the Dano liquidity source disbursed on chain; the accrual is simple interest at
**the APR the application itself displays in that row**; the result is truncated to two decimals,
which is the display rule the application follows consistently across all five rows.

**Wallet W1** — screenshot 1

| UI row | Borrowed (displayed) | APR (displayed) | Created by | Principal on chain | Age at capture | Principal + accrual | |
|---|---|---|---|---|---|---|---|
| 1 | 15.07 ADA | 8.47% | [`d240fab1…`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 15.015024 ADA | 16.8 d | 15.0736 → **15.07** | ✅ |
| 2 | 22.04 ADA | 3.07% | [`88579a30…`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 22.000857 ADA | 21.8 d | 22.0413 → **22.04** | ✅ |

**Wallet W2** — screenshot 2

| UI row | Borrowed (displayed) | APR (displayed) | Created by | Principal on chain | Age at capture | Principal + accrual | |
|---|---|---|---|---|---|---|---|
| 1 | 14.01 ADA | 3.07% | [`c426d9fa…`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 14.000108 ADA | 15.6 d | 14.0185 → **14.01** | ✅ |
| 2 | 13.01 ADA | 3.07% | [`1cf8f08b…`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 13.000010 ADA | 15.9 d | 13.0174 → **13.01** | ✅ |
| 3 | 5.01 **STRIKE** | 5.00% | [`0e26cc58…`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 5.000560 STRIKE | 13.9 d | 5.0101 → **5.01** | ✅ |
| 4 | 321.78 USDA | 7.00% | `67833d58…`, 2025‑10‑12 | — | 333 d | **not part of this milestone** | — |

Five rows, five transactions, five matches — including the non-ADA one. The values were not chosen
to fit: the principals are odd numbers produced by min-UTxO arithmetic (`22.000857`, `15.015024`,
`13.000010`), and each still lands on the displayed figure once its own APR and its own age are
applied.

**About W2's fourth loan.** W2 holds four Borrower NFTs, so the app correctly shows four. The
fourth is the **USDA** loan, minted on **2025‑10‑12** by transaction
`67833d580982a9be309fa322d81951acd8c4c82166e676cbde88c1478e60099a` — almost a year before this
milestone's work. It is **not** offered as evidence for anything here, and it is not reconciled
above. We name it only so the count adds up without a gap the reviewer has to wonder about.

---

## 5. What is *absent* from the screenshots, and why that is the point

Neither account lists a **Fluid** loan. That is not a rendering choice — those loans no longer exist
anywhere on Cardano.

Fluid mints a position NFT when a loan opens. Annex D shows all five being burned inside the
refinance transactions. This annex checks the consequence, today:

| Refinance tx | Fluid position NFT | Total supply on chain, now |
|---|---|---|
| `88579a30…` | `asset128rrfu48vwdclrdxe4hxqhq6hnd5q4h8glf7uq` | **0** |
| `d240fab1…` | `asset12nvvpmrv0qmeh9dz4xcnvx85lknrrzy9rh7uha` | **0** |
| `1cf8f08b…` | `asset1zxnfclhy8tfde90aduttcpp20c7s8r67slv9vr` | **0** |
| `c426d9fa…` | `asset16hjjsk7mzedy20a27aqe5etg0a6v3g7cdlglac` | **0** |
| `0e26cc58…` | `asset1hc8hc7kt4ksrx2ww55gw7l7g9kznsas6csadwx` | **0** |

A supply of zero is a stronger statement than a burn in a historical transaction: it says the token
has not reappeared, in any wallet, in any UTxO, since. The Fluid debts are settled and gone, and the
front end showing no Fluid position is the app telling the user the truth.

Read together with Annex C, this is the **repay** journey seen from the user's side: the debt the
borrower had before is not there afterwards, and the replacement debt is listed with the right
number against it.

---

## 6. Which of the four approved journeys this evidences

| Journey | What this annex contributes |
|---|---|
| **View** | Direct, primary evidence. The Eternl-connected front end lists the user's real mainnet positions — borrowed amount, collateral, APR, health factor, health status — and every borrowed amount reconciles to the ledger. |
| **Repay** | Corroborating. The settled Fluid debts are absent from the interface, and the tokens that represented them have zero supply chain-wide. |
| **Refinance** | Corroborating, and it closes the loop: Annex D shows the transaction, this annex shows the resulting position in the product, owned by the wallet that signed it. |
| **Open** | Corroborating. These five Dano loans were originated through the refinance path, and all five are still open and owned by the wallet that signed for them. |

---

## 7. What we are **not** claiming here

- **These screenshots are not independent-user evidence.** They were captured by us, from wallets we
  control, and they say nothing about whether a stranger could have reached the same screen unaided.
  That question is not answered anywhere in this package, and we make no claim about it. We are not
  repeating the mistake the previous submission made with this class of artifact.
- **We reconcile the borrowed amounts, not the USD figures.** The dollar values in the screenshots
  (`$3.5`, `$9.7`, `Total Debt $8.57`) are oracle-priced at render time and cannot be derived from a
  transaction hash. The native-unit amounts can, and those are what §4 checks.
- **Collateral is stated from the origination transaction, not the current UTxO.** A loan's
  collateral can be modified after it is opened, so we do not assert that today's collateral equals
  the amount carried across at refinance.
- **The fourth loan on W2 predates this milestone** and is excluded from every count and claim above.

---

## 8. What this annex rests on

Four claims, all read from `api.koios.rest`:

| Claim | Source |
|---|---|
| The Fluid loan position NFT burned by each refinance has **total supply 0** today | asset supply, live |
| The Dano Borrower NFT minted by each refinance is **still held by the signing wallet** — the loan is open, and it is that wallet's | the wallet's current assets |
| The number of Borrower NFTs each wallet holds equals the **"Loans N"** badge in its screenshot | the wallet's current assets |
| The amount the application **displayed** equals the on-chain principal plus simple interest at the APR the application displayed, truncated to 2 dp | the principals in Annex D §4 and each transaction's timestamp |

The Fluid policy id used for the supply figures is the one read out of the burn records in the
transactions themselves, not a constant carried in from elsewhere: a value copied into a document
can drift away from the chain, a value read from the chain cannot.
