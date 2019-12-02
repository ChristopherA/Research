Blockchain Commons Research
===========================

Some notes on upcoming blockchain and cryptographic research from Blockchain Commons supported by our Patrons.

## Cycle 2 (December January)

### Buiding community around this research

The research agenda below is huge, so we want to do what we can to support bringing others in on these topics, both as tehnical contributors as well as financial sponsoships and patronage.

Questions:

* How do we keep things moving forward — simultaneiously signaling there is good work going on here and to join us, while also not building too many barriers of entry for others to join us.

### Neutrino & Blockchain Access for Mobile Clients

Several of our patrons desire to have better privacy-preserving and non-correllatable access to bitcoin blockchain data from mobile clients.

There are two different, related initiatives here. 

The first is support for Neutrino, a proposed protocol that replaces SPV bloom filters for access to blockchain data. A problem has been slow support of it in bitcoin implementations to-date. One research project is to take the latest libbitcoin implementation (or PR if it is not merged) and see how functional it is, and to define a test bed for it. If it is functional, create a Swift library shim for it to integrate it into the reference iOS wallet.

The second initiative is to have two-way secure and non-correlatable access to full-node blockchain data from the remove device. An example of this might be a wallet-less full-node hosted on a VPS controlled via v3onion/ssh connection to a mobile client that holds the keys. On iOS and most Android devices these keys would be help in the moble devices secure enclave, but on Exodus they could be secured in the Trustzone. Alternatively, the mobile client could talk securely to a full-node at someone's home where the keys are held by bitcoind remotely and the remote has no keys. An example proof of concept of this is Bitcoin-Standup on the Mac and Fully-Noded on iOS.

Questions: 
* Does libbitcoin's Neutrino support work? Does it work in the wild with limited support of Neutrino by full node servers? If we have to setup full nodes to reliably support Neutrino clients anyhow, it could be that v3tor/ssh architecture to a full-node may be better in the short run. We don't know.
* We have already demonstrated that we can do transactions using just the publically available Blockstream.info server, however, there were some issues with using it .onion. We've also demonstrated feasibility to do v3tor/ssh and RPC to a bitcoind. And assuming the libbitcoin Neutrino support works, we will be able establish transactions using that. Is there some way abstract API for all three (or 4 if we include variants on where the keys are stored) so that users can choose?
* Longer term we may want to add esplora (Blockstream.info block explorer) as an API on top of bitcoind instead of talking to it via RPC. This could be installed as an option by Bitcoin Standup. This would also allow to extend the API to support tip-finding APIs needed by BTCR remote clients, as bitcoind by default does not index UTXOs.
* There may be some utility in having a Bitcoin Standup option to install a libbitcoin server instead of bitcoind, as it may support both Neutrino and indexing UTXOs needed by BTCR.

### Payment Channels

Current L1 public blockchain that support multiple fungible digital assets (colored coins like Omni, small POW blockchain's like Bitmark, etc.) are unlikely to scale much beyond the initial POC/Intermediate (5-7 TPS) level. There are some existing solutions to this, for instance, Blockstream's Liquid supports multiple assets uses a private-PBFT as its L2 consensus layer, and Ethereum is exploring sharding as a L1 solution.

Blockchain Commons believes that leveraging L2 payment channel technologies is another possible solution for long-term scaling as not all assets exchanges between peers need to be immediately reconciled on the ledger, and payment channels have other advantages for privacy and atomic transactions.

To verify this supposition we need to understand payment channels as a framework for tokens that are not quite the same as the underlying digital currencies currently used by other payment channels such as Lightning Network.

Questions: 

* If we begin with a presumption of using Bitcoin as the L1, and the Lightning Network as one form of L2 payment channel:
    * how much can we do to leverage LN vs. creating a separate L2 payment channel network that also settles to Bitcoin.
* Some other research agenda items in this document may be needed to advance in order to fully architect a L2 solution. What dependencies do we want to presume?
* What other questions about payment channels have come up in SeanG's research?
    * RGB Project & Single-Use Seal Prototocol: great approach, appealing, flexibility, but also very new and not clear how to enter the ecosystem or build our own. Abstracting a double spend to a cryptographic commitment "the single use seal" seems powerful. Forces a single state at any time. Allows us to be able to manage state in L2 using client-side validation. RGB can run on L1 or L2 LN. 
    * Design questions: Which layers do we begin with interoperability (bitcoin, payment channel, rgb layer)
    * What are the extension mechanisms.
        * Rust version in Single Seals framework
    * describing it better, get review
* Do we know enough about the combo of c-ocap and single-use seals. ChristopherA thinks they are in alignment, but we don't know.
* If there is a dependency on a payments network, how do scale up given adoption and stable currency issues.
* Migration path to Schnorr is an issue (or do we just skip ECDSA bitcoin and LN until they have it)

### Fungible Tokens vs Cryptographic Object Capabilities (c-ocap)

There is an argument that our future users need is not a fungible token, but instead what they need is a new form of bearer digital asset. These cryptographic tokens should be able to be atomically swapped for other tokens in and of themselves (including other digital currency and/or other digital assets), be able to allow the bearer access or right to update or append to datastores, attenuate to allow delegation of less privileges to others, prove to others the provenance and rights to access of data, and finally potentially spent (not in the exchange sense but more of a redemption causing a deletion sense) and never used again.

In general, this type of architecture has been known as object capabilities or ocap for short, and it addresses a number of fundamental security problems that current ACL-style access architectures do not (for instance but not exclusively see https://en.wikipedia.org/wiki/Confused_deputy_problem). However, most implementations to date are computationally focused rather than cryptographically focused. I personally have been much more interested in more cryptographic forms of ocap (aka C-ocap).

Macaroons (https://ai.google/research/pubs/pub41892)  are a very simple form of cryptographic ocap that has been used in recent years by Google which leverage HMACs chains but much more sophisticated cryptographic forms have been proposed. There has been some more recent proof of concepts where bitcoin is atomically exchanged for macaroons on the Lighting Network (see presentation https://docs.google.com/presentation/d/1QSm8tQs35-ZGf7a7a2pvFlSduH3mzvMgQaf-06Jjaow/edit#slide=id.g32fd7cbaff_0_209 and a demo video: https://twitter.com/roasbeef/status/1190098624010522624?s=20) that give access to a website. Another organization has independently done something similar called BoltWall https://github.com/Tierion/boltwall to associate payment with macaroons to give access to other services.

One particular advantage of Macaroons over other ocap approaches (z-caps in particular) is that because Macaroons were invented at Google, a lot of code for them is available in Go (https://github.com/go-macaroon/macaroon) which is a language that some of us are very comfortable with. However, it isn't clear to me how much these older macaroons are different in their atomic swappable Lightning Nework form. Also, there are some other ocap forms being proposed in the IPFS community that are also potentially interesting.

Questions:

* What can we leverage from existing code bases:
    * Macaroons
    * IPFS
* How will use of Schnorr impact using existing code bases.

RESEARCH
    * Marcelo maybe to research these.


### Schnorr-based Multisig

As we move toward needing teams and groups of people to be able to issue cryptographic ocap access & rights tokens, the long-term need for aggregatable & threshold & adapter signatures, sign-to-contract, state commitments, features etc. seems clear. Schnorr has this functionality, as well as some useful privacy features. Schnorr-based musig is something that both Bitcoin and the Lightning Network communities want to move toward. These cryptographic functions are also possible with BLS and the Ristretto-25519 forms of elliptic curves (but not the standard 25519), but there is less community support for musig in these communities.

At a minimum we may want to consider using Schnorr for signatures and proofs that don't depend on settlement to L1, and use ECDSA for Bitcoin and Lightning derived from the same private keys. There are a number of very basic tools for Schnorr that we might want to explore, especially as we try to leverage Schnorr over ECDSA inside c-ocap tokens. It is also possible that having these tools available will be critical path for exploring POCs on the other research topics listed here.

Questions:

* Can we plan for a Schnorr multisig future without going fully multisig now?
* There are issues of which languages to support, C++ is the best cross-platform language for mobile, but Go has more infrastructure and Rust is more secure.
* ChristopherA suspects that we should do some Shnorr fundementals during this cycle as other research items may depend on it.
    * CLI-based Schnorr tools
    * A simple CLI based Club System-style encrypted objects with simple cryptographic object capabilities

Projects:
* Go/Android (Kotlin?)/Swift/Rust based wrappers for the c++ Schnorr code
* CLI signing tools
* CLI encryptiont tools
* all the above demonstrating schnorr multisig architecture
* Start on a simple club system.

### Personal Data Stores & Group Data Stores

There has been a big surge of interest in encrypted personal data vaults that leverage decentralized identity over the last 6 months. A number of different blockchain communities have independently been working on a number widely different approaches to them, and via my W3C community and my #RebootingWebOfTrust we have been trying to reconcile some of the different approaches (or at least at the bottom-most layer, see https://github.com/WebOfTrustInfo/rwot9-prague/blob/master/final-documents/encrypted-data-vaults.md). At a conference call meeting last week to try to unify these approaches we have over 75 people attend, so this is a hot topic. 

There have been discussions by these parties, but no agreement on, for the possibility of leveraging ocap architectures for access to these encrypted data stores. I personally would like to see a Schnorr-based Club System as group-owned encrypted data store, but most of the other companies are focused on personal (aka single key) data stores and very simple non-cryptographic ocaps (sometimes call z-caps).

As it is clear from previous discussions that user control over their data at rest is essential, and thus must implement some form of personal data store. It isn't clear if leveraging these emerging standardization efforts will hinder short term deployment because of dependencies on others, or if we can take advantage of them to speed things up.

Questions:

* Should be become more involved with the standards activities for encrypted vaults?
    * A risk is that it could slow things down.
    * An advantage is that we can not only leverage other peoples work, but influence it in our direction.
    * It isn't clear the the standards activities will support cryptographic ocap, maybe just some form of zcap. Is that acceptable?

Projects
    * Cuongle to track, not be active

### Social Key Recovery

An essential part of all of these approaches are self-sovereign keys. BIP39 isn't enough, so no matter what cryptographic system we use, we need to leverage some form of resilience through social key recovery. Since our apps are essentially  social app, building this in from day one is likely important. Blockchain Commons has worked some with HTC on this and others, but we could make some real progress on a specific approach of using two-level SLIP39 for broader use early next year.

Questions:

* Work on some UX & I&R issues for this as part of Joe's engagement model work.
* The progress since #RWOT9 on social key recovery has been slow
    * We need to refactor the existing SSS library to support a simplified binary library.
    * So far volunteers only, HTC hasn't funded (yet.)
    * Some want to move to Rust or a more secure language.
    * We still don't have solid proposal from MarkF for envelope around SLIP39
    * How do we move toward VSS, and do wait on Shamir and do VSS instead? 

Project:

* Work backwards from UX. Past-user testing failed
    * needs a different attack
    * needs to go through risk management #SC adversaries (key resilience vs key compromise)
    * can we do the user "ceremonies", gamification, etc. using Shamir (vs VSS)

### Social Discovery

How can we do social discovery and preserve privacy? Maybe some zk methods?

### Smart Signatures

Related to the Schnorr work, should we implement some type of simple predicate smart signature language sooner than later?

### Other questions

What other big topic research areas am I missing?

