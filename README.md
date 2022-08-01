# Introduction

The Harmony Name Service (HNS) is a distributed, open, and extHNSible naming system based on the Harmony blockchain.

HNS’s job is to map human-readable names like ‘alice.one’ to machine-readable identifiers such as Ethereum addresses, other cryptocurrency addresses, content hashes, and metadata. HNS also supports ‘reverse resolution’, making it possible to associate metadata such as canonical names or interface descriptions with Ethereum addresses.

HNS has similar goals to DNS, the Internet’s Domain Name Service, but has significantly different architecture due to the capabilities and constraints provided by the Harmony blockchain. Like DNS, HNS operates on a system of dot-separated hierarchical names called domains, with the owner of a domain having full control over subdomains.

Top-level domains, like ‘.one’ and ‘.test’, are owned by smart contracts called registrars, which specify rules governing the allocation of their subdomains. Anyone may, by following the rules imposed by these registrar contracts, obtain ownership of a domain for their own use. HNS also supports importing in DNS names already owned by the user for use on HNS.

Because of the hierarchal nature of HNS, anyone who owns a domain at any level may configure subdomains - for themselves or others - as desired. For instance, if Alice owns 'alice.eth', she can create 'pay.alice.one' and configure it as she wishes.

HNS is deployed on the Harmony main network and on several test networks. If you use a library such as the [HNSjs](https://www.npmjs.com/package/@ENSdomains/ENSjs) Javascript library, or an end-user application, it will automatically detect the network you are interacting with and use the HNS deployment on that network.

You can try HNS out for yourself now by using the [HNS Manager App](https://hnsdomains.one), or by using any of the many HNS enabled applications on [our homepage](https://HNSdomains.one).

## HNS Architecture

HNS has two principal components: the [registry](contract-api-reference/ENS.md), and [resolvers](contract-api-reference/publicresolver.md).

![](<.gitbook/assets/ENS-architecture (1).png>)

The HNS registry consists of a single smart contract that maintains a list of all domains and subdomains, and stores three critical pieces of information about each:

> * The owner of the domain
> * The resolver for the domain
> * The caching time-to-live for all records under the domain

The owner of a domain may be either an external account (a user) or a smart contract. A registrar is simply a smart contract that owns a domain and issues subdomains of that domain to users that follow some set of rules defined in the contract.

Owners of domains in the HNS registry may:

> * Set the resolver and TTL for the domain
> * Transfer ownership of the domain to another address
> * Change the ownership of subdomains

The HNS registry is deliberately straightforward and exists only to map from a name to the resolver responsible for it.

Resolvers are responsible for the actual process of translating names into addresses. Any contract that implements the relevant standards may act as a resolver in HNS. General-purpose resolver implementations are offered for users whose requirements are straightforward, such as serving an infrequently changed address for a name.

Each record type - cryptocurrency address, IPFS content hash, and so forth - defines a method or methods that a resolver must implement in order to provide records of that kind. New record types may be defined at any time via the EIP standardization process, with no need to make changes to the HNS registry or to existing resolvers in order to support them.

Resolving a name in HNS is a two-step process: First, ask the registry what resolver is responsible for the name, and second, ask that resolver for the answer to your query.

![](<.gitbook/assets/data_flow.png>)

In the above example, we're trying to find the Ethereum address pointed to by 'foo.one'. First, we ask the registry which resolver is responsible for 'foo.one'. Then, we query that resolver for the address of 'foo.one'.

### Namehash

Resource constraints in smart contracts make interacting directly with human-readable names inefficient, so HNS works purely with fixed length 256-bit cryptographic hashes. In order to derive the hash from a name while still preserving its hierarchal properties, a process called Namehash is used. For example, the namehash of 'alice.one' is _0x787192fc5378cc32aa956ddfdedbf26b24e8d78e40109add0eea2c1a012c3dec_; this is the representation of names that is used exclusively inside HNS.

Namehash is a recursive process that can generate a unique hash for any valid domain name. Starting with the namehash of any domain - for example, 'alice.one' - it's possible to derive the namehash of any subdomain - for example 'iam.alice.one' - without having to know or handle the original human-readable name. It is this property that makes it possible for HNS to provide a hierarchal system, without having to deal with human-readable text strings internally.

Before being hashed with namehash, names are first normalized, using a process called UTS-46 normalization. This HNSures that upper- and lower-case names are treated equivalently, and that invalid characters are prohibited. Anything that hashes and resolves a name **must** first normalize it, to HNSure that all users get a consistent view of HNS.

For details on how namehash and normalization works, see the developer documentation on [name processing](contract-api-reference/name-processing.md).

## Getting Started

HNS has documentation for a variety of audiences, including dapp developers and contract developers, as well as reference documentation.

#### I'm a dapp developer and want to add HNS support to my dapp

Check out the dapp developer guide, starting with [HNS Enabling your Dapp](dapp-developer-guide/HNS-enabling-your-dapp.md). You'll want to choose one of the many available [HNS Libraries](dapp-developer-guide/HNS-libraries.md) to get started working with HNS.

#### I'm a contract developer and want to interact with HNS from my contract code

Check out the Contract Developer Guide, starting with [Resolving Names On-chain](contract-developer-guide/resolving-names-on-chain.md). You can also [write your own resolver](contract-developer-guide/writing-a-resolver.md) (to customise the process of looking up names), or your own [registrar](contract-developer-guide/writing-a-registrar.md) (to customise the process of registering new names).

#### I want reference documentation for the HNS smart contracts

Check out the Contract API Reference. We have reference documentation for HNS's core contract, the [registry](contract-api-reference/HNS.md), for [resolvers](contract-api-reference/publicresolver.md), and for commonly-used registrars such as the [Test registrar](contract-api-reference/testregistrar.md), [reverse registrar](contract-api-reference/reverseregistrar.md), and the [.one registrar](contract-api-reference/.one-permanent-registrar/).
