# BCMR Analysis

Considering the largest token so far : GURU Nfts

OP_RETURN: 6a0442434d52201fb40dc51c97086490818b9186b04cce2c0d1fffae2803e1d42d182bebf142c342697066733a2f2f62616679626569653379336a336f6c6965776b79707773336e6535717368366d6433376f6c366f7077347867776b6f7236656232613665796a676d

HASH:
1fb40dc51c97086490818b9186b04cce2c0d1fffae2803e1d42d182bebf142c3

URL:
ipfs://bafybeie3y3j3olieywkyqws3ne5qsh6md37ol6opw4xgwkork6eb26a6eyjgm

## Verifing the file hash

```
$ wget https://ipfs.io/ipfs/bafybeie3y3j3oliewkypws3ne5qsh6md37ol6opw4xgwkor6eb2a6eyjgm/ -O file.json
$ shasum -a 256 file.json
1fb40dc51c97086490818b9186b04cce2c0d1fffae2803e1d42d182bebf142c3
```

Hash is valid

## Concerns

```
$ du -sh file.json
2.3M	file.json
```

Having to download 2.3MB of datas to display 1 NFT is pretty bad.

Moreover, nothing prevent the file to be 100MB or more.

## Using an indexer

https://bcmr.paytaca.com/api/tokens/f54ce0297a4017cc922aacde5f7abe7a8397a1058b879f5eb9e2a643d4ec2301/

```
{
name: "BCH Gurus",
uris: {
web: "https://bch.guru/",
icon: "ipfs://bafybeigw5korsj6gfjc54xetmatbmas7dev5l52jnmavnotzn2zkbqqnee/guru-icon.png",
image: "ipfs://bafybeigw5korsj6gfjc54xetmatbmas7dev5l52jnmavnotzn2zkbqqnee/guru-image.png",
reddit: "https://www.reddit.com/user/bch_guru",
twitter: "https://twitter.com/bch_guru",
youtube: "https://www.youtube.com/@BCH_Guru",
telegram: "https://t.me/bch_guru",
instagram: "https://www.instagram.com/bch_guru"
},
token: {
symbol: "GURU",
category: "f54ce0297a4017cc922aacde5f7abe7a8397a1058b879f5eb9e2a643d4ec2301"
},
description: "BCH Guru Collectible NFTs, associated with the bch.guru price-prediction game.",
is_nft: true
}
```

There is no commitments list, so you have to know them to lookup for a specific NFT informations.

Moreover, there is no way to verify that those datas are valid because they are not signed.

https://bcmr.paytaca.com/api/tokens/f54ce0297a4017cc922aacde5f7abe7a8397a1058b879f5eb9e2a643d4ec2301/0d23/

```
{
name: "BCH Gurus",
uris: {
web: "https://bch.guru/",
icon: "ipfs://bafybeigw5korsj6gfjc54xetmatbmas7dev5l52jnmavnotzn2zkbqqnee/guru-icon.png",
image: "ipfs://bafybeigw5korsj6gfjc54xetmatbmas7dev5l52jnmavnotzn2zkbqqnee/guru-image.png",
reddit: "https://www.reddit.com/user/bch_guru",
twitter: "https://twitter.com/bch_guru",
youtube: "https://www.youtube.com/@BCH_Guru",
telegram: "https://t.me/bch_guru",
instagram: "https://www.instagram.com/bch_guru"
},
token: {
symbol: "GURU",
category: "f54ce0297a4017cc922aacde5f7abe7a8397a1058b879f5eb9e2a643d4ec2301"
},
description: "BCH Guru Collectible NFTs, associated with the bch.guru price-prediction game.",
type_metadata: {
name: "BCH Guru #3363",
uris: {
icon: "ipfs://bafybeibqwrxiwz363ezr54rzwchf6jfbweop2fvbpqh2w33pxye3uk2hxy/3363-icon.png",
image: "ipfs://bafybeibqwrxiwz363ezr54rzwchf6jfbweop2fvbpqh2w33pxye3uk2hxy/3363.png"
},
extensions: {
attributes: {
Eyes: "Raised Eyebrow Right",
Head: "Clown Hair",
Tone: "Tone 2",
Asset: "Bitcoin Cash",
Mouth: "Gritted Teeth",
Lyrics: "",
Clothes: "Pirate Guru",
Glasses: "",
Ear Left: "",
Ear Right: "",
Hand Left: "",
Background: "Underwater Scene",
Hand Right: "",
Crystal Ball: "Fiery Fortune Oracle"
}
},
description: "Guru #3363 from the BCH Guru NFT collection"
},
is_nft: true
}
```

You can query a specific commitment, but again, nothing is signed so the datas could be invalid.

## Thoughts

In it's current state, BCMR as severe limitations.

The file linked from the OP_RETURN should not be a full blob of datas but an index of what datas are available, and for each datas a hash.

From the URL containing the index, happending /hash should give access to a specific part of the BCMR datas.

That way, one can query a light signed index and only part of the datas and then check that those jsons hashes match the one in the index.

An advanced search would also be possible, for example:

```
URL/search?commitments[]=1,commitments[]=2,commitments[]=3
```

Would provide a json array with the 3 commitments, each one having to match the hash from the index.

## Main considerations

- You dont want to download more datas than required, that slow everything down
- You also dont want to do one query per piece of datas, you have to be able to select what you need using a DTO.
