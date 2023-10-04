# Identity System Using Cash Token - Proposition 01

## Objectives

Our goal is to allow BCH participants to register an identity by creating their own token category associated with their BCMR data.

The process has to be decentralized and censorship-resistant while protecting end users from impersonation and scammers as much as possible.

## Creating an Identity

To create a new identity:

- Mint a new NFT token category
  - The capability has to be mintable
  - The username is stored inside the commitment
  - The username can be up to 25 characters
  - Characters allowed are a-z, 0-9, underscore, and periods
  - The dot character is not allowed
- That transaction has to have a valid Identity BCMR payload

Reasoning:

- The dot character is reserved for sub-accounts separation

## Username Conflicts

There is no practical system that allows unique usernames on a blockchain without using either a central authority or a suffix like cash accounts do.

Instead, we use a system where multiple identical usernames can exist without a problem, and where anyone is free to create an identity for any username. No validation, no restriction.

## Identity Discovery

#### Scanning QR Code

By displaying your tokenID QR Code, others can register your identity inside their wallet and keep it up-to-date from your BCMR data.

#### Sending an NFT (i.e. business card)

If you know someone else's identity (i.e. tokenID) or BCH address, you can mint a new NFT from your own identity and send it to that person. That NFT has to be of capability none and should be considered like a business card. Only the owner of the identity must have a mintable NFT for it.

Depending on the wallet configuration, sending a business card NFT might not show up and might require that the wallet user search for the new identity using the username registered in the mintable NFT.

Scenario:
Alice sends a new Business Card NFT to Bob from her Identity NFT.
Bob doesn’t see Alice's NFT showing up in his wallet and has to search for "alic\*" to make it appear.
Bob can then choose to accept that identity.
If Alice and Bob are not already in some form of communication, Alice can't force her identity on Bob without asking him to look for it.

#### Searching for an Identity

A user can search for a username from his wallet.

- If there is only one username registered that matches, then it's presented
- If there are multiple registrations, we add the first digit of the tokenID of each at the end
- The user selects the proper identity according to its meta data
- It's the user's responsibility to check that this identity is valid
- It might make sense to not allow the user to send assets to an identity until he has declared that it's valid

Reasoning:

- By using "username" + "tokenID" we get the similar properties of cash accounts
- alice, if there is only one "alice" username registered
- alice1 and alice8 if there are multiple "alice" where 1 and 8 are the first digit of their tokenIDs
- Unless the user is in contact with multiple "alice" identities, the wallet will present the one he chooses as "alice" and not "alice1" or "alice8".

#### Communicating an identity verbaly

[Thinking process over there](./protpoal_01-verbaly.md)

## Identities Weight

- Each identity can vote for other identities by using a dedicated slot in its BCMR data
  - You've verified that this person is the "alice" you were looking for
  - So you vote for her to give her some weight
  - That doesn't mean other "alice" are not real, just that you're supporting that one

In this system, there is no central authority involved and a lot of username spam.

## Identities Registries

The system described so far allows identities to exist without any oversight. It's great for advanced users, less so for the average Joe.

Specialized registries (managed by wallet providers for example) can offer an extra level of filtering:

- Some identities are manually verified and granted maximum weight
  - They don't have any special power
  - They just have the ability to transfer weight to other linked identities from their BCMR
- These identities vote for other identities to pass weight

So, in a sense, those identities are trusted people at the root of the Bitcoin Cash ecosystem.

#### Usernames Groups

- Usernames are ranked in 4 groups
  - 1 - Not problematic (most usernames)
  - 2 - Can cause some damage if not the real person
  - 3 - Can cause a lot of damage
  - 4 - Domain names (username with dot)
- Group selection is based on the username length and its value on Web2 platforms
  - Most usernames have little value
  - We aim to protect against the rest
- Usernames of group 2 are only presented to users in their wallet if they have collected enough weight
  - They can also do a verification process to get manual weight
- Usernames of group 3 require a verification process to be presented to users
- Usernames of group 4 require a DNS check with the tokenID

#### Registry Role

The role of the registry is not to have control over who owns what username, because anyone can own any username by creating an NFT with the username as commitment.

Their role is to filter what information is presented to the users inside their own application based on the risk analysis of each username.

Any user can still add any other identity manually like he would do with his contacts list on his mobile phone. He is in control.
Registries also don't handle the metadata of identities, they are stored inside BCMRs.

Registries are transparent and open; anyone can use their data to filter user lists inside his own app without the need to do the extra work. Ideally, multiple registries collaborate to integrate identities from different geo locations.

#### Wallets

Wallets can set up one or more registries to handle all the identity filtering. But they should always allow the user to add any identity of his choosing manually.

#### Registry Pointers

A registry is a pointer toward a tokenID.

kumaly.com is one registry.
alice.kumaly.com points to one "alice" tokenID.
bob.kumaly.com points to one "bob" tokenID.

Inside kumaly.com registry, only one "alice" and "bob" can exist. But inside another registry, like paytaca.com, those usernames can point to different tokenIDs.

An identity that has been selected by the major registries to be "alice" should have the maximum chance to be presented to the end users when they search for "alice".

## Sub Accounts

Each identity can have sub-accounts declared inside the identity and used by the registries like:

perso.alice.kumaly.com

work.alice.kumaly.com

Those sub-accounts can only be manipulated by the tokenID pointed to by "alice".

## Identity for Newcomers

While crypto-holding users can easily create an identity, the need to create an NFT makes it challenging for newcomers until they acquire some BCH.

#### Sponsored by Wallet

Wallets can facilitate the identity creation for new users:

- Users select a username.
- Users complete a captcha.
- The wallet issues a registration NFT and sends it to the user's address.
  - The commitment is the username.
  - The NFT can solely be used to mint a new Identity NFT with the same commitment.
  - The NFT input includes sufficient satoshis for an additional transaction.
- The wallet detects the registration NFT and uses it to establish the user's identity.

Although the user now has an identity, they might still encounter difficulties updating their BCRM data without access to coins. However, they now have a presence in the system.

#### Sponsored by Friend

Many users will be introduced to the system through friends sharing their experiences.

A friend can assist in setting up the wallet and then transfer their NFT Business Card to be included in the new user's contacts.

Subsequently, this NFT becomes expendable and can be used as a registration NFT to craft the new user's identity. It's essential to ensure that the business card NFT carries enough BCH to cover an additional transaction.

Sending a Business Card NFT to a friend offers several advantages over having them scan a QR Code Identity and sending a small BCH amount for identity registration:

- It's simpler and demands fewer steps. New wallets might even auto-accept the initial friend identity, eliminating the approval phase.
- Gifting a Business Card NFT is free of social expectations, whereas providing a minimal BCH amount can feel awkward (who wants to send only 5 cents?). Including the BCH within the Business Card NFT sidesteps this issue.
- The new identity might receive an initial weight boost if the business card's identity carries any. Registries can ascertain this weight by examining the identity's creation process (i.e., was an NFT used, and who initiated it?).
- By identifying users who introduce active newcomers to the system (and whose business card NFTs are frequently utilized), the ecosystem might reward these evangelists. For instance, a wallet or app with active users brought in by an evangelist might send them BCH or other NFTs as a token of appreciation.
