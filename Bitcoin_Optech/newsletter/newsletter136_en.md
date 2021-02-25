# Bitcoin Optech Newsletter #136

Feb 17, 2021

This week’s newsletter describes the development of a new constant-time algorithm for generating and verifying transaction signatures, mentions a proposal to stop processing unsolicited transactions, summarizes a proposed BIP for setting up multisig wallets, shares insight from a discussion about managing escrows over LN, links to renewed discussion about bidirectional upfront LN fees, and mentions a new protocol for mitigating the risk of malicious hardware wallets. Also included are our regular sections with news about updated clients and services, announcements of new software releases and release candidates, and descriptions of notable changes in popular Bitcoin infrastructure software.

## News

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#faster-signature-operations) **Faster signature operations:** Russell O’Connor and Andrew Poelstra published a [blog post](https://medium.com/blockstream/a-formal-proof-of-safegcd-bounds-695e1735a348) announcing the implementation of an algorithm that can speed up Bitcoin Core’s signature verification by 15%. It can also reduce by 25% the amount of time it takes to generate signatures while still using a constant-time algorithm that reduces the risk of sidechannel data leaks, which may be of special interest to hardware wallet developers whose devices have limited resources.

  The blog post describes the development of the new algorithm by Daniel J. Bernstein and Bo-Yin Yang, an implementation of it for libsecp256k1 by Peter Dettman (mentioned in [Newsletter #111](https://bitcoinops.org/en/newsletters/2020/08/19/#proposed-uniform-tiebreaker-in-schnorr-signatures)), a novel and CPU-efficient method by Pieter Wuille for computing the maximum number of steps the algorithm requires to achieve its goal in constant time, a variation on Bernstein’s and Yang’s algorithm by Gregory Maxwell that’s even more efficient, and the implementation of Wuille’s bound verification program in the Coq proof assistant by O’Connor and Poelstra to help ensure its correctness. Dettman’s original implementation for libsecp256k1 has been updated with the recent developments by Wuille as [PR #831](https://github.com/bitcoin-core/secp256k1/issues/831).

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#proposal-to-stop-processing-unsolicited-transactions) **Proposal to stop processing unsolicited transactions:** normally nodes expect to receive the announcement of a new transaction in a P2P protocol `inv` message. If the node is interested in the transaction (e.g. it hasn’t previously received it from another peer), it requests the transaction using a `getdata` message and the broadcaster replies with the transaction in a `tx` message. However, for almost a decade, some light clients and other software have skipped the `inv` and `getdata` steps and proceed to just send unsolicited `tx` messages ([example](https://github.com/bitcoinj/bitcoinj/blob/7d2d8d7792ec5f4ce07ff82980b4e723993221e8/core/src/main/java/org/bitcoinj/core/TransactionBroadcast.java#L145)).

  This week, Antoine Riard [posted](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-February/018391.html) to the Bitcoin-Dev mailing list a proposal to stop processing such unsolicited `tx` messages. As discussed in a [Bitcoin Core PR](https://github.com/bitcoin/bitcoin/issues/20277), this will give a node more control over when it receives and processes transactions, giving it an additional way to limit the impact of peers that send expensive-to-validate transactions. Riard is proposing this change be made possibly as early as the next major version of Bitcoin Core, version 22.0.

  The downside of this approach is that all clients that currently send unsolicited `tx` messages won’t be able to send transactions unless they are upgraded before the last few 0.21.x or earlier versions of Bitcoin Core leave the network (or other Bitcoin implementations that act similarly). It usually takes several years for an old version to completely leave the network, so there should be plenty of time to accomplish client upgrades. We encourage the developers of affected clients to read the proposal and consider commenting on it.

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#securely-setting-up-multisig-wallets) **Securely setting up multisig wallets:** Hugo Nguyen [posted](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-February/018385.html) to the Bitcoin-Dev mailing list a draft BIP that describes how wallets, particularly hardware wallets, can securely exchange the information necessary to become signers for a multisig wallet. The information that needs to be exchanged includes the script template to use (e.g. P2WSH with 2-of-3 keys required to sign) and each signer’s [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) extended public key (xpub) at the key path it plans to use for signing.

  Briefly, Nguyen’s proposal (developed in correspondence with several hardware wallet producers) has a coordinator initiate the multisig federation process by choosing a script template and generating a private encryption and authentication credential. Those parameters are shared with the member wallets, who choose an appropriate key path and return its xpub along with a signature generated by its corresponding private key. The set of {identifying_parameters, key_path, xpub, signature} are encrypted and returned to the coordinator by each wallet. The coordinator then combines them into an [output script descriptor](https://bitcoinops.org/en/topics/output-script-descriptors/) that is encrypted and passed back to each member wallet, who verifies their xpub is included and then stores the descriptor as a script template they’re willing to sign for.

  The proposal received a significant amount of discussion on the mailing list, and some changes are planned. We’ll monitor the discussion for significant updates as the proposal matures and gets closer to implementation.

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#escrow-over-ln) **Escrow over LN:** a discussion [started](https://lists.linuxfoundation.org/pipermail/lightning-dev/2019-June/002028.html) on the Lightning-Dev mailing over a year ago about creating non-custodial onchain escrows for LN saw [renewed discussion](https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-February/002948.html) this past week. A particular standout in the discussion was a [post](https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-February/002955.html) by ZmnSCPxj that uses one of [De Morgan’s laws](https://en.wikipedia.org/wiki/De_Morgan's_laws) for boolean statements to greatly simplify the construction of escrows—and [possibly](https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-February/002970.html) many other LN-based contracts—with the tradeoff of requiring a seller to post a bond before receiving payment. ZmnSCPxj’s idea requires an upgrade of LN to use [PTLCs](https://bitcoinops.org/en/topics/ptlc/), which is not currently expected until after [taproot](https://bitcoinops.org/en/topics/taproot/) activates.

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#renewed-discussion-about-bidirectional-upfront-ln-fees) **Renewed discussion about bidirectional upfront LN fees:** Joost Jager [restarted](https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-February/002958.html) Lightning-Dev mailing list discussion about adding LN service fees that charge spenders and receivers for the time they consume using a channel’s limited availability of funding (“liquidity”) and concurrent payment capacity (“HTLC slots”). Jager builds on a previous proposal (see [Newsletter #122](https://bitcoinops.org/en/newsletters/2020/11/04/#bi-directional-upfront-fees-for-ln)), extending its fixed fees with fees proportional to payment processing duration (“hold fees”). The proposal received moderate discussion, with one [concern](https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-February/002967.html) being a reduction in sender/receiver privacy. The underlying issue here has been [known](https://lists.linuxfoundation.org/pipermail/lightning-dev/2015-August/000135.html) for five years and discussed at length many times, so we expect discussion to continue.

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#anti-exfiltration) **Anti-exfiltration:** Andrew Poelstra and Jonas Nick published a blog [post](https://medium.com/blockstream/anti-exfil-stopping-key-exfiltration-589f02facc2e) about a security technique being implemented for the Shift Crypto [BitBox02](https://shiftcrypto.ch/bitbox02/) and Blockstream [Jade](https://blockstream.com/jade/) hardware wallets. The goal is to allow both a hardware wallet and the computer controlling it to each be assured that the nonce used to generate a signature is actually an unguessable value. This prevents malicious hardware wallet firmware from generating nonces known to the firmware author, which the author could combine with one of the device’s transaction signatures found onchain to derive its private keys, allowing them to spend any other bitcoins controlled by those keys. The post describes the technique used, which was previously mentioned in a mailing list thread about this subject almost a year ago (see Newsletters [#87](https://bitcoinops.org/en/newsletters/2020/03/04/#proposal-to-standardize-an-exfiltration-resistant-nonce-protocol) and [#88](https://bitcoinops.org/en/newsletters/2020/03/11/#exfiltration-resistant-nonce-protocols)).

## Changes to services and client software

*In this monthly feature, we highlight interesting updates to Bitcoin wallets and services.*

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#blockstream-announces-lnsync) **Blockstream announces LNsync:** [LNsync](https://blockstream.com/2021/01/22/en-lnsync-getting-your-lightning-node-up-to-speed-quickly/) allows Lightning wallets that were offline for a while to quickly download recent updates to LN topology in order to optimally route a payment to its destination. An open source plugin, [historian](https://github.com/lightningd/plugins/tree/master/historian), provides this functionality for C-Lightning users.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#rust-light-client-nakamoto-released) **Rust light client Nakamoto released:** Alexis Sellier released [Nakamoto](https://cloudhead.io/nakamoto/), a “high-assurance Bitcoin light-client implementation in Rust, with a focus on low resource utilization, modularity and security” based on [compact block filters](https://bitcoinops.org/en/topics/compact-block-filters/).
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#blockstream-satellite-broadcasting-ln-data-and-bitcoin-core-source) **Blockstream Satellite broadcasting LN data and Bitcoin Core source:** Blockstream’s Satellite, in addition to broadcasting Bitcoin block chain data, now includes the [Bitcoin Core source code](https://blockstream.com/2021/02/02/en-blockstream-provides-backup-satellite-broadcast-for-bitcoin-core-source-code/), code for a satellite-optimized Bitcoin fork ([Bitcoin Satellite](https://github.com/Blockstream/bitcoinsatellite)), and [LN gossip data](https://blockstream.com/2021/01/29/en-new-blockstream-satellite-updates/).
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#blockstream-green-implements-csv) **Blockstream Green implements CSV:** Green Wallet previously used nLockTime as a wallet recovery mechanism, requiring users to receive backup emails with pre-signed transactions from Blockstream after each transaction in order to recover funds. By [implementing an `OP_CHECKSEQUENCEVERIFY` (CSV) recovery mechanism](https://blockstream.com/2021/01/25/en-blockstream-green-bitcoin-wallets-now-using-checksequenceverify-timelocks/), wallets can now be recovered without needing transaction-specific backup files or associating an email address with the wallet.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#muun-2-0-released) **Muun 2.0 released:** The [2.0 release](https://medium.com/muunwallet/announcing-muun-2-0-d61b0844dc0a) of mobile Bitcoin and Lightning wallet Muun includes multisig, wallet recovery features, and also open sources the Android and iOS mobile apps.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#joinmarket-0-8-1-released) **Joinmarket 0.8.1 released:** The [0.8.1 release](https://github.com/JoinMarket-Org/joinmarket-clientserver/blob/master/docs/release-notes/release-notes-0.8.1.md) contains additional [PSBT](https://bitcoinops.org/en/topics/psbt/) support for PSBTs created externally, [signet](https://bitcoinops.org/en/topics/signet/) support, and a fix for uppercase address support with [BIP21](https://github.com/bitcoin/bips/blob/master/bip-0021.mediawiki) URIs (see [Newsletter #127](https://bitcoinops.org/en/newsletters/2020/12/09/#thwarted-upgrade-to-uppercase-bech32-qr-codes)). A terminal-based UI for JoinMarket, [JoininBox](https://github.com/openoms/joininbox), was also updated to support 0.8.1.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#vbtc-allows-withdrawals-via-ln-and-segwit-batches) **VBTC allows withdrawals via LN and segwit batches:** VBTC, a Vietnamese exchange, [added a batched segwit withdrawal option](https://blog.vbtc.exchange/2021/batched-segwit-withdrawals-tutorial-5) after recently also [enabling Lightning withdrawals](https://blog.vbtc.exchange/2020/how-to-withdraw-bitcoin-lightning-network-tutorial-3). The incentivized segwit batch withdrawal occurs once a week, targeting a time when mempools are less likely to have large transaction backlogs.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#bitcoin-dev-kit-v0-3-0-released) **Bitcoin Dev Kit v0.3.0 released:** Rust wallet library Bitcoin Dev Kit announced its [v0.3.0 release](https://bitcoindevkit.org/blog/2021/01/release-v0.3.0/) including splitting out the CLI into its own repository. The recent [BDK v0.2.0](https://bitcoindevkit.org/blog/2020/12/release-v0.2.0/) release added Branch and Bound (BnB) coin selection, [descriptor](https://bitcoinops.org/en/topics/output-script-descriptors/) templates (including with the recently added `sortedmulti`), and more.

## Releases and release candidates

*New releases and release candidates for popular Bitcoin infrastructure projects. Please consider upgrading to new releases or helping to test release candidates.*

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#lnd-0-12-1-beta-rc1) [LND 0.12.1-beta.rc1](https://github.com/lightningnetwork/lnd/releases/tag/v0.12.1-beta.rc1) is a release candidate for a maintenance release of LND. It fixes an edge case that could lead to accidental channel closure and a bug that could cause some payments to fail unnecessarily, plus some other minor improvements and bug fixes.

## Notable code and documentation changes

*Notable changes this week in [Bitcoin Core](https://github.com/bitcoin/bitcoin), [C-Lightning](https://github.com/ElementsProject/lightning), [Eclair](https://github.com/ACINQ/eclair), [LND](https://github.com/lightningnetwork/lnd/), [Rust-Lightning](https://github.com/rust-bitcoin/rust-lightning), [libsecp256k1](https://github.com/bitcoin-core/secp256k1), [Hardware Wallet Interface (HWI)](https://github.com/bitcoin-core/HWI), [Rust Bitcoin](https://github.com/rust-bitcoin/rust-bitcoin), [BTCPay Server](https://github.com/btcpayserver/btcpayserver/), [Bitcoin Improvement Proposals (BIPs)](https://github.com/bitcoin/bips/), and [Lightning BOLTs](https://github.com/lightningnetwork/lightning-rfc/).*

- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#bitcoin-core-20944) [Bitcoin Core #20944](https://github.com/bitcoin/bitcoin/issues/20944) adds a new `total_fee` field to the object returned by the `getmempoolinfo` RPC and the `mempool/info` REST endpoint. `total_fee` indicates the sum of the transaction fees for all transactions currently in the mempool.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#lnd-4909) [LND #4909](https://github.com/lightningnetwork/lnd/issues/4909) adds new `getmccfg` and `setmccfg` RPCs that can, respectively, retrieve and temporarily change settings in LND’s *mission control* subsystem without restarting the daemon. Mission control uses information about past payment attempts to choose better routes for subsequent payment attempts.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#rust-lightning-787) [Rust-Lightning #787](https://github.com/rust-bitcoin/rust-lightning/issues/787) ensures that channel closures due to error messages only happen when the peer who sent the message is the channel’s counterparty. Previously, it was possible for malicious peers to force-close any channel if they knew the channel id.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#btcpay-server-2164) [BTCPay Server #2164](https://github.com/btcpayserver/btcpayserver/issues/2164) redesigns the wallet setup wizard, walking the user through setting up BTCPay’s internal wallet and optionally integrating it with the user’s own remote software or hardware wallet. This is the start of other redesign work on BTCPay’s interface.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#hwi-443) [HWI #443](https://github.com/bitcoin-core/HWI/issues/443) adds support for signing multisig inputs with the BitBox02 hardware wallet.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#bitcoin-core-19145) [Bitcoin Core #19145](https://github.com/bitcoin/bitcoin/issues/19145) extends the `gettxoutsetinfo` RPC’s `hash_type` option to accept a new `muhash` parameter that will produce the [MuHash3072](https://bitcoinops.org/en/newsletters/2021/01/13/#bitcoin-core-19055) digest of the UTXO set at the current block height. This is an alternative to producing the default SHA256 digest of the UTXO set. When run this way, MuHash has to process the same amount as data as the SHA256 function, so there’s not expected to be any significant change in performance of the `gettxoutsetinfo` RPC, which can take up to several minutes to return on slow single-board computers. However, a previously calculated MuHash object can have elements added or removed from it relatively cheaply, so a future PR is expected to quickly calculate a MuHash summary of the UTXO set at each block, save those summaries in a simple database, and allow returning them in digest form almost instantly upon demand. This will also benefit the [AssumeUTXO](https://bitcoinops.org/en/topics/assumeutxo/) project, which relies on the ability of users to compare UTXO set hashes at selected past block heights.
- [●](https://bitcoinops.org/en/newsletters/2021/02/17/#c-lightning-4364) [C-Lightning #4364](https://github.com/ElementsProject/lightning/issues/4364) changes how it notifies its channel partners about problems, separating existing error messages into true *errors* and those that are just *warnings*. The current [BOLT1](https://github.com/lightningnetwork/lightning-rfc/blob/master/01-messaging.md) specification requires that any error result in channels being closed (although this does not seem to be universally implemented), but a [proposed specification update](https://github.com/lightningnetwork/lightning-rfc/issues/834) introduces the warning message type that can allow for a more flexible response. Anyone interested may also wish to read a [post](https://lists.linuxfoundation.org/pipermail/lightning-dev/2021-February/002964.html) to the Lightning-Dev mailing list this week by Carla Kirk-Cohen about extended error message descriptions, a topic she described as “related, but not directly relevant to a soft warning error.”