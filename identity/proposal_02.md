# Identity System Using Cash Token - Proposition 02

This proposition resolves some of the issues identified in Proposition 01.

It presents a new solution to perform a Username to AuthBase mapping without using DNS lookups.

Attention has also been focused on having a simple and scalable solution.

## Username Format

All usernames will have the following format:

```
username@domain.tld
```

We identify two types of registrations:

1. Private registration
2. Public registration

## Private Registration

Businesses or people that control their own domain name want a username looking like this:

```
tim@apple.com
```

For them, it doesn't make sense that the attribution of a username is permanent. They need to be able to revoke the identity of an employee who leaves the company.

## Public Registration

Most people have their identity tied to an email address they got from an email provider. They consider that this identity is definitive. That should be the case.

Such usernames will look like this:

```
alice@kumaly.com
alice@paytaca.com
alice@domain.com
```

Like email providers, many domains can be used to give free usernames to users.

## Domain Lookup for AuthBase

To resolve a username, the wallet first has to get the domain AuthBase.

### Private Domains

In the case of private domains, the wallet does a DNS lookup to:

```
authbase.apple.com > AuthBase
```

### Public Domains

In the case of public domains, the AuthBase is hardcoded in the wallets.

```
kumaly.com > AuthBase_1
paytaca.com > AuthBase_2
domain.com > AuthBase_3
```

The user already trusts the wallet to protect his private key and will trust it to have properly hardcoded public domains.

## Username Lookup for AuthBase

Having the domain AuthBase is only half the job; what we want is an AuthBase for the actual username identity.

### Private Domains

As private domains keep ownership of their usernames, they list all the mapping inside their identity data.

From the domain AuthBase, the wallet has resolved the AuthHead and can see the following:

```
tom > AuthBase_1
marc > AuthBase_2
john > AuthBase_3
```

### Public Domains

Public domains do not keep ownership of their usernames; once a username has been allocated to a user, it's definitive.

That means that we can't store the mapping inside the public domain identity data because it's under direct control of the domain owner.

Moreover, storing potentially millions of usernames there would not be a scalable solution.

We will explain how to resolve a public username in the following section.

## Resolution of a Public Username

We consider a public domain hardcoded inside the user's wallet:

```
domain.tld > AuthBase_1
```

We also consider a username to resolve:

```
username@domain.tld > ???
```

We use that username as the Public Key to derive a BCH address:

```
bitcoincash:qp8f45vwmlplyckyq2t4cc94smuraj3795fmzldcma
```

That conversion is deterministic.

We do not own the Private Key to that address; probably no one does.

We get the transaction history of that address using one RPC call:

```
6a23eb9eb7825cb697b4b6d71859611eef97939134dd0df86b99a64ac8f499a7
```

If there are zero transactions on that address history, then the username resolution fails.

With the list of transactions, we do an output check:

- If any output has been spent by any of the transactions in the history, then the username is burned because that means that someone has the private key to that address.
- Otherwise, we look for the oldest transaction with one input and one OP_RETURN.

The transaction has to look like this:

```
Input 0 : Signed by AuthBase_1
Output 0 : OP_RETURN with AuthBase_2 as data
```

Any other transaction is ignored.

If there are multiple matching transactions within the same oldest block, then the username is burned from inception.

We now have the username AuthBase:

```
AuthBase_2
```

We only need to resolve it to the most recent AuthHead and get the username identity data from the BCMR payload.

## Performance Considerations

- No indexer required
- No DNS required
- No squatting
  - The same username@ can exist on multiple public domains
  - You need the domain owner's signature to get the username, so someone else can't try to reserve it when you do, as they could with CashAccounts, for example.
- No third party in control of your username
  - Once allocated, the public domain owner has no control or right over an username
- Multiple public domains to choose from
- Easy username to BCH address resolution
  - That address is never presented to the user
  - It's only used to access the OP_RETURN in a deterministic way
  - The final identity BCH address is stored in its BCMR data
  - Which makes it possible to rotate it to increase privacy with each identity datas update
- Only one BCH tx to register a username
- Only one or two light RPC calls to get the username resolution

It should be noted that any given address can be spammed with tons of UTXOs to slow down the resolution, but it's not something impactful at scale for the whole system.

## Scams

With that design, reputable actors will seek to use their own domain names to enforce their credibility and reassure users that they are looking at the correct identity.

We suggest establishing a list of popular brands (Apple, Binance, etc.) and key accounts (support, admin, etc.) that public domains should ignore when registering usernames.

Wallets could also embed the same list to filter out those usernames.

## Security

While it might seem dangerous to store the AuthBase anchor inside an address no one has the private key for, it's not the case.

If by miracle someone gains access to the private key matching a username, they can't do anything because:

- The creation of the username has to be signed by the public domain owner
- The information of the AuthBase is stored on the first valid OP_RETURN, which already exists
- If the address has any outgoing transaction, then the identity is burned
- So, worst-case scenario, the user would have to migrate to another identity.

Considering how hard it would be for anyone to get the Private Key matching one of these addresses and considering all the benefits of this design, we think that it's a valid option.

## CashToken

This proposal has focused on AuthBase instead of CashToken for clarity.

As it's possible to use any tokenID as a new AuthBase, we can still combine both.

By doing so, a user can create a new NFT category with an output zero that will be used as his AuthBase.

### Business Card

By sending another person an NFT made from his AuthBase token category that has the username as a commitment, a user is sending his business card.

The receiving wallet will detect the username as a commitment, resolve it, and confirm that the AuthBase for that username matches the tokenID. If so, the business card is valid.

The rest of our previous proposal stays the same, and the business card can be used to send the user username + funds to allow the receiver to have everything he needs to create his own account easily.
