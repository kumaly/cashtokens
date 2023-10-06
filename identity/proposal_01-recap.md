# Identity

An identity is a unique tokenID NFT with mintable properties, managed by an identity covenant. The NFT can:

- Be transferred to a new address
- Be used to mint a new Business Card NFT with no capabilities but the same commitment
- Be transferred to change the BCMR payload

# Registries

- kumaly.com
- paytaca.com

Registries are special identities that provide pointers to other identities. They are managed by wallet providers that already have the users' trust.

Their tokenID is recorded at the DNS level:

- identity.kumaly.com => tokenID
- identity.paytaca.com => tokenID

Each registry references the other registries' tokenIDs in its metadata. Clients start with one registry (the one from their wallet) and then check other registries to validate that the original tokenID from the DNS is correct.

# Identities

Identities are registered in registries once they've been created on-chain.

Each identity has a pointer to its tokenID in the following format:

- alice => alice@kumaly.com => tokenID
- bob => bob@paytaca.com => tokenID

Registries can record different identities for the same pointer:

- alice => alice@kumaly.com => tokenID_1
- alice => alice@paytaca.com => tokenID_2

Registries maintain an index of other registries' identity pointers. When a client requests an identity's tokenID, it should query multiple registries at once.

- What is alice@kumaly.com's tokenID?
  - kumaly.com: tokenID_1
  - paytaca.com: kumaly.com should answer tokenID_1

If a registry changes an identity's tokenID, other registries shouldn't update before a quarantine period and should continue to report the previous tokenID, thereby forcing the wallet to alert the user to a potential issue. In the case where a registry's identity is corrupted, other registries will prevent the user from accessing a rogue tokenID.

# Sub-Identities

Each identity can declare sub-identities within its own metadata:

- alice => tokenID_1
- work.alice => tokenID_2
- perso.alice => tokenID_3

# Addresses

Identities define their on-chain addresses within their metadata:

- bch => bitcoincash:q...
- evm => 0x...

Addresses can be accessed using:

- alice#bch => bitcoincash:q...
- alice#evm => 0x...
- alice#bch@kumaly.com => bitcoincash:q...

# Token Pointers

Identities can create tokenIDs, for example, an NFT collection. They can declare a pointer to a tokenID in their metadata:

- alice:collection1 => tokenID
- alice:collection1@kumaly.com => tokenID

A collection can be on both BCH and EVMs:

- alice:collection1#bch@kumaly.com => tokenID
- alice:collection1#eosevm@kumaly.com => 0x

# Email Format

Send an NFT to alice:

- alice@kumaly.com

Send an NFT to alice using BCH:

- alice#bch@kumaly.com

Send an NFT to alice using EOS EVM:

- alice#eosevm@kumaly.com

Send an encrypted message to alice:

- alice@kumaly.com

# Messaging

To send an encrypted message:

- Write the message
- Select alice@kumaly.com
- Your wallet signs and encrypts the message using eccrypto
- Your wallet looks up the tokenID for alice@kumaly.com
- Your wallet registers the message for that tokenID on a messaging service
- Alice's wallet checks for messages on the messaging service
  - Only messages with a valid signature from her identity are revealed
- Alice's wallet decrypts the message using her private key
- Alice's wallet checks the message signature against your identity for the security of origin
- Alice reads your message
