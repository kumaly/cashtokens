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

The current status of the application with Cash Tokens (CT) integration can be viewed on [Youtube](https://www.youtube.com/watch?v=0VgP1Ujbl-c) [5 min]. Feel free to join our Telegram channel to access the live version and try it out!

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

[Youtube Video](https://www.youtube.com/watch?v=0VgP1Ujbl-c) of our weekly progress

Join [Telegram](https://t.me/kumalycom) NOW!

Join [Discord](https://discord.gg/ceXWjBQtZe) NOW!
