# 非同质化 token 圣经：你所要知道的关于 NFT 的所有事

原文链接：[The Non-Fungible Token Bible: Everything you need to know about NFTs](https://opensea.io/blog/guides/non-fungible-tokens/#Blockchain-based_non-fungible_tokens)

原文作者：Devin Finzer

![image-20210312234541142](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210312234542.png)

> 非同质化 token (NFTs)是独特的、拥有区块链管理所有权的数字资产。例如收藏品、游戏道具、数字艺术品、活动门票、域名，甚至实体资产的所有权记录。

如果你已经在加密世界生活了一段时间，你可能听说过 “非同质化 token ” ，或 “NFT” 。也许你是一个怀疑论者，或是一个信徒，或者也许你仍然不知道什么是真正的非同质化 token 。无论如何，这篇文章是为你准备的!

作为一个 NFT 交易市场，OpenSea 存在一个独特的优势：自 2017 年末第一个 NFT 标准出现以来，我们几乎见证了所有与 NFT 相关的项目。事实上，我们可以跟你打赌一张 [Gods Unchained Card](https://opensea.io/assets/gods-unchained)，如果你问我们一个关于 NFT 项目的事，我们很有可能已经听说过了，而且更可能在某个时候已经和开发人员聊过了！NFT 的生态系统是由一群不可思议的创新者组成的紧密组织：从狂热者到开发者、游戏玩家、企业家到艺术家。我们很荣幸能成为这个社区的一员。

这篇文章提供了一个关于非同质化 token 的深入概述：ERC721 的技术剖析，NFT 的历史，关于 NFT 的常见误解，以及 NFT 的市场现状。我们希望它既适用于刚接触这个领域的人，也适用于那些已经了解 NFT 但想更好地了解其内部工作方式的细微差别的人。

## 目录

- [什么是非同质化 token](#什么是非同质化-token)
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

> 非同质化 token 资产只是正常的东西。而同质化资产才是奇怪的！

大多数关于非同质化 token 的讨论都是从引入同质化的概念开始的，同质化定义为 “能够替换或被另一个相同的资产所替换” 。我们认为这把事情复杂化了。为了更好地理解什么可能构成非同质化资产，只要想想你所拥有的大部分东西。你现在坐的椅子，你的手机，你的笔记本电脑，任何你可以在 eBay 上出售的东西。所有这些都属于非同质化的东西。

![image-20210313104752826](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210313104801.png)

事实证明，同质化资产实际上是很少见的。货币是同质化资产的典型例子。5 美元永远都是 5 美元，不管钞票上的序列号是什么，也不管那 5 美元是不是存在你的银行账户里。用另一张 5 美元钞票(或 5 个 1 美元，就此而言)取代一张 5 美元钞票的能力是货币同质化的原因。

注意，同质化是相对的；它只适用于比较多个事物。考虑商务舱、经济舱和头等舱的机票。每张机票都可以在同一舱位内做到同质，但你不可能公平地将头等舱机票换成商务舱机票。即使是你坐着的椅子，也可以用相同型号的椅子代替，除非你对你的椅子有特别的依恋。

有趣的是，同质化也可以是主观的。依旧是机票的例子：一个人想要坐在靠窗的座位还是靠过道的座位，他可能不会考虑两张经济舱机票可以互换。同样，一枚罕见的硬币对我来说可能值 1 美分，但对硬币收藏者来说价值则远远不止。我们将看到，当在区块链上表示这些东西时，其中的一些细微差别变得非常重要。

#### 基于区块链的非同质化 token

正如我们在加密货币出现之前就有了数字货币(比如航空点、游戏内置货币)，自互联网诞生以来，我们就有了非同质化的数字资产。域名、赛事门票、游戏道具，甚至 Twitter 或 Facebook 等社交网络上的帐号都是非同质化的数字资产；它们只是在可交易性、流动性和互操作性方面有所不同。其中很多都非常有价值：[Epic Games 仅在 2018 年通过免费游戏《堡垒之夜》( Fortnite ) 的服装销售就获得了 24 亿美元的收益](https://www.investopedia.com/tech/how-does-fortnite-make-money/)，[活动门票市场预计在 2025 年将达到 680 亿美元](https://www.grandviewresearch.com/press-release/global-online-event-ticketing-market)，[同时域名市场也将继续稳步增长。](https://www.thedomains.com/2019/10/05/the-number-of-domain-names-sold-in-2019-has-already-surpassed-2018/)

> 我们有大量的数字产品，但实际上我们并没有拥有过它们。

很明显，我们已经有了大量的数字产品。但我们在多大程度上 “拥有” 了这些数字产品？如果数字所属权意味着一件物品只属于你而不是其他人，那么在某种意义上，你拥有它们。如果数字所属权更像是物理世界的所有权(无限持有和转让的自由)，但数字资产似乎并不总是如此。相反，你在特定的环境中拥有这些资产，这可能或多或少会使移动它们变得容易。试着在 eBay 上出售堡垒之夜的皮肤，你会发现将数字资产从一个人转移到另一个人的困难程度。

这就是区块链的用处所在！区块链为数字资产提供了一个协调层，赋予用户所有权和管理权限。区块链为非同质化资产添加了一些独特的属性，改变了用户和开发人员与这些资产的关系。

![](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210315120252.png)

##### 规格化

传统的数字资产——从赛事门票到域名——在数字世界中没有统一的表现形式。一款游戏可能会以一种完全不同于事件售票系统的方式呈现其游戏内部的收藏品。通过在公共区块链上表示不可替代的令牌，开发人员可以构建与所有不可替代令牌相关的公共、可重用、可继承的标准。这包括所有权、转移和简单访问控制等基本原语。附加的标准(例如，关于如何显示非功能性测试的规范)可以被分层在应用程序的顶层以实现丰富的显示。

这类似于数字世界的其他构建块，如用于图像的JPEG或PNG文件格式，用于计算机之间的请求的HTTP，以及用于在web上显示内容的HTML / CSS。区块链在上面添加了一层，为开发人员提供了一组全新的有状态原语，可以在其上构建应用程序。

##### 互操作性

非可替换令牌标准允许非可替换令牌轻松地跨多个生态系统移动。当开发者启动一个新的NFT项目时，这些NFT可以立即在几十个不同的钱包供应商中看到，可以在市场上交易，最近还可以在虚拟世界中显示。这是可能的，因为开放标准为读取和写入数据提供了清晰、一致、可靠和经过许可的API。