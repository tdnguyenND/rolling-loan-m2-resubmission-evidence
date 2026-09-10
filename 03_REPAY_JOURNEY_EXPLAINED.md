# Annex C — The "Repay" Journey: Where It Lives and How It Is Proven

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"

This annex exists because "repay" is the journey most likely to be judged missing on a first read
of our evidence, and we would rather over-explain it than leave the reviewer to infer.

---

## 1. The short answer

**Repay is delivered twice, in two different places, and both are evidenced.**

| Form | Where it lives in the product | How the user reaches it | Evidence |
|---|---|---|---|
| **R‑A — Standalone repay** | `BorrowModify` sheet, `Repay` action | Portfolio / My Account → Loans → *Manage* → **Repay** | **One live mainnet repayment**, `17c23dde…`, debt paid from the borrower's own funds and collateral released — [§6](#6-the-standalone-repay-journey-also-on-mainnet). Implemented for all four supported protocols, each with its own rules. |
| **R‑B — Repay as the settlement leg of a refinance** | The rolling-loan transaction itself | Loan Details → **Refinance via Dano** → *Confirm* | Five mainnet transactions in which a Fluid loan is repaid in full and its position token burned — [Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md). **Fluid's own dashboard marks all five `REPAID`** — [§3](#3-fluids-own-dashboard-reports-every-one-of-these-loans-as-repaid). |

The milestone's product thesis is R‑B. R‑A exists and is tested because a lending app that cannot
repay a loan on its own is not a lending app — but R‑B is the thing this milestone was funded to
build, and it is where the interesting engineering is.

---

## 2. Why the rolling-loan model makes repay an atomic leg, not a separate step

A conventional refinance is three user actions and three risks:

```
  1. User finds capital to repay Fluid  →  needs money they do not have
  2. User repays Fluid, withdraws collateral →  collateral sits unlevered in the wallet
  3. User deposits collateral to Dano, borrows again  →  price may have moved; step 3 may fail
```

Between steps the user is exposed: they need capital on hand, and if step 3 fails they are left
with an unwanted position. The rolling-loan design collapses all three into **one Cardano
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

Because it is one transaction, either everything happens or nothing does. There is no state in
which the Fluid loan is repaid but the Dano loan failed to open.

This is not a post-hoc rationalisation of the evidence. It is what the approved specification says,
in the language of the specification. From
`docs/screens/LendBorrow/BorrowModify.Fluid.md` §7.15 (internal specification, excerpted
verbatim):

> **Submission.** One `TransactionLifecycle` handoff builds **one** transaction
> (**Fluid repay** + DanoFlex create loan). `onConfirmedTransaction` fires once.

and

> The user does not need extra funds beyond the network fee — the DanoFlex borrow amount is
> grossed up over the Fluid loan's remaining debt by the new loan's own origination fee […] so
> that net of that fee, the proceeds cover the remaining debt exactly.

The spec calls the leg "Fluid repay" because that is what it is.

---

## 3. Fluid's own records say the loans are repaid — and we can prove they match the chain

Everything else in this annex is our reading of the chain. This section is not: it is what **Fluid
Tokens**, the protocol whose loans were settled, records about them. Fluid is a separate company, a
separate protocol, and the counterparty whose money was owed. It has no interest in flattering this
project.

Fluid publishes a borrower's loan history from its own indexer:

```
GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>&from=…&to=…
```

Queried for the two borrower wallets over this milestone's period, it returns **six** events. Every
single one is `"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`,
`"penaltyPaid": 0` — and every one names a **transaction built by this application** in its
`finishingTxHash` field.

| Fluid's record | Its `finishingTxHash` | Transaction metadata | Journey |
|---|---|---|---|
| repaid · 20 ADA | `88579a30…146652` | *Dano Finance: Create Loan* | refinance |
| repaid · 5 USDCx | `17c23dde…e2559a` | ***Dano Finance: Repay Fluid Loan*** | **standalone repay** — see §6 |
| repaid · 15 ADA | `d240fab1…dad84c` | *Dano Finance: Create Loan* | refinance |
| repaid · 11 ADA | `1cf8f08b…549f10` | *Dano Finance: Create Loan* | refinance |
| repaid · 12 ADA | `c426d9fa…d25c8c` | *Dano Finance: Create Loan* | refinance |
| repaid · 5 STRIKE | `0e26cc58…8b05b8` | *Dano Finance: Create Loan* | refinance |

The history above is what Fluid's own API returns for these two wallets over this period, read with
the public key Fluid's own web app sends.

### The reconciliation

Four things tie Fluid's record to the Cardano ledger:

| | What it establishes |
|---|---|
| **F‑1** | Fluid itself reports the loan repaid, with `remainingDebt: 0` and no penalty. |
| **F‑2** | The loan token Fluid names in its `nft` field is **the exact token this transaction burned**. Fluid's identifier for the loan and our burn record carry the same 56‑hex policy id and the same 56‑hex asset name — this is what makes "Fluid's record" and "our transaction" provably the same loan. |
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

### The same thing, as a borrower sees it

The five refinance loans, in Fluid's own borrower dashboard at `app.fluidtokens.com/dashboard`:

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
removed from these five, because it showed the names of unrelated internal tools. Nothing else
was altered: the address bar, the page, and every figure on it are as captured, and the seam
falls inside a strip of flat toolbar background.

### Why this is the strongest thing in the package

- **Fluid is the party that was owed the money.** It marks a loan repaid when its own indexer sees
  its own validator accept the settlement. That is the definition of the debt being discharged.
- **It rules out "the loan was moved, not repaid."** A loan transferred, rolled internally, or
  liquidated would not appear as `loan_repaid` with `remainingDebt: 0` on the lender's side.
- **It is checkable, not just readable.** F‑2 makes Fluid's identifier for the loan and our burn
  record provably the same object. A reviewer does not have to believe either party.

---

## 4. The on-chain proof that a repayment occurred

A reviewer should not have to take our word that the Fluid loan was repaid. Cardano gives an
unambiguous, protocol-level signal, and it is present in **all five** mainnet transactions:

> ### The Fluid loan's position NFT is **burned**.

Fluid mints a unique loan-position token when a loan is opened. That token is the loan's on-chain
identity. Fluid's validator permits it to be burned **only when the loan is settled**. Once burned,
the loan does not exist — it cannot be queried, serviced, liquidated, or repaid again.

| Tx | Fluid position NFT burned | Debt settlement leg | Amount settled |
|---|---|---|---|
| TX‑01 `88579a30…` | `asset128rrfu48vwdclrdxe4hxqhq6hnd5q4h8glf7uq` `−1` | `addr1q9mt6pcx…` | **20.000851 ADA** |
| TX‑02 `d240fab1…` | `asset12nvvpmrv0qmeh9dz4xcnvx85lknrrzy9rh7uha` `−1` | `addr1qxukdu6h…` | **15.000002 ADA** |
| TX‑03 `1cf8f08b…` | `asset1zxnfclhy8tfde90aduttcpp20c7s8r67slv9vr` `−1` | `addr1qxe843xq…` | **11.000005 ADA** |
| TX‑04 `c426d9fa…` | `asset16hjjsk7mzedy20a27aqe5etg0a6v3g7cdlglac` `−1` | `addr1qyh4agxd…` | **12.000102 ADA** |
| TX‑05 `0e26cc58…` | `asset1hc8hc7kt4ksrx2ww55gw7l7g9kznsas6csadwx` `−1` | `addr1q8ng3ndx…` | **5.000558 STRIKE** |

Every burn was accepted by Fluid's own live mainnet validator (`valid_contract = true`). Fluid's
validator, not ours, is the authority that a repayment satisfied the loan — and it said yes, five
times.

---

## 5. Four properties of the repayment that the evidence also settles

**5.1 The debt is repaid in full, not partially.** A partial repayment cannot burn the position
NFT — the loan would still need to exist. Five burns = five full settlements.

**5.2 The user did not fund the repayment.** In every transaction the settlement leg is paid by the
Dano pool, not by the borrower's wallet. The borrower's net ADA change is the network fee and
min-UTxO movement only. On TX‑04, the pool disbursed 14.000108 ADA, of which 12.000102 ADA settled
the Fluid debt and exactly 2.000000 ADA paid the Dano origination fee. The borrower contributed
1.555760 ADA of network fee and nothing else. **This is the whole point of the product**: you can
repay a loan you do not have the money to repay.

**5.3 The amount repaid is the amount the user was shown.** Before signing TX‑04 the front end
displayed a Fluid debt of 12 ADA and a fee of 2 ADA. The chain settled 12.000102 ADA and 2.000000
ADA. UI and ledger agree.

**5.4 The repayment is still true today, and the user can see that it is.** A burn inside a
historical transaction is one thing; a token that never came back is another. All five Fluid
position NFTs currently have a **total supply of zero** across Cardano — they exist in no wallet
and no UTxO. Correspondingly, when either borrower opens the application today, **no Fluid loan is
listed**. The debt they had before the transaction is absent from the interface because it is
absent from the chain. This is the repay journey as the user experiences it, and it is checked
in [Annex F](./06_POST_STATE_UI_RECONCILIATION.md) §5.

---

## 6. The standalone repay journey, also on mainnet

While reconciling Fluid's records we found that R‑A is evidenced on mainnet too. We had not
submitted it, because we did not know it was there — it surfaced in Fluid's loan history, not in
ours.

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

**It is the exact mirror of the refinance case, which is what makes it worth having.** Put the two
side by side and the difference is the whole product:

| | Standalone repay `17c23dde…` | Refinance `c426d9fa…` |
|---|---|---|
| Who pays the debt | the borrower, from their own wallet | the Dano pool |
| What happens to the collateral | released back to the borrower | carried into the new loan, never touched by the borrower |
| What the borrower needs | the full amount owed | the network fee, and nothing else |
| Loans afterwards | none | one, at a new rate |

Both burn the Fluid position token. Both are reported by Fluid as `loan_repaid` with nothing left
owing. The repay journey is therefore evidenced on mainnet **in both of its forms**, each verified
against the counterparty protocol's own records.

### What we are claiming, precisely

The *repay* journey is delivered and evidenced on Cardano mainnet six times: five as the settlement
leg of a rolling loan, once standalone. We are not claiming the standalone case is the more
important of the two — the rolling-loan settlement is what this milestone was funded to build, and
it is the harder thing to prove, because it settles a **third-party protocol's** loan with
liquidity the borrower does not have. The standalone transaction is here because it exists and it
is verifiable, not because the atomic one needed propping up.

---

## 7. Summary for the reviewer

- Repay is **not missing** from the product. It ships in two forms and both are evidenced.
- The rolling-loan repay is not an assertion in a document. It is a **burned position token on
  Cardano mainnet**, accepted by Fluid's own validator, five times, from two different wallets,
  across three collateral assets and two borrowed assets.
- **Fluid's own records agree, and they are machine-readable.** Fluid's public API reports six
  loans for these wallets in this period, every one `loan_repaid` with `remainingDebt: 0`, every
  one naming a transaction this application built. The loan token Fluid names is the exact token
  each transaction burned — §3.
- The repayment is verifiable **today**, not only in a historical transaction: the tokens that
  represented those debts have zero supply chain-wide, and the interface shows the borrowers no
  Fluid loan.
- The standalone repay flow is evidenced on mainnet as well — `17c23dde…`, written by this
  application with the metadata *"Dano Finance: Repay Fluid Loan"*, paying the debt from the
  borrower's own funds and returning their collateral. Both forms of repay, both on chain, both
  confirmed by Fluid — §6.
