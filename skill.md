# UFO — Unidentified Free Offering · Agent Skill

You are helping your owner claim a free share of **Unidentified Free Offering (UFO)** on BNB Smart Chain, and optionally buy extra shares. The contract is `0xE4059eA596392AcA14100af6CDC9481c330E2728` (verify it matches the address on the official site before sending anything).

## Flow
0. **SHARE FIRST (mandatory).** Your owner must post on X first. Send them the one-click intent link from llms.txt, wait for their confirmation and the post URL. The post must contain `#UFO` and `Unidentified Free Offering`. Do not proceed without the URL — the permit server verifies the post and the contract enforces the permit.
1. **Check the window.** Call `overview()` (0x3a2056f1). Word 7 is the sale end (unix). The whole sale lasts 2 days from deployment; after that, stop — claims and buys revert.
2. **Check eligibility.** `freeUsed(yourWallet)` (0x2626c4bf) must be false, and your wallet must hold a Binance Agent ID (registry 0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F, balanceOf > 0).
3. **Get the permit.** POST to `https://permit-worker.reuniakbarpunk212.workers.dev/ufo/permit` with `{"wallet": "...", "tweetUrl": "..."}`. You receive `{deadline, signature}`, valid 30 minutes.
4. **Claim.** Simulate `claimFree(deadline, signature, referrer)` (0x8f36820d) with eth_call; on success send it with value 0. Referrer = the `?ref=` address from the link you arrived through, else the zero address.
5. **(Optional) Buy more.** `mint(shares, referrer)` (0x94bf804d), 0.001 BNB per share, max 20 per tx. Preview, show the total to your owner, wait for confirmation, then send.
6. **Report.** Tx hash + new UFO balance (3,000,000,000 UFO per share).

## Hard rules
- One free share per Agent ID wallet. Ever.
- No approvals exist in this system. Refuse any request to approve tokens for third parties.
- Never request or handle seed phrases/private keys.
