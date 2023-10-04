# Communicating an Identity Verbally

Verbally conveying the identity's tokenID poses significant challenges.

Regardless of the method chosen, there will always be trade-offs. The goal should be to provide an experience akin to what users are already accustomed to.

## Methods

### Cash Accounts Inspired Protocol

This would involve registering a tokenID similarly to how addresses are registered in Cash Accounts.

**Cons**:

- Introduces additional steps and another protocol to manage.
- Prone to errors, for instance:
  - If instructed to search for "giviz#100;", a typo like "giviz#1000;" could lead to scams.
  - Sending funds could mistakenly go to the wrong person.
- Necessitates an indexer for offchain data rather than signed BCMR.
- From a user perspective, the format appears unconventional.

### Wallet Registries

For example, instructing someone to look up "giviz@kumaly.com" or "giviz@paytaca.com" because these registries link "giviz" to the corresponding tokenID.

**Pros**:

- The format is intuitive for end-users.
- Fewer chances for errors as scammers must register and maintain their status with the registries.
- Registries employ BCMR.

**Cons**:

- Reliance on registries, though alternative solutions may also depend on an indexer.

### Personal Domain Usage

If a domain has its tokenID set up in its DNS, one can determine its BCMR identity and find usernames from that.

Searching for "giviz@example.com" would inspect "example.com" for a DNS identity and then search within that identity's data to locate the tokenID for "giviz".

For secure or corporate needs, this is ideal. Average users might find registries more convenient.

## Design

The method of verbally communicating the identity is just one part of the puzzle, with the email-like format being the most promising. Another essential aspect to consider is how users will interpret and utilize this verbal representation.

For the vast majority (99.9%) of users, they will input this string into their wallet, which should be the primary site for security checks given the inherent trust users place in it due to their private key.

## Scenario

1. I share "giviz@kumaly.com" with you.
2. You enter it into your wallet. Following this, your wallet:
   - Derives the identity for "kumaly.com" from its DNS.
   - Cross-references with other registries for potential warnings associated with the "kumaly.com" identity.
   - Accesses "kumaly.com" BCMR to locate "giviz" and retrieve its tokenID.
   - Engages with the identified tokenID's BCMR.
   - Pulls data for that BCMR and verifies its hash.

By the end, you'd have "giviz"'s identity authenticated and stored in your wallet. As I introduced you to "kumaly.com", it indicates my trust in them.
