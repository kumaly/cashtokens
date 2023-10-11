# Identity System Using Cash Token - Proposition 01 Debrief

This is a debrief about [proposal 01](./proposal_01-recap.md) we had on [Twitter Spaces](https://twitter.com/GeneralProtocol/status/1710485422781141372).

## Using NFT for identity registration

The core of the proposal to use a tokenID to create an identity didn't face any objections.

## NFT Business Card

The ability to use NFTs created from one's identity as a business card was well-received.

## Search for a username requires the use of full usernames

Having the `alice` username registered in multiple registries and pointing to different tokenIDs is considered confusing for users. It was discussed that when a user searches for an identity, the full username pointer should be revealed, such as `alice.kumaly.com` and `alice.paytaca.com`, to allow the user to make the correct choice. Afterward, the identity can be presented as `alice`.

## Dependency on DNS for Registries

Concerns were raised about building a system where the registry's identity (i.e., their tokenID) is sourced from the DNS configuration of their domain. This is not viewed as a secure setup since domains can be seized or corrupted.

## Performance of Registries

There are questions about how the system will scale once registries handle millions of identities without becoming centralized.

## Make use of composition

There's a preference for a system designed with building blocks that don't rely on each other for the entire system to function. Instead, each block should add value to other blocks if deployed.

## Registries push more responsibility to wallets

If registries are managed by wallet providers, this adds an additional responsibility that providers might not want to assume.

## Coherence between wallets

The system's efficacy hinges on all wallets agreeing to utilize it in a consistent manner.

## Sharing part of your private identity

While having a public identity is beneficial, a significant portion of one's identity should remain private and be shared on a case-by-case basis. Considerations include how this is managed and how data is securely transferred.

## Use of Cash Accounts

Utilizing Cash Accounts to associate a username with a tokenID is feasible. A notable feature is that the system can operate without an indexer.

## Restoration of user data

In scenarios where a user loses access to their device and must restore from a mnemonic, how are their data, such as contact lists, restored?
