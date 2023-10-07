# BCMR

This document expresses what BCMR could be if it were designed like our current system developed for Kumaly.

## Fundamentals

- Each identity has a JSON file
- File hash is stored on-chain with URL
- On-chain data is read by indexers
- Clients use data from indexers for performance

## File Format

- Identity information not localized
- Locales
  - en: hash + URL
  - fr: hash + URL
- Resources
  - image: hash + URL
  - vignette: hash + URL
- Indexes
  - Type A
    - Item A1: hash + URL
    - Item A2: hash + URL
  - Type B
    - Item B1: hash + URL
    - Item B2: hash + URL

By accessing an identity in BCMR, the client gets the core information and a list of additional resources.

- Payload is reduced
- Client can query more data if needed
- When payload is refreshed
  - Client can identify parts of the data that changed by comparing hashes
  - Client can query only updated data instead of everything at each update

## Query

A client has a list of identities they want to keep in sync. For each identity, they know their tokenID.

#### DTO

Downloading a registry to access one identity is not optimal.

The client has to be able to specify multiple identities they want to download.

By connecting to an indexer, clients can:

- Construct a DTO that specifies what they need
  - Identities A, B, and C with locales in EN and the image resource
  - Identities that have this or that feature
- Pass a bloom filter of their local data
  - I already have identities A with hash A1 and B with hash B1
  - Indexer compares the bloom filter and only returns updated identities
  - This reduces the payload and speeds up everything

#### Items

Items are additional chunks of data associated with the identity. For example, an NFT collection and data for each NFT.

Most clients won't care about these data when they access the identity, and they might care about some of these data in certain cases.

If a client receives an NFT like a BCH Guru, then it will access the associated identity and then request the data for the specific item. It doesn't need the data for the other 9,999 NFTs in that collection!

Items could be in a free format, but that will provide little value for clients who won't be able to consume them.

Instead, items should be formatted from JSON Schemas specific to their use case and understood by clients.
