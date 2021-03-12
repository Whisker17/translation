# 非同质化 token 圣经：你所要知道的关于 NFT 的所有事

![image-20210312234541142](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210312234542.png)

> 非同质化 token (NFTs)是独特的、拥有区块链管理所有权的数字资产。例如收藏品、游戏道具、数字艺术品、活动门票、域名，甚至实体资产的所有权记录。

如果你已经在加密世界生活了一段时间，你可能听说过 “非同质化 token ” ，或 “NFT” 。也许你是一个怀疑论者，或是一个信徒，或者也许你仍然不知道什么是真正的非同质化 token 。无论如何，这篇文章是为你准备的!

作为一个 NFT 交易市场，OpenSea 存在一个独特的优势：自 2017 年末第一个 NFT 标准出现以来，我们几乎见证了所有与 NFT 相关的项目。事实上，我们可以跟你打赌一张 [Gods Unchained Card](https://opensea.io/assets/gods-unchained)，如果你问我们一个关于 NFT 项目的事，我们很有可能已经听说过了，而且更可能在某个时候已经和开发人员聊过了！NFT 的生态系统是由一群不可思议的创新者组成的紧密组织：从狂热者到开发者、游戏玩家、企业家到艺术家。我们很荣幸能成为这个社区的一员。

这篇文章提供了一个关于非同质化 token 的深入概述：ERC721 的技术剖析，NFT 的历史，关于 NFT 的常见误解，以及 NFT 的市场现状。我们希望它既适用于刚接触这个领域的人，也适用于那些已经了解 NFT 但想更好地了解其内部工作方式的细微差别的人。

## 目录

- [什么是非同质化 token ?](#什么是非同质化_token_?)
  - Blockchain-based non-fungible tokens
    - [Standardization](https://opensea.io/blog/guides/non-fungible-tokens/#Standardization)
    - [Interoperability](https://opensea.io/blog/guides/non-fungible-tokens/#Interoperability)
    - [Tradeability](https://opensea.io/blog/guides/non-fungible-tokens/#Tradeability)
    - [Liquidity](https://opensea.io/blog/guides/non-fungible-tokens/#Liquidity)
    - [Immutability and provable scarcity](https://opensea.io/blog/guides/non-fungible-tokens/#Immutability_and_provable_scarcity)
    - [Programmability](https://opensea.io/blog/guides/non-fungible-tokens/#Programmability)
- Non-fungible token standards
  - [ERC721](https://opensea.io/blog/guides/non-fungible-tokens/#ERC721)
  - ERC1155
    - [Composables](https://opensea.io/blog/guides/non-fungible-tokens/#Composables)
  - [Non-Ethereum standards](https://opensea.io/blog/guides/non-fungible-tokens/#Non-Ethereum_standards)
- Non-fungible token metadata
  - On-chain vs. off-chain
    - [On-chain Metadata](https://opensea.io/blog/guides/non-fungible-tokens/#On-chain_Metadata)
    - [Off-chain Metadata](https://opensea.io/blog/guides/non-fungible-tokens/#Off-chain_Metadata)
  - Off-chain storage solutions
    - [Centralized servers](https://opensea.io/blog/guides/non-fungible-tokens/#Centralized_servers)
    - [IPFS](https://opensea.io/blog/guides/non-fungible-tokens/#IPFS)
- History of non-fungible tokens (2017 – 2020)
  - [– 0 BC: Before CryptoKitties](https://opensea.io/blog/guides/non-fungible-tokens/#_0_BC_Before_CryptoKitties)
  - 0 BC: Birth of CryptoKitties
    - [Speculative mechanics](https://opensea.io/blog/guides/non-fungible-tokens/#Speculative_mechanics)
    - [Viral story](https://opensea.io/blog/guides/non-fungible-tokens/#Viral_story)
  - 2018: Hype, hot-potato games, and layer 2
    - [Layer 2 games and experiences](https://opensea.io/blog/guides/non-fungible-tokens/#Layer_2_games_and_experiences)
    - [Hot Potatoes](https://opensea.io/blog/guides/non-fungible-tokens/#Hot_Potatoes)
    - [Venture Capital interest](https://opensea.io/blog/guides/non-fungible-tokens/#Venture_Capital_interest)
  - 2018 – 2019: Back to building
    - [Digital art](https://opensea.io/blog/guides/non-fungible-tokens/#Digital_art)
    - [NFT Minting Platforms](https://opensea.io/blog/guides/non-fungible-tokens/#NFT_Minting_Platforms)
    - [Traditional IP dips its feet](https://opensea.io/blog/guides/non-fungible-tokens/#Traditional_IP_dips_its_feet)
    - [Japan leads the way](https://opensea.io/blog/guides/non-fungible-tokens/#Japan_leads_the_way)
    - [Virtual worlds expand](https://opensea.io/blog/guides/non-fungible-tokens/#Virtual_worlds_expand)
    - [Trading card games](https://opensea.io/blog/guides/non-fungible-tokens/#Trading_card_games)
    - [Decentralized naming services](https://opensea.io/blog/guides/non-fungible-tokens/#Decentralized_naming_services)
    - [Other experiments](https://opensea.io/blog/guides/non-fungible-tokens/#Other_experiments)
    - [Casualties and resuscitation](https://opensea.io/blog/guides/non-fungible-tokens/#Casualties_and_resuscitation)
- Non-fungible token myths
  - Scarcity alone drives demand
    - [Smart contracts mean assets last forever](https://opensea.io/blog/guides/non-fungible-tokens/#Smart_contracts_mean_assets_last_forever)
    - [Abstracting the chain away will solve all our problems](https://opensea.io/blog/guides/non-fungible-tokens/#Abstracting_the_chain_away_will_solve_all_our_problems)
- The Non-Fungible Token Market
  - [Current market size](https://opensea.io/blog/guides/non-fungible-tokens/#Current_market_size)
  - [Market growth](https://opensea.io/blog/guides/non-fungible-tokens/#Market_growth)
  - [Sale Mechanisms](https://opensea.io/blog/guides/non-fungible-tokens/#Sale_Mechanisms)
  - [NFT Distribution](https://opensea.io/blog/guides/non-fungible-tokens/#NFT_Distribution)
  - [What’s next for NFTs? Our 2020 predictions](https://opensea.io/blog/guides/non-fungible-tokens/#Whats_next_for_NFTs_Our_2020_predictions)

### 什么是非同质化 token