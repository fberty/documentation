---
title: The Merge
---

:::caution

This page will be continually updated in the lead up to the Merge. 

:::

# The Merge

In early December 2022, Gnosis will undergo the Merge. The current Gnosis execution layer (formerly xDai) will merge with the [Gnosis Beacon Chain](../consensus/gbc.md), in a process similar to the [Ethereum merge](https://ethereum.org/en/upgrades/merge/). 

The Merge will also represent a important shift for Gnosis, replacing the legacy [Proof-of-Authority consensus](../consensus/aura.md) with the open and unpermissioned [Beacon Chain Consensus](../consensus/gbc.md). This allows Gnosis to transition to a fully decentralized and permissionless proof-of-stake network. 

The Merge is also another step in Gnosis' journey together with Ethereum. From the [early days of xDai at ETHDenver](https://developers.gnosischain.com/about-gc/use-cases/cryptocurrency-for-events-and-conferences/ethdenver), Gnosis has had a role in Ethereum's journey. With the adoption of Ethereum's consensus mechanism, Gnosis will continue to contribute to Ethereum's growth as an experimental chain, where newcomers, experiments and ideas are welcome. 

## When will the Merge happen?

:::tip

The week of 5th Dec 2022

:::

On the 9 Nov 2022 Gnosis Core Devs call, it was agreed to target **the week of 5th Dec 2022** for the Merge. Due to variances in block time, the Merge will likely happen sometime from **5th to 11th Dec 2022.**

### TTD

With the 5th Dec 2022 target in mind, the Core Devs have proposed the following TTD value: 

```
8626000000000000000000058750000000000000000000
```

This number is not chosen randomly: to pay tribute to the Ethereum Merge, Core Devs have proposed to include [Ethereum's TTD](https://notes.ethereum.org/@MarioHavel/merge-ttd), `58750000000000000000000`, in the Gnosis Merge TTD. 

### Bellatrix

Similar to Ethereum, the Gnosis Beacon Chain will have a [Bellatrix epoch](https://blog.ethereum.org/2022/08/24/mainnet-merge-announcement) that will occur prior to the Merge. 

## Timeline

| Date                | Event                                           |
| ------------------- | ----------------------------------------------- |
| 15 Nov 2022 (Wed)   | Merge Date announced publicly                   |
| 23 Nov 2022 (Wed)   | Release of merge-ready Client Images            |
| 23 Nov 2022 (Wed)   | Release of revamped Validator Docs              |
| 30 Nov 2022 (Wed)   | Bellatrix epoch hit for Consensus Layer clients |
| 5-11 Dec 2022 (Mon) | Estimated TTD window for Gnosis Merge           |

## How to Prepare

### For Validators

:::caution

Core Devs have not released merge-ready Client Images yet. Please wait for further announcements

:::

- On 23 Nov 2022, Core Devs will be releasing merge-ready Client Images with Bellatrix epoch and TTD values baked in
- Announcements will be made in the #validators channel in Gnosis Discord and on Twitter

### For DApps

- The Merge will deprecate Gnosis' current [RandomAuRa on-chain randomness](https://developers.gnosischain.com/for-developers/on-chain-random-numbers/randomness-faqs), and developers should find alternatives
- Gnosis' Merge will be similar to the Ethereum Merge, and DApps should prepare accordingly for changes to `block.difficulty`, blockhash randomness, and block times and finalization.
- We recommend the Ethereum.org article on [How the Merge affects the Application Layer](https://blog.ethereum.org/2021/11/29/how-the-merge-impacts-app-layer)
- We recommend 0xMacro's post on [What Solidity Devs should know about Ethereum's Merge](https://0xmacro.com/blog/what-solidity-devs-should-know-about-ethereums-merge/)
