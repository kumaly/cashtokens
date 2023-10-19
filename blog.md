# Integrating CashTokens in our app Kumaly

## History

At Kumaly, we have spent the last two years researching and developing various projects based on blockchain technology. Our goal has consistently been to create something user-friendly for the average person, ensuring low fees and prompt response times.

We began our journey with Bitcoin Cash, intending to develop an app using SLP tokens. However, after several months of experimentation, it became evident that this model was unfeasible. Consequently, we decided to explore EVM on Smart Bitcoin Cash.

Transitioning from SLP to ERC721 was relatively straightforward, as the EVM toolset is far more advanced. Our primary concern was the block time of 5 seconds on Smart Bitcoin Cash, which resulted in the app feeling sluggish.

Subsequently, we shifted our development focus to EOS EVM, which offered low fees and 1-second blocks— a substantial improvement. Despite this, having to conduct a transaction for every NFT transfer remained cumbersome and costly, prompting us to develop a protocol for SFTs (Semi-Fungible Tokens) that better suited our needs. This required learning Solidity and becoming accustomed to tools like Hardhat.

We continuously monitored developments in Bitcoin Cash and were aware that CashTokens, a new solution for creating Fungible and Non-Fungible Tokens, was slated for release in May 2023. A few months later, we decided to integrate it into our app, recognizing the potential advancements it could bring to our projects.

# Week 1 - 18 Sept 2023

We spent the first week delving into the specifics of CashTokens and experimenting with the mainnet.cash library for seamless integration.

The first distinctive feature of EVM is that one doesn't have to deploy a smart contract to create a new token; formatting a UTXO in a specific way suffices. Thus, there's no need for tools like Hardhat.

Here’s some feedback on working with mainnet.cash so far:

- **Pros**
  - The library is well-thought-out and user-friendly.
  - The documentation is comprehensive.
  - The lead developer is active and extremely supportive on Telegram.
- **Cons**
  - Projects must be migrated to ESM to use it.
  - Getting it to work correctly in web workers is challenging.
  - It's heavy, with at least 1.5MB of decompressed payload (versus 0.5MB for ether.js).

The first half of the week was spent integrating mainnet.cash into our CLI, while the remainder was used exploring the various methods to create tokens with CT and posing questions on Telegram. We extend our gratitude to the very helpful community there!

Here are a few key takeaways from what we've learned:

- You create a token category instead of deploying a contract.
- The token category is the equivalent of the contract address on EVM.
- It allows the creation of Fungible Tokens (FT) and Non-Fungible Tokens (NFT).
- NFTs can have an associated FT; for instance, an NFT can represent a container like a popcorn box or a car, and the FTs can represent the items inside like popcorn or fuel, which can be transferred with or without the associated NFT.
- Unlike ERC-721, NFTs don't have an incremental ID. Instead, each has a unique ID termed "commitment," which can be any encoded hex value storing up to 40 characters. Although 2 NFTs in CashTokens can share an ID—offering a feature for creating SFTs—it’s the minter's responsibility to maintain uniqueness.
- To establish a new token category, one needs to consume a UTXO at vout=0, which, albeit somewhat inconvenient, has been automated in our bch library, eliminating the need for manual management.

# Week 2 - 25 Sept 2023 - Collectopia Integration Update

### Overview

This week has primarily focused on transitioning our application, Collectopia, from EOS EVM to BCH utilizing Cash Tokens. Collectopia is a Panini style game featuring 20 collections with a total of 1462 different assets.

### Application Status

The current status of the application with Cash Tokens (CT) integration can be viewed on [Youtube](https://www.youtube.com/watch?v=xvs61g_uZAQ&feature=youtu.be) [5 min]. Feel free to join our Telegram channel to access the live version and try it out!

Our [Telegram](https://t.me/kumalycom)
and
[Discord](https://discord.gg/ceXWjBQtZe)
channels.

### Integration Details

Previously, we used a single smart contract with our custom SFT protocol on EVM, which was effort-intensive to create but smoother than ERC721 for multiple asset manipulations. With Cash Tokens, we're opting for a single tokenID and the asset internal ID as commitment. Integration tests with CT are performing well, enabling the creation and transfer of over 1000 assets in a single transaction, significantly overcoming the limitations we had with EVM.

### Development of Booster System for Collectopia

We introduced a straightforward Boosters system where users can purchase and open SFT boosters for each collection or a pack containing random assets from all collections. This system is wallet-compatible and operates without the need for custom protocols or use of OP_RETURN, making it universally accessible.

In a former project employing EVM, we implemented a lootboxes system incorporated directly into a smart contract. While functional, it was expensive and lacked flexibility regarding modifications to drop rates post-deployment, with constraints limiting users to 9 lootboxes concurrently due to EVM’s inherent restrictions. Conversely, our integration with CT has enhanced system efficiency, transferring drop rolls to the backend script, enabling the acquisition/opening of up to 99 boosters devoid of blockchain restrictions. This modification not only facilitates smoother transactions but also ensures transparency and accuracy in the computation and validation of actual drop rates against those declared by Collectopia.

### Limitations and Challenges

Currently, we are experiencing a limitation related to the inability to consume boosters and mint assets in one transaction due to constraints from mainnet.cash, not CT, potentially resolvable in future updates. Collectopia has been deployed on chipnet, revealing smooth transactions with the Cashonize wallet, albeit without integration with BCRM, affecting metadata for our assets in other wallets, a situation slated for resolution next week.

CT deployment on testnet has exposed latency issues, with booster acquisitions taking 6 to 8 seconds compared to 3 seconds on EOS EVM testnet. Optimizations are underway to mitigate this delay. Additionally, CT has presented challenges in data readability compared to EVM, as it lacks the ability to listen to contract events logs or query a global state, with available solutions posing scalability concerns. However, the new "scantxoutset" RPC endpoint on the BCH node has addressed some of these concerns, enabling collection of all utxos for a given token category efficiently, albeit with some limitations related to transactions inside the mempool.

### Conclusion and Invitation

After two productive weeks working with CT, we find it to be a robust and efficient technology for our application's needs. We warmly invite users to join our Telegram and participate in testing our current integration on testnet.

[Youtube Video](https://www.youtube.com/watch?v=xvs61g_uZAQ&feature=youtu.be) of our weekly progress

Join [Telegram](https://t.me/kumalycom) NOW!

Join [Discord](https://discord.gg/ceXWjBQtZe) NOW!

# Week 3 - 2 Oct 2023 - Collectopia Integration Update

## Overview

This week was a blend of challenges and achievements:

- Faced issues with public Fulcrum nodes on chipnet, requiring us to set up our own.
- Struggled with crafting a seamless docker setup for BCHN and Fulcrum.
- Encountered a transaction size limit when users purchased multiple packs simultaneously.
- Had to overhaul our backend scripts to work around mainnet.cash limitations.

On the flip side:

- Drafted the initial set of game rules.
- Integrated BCH prizes into the leaderboard.
- Enabled buying and opening of packs/boosters in a single transaction.
- Added a step to display assets once a booster is opened.

## Progress Report

We've made significant headway, especially in transitioning our code base from EVM to UTXOs. CashTokens promise better scalability as we move forward.

## Limitations and Challenges

### Leaderboard

Querying real-time data for a dynamic leaderboard is tricky. While BCHN's "scantxoutset" endpoint is useful, it does not consider mempool data, causing leaderboard lag. We're working on a workaround, although it's not perfect.

### Mainnet.cash

The library is responsive; issues are usually fixed within hours. Our challenges aren't with the library per se but with its simplified interfaces like `tokenMint()`. While user-friendly, they don't mesh well with our advanced use-cases, necessitating a shift to raw functions and manual UTXO management.

### BCMR Integration

We intended to integrate our metadata with BCMR this week but found that it lacks scalability as asset and app numbers grow. More details on this issue can be found [here](./bcmr/analysis.md).

#### Identity Management

We're also grappling with substituting our EVM-based identity system with one built on CashTokens. Discussions with BCH community members are ongoing.

## Conclusion and Next Steps

Check out our weekly progress via our [Weekly YouTube Video](https://www.youtube.com/watch?v=bMjIYzpM7HY) and consider joining our [Telegram](https://t.me/kumalycom) and [Discord](https://discord.gg/ceXWjBQtZe) to test our current integration.

# Week 4 - 9 Oct 2023 - Collectopia Integration Update

## Overview

We spent most of the week optimizing performance and improving minor details. The app is now significantly smoother.

## Limitations and Challenges

### Fulcrum limitations

While conducting intensive testing on Chipnet, we opened numerous boosters and minted more than 7,500 NFTs in our test account. With so many UTXOs in a single address, the app's performance deteriorated to the point of being unusable.

One major limitation is that tools like Fulcrum only allow retrieval of the full list of an address's UTXOs without any filtering options. This results in a payload of around 3MB for 7,500 NFTs' UTXOs.

Each time a UTXO is consumed or created, the client must download the full 3MB again, leading to excessive bandwidth consumption and lag.

### Attack vector

Before the introduction of Cash Tokens, if someone spammed you with 7,500 BCH UTXOs, you could easily combine them into a single UTXO. With Cash Tokens, this is not possible because you first have to decide which tokens to keep and which to burn.

This creates an attack vector where a rogue actor could spam users with tens of thousands of cheap NFTs, locking their wallets in a state where they are unable to sync due to the oversized UTXO payload.

### Dealing with the problem

To address this issue within our app, we created a command to remove all unrelated tokens. However, even with this solution, some users with low-end mobile devices struggled to reduce their large UTXO counts and return the app to a functional state.

We suspect that this will be an ongoing issue for BCH apps until tooling improves to allow better UTXO filtering on the indexer side.

## Conclusion and Next Steps

Consider joining our [Telegram](https://t.me/kumalycom) and [Discord](https://discord.gg/ceXWjBQtZe) to test our current integration.
