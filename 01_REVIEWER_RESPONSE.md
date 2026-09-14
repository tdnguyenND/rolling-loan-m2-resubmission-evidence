# Annex A — Response to the Reviewer

**Project** 1400107 · **Milestone 2** · Rolling Loan / "Refinance via Dano"
**Previous outcome** Not Approved

We accept the review. Both objections were correct and neither was a misunderstanding of our
submission — they were accurate descriptions of what our evidence did and did not contain. This
annex answers each objection with what changed, and marks plainly where a gap remains.

---

## Objection 1 — A developer's demo is not evidence of an intuitive interface

> *"The fact that a developer has successfully demonstrated the application does not automatically
> establish that the interface is intuitive to independent users. The submission provides
> screenshots and a demo, but there is no clear evidence of user testing by independent testers.
> […] The app is going to be a public one, and there is no evidence to show that users even
> completed a specific workflow without external guidance."*

### What was wrong with our previous submission

Our integration test report contained the line:

> *"Tester completed the flow without external guidance — ✅ Yes"*

The testing behind that row happened, and the row describes it accurately. What was wrong was the
weight we put on it: a walkthrough by a member of the team shows that the flow completes end-to-end,
not that the interface is discoverable to someone who did not build it. We filed real testing under
the wrong heading.

### What we are doing about it

**We are restating that row as the testing it actually records.**

All four journeys were tested end-to-end through the Eternl-connected front end. The open, repay
and refinance journeys were tested repeatedly on mainnet: six sessions, two wallets, nine days,
each one ending in a transaction anyone can pull from the ledger.
[Annex B](./02_USER_JOURNEY_EVIDENCE_MATRIX.md) sets out what was tested per journey, and
[Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md) lists the transactions those sessions produced.

This is internal testing, and the package labels it as such wherever it appears. It carries the
claim it can carry — that each journey completes through the released interface and that the chain
records the result — with no usability verdict attached to it. Around it sits evidence of a kind we
cannot influence: the Cardano ledger read from the public Koios API, and Fluid Tokens' own records
of the loans that were settled.

### What this means for the review

What is in front of the reviewer is testing that took place and that can be checked from outside:
every journey exercised through the released interface, six of those sessions verifiable on mainnet
down to the transaction hash. A usability study run with participants recruited from outside the
team is a separate exercise from that, and wherever this package touches usability it says which of
the two it means. The change since the previous submission is that the testing is now reported as
what it is, and tied to public records that confirm it happened.

### The flow was exercised repeatedly, not demonstrated once

The mainnet transactions in [Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md) were signed by **two
distinct wallets** on six separate occasions across nine days. Each of those is a testing session
that ran the whole way through the interface to a settled transaction — more than one person, on
more than one day, against more than one pool configuration. It is testing from inside the team,
stated that way; what makes it checkable is that every session ends in a public transaction.

---

## Objection 2 — Evidence per journey, and the integration-test results

> *"Also, I would like you to provide evidence demonstrating that the four approved user journeys
> (view, open, repay and refinance) were each successfully tested through the Eternl-connected
> front end, and provide the corresponding integration-test results."*

### What was wrong with our previous submission

Two things.

**(a) We collapsed the journeys.** Our transaction annex said, of open and repay:

> *"Open-loan and repay-loan are covered implicitly: each refinance both closes the source Fluid
> loan and opens a Dano loan atomically."*

That is architecturally true, and it is still true — but "implicitly" is not evidence, and asking a
reviewer to infer two journeys from one transaction is asking them to do our work. Each journey now
has its own row and its own on-chain artifact where one exists.

**(b) We rested the case on our own test reporting.** We published a single figure — *8/8 refinance
UI tests passing*. It was true, and it was the wrong kind of thing to lead with: a count of our own
tests, produced by us, about our own work.

### What changed

**We stopped asking you to take our word for anything.** This resubmission is built on two sources
we cannot influence: the **Cardano ledger** (read from the public Koios API) and **Fluid Tokens' own
loan records** (read from Fluid's public API). Every figure in this package is sourced from one of
those two, and none of it passes through a Danogo server, indexer or database.

The application is covered by an automated suite, and it is how we caught what we caught during
development. We are not submitting its figures as evidence, because they are the same class of
artifact as the usability verdict we restated: our own assessment of our own work.

**Per-journey evidence matrix** — [Annex B](./02_USER_JOURNEY_EVIDENCE_MATRIX.md). Four journeys ×
two independently checkable evidence classes (Eternl front end · on-chain settlement), with the
scope of each claim stated in its own section rather than glossed.

**On-chain ledger, expanded from two transactions to five** —
[Annex D](./04_ONCHAIN_TRANSACTION_LEDGER.md). Every figure independently re-derived from the public
Koios API rather than from our own backend.

**The resulting loans, in the application, matched to the ledger** —
[Annex F](./06_POST_STATE_UI_RECONCILIATION.md). This is new in this resubmission, and it is the
part we think most directly meets the spirit of your second objection. A transaction hash shows
that a validator accepted something; it does not show that the borrower can then open the app and
see their loan. Annex F closes that: the *My Account → Loans* screen of each signing wallet,
captured from the live application, with **every displayed borrowed amount re-derived from the
chain** — the on-chain principal plus interest accrued at the APR that same row displays. Five
rows, five transactions, five matches, including the non-ADA one. The loan count the app shows
equals the number of Danogo *Borrower NFTs* the wallet holds on chain, and the settled Fluid
positions are absent from the interface because their tokens now have zero supply chain-wide.

The five transactions are not five copies of one demo. They span:

| Dimension | Values |
|---|---|
| Signing wallets | 2 |
| Occasions | 5, across 9 days (19 → 27 Aug 2026) |
| Collateral assets | USDM · SNEK · DJED |
| Borrowed assets | ADA · **STRIKE** (non-ADA) |
| Dano liquidity source | Flexible Pool · **Staking (fixed-term) contract** |
| Origination fee configuration | 2.000000 ₳ · 0.969750 ₳ · **none** |
| Plutus scripts executed | 7 · 12 · 7 · 7 · 7 — **all valid** |

---

## Objection 2, continued — the "repay" journey specifically

We expect this to be the point of contention, so it has its own annex:
[Annex C — The "Repay" Journey](./03_REPAY_JOURNEY_EXPLAINED.md).

The summary:

**Repay ships in two forms.** A standalone *Repay* flow, implemented for all four lending
protocols with their own rules (Surf permits full repayment only; Fluid enforces recast
constraints), and repay as the settlement leg of the rolling-loan transaction. **Both are on
mainnet** — five as the settlement leg of a refinance, one standalone.

**The rolling-loan repay is proven on-chain, unambiguously.** Fluid mints a unique position token
when a loan opens; its validator permits that token to be burned **only when the loan is settled**.
In all five transactions, the Fluid position NFT is burned:

| Tx | Fluid position NFT burned | Debt settled | Paid by |
|---|---|---|---|
| `88579a30…` | `asset128rrfu48…` `−1` | 20.000851 ADA | Dano pool, not the borrower |
| `d240fab1…` | `asset12nvvpmrv…` `−1` | 15.000002 ADA | Dano staking contract |
| `1cf8f08b…` | `asset1zxnfclhy…` `−1` | 11.000005 ADA | Dano pool |
| `c426d9fa…` | `asset16hjjsk7m…` `−1` | 12.000102 ADA | Dano pool |
| `0e26cc58…` | `asset1hc8hc7kt…` `−1` | 5.000558 STRIKE | Dano pool |

Fluid's own live validator accepted every one of those burns. Fluid — not us — is the authority on
whether a Fluid loan was repaid, and it said yes five times.

**And Fluid's own records say so, in machine-readable form.** Fluid publishes a borrower's loan
history from its own indexer. Queried for these two wallets over this period, it returns **six**
events — every one `"action": "loan_repaid"`, `"status": "repaid"`, `"remainingDebt": 0`, and every
one naming a transaction **this application built** in its `finishingTxHash` field. The loan token
Fluid names is the exact token each transaction burned, and each transaction pays at least the
total Fluid says was due. Fluid is a separate protocol, a separate company, and the counterparty whose money was owed. The
reconciliation and the borrower-dashboard screenshots are in
[Annex C](./03_REPAY_JOURNEY_EXPLAINED.md) §3.

**The approved specification uses the same word.** `BorrowModify.Fluid.md §7.15`, verbatim: *"One
`TransactionLifecycle` handoff builds **one** transaction (**Fluid repay** + DanoFlex create
loan)."* The atomic design is not a retrofit to fit the evidence; it is what was specified and
built.

**Still true today, not only at the moment of settlement.** All five of those Fluid position
tokens now have a **total supply of zero** across Cardano — they have not reappeared in any wallet
or UTxO since. And neither borrower's *My Account* screen lists a Fluid loan: the debt they had
before the transaction is gone from the interface because it is gone from the chain
([Annex F](./06_POST_STATE_UI_RECONCILIATION.md) §5).

**The standalone repay flow is on mainnet too.** Querying Fluid's history surfaced a sixth
repayment that we had not submitted, because we did not know it was in Fluid's records:
`17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a`, 2026‑08‑24, written by this
application with the metadata **"Dano Finance: Repay Fluid Loan"**. It is the exact mirror of the
refinance case — the borrower pays the debt from their **own** wallet, the collateral is
**released back to them**, and **no** Dano loan is created. Both forms of the repay journey are
therefore evidenced on chain — [Annex C](./03_REPAY_JOURNEY_EXPLAINED.md) §6.

---

## What is fixed from the previous submission, itemised

| # | Previous submission said | Status | Now |
|---|---|---|---|
| 1 | *"Tester completed the flow without external guidance ✅"* (offered as a usability verdict) | **restated** | The testing stands and is reported as internal testing: every journey walked end-to-end through the connected wallet, six sessions settling on mainnet. No usability verdict is attached to it — see Objection 1 above |
| 2 | *"Open-loan and repay-loan are covered implicitly"* | **withdrawn** | Per-journey evidence matrix — Annex B; repay given its own annex — Annex C |
| 3 | *"8/8 automated refinance tests pass"* | **not relied upon** | This submission rests on the Cardano ledger and Fluid's own records, both queryable by the reviewer. Our test figures are our own reporting and are no longer offered as evidence |
| 4 | Two mainnet transactions | **superseded** | Five, spanning two wallets, three collateral assets, two borrowed assets, three fee configurations — Annex D |
| 5 | *"TC‑24 — no-origination-fee pool: tx `88579a30…`"* | **corrected** | On-chain, `88579a30…` **does** carry a 2.000000 ₳ origination-fee leg — that pool's fee is capitalised into the new loan, so a 20 ₳ Fluid debt became a 22 ₳ Dano loan. The zero-fee example is `0e26cc58…`. See [`08_CORRECTIONS.md`](./08_CORRECTIONS.md). |
| 6 | Figures sourced from our own reporting | **strengthened** | Every on-chain figure re-derived from the public Koios API rather than from our own backend — no script of ours in the chain of evidence |
| 7 | Evidence stopped at the transaction | **extended** | The state the transactions produced is now shown *in the application* and reconciled to the ledger row by row — Annex F |

Item 5 is an error in our own previous submission that nobody asked us about. We found it while
re-verifying the transactions for this resubmission and are correcting it unprompted, because a
reviewer who checks our numbers should find that we checked them first.

---

## Where we stand

| | Delivered | Outstanding |
|---|---|---|
| Per-journey evidence matrix | ✅ | |
| On-chain settlement, independently verifiable | ✅ 6 transactions | |
| Resulting loans shown in the app, reconciled to the ledger | ✅ Annex F | |
| Repay journey explained and proven | ✅ Annex C — 6 repayments on mainnet, confirmed by Fluid's own records | |
| User testing | ✅ every journey end-to-end; 6 sessions settled on mainnet | Study with participants recruited from outside the team |
| Corrections to the previous submission | ✅ | |

Everything in the left-hand column is in the repository and rests on public data. The right-hand
column names the one exercise this package does not contain, so it is not read into the rest.
