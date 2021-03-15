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

传统的数字资产——从赛事门票到域名——在数字世界中没有统一的表现形式。一款游戏可能会以一种完全不同于赛事售票系统的方式呈现其游戏内部的收藏品。通过在公共区块链上发行非同质化 token ，开发人员可以构建与所有的非同质化 token 相关的公共、可重用、可继承的标准。这包括所有权、转移和简单访问控制等基本原语。附加的标准(例如，关于如何展示一个 NFT 的规范)可以被分层在应用程序的顶层以实现丰富的显示。

这类似于数字世界的其他构建块，如用于图像的 JPEG 或 PNG 文件格式，用于计算机之间请求的 HTTP 协议，以及用于在 web 上显示内容的 HTML / CSS 。区块链在上面添加了一层，为开发人员提供了一组全新的状态原语，以在其上构建应用程序。

##### 互操作性

非同质化 token 标准允许非同质化 token 轻松地跨多个生态系统移动。当开发者开启一个新的 NFT 项目时，这些 NFT 可以立即在几十个不同的钱包供应商上可见，可以在市场上交易，最近还可以在虚拟世界中显示。这是可能的，因为开放标准为读取和写入数据提供了清晰、一致、可靠和经过许可的API。

##### 可交易性

互操作性带来的最引人注目的功能是开放市场上的自由贸易。 用户首次可以将商品移出其原始环境，并进入一个市场，在那里他们可以利用复杂的交易功能，例如 [eBay 式拍卖](https://opensea.io/blog/announcements/introducing-ebay-style-auctions-for-crypto-collectibles/)，[投标](https://opensea.io/blog/tutorials/how-to-bid-on-crypto-collectibles/)，[捆绑销售](https://opensea.io/blog/announcements/introducing-bundles/)以及以任何货币出售的功能，例如[稳定币](https://opensea.io/blog/announcements/buy-and-sell-crypto-collectibles-with-dai/)和[应用程序所特有的货币](https://opensea.io/blog/announcements/buy-and-sell-crypto-collectibles-with-mana/)。

特别是对于游戏开发商来说，资产的可交易性代表着从封闭经济向开放自由市场经济的过渡。 游戏开发商不再需要管理经济的每一个环节：从资源供应到定价再到资本控制。 相反，他们可以让自由市场承担沉重的负担！

##### 流动性

非同质化 token 的即时可交易性将导致更高的流动性。 NFT 市场可以满足各种受众面的需求，从硬核的交易员到新手，都可以使资产更广泛地接触更多的购买者。 与 2017 年的 ICO 繁荣催生出由即时流动代币驱动的新资产类别一样，NFT 扩大了独特数字资产的市场。

##### 不变性和可证明的稀缺性

智能合约允许开发人员对非同质化 token 的供应设置硬上限，并强制在发行 NFT 之后无法修改永久属性。 例如，开发人员可以以编程方式强制执行以下行为，只能创建特定数量的特定稀有物品，同时保持更多常见物品的供应量无限。 开发人员还可以通过链上编码来强制特定属性不随时间变化。 对于艺术而言，这尤其有趣，因为**艺术在很大程度上取决于原始作品的可证明的稀缺性。**

##### 可编程性

当然，像传统的数字资产一样，NFT 也是完全可编程的。 CryptoKitties（我们将在后面讨论）将繁衍机制直接写进代表数字猫的合约中。 当今许多 NFT 具有更复杂的机制，例如锻造，制作，兑换，随机生成等。设计空间充满了可能性。

### 非同质化 token 的标准

标准是使非同质化 token 变得强大的部分原因。 他们向开发人员保证资产将以特定方式运行，并准确描述如何与资产的基本功能进行交互。

#### ERC721

由 CryptoKitties 开创的 [ERC721](http://erc721.org/) 是代表非同质化的数字资产的第一个标准。 ERC721 是可继承的 Solidity 智能合约标准，这意味着开发人员可以通过从 [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/ERC721.sol) 库中导入它来轻松创建符合 ERC721 的新合约（我们在[此处](https://docs.opensea.io/docs)提供了有关创建第一个 ERC721 合约的有用教程）。 ERC721 实际上相对简单：它提供唯一标识符（每个标识符代表一个资产）到代表该标识符所有者的地址的映射。 ERC721 还使用 `transferFrom` 方法提供了一种许可的方式来转移这些资产。

```java
interface ERC721 {
  function ownerOf(uint256 _tokenId) external view returns (address);
  function transferFrom(address _from, address _to, uint256 _tokenId) external payable;
}
```

如果你细想一下，这两种方法实际上就是代表 NFT 所需要的：一种检查谁拥有什么的方法以及一种转移事物的方法。 该标准还有其他一些特色（其中有些对 NFT 市场非常重要），但是 ERC721 的核心是非常基本的。

#### ERC1155

由 [Enjin](https://enjinx.io/) 团队开创的 [ERC1155](https://blog.enjin.io/erc-1155-token-standard-ethereum/) 将半同质化的思想带入了 NFT 世界。 在 ERC1155 中，ID 代表的不是资产，而是资产的类别。 例如，一个 ID 可能代表 “剑” ，而一个钱包可能拥有其中的 1000 把剑。 在这种情况下，balanceOf 方法将返回钱包拥有的剑的数量，并且用户可以通过使用 “ sword” 的 ID 调用 `transferFrom` 来转移这些剑中的任意数量。

```java
interface ERC1155 {
  function balanceOf(address _owner, uint256 _id) external view returns (address);
  function transferFrom(address _from, address _to, uint256 _id, uint256 quantity) external payable;
}
```

这种系统的一个优势是效率：如果使用 ERC721，用户想要转移 1000 把剑，他们将需要针对 1000 个唯一 token 修改智能合约的状态（通过调用 transferFrom 方法）。 而如果使用 ERC1155 ，开发人员仅需要对数量为 1000 的对象调用 transferFrom ，从而执行一次转移操作。 当然，这种提高的效率伴随着信息的丢失：我们再也无法追踪单把剑的历史了。

另请注意， ERC1155 是一个提供了 ERC721 功能的超集，这意味着可以使用 ERC1155 来构建 ERC721 资产（您只需为每个资产分配一个单独的 ID 和数量 1）。 由于这些优势，我们最近目睹了 ERC1155标准被越来越多的采用。 OpenSea 最近在 Github 上开发了一个 [repo](https://github.com/ProjectOpenSea/opensea-erc1155) ，以开始使用 ERC1155 标准。

![ERC20，ERC721 和 ERC1155 标准的剖析。 ERC20 将地址映射到金额，ERC721 将唯一的 ID 映射到所有者，而 ERC1155 具有将 ID 映射到所有者到金额的嵌套映射。](/home/whisker/.config/Typora/typora-user-images/image-20210315165832735.png)

##### 可组合资产

由 [ERC-998 标准](https://github.com/ethereum/eips/issues/998)主导的可组合资产提供了一个模板，NFT 可以通过该模板拥有非同质化和同质化的资产。 在主网上只部署了几个可组合的 NFT ，但是我们认为未来将有很多机会将它们投入使用！

> ...加密猫可能拥有抓挠柱和餐盘； 餐盘可能包含一些同质化的 “食物” 代币。 如果我出售加密猫，我将出售加密猫相关的所有财产。

#### 非以太坊标准

尽管以太坊是目前大多数行动的发生地，但其他链上还是出现了其他几种 NFT 标准。 由 [Mythical Games](https://mythical.games/) 团队首创的 [DGoods](https://dgoods.org/) 致力于在EOS 上提供一个功能丰富的跨链标准。 Cosmos 项目还正在开发一个 [NFT 模块](https://github.com/cosmos/cosmos-sdk/issues/4046)，该模块可作为 [Cosmos SDK](https://github.com/cosmos/cosmos-sdk) 的一部分使用。

### 非同质化 token 元数据

正如之前提到的，`ownerOf` 方法提供了一种查找 NFT 所有者的方法。 例如，通过在 [CryptoKitties 智能合约](https://etherscan.io/address/0x06012c8cf97bead5deae237070f9587f8e7a266d#readContract)上查询 `ownerOf（1500718）` ，我们可以看到在写下本文时 CryptoKitty #1500718 的所有者是一个地址为 0x6452... 的帐户。这可以通过在 [OpenSea](https://opensea.io/assets/0x06012c8cf97bead5deae237070f9587f8e7a266d/1500718) 或 [CryptoKitties.co](https://www.cryptokitties.co/kitty/1500718) 访问其 CryptoKitty 来验证。

![image-20210315181757317](/home/whisker/.config/Typora/typora-user-images/image-20210315181757317.png)

但是，OpenSea 和 CryptoKitties 如何知道 CryptoKitty＃1500718 长什么样呢？ 而且如何知道它的名字和特殊属性？

这就是元数据的功劳了。元数据为特定 token ID 提供描述性信息。 对于 CryptoKittty ，元数据是猫的名字，猫的图片，一段描述以及任何其他特征（在 CryptoKitties 中称为 “cattributes” ）。 对于赛事门票，元数据除了名称和描述之外，还可能包括赛事的日期和门票的类型。 上面描述的猫的元数据可能看起来像这样：

```json
{
  "name": "Duke Khanplum",
  "image": "https://storage.googleapis.com/ck-kitty-image/0x06012c8cf97bead5deae237070f9587f8e7a266d/1500718.png",
  "description": "Heya. My name is Duke Khanplum, but I've always believed I'm King Henry VIII reincarnated."
}
```

问题在于如何和在何处存储此数据，以便关注 NFT 的应用程序可以访问它。