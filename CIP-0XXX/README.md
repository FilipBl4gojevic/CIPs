---
CIP: ?  
Title: CNFT Community Royalties Standard 
Authors: NFT Guild <cardanonftguild@gmail.com>, CENT Stake Pool <social@cent.stakepoolcentral.com>, Filip Blagojević <filip.blagojevic12@gmail.com>  
Discussions-To: https://forum.cardano.org/t/cip-royalties/68599   
Comments-URI: https://forum.cardano.org/t/cip-royalties/68599
Status: Draft  
Type: Informational
Created: 2022-10-16  
License: CC-BY-4.0   
Requires: CIP 27  
Replaces: CIP 27  
---

## Simple Summary

Royalties are an important part of the Cardano NFT ecosystem and its sustainability. They are one of the main ways to continuously fund project creators, authors and artists through sales on the secondary market and, if implemented correctly, remove the need for outside funding. This CIP proposes an update to [CIP 27](https://github.com/cardano-foundation/CIPs/tree/master/CIP-0027) which has not really been adopted by the community. The aim is to have a simple, easy to use standard that supports distributing royalties to multiple addresses and updateable royalties.

## Abstract

This CIP proposes a creation of two different policies:
1. Main project policy - can be open or closed - contains all the authors NFTs
2. Royalty policy - can be open or closed, but has to be open to have updateable royalties - contains only a single royalty NFT

Main project policy can be open or closed, only important thing is that it contains a royalty reference token. Royalty reference token points to the royalty policy which always contains a single royalty NFT. This royalty NFT can be burned and re-minted at any time. This gives flexibility to the authors to update the royalties as they wish and an exact place for marketplaces to read the data. Expanded royalty NFT standard is presented in this CIP to support multi-address royalty dispersion.

## Motivation

The CIP 27 was written soon after NFTs became possible on Cardano. Motivation behind it was to have some sort of a quick royalty standard that the community can follow without the need to implement smart contracts. It is nice and simple, but somewhat limited. Unfortunately, it was not largely accepted by the community and by the key stakeholders in the Cardano NFT ecosystem. Authors wanted more flexibility with the royalties while keeping the simplicity of CIP 27. It also became an unwritten rule that NFT policies should be locked. That is why we expanded CIP 27 to adapt to the development of Cardano. 

## Specification

### Royalty token standard (777)
We are expanding the 777 token described in CIP 27 simply by stating that more than one address can be added to the array within JSON and changing the rate property to array type. 

```
{
	"777": {
		"rate": [
            "0.1",
            "0.12",
            "0.05]
		"addr": [
			"addr1q8g3dv6ptkgsafh7k5muggrvfde2szzmc2mqkcxpxn7c63l9znc9e3xa82h",
			"pf39scc37tcu9ggy0l89gy2f9r2lf7husfvu8wh",
            "addr1q8l6e8chpp2hna268ktg3tu6l37qe3e3n8qez9csjxw75s8l4j03wzz4086",
            "450vk3zhe4lrupnrnrxwpjyt3pyvaafqqc94x78",
            "addr1qycgag4mkeghnhfa2dcm9pgqrdnteu6t6rh7exkz8r6cdhutz5rw6ughfl3",
            "z0ct20ayusyaqkzwvwssgjce83re3xwvqe09gm4"
		]
	}
}
```

In this example following royalties are applied to following addresses:

| Royalty  |                                      Address that receives the royalty                                  |
| -------- | ------------------------------------------------------------------------------------------------------- |
| 10%      | addr1q8g3dv6ptkgsafh7k5muggrvfde2szzmc2mqkcxpxn7c63l9znc9e3xa82hpf39scc37tcu9ggy0l89gy2f9r2lf7husfvu8wh |
| 12%      | addr1q8l6e8chpp2hna268ktg3tu6l37qe3e3n8qez9csjxw75s8l4j03wzz4086450vk3zhe4lrupnrnrxwpjyt3pyvaafqqc94x78 |
| 10%      | addr1qycgag4mkeghnhfa2dcm9pgqrdnteu6t6rh7exkz8r6cdhutz5rw6ughfl3z0ct20ayusyaqkzwvwssgjce83re3xwvqe09gm4 |

Addresses are broken up in 2 array elements because of 64 character per line limitation.

### Royalty reference token standard (776)
We are defining a new token standard to reference a policy ID which keeps the royalty token.

```
{
	"776": {
		"policy": "d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc"
	}
}
```

## Rationale

Several other designs were considered:

#### 1. Simply keeping the NFT policy open and minting and burning the royalty token
This would be a minimal expansion of CIP 27 but would force the policy creator to have an open policy

#### 2. Create a standardized royalty transaction
This on-chain transaction would contain a standardized message in metadata with updated royalty fee. This idea was abandoned because of a radical change from the current practice.

#### 3. Referencing the royalty policy ID from the main policy ID
This idea considered adding a field in the policy script that would reference another policy that contains the 777 token. This solution would be a bit more elegant because it would remove the need to mint a 776 token, but it would require a protocol level change, so it was abandoned.

#### 4. Creating a fungible token
This idea consisted of reference token like in the currently proposed solution, but instead of having a 777 token, the referenced policy would contain fungible tokens and royalties would be distributed to the addresses that own fungible tokens in percentage that is owned. For example, if the total supply of fungible tokens is 1000 and wallet A holds 100 tokens, wallet A receives 10% royalties. We are still considering this idea and how to possibly include it.

#### 5. Smart contract
Creating a standard smart contract for royalty dispersion, auditing it and testing it would be a final solution. Currently, this idea takes a lot of time, money and energy and we came to the conclusion that it is not the right moment for it. We do, however, see a smart contract solution replacing this CIP in the future.

  
## Backward Compatibility

This CIP was developed keeping in mind the state of the Cardano NFT ecosystem at the time of writing and we made our best efforts how to keep it as backwards compatible as possible. Existing projects with open policies can easily mint a 776 token and open a new policy with the royalty token, so this CIP is fully backwards compatible with them. Unfortunately, projects with closed policies are not compatible with this CIP. If enough interest is shown from all stakeholders, it is possible to implement a standard for closed policy projects based on idea 2 described above.


## Copyright

  

This CIP is licensed under Apache-2.0
