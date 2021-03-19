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

![ERC20，ERC721 和 ERC1155 标准的剖析。 ERC20 将地址映射到金额，ERC721 将唯一的 ID 映射到所有者，而 ERC1155 具有将 ID 映射到所有者到金额的嵌套映射。](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210315200923.png)

##### 可组合资产

由 [ERC-998 标准](https://github.com/ethereum/eips/issues/998)主导的可组合资产提供了一个模板，NFT 可以通过该模板拥有非同质化和同质化的资产。 在主网上只部署了几个可组合的 NFT ，但是我们认为未来将有很多机会将它们投入使用！

> ...加密猫可能拥有抓挠柱和餐盘； 餐盘可能包含一些同质化的 “食物” 代币。 如果我出售加密猫，我将出售加密猫相关的所有财产。

#### 非以太坊标准

尽管以太坊是目前大多数行动的发生地，但其他链上还是出现了其他几种 NFT 标准。 由 [Mythical Games](https://mythical.games/) 团队首创的 [DGoods](https://dgoods.org/) 致力于在EOS 上提供一个功能丰富的跨链标准。 Cosmos 项目还正在开发一个 [NFT 模块](https://github.com/cosmos/cosmos-sdk/issues/4046)，该模块可作为 [Cosmos SDK](https://github.com/cosmos/cosmos-sdk) 的一部分使用。

### 非同质化 token 元数据

正如之前提到的，`ownerOf` 方法提供了一种查找 NFT 所有者的方法。 例如，通过在 [CryptoKitties 智能合约](https://etherscan.io/address/0x06012c8cf97bead5deae237070f9587f8e7a266d#readContract)上查询 `ownerOf（1500718）` ，我们可以看到在写下本文时 CryptoKitty #1500718 的所有者是一个地址为 0x6452... 的帐户。这可以通过在 [OpenSea](https://opensea.io/assets/0x06012c8cf97bead5deae237070f9587f8e7a266d/1500718) 或 [CryptoKitties.co](https://www.cryptokitties.co/kitty/1500718) 访问其 CryptoKitty 来验证。

![image-20210315200942227](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210315200943.png)

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

#### 链上和链下的对比

开发人员首先要决定是用什么元数据来表示链上和链下。也就是说，你是将元数据直接编码进表示 token 的智能合约中，还是单独托管它?

##### 链上元数据

在链上表示元数据的好处是：1) 它永久驻留在 token 中，在任何给定应用程序的生命周期之外保持，2) 它可以根据链上逻辑进行更改。如果资产想要拥有远超原始创造的持久价值，那么第 1 点很重要。例如，一件数字艺术作品被认为可以保存到各个时代，不管最初用来创作艺术作品的网站是否还存在。因此，它的元数据必须与 token 标识符的生命周期保持一致。

此外，链上逻辑可能需要与元数据交互。以 CryptoKitties 为例，CryptoKitties 的 “代数” 会影响 CryptoKitties 的繁殖速度，而繁殖过程是链式的(代数更高的猫繁殖速度较慢)。因此，智能合约内部的逻辑需要能够从其内部状态读取元数据。

##### 链下元数据

尽管链上元数据存在这些好处，大多数项目都是在链下存储元数据，这只是因为以太坊区块链当前的存储限制。因此， ERC721 标准包含一个名为 `tokenURI` 的方法，开发人员可以实现该方法来告诉应用程序在何处查找给定事物的元数据。

```java
function tokenURI(uint256 _tokenId) public view returns (string)
```

`tokenURI` 方法返回一个公共 URL 。这将返回一个 JSON 数据字典，类似于上面的 CryptoKitty 示例字典。这个元数据应该符合官方的 [ERC721 元数据标准](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md)，以便 OpenSea 等应用程序能够使用它。在 OpenSea 上，我们希望让开发者能够构建更丰富的元数据，以便在我们的市场中显示，因此我们向 [ERC721 元数据标准添加了扩展](https://docs.opensea.io/docs/metadata-standards)，允许开发者去包含特征、动画和背景颜色等内容。

![image-20210315214046901](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210315214048.png)

#### 链下存储解决方案

如果你在链下存储元数据，你有几个选择:

##### 中心化服务器

最简单的存储元数据的方法是在某个中心化的服务器上，或者像 AWS 这样的云存储解决方案上。当然，这也存在着缺点：1) 开发人员可以随意更改元数据；2) 如果项目离线，元数据可能会从原始来源中消失。为了缓解问题 2 ，现在有一些服务(包括 OpenSea )将元数据缓存到自己的服务器上，以确保即使原始主机解决方案出现故障，也能有效地为用户提供服务。

##### IPFS

越来越多的开发者，尤其是数字艺术领域的开发者，正在使用[星际文件系统(IPFS)](https://ipfs.io/)在链下存储元数据。IPFS 是一种点对点文件存储系统，允许跨计算机托管内容，这样文件就可以在许多不同的位置被复制调用。这确保了 A) 元数据是不可变的，因为它是由文件的哈希唯一寻址的，并且 B) 只要有节点愿意托管数据，数据就会随着时间持续存在。现在有像 [Pinata](https://pinata.cloud/) 这样的服务，通过处理部署和管理 IPFS 节点的基础设施，使开发人员的这一过程更简单，而[备受期待的 Filecoin 网络](https://docs.filecoin.io/go-filecoin-tutorial/Storing-on-Filecoin.html#table-of-contents)(理论上)将在 IPFS 上添加一层，以激励节点托管文件。

### 非同质化 token 的历史（2017-2020）

既然我们了解了什么是非同质化 token 以及如何构建它们，那么让我们深入了解它们是怎么来的。

#### — 公元 0 年：在 CryptoKitties 之前

NFT 的实验始于比特币网络上[彩色硬币](https://en.bitcoin.it/wiki/Colored_Coins)的出现。在比特币交易对手系统上构建的青蛙角色 PePe 插画，[Rare Pepes](http://rarepepedirectory.com/) 是首批作品之一。
其中一些在 [eBay 上出售](https://www.dailydot.com/unclick/rare-pepe-frog-meme-economy/)，还有一组罕见的青蛙 PePe 组合后来[在纽约的现场拍卖会上出售](https://www.vice.com/en_us/article/ev57p4/i-went-to-the-first-live-auction-for-rare-pepes-on-the-blockchain)。

第一个基于以太坊的 NFT 实验是 [CryptoPunks](https://www.larvalabs.com/cryptopunks)，它由 1 万个独特的可收集朋克组成，每个朋克都有一组独特的特征。CryptoPunks 由 [Larva Labs](https://larvalabs.com/) 打造，以链上市场为特点，可用于 MetaMask 等钱包，降低了与 NFT 交互的准入门槛。现如今，由于 CryptoPunks 的供应有限，并且在早期用户社区中拥有强大的品牌， CryptoPunks 可能是真正的数字古董的最佳选择。此外，在以太坊网络上的朋克生活使他们可以与市场和钱包互操作(由于它们早于 ERC721 标准，所以比起新资产不那么吸引人)。

![image-20210317124726352](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210317124737.png)

#### 公元 0 年：CryptoKitties 的诞生

[CryptoKitties](https://cryptokitties.co/) 是第一个将 NFT 纳入主流的项目。 CryptoKitties 于 2017 年末在 ETH Waterloo hackathon 上推出，其特色在于这是一个原生的链上游戏，允许用户将数字猫一起繁殖以生产出稀有程度不同的新猫。在一次[荷兰式拍卖行](https://www.investopedia.com/terms/d/dutchauction.asp)中， “ 第 0 代” 猫被拍卖了，与此同时新猫也可以在二级市场上出售。

尽管游戏社区中的一些人后来将 CryptoKitties 标记为 “不是真正的游戏” ，但鉴于区块链的设计约束，该团队实际上在开拓链上游戏机制方面做了很多工作。为此，他们建立了一种链上繁殖算法，该算法隐藏在确定猫的遗传密码的闭源智能合约中（反过来又确定了猫的 “类别” ）。 CryptoKitties 团队甚至还通过[复杂的激励系统](https://medium.com/cryptokitties/why-love-isnt-free-for-cryptokitties-7cc00dd2bf6d)确保了繁殖的随机性，并且具有预见性，可以保留某些小 ID 的猫，以便以后用作促销工具。最后，他们开创了荷兰式拍卖的智能合同，后来成为 NFT 的主要价格发现机制之一。 CryptoKitties 团队卓越的远见卓识为 NFT 空间的发展提供了巨大的推动力。

我们认为 CryptoKitties 的病毒性可以归结为：

##### 投机机制

CryptoKitties 的繁殖和交易机制带来了一条清晰的获利途径：买几只猫，将它们繁殖成一只稀有的猫，将它转手，重复以上过程（或简单地买入一只稀有的猫，希望有人会来并购买）。这推动了繁殖者社区的发展：致力于繁殖和倒买倒卖稀有猫的用户。只要有新的用户进入并玩游戏，价格就会上涨。

在市场最疯狂的时候，CryptoKitties 的交易量接近 5000 ETH，[Founder Cat＃18](https://opensea.io/assets/0x06012c8cf97bead5deae237070f9587f8e7a266d/18) 的售价为 253 ETH（当时价值 110000美元 ）。后来这笔交易被 [Dragon 的 600 ETH 交易](https://opensea.io/assets/0x06012c8cf97bead5deae237070f9587f8e7a266d/896775)所超越，当时的交易价格为 170000 美元（2018年9月），尽管许多人猜测 [Dragon 交易是非法的](https://thenextweb.com/hardfork/2018/09/05/most-expensive-cryptokitty/)。这些高昂的价格吸引了更多的用户进入淘金热。

##### 病毒式爆发的故事

CryptoKitties 成功的另一个因素就是[这个故事写的那样](https://techcrunch.com/2017/12/03/people-have-spent-over-1m-buying-virtual-cats-on-the-ethereum-blockchain/)。 CryptoKitties 既可爱又可共享，而且很有趣，而购买 1000 美元的数字猫的想法是如此荒唐，以至于成为了一个好新闻。此外，智能合约继续一款 “以太坊” 的爆款应用，这本身就构成了一个故事。由于以太坊一次只能处理有限数量的交易（约 15 笔交易/秒），因此网络上的更高吞吐量导致 pending 状态的交易池的增长和 gas 价格的上涨。每日平均待处理交易从 1500 个交易增加到 11000 个交易。新的潜在猫买家支付天价费用，并需要最终等待数个小时才能确认交易。

这些因素导致了“ CryptoKitty 泡沫” ：进入 CryptoKitty 世界的新需求，价格上涨以及价格上涨带来了新需求。当然，所有泡沫最终都必会破灭。 12 月初，小猫的平均价格开始下降，数量下降。许多人意识到，相对于 “真实游戏” 而言，CryptoKitties 游戏本来就很原始，不会吸引更多的投机者。一旦新奇感消失了，市场就蒙受了损失。如今，CryptoKitties 每周的交易量约为 50 ETH 。

![image-20210317220531340](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210317220544.png)

![image-20210317220554585](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210317220555.png)

#### 2018年：炒作，棘手的游戏以及 layer 2

尽管市场低迷，但CryptoKitties的成立为许多人提供了一个神奇的时刻。 第一次，一个团队部署了一个基于非金融区块链的应用程序，该应用程序已经进入了技术主流，尽管只有几个星期。 在CryptoKitties之后，随着投资者和企业家开始思考拥有数字资产的新方式，NFT在2018年初经历了第二次小规模炒作。

![image-20210317220625250](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210317220626.png)

##### layer 2 的游戏及其体验

在 CryptoKitties 出现了创新的 [“layer 2” 游戏](https://www.coindesk.com/the-emerging-trends-in-the-blockchain-gaming-world)之后的那个时期，这种游戏是由第三方开发人员在 CryptoKitties 之上构建的，与原始 CryptoKitties 团队没有任何关系。 CryptoKitties 的魔力在于可以 “无准入机制地” 开发这些体验：开发人员可以简单地将他们自己的应用程序置于公共 CryptoKitty 智能合约之上。从某种意义上讲， CryptoKitty 可以在原始环境之外拥有自己的生活。例如，[Kitty Race](https://kittyrace.com/) 允许您的 CryptoKitties 通过相互竞争以赢得 ETH ，而 [KittyHats](https://kittyhats.co/#/) 则允许用户使用帽子和绘画来装饰自己的 CryptoKitties 。后来， [Wrapped Kitties](https://wrappedkitties.com/) 让 Kitty 和 [DeFi](https://blog.coinbase.com/a-beginners-guide-to-decentralized-finance-defi-574c68ff43c4) 结合起来，让您将 CryptoKitties 转换为同质化的 ERC20 代币，以便在去中心化交易所进行交易，这对 CryptoKitty 市场产生了各种有趣的影响。 Dapper Labs（ CryptoKitties 背后的新成立公司）成立了 [KittyVerse](https://www.cryptokitties.co/kittyverse) ，以拥抱这些项目。

![image-20210318000227986](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210318000229.png)

###### 棘手的问题

这一时期也出现了 “烫手山芋般” 的游戏。如果你已经知道什么是 “烫手山芋般” 的游戏，那你就是一个真正的 NFT 的老炮儿了。2018 年 1 月，一款名为 CryptoCelebrities 的游戏发布。它的机制很简单。首先，买一个可以收藏的明星 NFT 。当即，明星就变成了可以以更高的价格购买(或 “可掠夺” )的，这是之前价格的增量。当有人购买了你的明星，你就可以在你的购买价格和新购买价格之间做出调整(减去开发者费用)。只要有人愿意买你的明星，你就会获利。然而，如果你被发现是最后一个拥有明星的人，你就惨了。

由于这种投机机制，CryptoCelebrity 机制被证明了是一种难以置信的病毒式传播，像 [Donald Trump](https://opensea.io/assets/0xbb5ed1edeb5149af3ab43ea9c7a6963b3c1374f7/271) 这样的明星以天文数字的高价出售( 123 ETH，当时约合 13.7 万美元)。虽然 CryptoCelebrity 游戏可能会损害整个空间，但实际上我们认为，对定价和拍卖机制的试验是 NFTs 设计空间中令人兴奋的一部分。

##### 风险资本利息

2018年初，风险资本和加密基金也对 NFT 领域产生了好奇。CryptoKitties 从[顶级投资者那里筹集了 1200 万美元](https://techcrunch.com/2018/03/20/cryptokitties-raises-12m-from-andreessen-horowitz-and-union-square-ventures/)，去年 11 月又[筹集了 1500 万美元](https://fortune.com/2018/11/01/cryptokitties-samsung-google-venrock/)。由 Farmville 联合创始人创建的 Rare Bits 公司在 2018 年初筹集了 600 万美元，区块链游戏工作室 Lucid Sight 也[筹集了600万美元](https://venturebeat.com/2019/04/02/lucid-sight-raises-6-million-to-take-blockchain-games-to-traditional-platforms/)。后来， [Forte 与 Ripple 共同募集了 1 亿美元的区块链游戏基金](https://venturebeat.com/2019/03/12/forte-and-ripple-form-100-million-fund-for-mainstream-blockchain-games/)。Immutable( Gods Unchained 背后的公司)[从 Naspers Ventures 和 Galaxy Digital 融资 1500 万美元](https://www.bloomberg.com/press-releases/2019-09-23/immutable-raises-15-million-in-series-a-funding-from-naspers-ventures-and-galaxy-digital-eos-vc-fund)。在 Javelin Venture Partners 的带领下，神话游戏公司为 EOS 上的一款[旗舰游戏 Blankos Block Party 筹集了 1900 万美元](https://venturebeat.com/2019/11/20/mythical-games-raises-19-million-for-blockchain-based-games-with-player-owned-economies/)。

OpenSea 获得了[适度的种子轮](https://opensea.io/blog/announcements/opensea-raises-2-million/)和战略投资，以进一步推进我们的愿景，建设一个普遍的开放市场。非常感谢我们所有的投资者!

#### 2018-2019：回归基础建设

在 2018 年初经历了一个小规模的炒作周期后，NFT 项目稳定下来，人们又重新开始基础建设。像 [Axie Infinity](https://axieinfinity.com/) 和 [Neon District](https://www.neondistrict.io/) 这样的团队（在 CryptoKitties 成立后不久就开始了他们的创业之路）在他们的核心粉丝群体上投入了双倍的资金。 [NonFungible.com](http://nonfungible.com/) 为 NFT 市场建立了一个跟踪平台，并巩固了 “非同质化” 一词作为描述新资产类别的主要术语。

##### 数字艺术

在这个时候，艺术界开始对 NFT 产生兴趣。事实证明，数字艺术非常适合非同质化 token 。让实体艺术有价值的一个核心要素是，能够可靠地证明一件作品的所有权，并在某个地方展示它，这在数字世界中是从未有过的。一群兴奋的数字艺术家开始试验。

与此同时，数字艺术平台也出现了。[SuperRare](https://superrare.co/) ，[Known Origin](https://knownorigin.io/) ，[MakersPlace](https://makersplace.com/) 和 [Rare Art Labs](https://rareart.io/) 都建立了致力于发布和发现数字艺术的平台。像 [JOY](https://opensea.io/assets/joyworld-s2) 和 [Josie](https://opensea.io/assets/josie) 这样的其他艺术家也部署了自己的智能合约，在这个领域为自己创造了真正的品牌。拥有独特微支付系统的社交网络 [Cent](http://cent.co/) 成为人们分享和讨论加密艺术的热门社区。

![img](https://lh3.googleusercontent.com/sJ-5wTKPHUJNePVxCGRV4UTsOf8TvY6r0ML89J9EuDWNrA8CljGT9TAhCv9afmyJJI2s8QNKcvbXP5erh0EKBjwX)

##### NFT 铸币平台

NFT 铸币平台使得任何人都可以更容易地铸一个 NFT ，无论他们是否具备部署智能合约的开发技能。

2018 年年中， [Digital Art Chain](https://digitalartchain.com/) 启动了一个项目，允许用户用上传的任何数字图像制作 NFT ，这是该项目的第一个项目。同年，一个叫做 [Marble Cards](https://marble.cards/) 的项目增加了一个有趣的变化，允许用户在一个叫做 “marbling” 的过程中根据任何 URL 创建独特的数字卡片。这将根据 URL 的内容自动生成一个独特的设计和图像，并在数字艺术界引发了一些争议，以回应加密艺术的 “marbling” 。

2019 年，铸币工具显著成熟，尽管在管理过程中仍面临摩擦。 [Mintbase](https://mintbase.io/) 和 [Mintable](https://mintable.app/) 建立了各自的网站，致力于让普通人更容易创建自己的 NFTs 。 [Kred 平台](https://www.coin.kred/)让有影响力的人可以很容易地创建名片、收藏品和优惠券。 Kred 还与 CoinDesk 的[共识会议](https://www.coindesk.com/events/consensus-2019)合作，为参与者创建一个数字 NFT “赠品包” 项目。 OpenSea 创建了一个[简单的店面管理器](https://opensea.io/blog/developers/how-to-create-your-own-marketplace-on-opensea-in-three-minutes-or-less/)来部署智能合约，并在其中添加 NFTs 。

**更新**：在 2020 年，这些平台都有一定的进步，连同 [Rarible](https://rarible.com/) 和 [Cargo](https://app.cargo.build/) ，有更多的批量创建、可解锁内容和富媒体的功能。这使得艺术家、数字创造者甚至音乐家无需编写智能合约就可以创建 NFT 。到今年年底， OpenSea 不再需要支付与铸币相关的 Gas 费用，[使得 NFT 的制作免费](https://opensea.io/blog/announcements/introducing-the-collection-manager/)。

##### 传统 IP 正在走下坡路

继 CryptoKitties 之后，传统 IP 所有者多次进入加密收藏领域。 MLB 与 Lucid Sight 合作，于 2018 年 4 月推出 [MLB Crypto](https://fortune.com/2018/08/13/mlb-crypto-baseball-blockchain/) ，主要是一款链上棒球游戏。一级方程式赛车与 Animoca Brands 合作推出了 [F1DeltaTime](https://www.f1deltatime.com/) ，这款由 OpenSea 驱动的 [1-1-1 赛车的售价为 10 万美元](https://opensea.io/assets/0x3c62e8de798721963b439868d3ce22a5252a7e03/111)。《星际迷航》(Star Trek)在一款名为 [CryptoSpaceCommanders](https://opensea.io/assets/cryptospacecommanders) 的游戏中推出了一组飞船，几家获得许可的足球交易卡公司也上线了，包括 [Stryking](https://opensea.io/assets/stryking) 和 [Sorare](https://opensea.io/assets/sorare) 。最近，最大的实体收藏品销售商之一 [Panini America](https://en.wikipedia.org/wiki/Panini_Group) 宣布推出[基于区块链的交易卡收藏品](https://sludgefeed.com/panini-america-enters-blockchain-collectibles-market/)。 MotoGP [还与 Animoca 合作开发区块链游戏](https://www.coindesk.com/animoca-to-develop-motogp-blockchain-game-with-crypto-collectibles)。

##### 日本领衔行业

日本游戏开创了更高级的用户玩法，吸引了早期用户群体。 MyCryptoHeroes ，一款具有复杂的游戏内部经济的 RPG 游戏，登上了舞台，并持续在 DappRadar 的排行榜上名列前茅。
 MyCryptoHeroes 是第一个将链上所有权与更复杂的链下玩法相结合的游戏。用户可以在游戏中使用他们的英雄，然后当他们想在二级市场上出售他们时，将他们转移到以太坊。

![image-20210318134350588](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210318134402.png)

##### 虚拟世界的扩大

新的原生区块链虚拟世界启动了土地所有权和世界内资产的 NFT 。[Decentraland](https://decentraland.org/) 通过 ICO 为其 MANA token 筹集了 2500 万美元，启动了一项 1000 万美元的虚拟现实元宇宙土地销售。在 2018 年的大部分时间里， Decentraland 的 LAND NFT 的交易量超过了其他任何 NFT 。Decentraland 项目现在有了一个带有一些非常棒的早期体验的公测，如 [Battle Racers](https://battleracers.io/) ，这是一款可在游戏世界中玩的赛车游戏。

> NFT Crypto News Tower in Decentraland – 最新销售的 OpenSea 顶级 Dapps！
> 下面是公测地址： https://t.co/epU9Yd4FdI[@opensea](https://twitter.com/opensea?ref_src=twsrc^tfw) [@decentraland](https://twitter.com/decentraland?ref_src=twsrc^tfw) [@mycryptoheroes_](https://twitter.com/mycryptoheroes_?ref_src=twsrc^tfw) [@CryptoKitties](https://twitter.com/CryptoKitties?ref_src=twsrc^tfw) [@AxieInfinity](https://twitter.com/AxieInfinity?ref_src=twsrc^tfw) [@MLBChampions](https://twitter.com/MLBChampions?ref_src=twsrc^tfw) [@ChibiFighters](https://twitter.com/ChibiFighters?ref_src=twsrc^tfw) [@BlockchainCutie](https://twitter.com/BlockchainCutie?ref_src=twsrc^tfw) [@MixHyperDragons](https://twitter.com/MixHyperDragons?ref_src=twsrc^tfw) [#nft](https://twitter.com/hashtag/nft?src=hash&ref_src=twsrc^tfw) [pic.twitter.com/BDsAQA61nc](https://t.co/BDsAQA61nc)
>
> *— NFT Crypto News (@NFTCrypto)* [August 4, 2019](https://twitter.com/NFTCrypto/status/1158087390814040064?ref_src=twsrc^tfw)

另一个虚拟世界项目 [Cryptovoxels](https://www.cryptovoxels.com/) 采取了一种更精简的方法。 CryptoVoxels 在 2018 年年中推出了非常简单的由单个开发者领导的 webVR 体验，它逐渐扩大了自己的领域，谨慎的不出售超过需求的土地。如今， CryptoVoxels 的交易量已经超过 1700 ETH，土地的平均价格也在稳步上升。

![ CryptoVoxels 世界的数字艺术博物馆。点击[这里](https://www.cryptovoxels.com/play?coords=NE@83E,1U,237S)访问。](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210319000334.png)

CryptoVoxels (以及 Decentraland )最令人兴奋的元素是能够在数字世界中展示你的 NFT 。收藏爱好者已经创建了 CryptoKitty 博物馆、赛博朋克艺术画廊、NFT 日历、充满顶级 NFT 项目的塔，以及虚拟世界商店，在那里你可以为自己的化身购买可穿戴的物品。Cryppovoxels 环境在数字艺术家中迅速发展，尤其是在 [Cent](https://cent.co/) 的用户中。Cent 是一个新的内容平台，专注于加密人群。一些艺术家甚至使用 [Roll](http://tryroll.com/) 创造他们自己的货币，或者 “社交货币” ， Roll 是一款应用程序，可以方便地部署新的 ERC20 代币，并将他们的作品以他们的社交货币出售。

其他的虚拟世界项目也已经进入了这个领域，包括 [Somnium Space](https://somniumspace.com/) 和 [High Fidelity](http://highfidelity.com/) ，一个由 [Second Life](https://secondlife.com/) 的创造者开发的项目。最近，[The Sandbox](https://www.sandbox.game/en/) 为其类似 roblox 宇宙推出了一项土地出售活动，旨在为建造者和内容创造者提供支持。这是近期最受期待的区块链游戏之一。

2017 年末，[Enjin](https://enjin.io/) 在其 ICO 上筹集了 75041 个ETH，并扩展了其 “multiverse” 平台，这是一个基于 ERC1155 标准的游戏生态系统。 Enjin 的核心价值主张之一是能够轻松地将道具从一款游戏带到另一款游戏中。例如， Enjin 团队发布了一款 “通用的” (不是针对特定游戏) [Oindrasdain Axe](https://opensea.io/assets/0xfaafdc07907ff5120a76b34b731b278c38d6043c/36411184310952944508859562575390614563768575651911745716961922930335654352507) 。[遗忘神器](https://forgottenartifacts.io/)在游戏中添加了这把斧头作为装备武器，给已经拥有它的玩家一个理由来试玩他们的游戏。

##### 交易卡牌游戏

从一开始，交易卡牌游戏就很[适合 NFT](https://opensea.io/blog/trading-cards/sell-hearthstone-cards/) 。像[《万智牌》](https://magic.wizards.com/en)这样的实体纸牌游戏不只是一款游戏。它是一个[完整的经济体系](https://www.mtgsalvation.com/forums/magic-fundamentals/magic-general/689056-total-mtg-market-cap-secondary-market)，有几十个用于买卖和物物交换的相关网站和市场。虽然万智牌的数字版本，如[《炉石传说》](https://playhearthstone.com/en-us/)，理论上可以为其纸牌建立一个游戏内置市场，但这种做法可能会很麻烦，而且不一定与销售新卡包的商业模式相一致。
区块链使即时二级市场可以在游戏之外运作。

在进行了 500 万美元的卡片预售之后， [Immutable](https://immutable.com/) 发行了[《Gods Unchained》](https://opensea.io/assets/gods-unchained)，这大概是目前市场上最受追捧的区块链游戏。
当数字交易卡牌游戏《炉石传说》(Hearthstone)禁止一名职业玩家进入主流游戏领域时，他们进入了主流游戏的视线。
被释放的神作了如下声明:

![image-20210319103352229](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210319103353.png)