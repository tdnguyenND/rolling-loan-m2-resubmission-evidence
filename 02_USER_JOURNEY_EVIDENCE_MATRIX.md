# Annex B — Evidence Matrix: the Four Approved User Journeys

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"

> *"I would like you to provide evidence demonstrating that the four approved user journeys (view,
> open, repay and refinance) were each successfully tested through the Eternl-connected front end,
> and provide the corresponding integration-test results."* — reviewer

This annex answers that request as a single table, journey by journey, with four independent
classes of evidence per journey. Nothing here is a claim without an artifact behind it.

---

## 1. The two classes of evidence

| Class | What it is | Why it counts |
|---|---|---|
| **E1 — Eternl-connected front end** | The journey performed in the live app at https://v2.dano.finance/ with the Eternl browser extension connected, screenshotted and/or recorded. | Answers "through the Eternl-connected front end". |
| **E2 — On-chain settlement** | A Cardano mainnet transaction, publicly verifiable, accepted by live validators. | Answers "did it actually work", without trusting us. |

Two classes, deliberately. This submission rests on what can be checked by a third party from
public data — the Cardano ledger, and the records of the protocol on the other side of each
transaction. It does not rest on our own test reporting, and it makes no claim about whether the
interface is intuitive to people who did not build it. Both omissions are stated in §4.

---

## 2. The matrix

Legend: ✅ delivered in this package. Every cell links to the artifact behind it.

| Journey | E1 · Eternl front end | E2 · On-chain mainnet |
|---|---|---|
| **1. View** — see my loans, debt, collateral, health factor, APR | ✅ **My Account → Loans, captured from the live app on two connected mainnet wallets** — Annex F §1 | ✅ **every displayed amount reconciled to the ledger** — Annex F §4 |
| **2. Open** — originate a loan against collateral | ✅ *Create loan* / *Increase loan* sheets, Eternl-connected · ✅ the five originated loans are **listed in the app and still open** — Annex F §3 | ✅ **5 Dano loans originated on mainnet** — one loan-position NFT minted per transaction, Annex D INV‑5 |
| **3. Repay** — settle a loan | ✅ *Repay* sheet, Eternl-connected, all four protocols · ✅ the settled loans are **absent** from the post-refinance screens — Annex F §5 · ✅ **Fluid's own dashboard marks them `REPAID`** — Annex C §3 | ✅ **5 Fluid loans repaid in full as the settlement leg of a refinance** — one position NFT burned per transaction, Annex D INV‑4; all five now at **zero total supply** chain-wide, Annex F §5 · ✅ **1 standalone repayment on mainnet** — `17c23dde…`, debt paid from the borrower's own funds, collateral released, Annex C §6 · ✅ **the counterparty protocol's own indexer reports all six as `loan_repaid`, `remainingDebt: 0`** and names the exact token each transaction burned — Annex C §3 |
| **4. Refinance** — roll a Fluid loan into a Dano loan | ✅ Refinance card → preview → Eternl signature → confirmation, screenshotted end-to-end · ✅ the **resulting** Dano loans shown in the app — Annex F | ✅ **5 signed mainnet refinances**, 2 wallets, 3 collateral assets, 2 borrowed assets, 9 days — Annex D |

---

## 3. Journey-by-journey walkthrough of the E1 evidence

Each journey below is described as the *user's* path, because that is what the reviewer is
evaluating — not the code path.

### Journey 1 — View

```
  Open v2.dano.finance  →  Connect Wallet  →  Eternl  →  approve
      →  Portfolio                    : all positions, allocation %, borrow rows in negative tone
      →  My Account → Loans           : one row per open loan, per protocol
      →  Manage                       : Loan Details popup — debt, collateral list, APR,
                                        health factor with band label, utilisation
```

Nothing is signed. The user reads their real on-chain position. It is the journey where a wrong
number is most damaging and least visible, which is why it is the one we reconcile figure by
figure against the ledger rather than merely screenshotting.

**Captured, and reconciled.** [Annex F](./06_POST_STATE_UI_RECONCILIATION.md) shows this screen for
both mainnet wallets, and checks every number on it against the ledger: each displayed borrowed
amount equals the on-chain principal plus interest accrued at the APR the same row displays, and the
number of rows equals the number of Danogo Borrower NFTs the connected wallet holds — a count that
is held on chain, not in our database.

### Journey 2 — Open

```
  Borrow Market  →  pick a market  →  Borrow
      →  choose collateral, enter amount
      →  preview: max borrow, projected health factor, origination fee, loan impact
      →  Confirm  →  Eternl signature prompt  →  submit  →  confirmation + tx hash
```

On-chain, this journey is what produces a loan-position NFT inside the Dano Flexible Loan contract.
The five ledger transactions each perform exactly this origination as their second leg.

### Journey 3 — Repay

```
  My Account → Loans  →  Manage  →  Repay
      →  amount pre-fills to min(wallet balance, full repayment)
      →  preview: new debt, new health factor, "↘ Repaid" badge on full repayment
      →  Confirm  →  Eternl signature prompt  →  submit  →  confirmation
```

Protocol-specific rules are enforced and tested: Surf allows full repayment only and disables the
CTA with a named reason when the wallet cannot cover it; Fluid enforces recast rules and rejects a
repayment that does not reduce principal; Liqwid offers the receive-as-underlying toggle. See
Annex C for how this journey also occurs as the settlement leg of a refinance.

### Journey 4 — Refinance

```
  My Account → Loans  →  a Fluid loan row shows "Dano save 4.56% net cost"
      →  Manage  →  Loan Details  →  Refinance via Dano  (card expands in place)
      →  preview: Net cost (Fluid → Dano) 4.00% → −0.56%
                  Health Factor 1.77 Healthy → 1.52 Fair
                  Fee 2 ADA
                  ✓ Same loan, Same collateral
                  "You don't need extra funds to close your Fluid loan."
      →  Confirm  →  Eternl shows ONE transaction, labelled "Dano Finance: Create Loan"
      →  sign  →  "Transaction confirmed" + hash
      →  Portfolio: the Fluid position is gone; a Dano position stands in its place
```

The preview numbers above are from the journey that produced **TX‑04** (`c426d9fa…`). On-chain,
that transaction settled 12.000102 ADA of Fluid debt and paid exactly 2.000000 ADA of origination
fee, carrying DJED 6.000000 across. **The number the user was shown is the number that settled** —
which is the concrete form of "no data mismatch".

---

## 4. The scope of these claims

Stated explicitly, so the reviewer does not have to work out where each claim stops:

1. **The user testing behind this package is internal testing, and is reported as such.** Every
   journey was walked end-to-end through the Eternl-connected front end by our own testers, and the
   open, repay and refinance sessions settled on mainnet six times across two wallets. The previous
   submission reported that as *"Tester completed the flow without external guidance ✅ Yes"* — a
   fair description of what happened, placed under the wrong heading. It establishes that each
   journey completes through the released interface; it is not a measure of how the interface reads
   to someone who did not build it. See [Annex A](./01_REVIEWER_RESPONSE.md), Objection 1.
2. **Journeys 3 and 4 share their transactions, and we are not double-counting them.** Five
   transactions each perform a repay *and* a refinance; a sixth (`17c23dde…`) performs only the
   repay. That is six repayments and five refinances, not eleven demonstrations.
   [Annex C](./03_REPAY_JOURNEY_EXPLAINED.md) sets out exactly which transaction is which.
3. **Journeys 1 and 2 have no dedicated per-journey mainnet transaction of their own** beyond what
   the refinance transactions contain. Loan origination is genuinely evidenced on-chain six times;
   "view" is a read journey and has no transaction by nature.
4. **We are not submitting test-suite figures.** The application is covered by an automated suite,
   but a count of passing tests is our own reporting about our own work, and this package is built
   on evidence a third party can check without us. Nothing here should be read as a claim about
   test coverage.
