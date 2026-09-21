# Fluid Tokens API — raw responses

The claim in
[`05` §5.3](../05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#53-fluids-own-records-say-the-loans-are-repaid)
is that **Fluid Tokens** — the counterparty protocol, a separate company, the party that was owed the
money — independently reports **seven** loans repaid with nothing outstanding. These are the
unmodified responses behind it, so the claim does not rest on our transcription of them.

## What was fetched

```
GET https://api.fluidtokens.com/wallet-lending-history?address=<borrower>
```

| File | Borrower wallet | Bytes | SHA-256 |
|---|---|---|---|
| [`w1-wallet-lending-history.json`](./w1-wallet-lending-history.json) | `addr1q8009gf2f66x5nnk3xd7f3kagn3avqtyhk5uf4zhnejmjrw7lqahdkjjknfuxdj9kevvyqmlu3zyx3x547dqw2pevx0scdedcr` | 5798 | `1d3276579675a127e6f4a04e657491f470547218d409dc9b6098b1a9520a119e` |
| [`w2-wallet-lending-history.json`](./w2-wallet-lending-history.json) | `addr1q8epy5jaharr0857d0lcwhlyg7tnarlajcml084mlhv92h39xk00fdnqnawyvkcs43kmt7hv4uqwetw9yd6lkjl5vxhs4p4rfq` | 8131 | `1f2ad3b41d089d178ff11d3e993903f068d90359c58f1ab48bce94f8719027a5` |

Fetched **21 September 2026, 03:05:51 UTC** (the `date` header of each response is kept alongside it
in the matching `.headers.txt`). The two wallets are W1 and W2 of
[`05` §6](../05_ONCHAIN_TRANSACTIONS_AND_REPAY.md); their addresses are public on chain.

**Nothing was edited.** The `.json` files are the response bodies byte for byte — not re-indented,
not re-ordered, not filtered. The endpoint takes optional `from` / `to` parameters; they were
omitted, so each file is the wallet's **whole** lending history as Fluid reports it, which is also
why nothing can have been excluded by a chosen window.

## Reproducing it

```bash
curl -sG https://api.fluidtokens.com/wallet-lending-history \
     --data-urlencode "address=<one of the two addresses above>"
```

The endpoint is public and needs no key. Figures may move if Fluid re-indexes, but the seven events
below are historical and settled.

## What the files contain

Three events for W1, four for W2 — **seven in total, every one `loan_repaid` / `repaid`, every one
`remainingDebt: 0` and `penaltyPaid: 0`**:

| | `details.loanUtxoId` | `details.finishingTxHash` | `totalPaid` | `interestPaid` | action / status | remainingDebt / penalty |
|---|---|---|---|---|---|---|
| **W1** | `a3ed946c#1` | [`d240fab1…`](https://cardanoscan.io/transaction/d240fab1d260b8553a60bf5bae7eb4a1500f9011446a5f01a3c156d6f0dad84c) | 14999998 | -2 | `loan_repaid` / `repaid` | 0 / 0 |
| **W1** | `247e1218#1` | [`17c23dde…`](https://cardanoscan.io/transaction/17c23dde1797e414d1ac14bb1fb507b4cb119f8f938ced61788a18f4ebe2559a) | 5000007 | 7 | `loan_repaid` / `repaid` | 0 / 0 |
| **W1** | `917bdfe1#1` | [`88579a30…`](https://cardanoscan.io/transaction/88579a30a7c11bea37e8483af75b3c4ed56845f1b1378f0e0ed8c3aa81146652) | 20000848 | 848 | `loan_repaid` / `repaid` | 0 / 0 |
| **W2** | `9b3aa00c#1` | [`0e26cc58…`](https://cardanoscan.io/transaction/0e26cc585890eeb13c9bc1e4a37f752eaf770abaf72f8fbde199cf13908b05b8) | 5000555 | 555 | `loan_repaid` / `repaid` | 0 / 0 |
| **W2** | `4901277c#1` | [`c426d9fa…`](https://cardanoscan.io/transaction/c426d9fa4bb213e95efe7bdef96ccc4d9a97b89f750527b1375608d89cd25c8c) | 12000098 | 98 | `loan_repaid` / `repaid` | 0 / 0 |
| **W2** | `7a6caf51#1` | [`1cf8f08b…`](https://cardanoscan.io/transaction/1cf8f08b65574186d4d53c6288e848207b1bda42040e1a97e519839c57549f10) | 11000000 | 0 | `loan_repaid` / `repaid` | 0 / 0 |
| **W2** | `38c7802d#0` | [`ea823365…`](https://cardanoscan.io/transaction/ea823365562e1eefec0cb3be614a54963ec128dcb3ef8430099d308902bfe04d) | 2011598 | 8885 | `loan_repaid` / `repaid` | 0 / 0 |

Three things a reviewer can check from these files alone:

- **Every `finishingTxHash` is a transaction this application built** — each one is in
  [`05` §1](../05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#1-the-five-refinance-transactions) or
  [§5](../05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#5-evidence-for-repayment-two-direct-repay-flows-and-repayment-within-a-refinance),
  and settled on Cardano mainnet.
- **Every `details.nft` is under policy `30f1095a…`** — the Fluid position NFT held by the loan
  script, the same token the transaction burns. That is what makes Fluid's record and our burn
  provably the same loan ([`05` §2.2](../05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#22-the-three-fluid-tokens-minted-at-open-and-which-one-the-pairing-uses)).
- **`interestPaid: -2` on `d240fab1…` is Fluid's own figure**, reproduced rather than tidied — which
  is why Fluid's dashboard renders it as “−0.000 ADA”.

Six of the seven `loanUtxoId` values are the opening transactions listed in
[`05` §1.1](../05_ONCHAIN_TRANSACTIONS_AND_REPAY.md#11-the-transactions-that-opened-these-loans).
The seventh (`38c7802d…`) opened in July 2026, before this milestone's window, and no claim is made
about its opening transaction.
