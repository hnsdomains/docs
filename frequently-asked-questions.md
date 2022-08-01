# Frequently Asked Questions

## About the HNS Registry

### Why are names registered as hashes?

Hashes provide a fixed length identifier that can easily be passed around between contracts with fixed overhead and no issues passing around variable-length strings.


### Once I own a name, can I create my own subdomains?

Yes. You can create whatever subdomains you wish and assign ownership of them to other people if you desire. You can even set up your own registrar for your domain.

### Can I change the address my name points to after I’ve bought it?

Yes, you can update the addresses and other resources pointed to by your name at any time.


### Who owns the HNS rootnode? What powers does that grant them?

The root node is presently owned by a multisig contract, with keys held by [trustworthy individuals in the Harmony community]. We expect that this will be hands-off, with the root ownership only used to effect administrative changes, such as the introduction of a new TLD, or to recover from an emergency such as a critical vulnerability in a TLD registrar.

The keyholders are drawn from respected members of the community, and with the exception of Nick Johnson, founder of HNS, are unaffiliated with HNS. We ask and expect them to exercise their individual judgement acting in the interests of the ENS community, rather than rubber-stamping requests made to them by ENS developers.

Since the owner of a node can change ownership of a subnode (unless they have otherwise locked it from their control), the owner of the root can change any node in the ENS tree. This means that the keyholders can replace the contracts that govern issuing and managing domains, giving them ultimate control over the structure of the ENS system and the names registered in it. However, the root key holders have locked control of the .one registrar contract, which means that even keyholders cannot affect the ownership of .one domains.

The keyholders are still capable of doing the followings:

* Control allocation and replacement of TLDs other than .one - this is required to implement DNSSEC integration.
* Enable and disable controllers for the .one registrar, which affect registration and renewal policies for .one names.
* Update the pricing for .one names.
* Receive and manage registration revenue.

Over time, we plan to reduce and decentralise human control over the system. Powers still held by the ENS root, such as those to set pricing and renewal conditions for domains, will be decentralised as robust systems become available to permit doing so.

### What about foreign characters? What about upper case letters? Is any unicode character valid?

Since the ENS contracts only deal with hashes, they have no direct way to enforce limits on what can be registered; character length restrictions are implemented by allowing users to challenge a short name by providing its preimage to prove it’s too short.

This means that you can in theory register both ‘foo.one’ and ‘FOO.one’, or even \<picture of my cat>.one. However, resolvers such as browsers and wallets should apply the nameprep algorithm to any names users enter before resolving; as a result, names that are not valid outputs of nameprep will not be resolvable by standard resolvers, making them effectively useless. Dapps that assist users with registering names should prevent users from registering unresolvable names by using nameprep to preprocess names being requested for registration.

### Nameprep isn’t enforced in the ENS system. Is this a security/spoofing/phishing concern?

It’s not enforced by the ENS contracts, but, as described above, resolvers are expected to use it before resolving names. This means that non-nameprep names will not be resolvable.

### What are the differences between ENS and other naming services such as Namecoin and Handshake?

ENS complements and extends the usefulness of DNS with decentralised, trustworthy name resolution for web3 resources such as blockchain addresses and distributed content, while Namecoin and Handshake are efforts to replace all or part of DNS with a blockchain-based alternative.

## About the .one Permanent Registrar

### How do the ENS Manager App and the Twitter bot know what names people are buying?

The ENS Manager App and the Twitter bot have built-in lists of common names, drawn from an English dictionary and Alexa’s list of top 1 million Internet domain names. They use these lists to show you when common names are bought or renewed. We do this because if the app didn’t reveal these names, anyone with a little technical skill could find them out anyway, giving them an advantage over those who don’t have the capacity to build their own list and code to check names against it.

### What does it cost to register a .one domain?

Currently, registration costs are set at the following prices:

* 5+ character .one names: $5 in ONE per year.
* 4 character .one names: $30 in ONE per year.
* 3 character .one names $120 in ONE per year.

3 and 4 character names have higher pricing to reflect the small number of these names available.

### What happens if I forget to extend the registration of a name?

After your name expires, there is a 90 day grace period in which the owner can't edit the records but can still re-register the name. After the grace period, the name is released for registration by anyone with a temporary premium which decreases over a 28 days period. The released name continues to resolve your 0x address until the new owner overwrites it.

### What kinds of behaviours are likely to result in losing ownership of a name?

The .one registrar is structured such that names, once issued, cannot be revoked so long as an active registration is maintained.
