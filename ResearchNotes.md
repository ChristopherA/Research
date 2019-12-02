Blockchain Commons Research
===========================

Some notes on upcoming blockchain and cryptographic research from Blockchain Commons supported by our Patrons. In reverse chronological order by Cycle.
  
## Cycle 2 (December January)

These high-level topic areas to research are under consideration for Cycle 2. See [Cycle 1](#cycle-1-october-november) for current project status.

### Actively-funded for this cycle

TBD by December 5th

### Buiding community around this research

The research agenda below is huge, so we want to do what we can to support bringing others in on these topics, both as tehnical contributors as well as financial sponsoships and patronage.

Questions:

* How do we keep things moving forward — simultaneiously signaling there is good work going on here and to join us, while also not building too many barriers of entry for others to join us.

### Bitcoin Multisig Coordinators

Many bitcoin multisig scenarios require facilitation to create the multisig addresses and prove that funds sent to such addresses are valid transactions. We call this new role a "coordinator". This is a relatively new area as most coordinators are integrated into solo wallets that do everything (electrum), but there is a desire to support multiple hardware wallets. There needs to be more design work to make the UX better for PSBT (partially signed bitcoin transaction) multisig scenarios. We need to define better architecture and evangelize naming conventions.

Questions:
* Caravan seems to be the best example of this so far: https://www.unchained-capital.com/blog/the-caravan-arrives/
    * https://github.com/unchained-capital/caravan
* How can we get more organizations involved?

### Self-Sovereign Wallet Architectures

In general as we move forward on wallet architectures there have been some major differences between identity wallets and currency wallets, yet as we move to a more unified self-sovereign wallets we need a new architecture and UX. An early draft is "What's in Your Wallet" presented at #RWOT9 in Prague: https://github.com/WebOfTrustInfo/rwot9-prague/blob/master/topics-and-advance-readings/whats-wallet.md. Joe and I did more work on this that needs more cleanup before release.

Questions:

* Can we get funding to do a full 15-step engagement model scenario? Hopefully also get through some initial Intention & Responsibility tables for that model.
* Name all the nodes and all the protocols between the nodes.
* Note there is a relationship with multisig coordinator project.

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

### Wyoming Custodial Technical System Requirements

The Wyoming Banking Commission has issued new Digital Asset Custody Regulations https://drive.google.com/file/d/1U7gIrVwpWElz3C10bC92-33zAKQnS-8E/view — Blockchain Commons would like to turn this into a technical systems requirements report for implementation, hopefully funded by the ecosystem, including not only the Banking Commmission but also Kraken, other exchanges planning to move to Wyoming, emerging custodial banks, etc.

Question:
* We have team who can able to do this, but first we need to create a formal proposal to have a plan and hours estimate to get full funding. So we need to fund the proposal creation process.

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
    
### BIP-Schnor & Musig

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

## Cycle 1 (October-November)

### Actively funded during this cycle

#### Bitcoin Standup 

* Tools, Best Practices & Standards to Standup a Bitcoin-Core full node on a fresh computer or VPS
    * https://github.com/BlockchainCommons/Bitcoin-Standup
    * Finished: MacOS Bitcoin Standup App
    * Next Steps: Linux & Linode; UX/UI improvements

#### Quick Connect

Deep link URI and a scannable QR Code for connecting a remote client to a full-node (used by Bitcoin Standup)
* https://github.com/BlockchainCommons/Bitcoin-Standup#quick-connect-url-using-btcstandup
    * Finished: MacOS to Fully Noded on iOS
    * In-progress: Non-graphic clients (e.g. linode cli); Adoption by other vendors
    * Next Steps: Discover more requirements for more kinds of services and connections (mixers like Wasabi, services like BTCPay, L2 like Lightning, etc.)

#### Tor v3 Authentication

Take advantage of new Tor v3 Client Authentication for additional privacy and security between two devices
* https://github.com/BlockchainCommons/Bitcoin-Standup#tor-v3-authentication-using-standup-and-fullynoded
    * Finished: MacOS and Linux Desktop to Fully Node on iOS
    * In-progress: Non-graphic clients (e.g. linode cli)
    * Next steps: Optional other tor services?

#### Bitcoin Input Transaction Support from Blockstream.info
Feasiblity to leverage onion connection to Blockstream.info API for to support discover of input transactions for creation of a bitcoin transaction.
    * In-progress: This works, but UI is primitive
    * Next steps: Can we generalize this for RPC to a full-node or light-client support using Neutrino
    
#### BTCR Method Decentralized Identifier
Create establishing BTCR DID transactions on Testnet
    * In-progress: This works, but UI is primitive
    * Next steps: Where do we put DDO (github was plan for 0.1?) and how do we sign it? (need JSON-LD signing library)
    
#### Updates to #SmartCustody
Update Smart Custody book for Trezor; add additional wallets
    * Finished: Trezor PR is ready
    * In-progress: How do we generalize adding instructions for multiple wallets
    * Next steps: Add SLIP-39 to scenarios
    
#### Tor Infrastructure
Support Tor infrastructure such as funding exit nodes, better tools for Tor, etc.
    * Finished: A swamp-C range was donated to Matt Corello, a Tor relay exit node is now up at NYC Mesh https://metrics.torproject.org/rs.html#details/644074F47257F9A906F9AA5C6B8926C1540A1DA8
    * Next Steps: Establish a Tor Exit Node in Asia; support dedicated peering in Asia; Investigate installing optional tor infrastructure services for Bitcoin Standup persistent servers 
    
### Active, but unfunded (volunteer only)

#### Shamir-Based Social Secret Recovery
* https://github.com/BlockchainCommons/sss
    * Finished: Support of final SLIP39, addition of hazmat G256 data [#ba1192f](https://github.com/howech/sss/commit/ba1192fea95b447e3041ee558569459d1247cd4d); multi-platform build fixes and use trezor files rather than rely on openssl library [#fbf6508](https://github.com/BlockchainCommons/sss/commit/fbf6508c0d6e7a553792ce256c82b9fdde4629d7) 
    * Next steps: refactor into a binary only library and mnemonic library; draft architecture for skr enveloping for metadata; Verifiable Secret Sharing using bip-schnorr musig

### BIP-Schnorr & musig

Track current efforts for standardizing BIP-Schnorr and musig developer in preparation for use in future Blockchain Commons projecs.
    * Finished: x-only BIP-Schnorr seems to be final. Best implmentation is Peter Wuille's secp256k1 libary. Repo with current PR is at https://github.com/BlockchainCommons/secp256k1-schnorrsig
    * Next steps: create some test CLI tools for signing, multisig, and longer-term also diffie-helman peering and encryption; investigate adapting did:git to use Schnorr.

## Currently Inactive or On-Hiatus Research

#### Bitcoin on the Command-Line

This online course to teach users how to learn Bitcoin by leveraging Bitcoin-Core applications and bitcoind has been a popular intro for engineers.
    * Finished: The basics for Bitcoin were finished over a year ago
    * Next steps: Update for Bitcoin 0.19, in particular switching to bech32 addresses, teach PSBT, better linux installation advice (leverage Bitcoin Standup), more RPC example code, and teach Lighting basics.

#### Standards for Air-gapped signing and cold storage using QR

A number of air-gapped devices have demonstrated this is possible, but there is no cross-wallet compatibility. Can we abstract out a better solution as an industry standard?
    * Finished: Implemented POC in iOS bitcoin reference wallet against a Mac java-based desktop wallet; first pass at QR codes at https://github.com/BlockchainCommons/AirgappedSigning, some lessons learned (for instance, start at network wallet)
    * Next Steps: The QR codes in the POC can be quite big, investigate alternatives (compression, sequential framing; streaming, etc.); Investigate further Unchained Capital's Hermet; get funding for more work on this project from companies that want air-gapped custodial projects; architect a propsal for HTC Exodus as a substitute for a Trezor;
    
#### SecQ

There is an opportunity to leverage a “mirror” elliptic curve to the secp256k1 curve, that we are calling SecQ, to allow for an entirely new class of bulletproofs that can offer zero-knowledge cryptographic arithmetic circuit proofs about points on a elliptic curve, thus about signatures and keys. These offer powerful new opportunities for zero-knowledge proof protocols.
    * Finished: Description of project is at https://github.com/BlockchainCommons/secp256k1-schnorrsig/issues/1#issuecomment-410482607 ; proposals were sent to Ethereum Foundation and Z-Cash Foundation but we were not funded
    * Next Steps: There is validation of our approach because of Halo https://github.com/BlockchainCommons/secp256k1-schnorrsig/issues/1#issuecomment-530127487 — we should redo proposals and send again.