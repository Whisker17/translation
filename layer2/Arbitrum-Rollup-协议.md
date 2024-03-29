# Arbitrum Rollup 协议

原文链接：[Arbitrum Rollup Protocol](https://developer.offchainlabs.com/docs/rollup_protocol)

## Arbitrum Rollup 协议设计

Arbitrum Rollup 是一个由链上以太坊合约管理的链下协议。 dApp 开发人员可能会希望将自己以 Solidity 编写的一个或一组合同部署到 Layer 2 Arbitrum Rollup 链（ArbChain）上。

本文档从概念上描述了该设计，然后在某种程度上进行了形式上的描述。 最后，我们证明了该协议在一个诚实的质押者可确保安全以及最终确定性。

### Rollup 的基础

让我们从基础知识开始。 虚拟机（VM）的状态被编码为一颗 Merkle 树，因此可以计算 VM 状态的加密哈希。 在协议的任何一个时间点，VM 的某些状态都已被完全确认并处于最终状态。 这样的状态哈希就会被存储在链上。

协议的参与者可以提出一个有争议的断言（DA），其中包括在一定的技术先决条件下从某种状态哈希开始，VM 可以执行指定数量的计算步骤，从而得出指定的新状态哈希， 并且 VM 在该计算期间进行指定的付款并发出指定的日志事件。 DA 可能是有效的（即真实），也可能是无效的。 提出 DA 的一方将被要求在 DA 的有效性上提交押金。 （有关押金及其运作方式的更多信息，请参见下文。）

![img](https://developer.offchainlabs.com/docs/assets/rollupGraph1.png)

如上图所示，一个争议断言创建了一个协议最终必须解决的逻辑决策点。 如果 DA 有效，则系统将进入右上角的新状态，生成新的状态哈希，并具有 DA 中指定的附带效果（付款和日志）。 若争议断言是无效的，则系统向右下角方向更新，争议断言被系统拒绝，原来的 VM 状态不会发生变化。

### Arbitrum 2.0 协议

当前的 Arbitrum 协议比原始的 Arbitrum 协议有了重要的进步，因为它支持多个流水线 DA 。在新协议中，每个状态最多可以跟随一个 DA 。 如果一个状态后面没有跟随 DA ，则任何人都可以创建跟随它的 DA ，从而创建新的分支点。 结果如下图所示，将生成一棵存在平行宇宙的树。

![img](https://developer.offchainlabs.com/docs/assets/rollupGraph2.png)

### 押注

该协议的另一个重要部分是押注。任何人都可以在树上的一个正方形上下注。通过对正方形的下注，您可以断言该正方形将最终被协议确认。换句话说，你断言从当前状态开始到你所下注的正方形里的所有的 DA 做出的选择都是正确的。如果您错了，那么您可能会失去赌注。

押注操作无法撤消。您可以将赌注向右移动——选择在每个分支点上移或下移——但不能向左移动，因为这等于撤销了您先前做出的下注承诺。

提出有争议断言的一方必须与该 DA 的“认可其断言有效”的后继方框上下注。通常，他们可以通过将现有押注向右移动以将其放置在所需的后继方上来满足此要求。如果他们还没有下注，他们将不得不下注。

关于押注的更多详细信息：如果您下注的正方形被确认并成为公认的历史记录，则您可以选择收回押注。这意味着，如果您是对的，则可以将赌注保持在原位置，然后等待系统“追赶”您的赌注，然后您就可以收回赌注。

在这一点上，您可能会担心平行世界树会变得很大且有许多分支。实际上这不太可能发生，因为这将需要多方参与押注并且互不相同。他们中只要有一个是诚实的，其他所有人都将失去本金。更有可能的是，“树”实际上是有效的 DA 组成的一条链，所有押注都会存在在同一个分支上。下图显示了这种典型情况：基本上，一组“流水线” DA 最终可能会全部被接受。

![img](https://developer.offchainlabs.com/docs/assets/rollupGraph3.png)

### 押注截止日期

我们需要系统在尽可能短的时间内就每个有争议的断言做出决定。 因此，当将 DA 上链并创建分支点时，会有一个最后期限与该 DA 相关联。 由于这个期限足够长，每个人都有时间检查 DA 是否有效，并选择是否进行链上交易以抵押 DA 的结果。 如果有人想为该 DA 的有效性做出承诺或反对，则必须在截止日期之前这样做。 （你也可以在过期后押注，但是它们不会参与决定该 DA 的有效性。）一旦达到截止日期，与确定该 DA 相关的所有押注将被公开。 

### 争论

如果 Alice 和 Bob 质押在不同的正方形上，那么以下两种情况之一必然会发生。 要么存在从其中一个到另一个向右移动的路径——这意味着他们的主张彼此一致——要么没有这样的路径。 如果不存在将 Alice 和 Bob 下注的正方形连接起来的向右移动的路径，那么他们必然在某些问题上存在分歧。 它们之间始终存在唯一的争议点——一个唯一的 DA ，其中一个下注该 DA 有效，而另一个下注该 DA 无效。

![img](https://developer.offchainlabs.com/docs/assets/rollupGraph4.png)

每当两方之间发生争议时，系统都可以启动双方之间的交互式争议解决协议。争端解决协议在[这篇文章](https://developer.offchainlabs.com/docs/Dispute_Resolution)中进行了描述——在这里足以说它是一种二分法交互式协议，与我们在其他 Arbitrum 文档中所描述的类似。争端协议要求当事各方轮流采取行动，并规定每项行动的最后期限。未能在截止日期前采取行动的一方将放弃该挑战。

争端解决协议的结果是，将发现一方不正确。这一方将会被没收他们的质押金。押金将从其所在的正方形上删除。这笔钱中的一部分将提供给另一方质押人，其余的将被烧掉。

争议解决可以确定两个争议方之一是不正确的，但是我们不能推断出另一方一定是正确的。也许双方都犯了错误，或者也许双方正在串通以“控制”争端的结果。由于存在这些可能性，因此在争端结束时，只会发生失败者失去了他们的质押金。 （失败者的一半质押金将会发送给争端的胜者，另一半则被烧掉。）

可以同时进行多个争议，但是每个质押人最多可以同时处理一个争议。由于输家会失去他们的质押金，因此每次纠纷都会减少系统中的分歧。失去质押金的参与者可以根据需要重新进行下注，但是新质押金将无法影响已经超过质押截止时间的争议断言。这样的结果是，在 DA 的质押截止日期过去之后，争议将逐渐消除关于如何处理该 DA 的所有分歧。

### 结果的确认

一旦超出 DA 的质押截止时间，并且所有及时下注的质押金都存在与同一条分支上，则系统可以确认该 DA 的结果。 DA 被接受或拒绝，并且当前状态移至 DA 右侧的正确正方形。 如果 DA 被确认为有效，则其附带效果（如付款）将在链上实现。 这就是 VM 的状态更新的方式。

在通常情况下，参与者都会诚实行事，因为谁都不想因为对错误的主张下注而失去质押金。 在一条链中仅会对有效的 DA 进行断言，而没有人会对任何 DA 的无效分支进行质押。 在这种情况下，每个 DA 都会在质押期限后立即得到确认。

### 为什么它是无需信任的

Arbitrum Rollup 的一个重要特性是它无须可信——单个诚实方就可以让 VM 正确运行。 要了解为什么，请想象一下，Alice 总是在每个 DA 的正确分支上质押，并且如果不存在争议断言，她就会自己进行争议断言。 如果其他人做出了错误的 DA ，Alice 则会反对。

因为 Alice 只会在正确的分支上下注，所以她将赢得所遇到的每一个争议。 如果其他人不同意 Alice ，则他们要么（a）与一个不相关的第三方产生争议然后失去质押金，要么（b）最终与 Alice 发生争议并将质押金输给 Alice 。 无论哪种方式，每个不同意 Alice 的人都将最终失去他们的质押金。 只有同意 Alice 的质押的人才能幸存，因此，Alice 在这棵树上的路径最终将是唯一一个及时质押的分支——并且 Alice 的路径将得到确认。

![img](https://developer.offchainlabs.com/docs/assets/rollupGraph5.png)

因为在这种方案下的系统是无须可信的，所以如果 Alice 质押了一个正方形，并且她知道通过这个正方形的路径是正确的，那她就可以确定自己下注的正方形最终一定会被确认。 就 Alice 而言，这条路就像是敲定好了一样。

即使您没有质押某条路径，如果您看到有人质押了该路径，并且您相信其中至少有一个人是诚实的，那么您可以确定该路径最终会得到确认——就您而言，这条路就像是被敲定好一样。

### 无需信任的终局性的好处

为什么说无需信任的终局性很有价值？经典示例来自之前对其他 rollup 协议的讨论。假设一个虚拟机要向 Alice 付款。这个付款的行为本来就是诚实的，但要过一会儿才能在链上确认包含这笔支付的正方形。

无需信任的终局性决定了 Alice 可以立即拿到这笔钱。如果 Bob 有多余的钱，他可以立即将其转给 Alice ，以换取 Alice 未来尚未确认的收款（加上向 Bob 支付最低限度的小费）。Bob 只有在可以确定收款确实会发生的情况下才会这样做。Bob 可以通过质押诚实的结果来确保这一点——然后，他将对付款最终会产生抱有无须信任的信心。不仅 Bob 可以做到这一点。任何有资金的人都可以以同样的方式将其借给 Alice 和其他类似她的人。这些人可以通过提供较低的小费来相互竞争，从而降低了 Alice 立即获得其资金的费用。

关键是这种市场机制的生存能力取决于无须信任的终局性。如果“每个人”都已经知道某件事最终将会被确认，那么链上确认的延迟就不会造成不便。

这不仅适用于支付，而且适用于 VM 执行的其他操作。如果虚拟机要生成一个记录发生了什么事情的日志，那么无须信任的终局性意味着任何人都可以立即采取行动，因为这个日志最终一定会被链上确认的。

### 延迟攻击

由于该系统是无须信任的，因此不良行为者不能强行制造错误的结果。他们只能做到的是减慢系统的进度。这样做需要他们牺牲质押金，如果质押金的数量很大，代价将非常昂贵。

假设有人有动机宁愿牺牲质押金而发起延迟攻击。他们能造成的最严重的伤害是什么？

首先要注意的是，作恶者不能阻止诚实方继续构建诚实的分支。而且他们也不能阻止诚实方在最终确认诚实的分支上的无须信任的信心。

攻击者所能做的就是在错误的分支上下注，以延迟诚实路径上的链上确认。他们所投入的每笔质押金都会对诚实方造成另一场争议，诚实方可以拿走攻击者的一大部分质押金。一旦攻击者所有的质押金都被拿走，链上确认将继续进行。

如果攻击者在错误的结果上多次质押怎么办？那么这些质押金将会在争议中接连被罚没。如果有多个人质押诚实的结果，那么这些人都可以对攻击者提出争议，同时进行工作以取得攻击者的质押金。并且请注意，对于所有人而言，发生的事情将是显而易见的，很多人都希望能分一杯羹，质押正确的结果，以便他们能够利用争议平分攻击者的质押金。如果有 K 个诚实方进行质押，那么对于攻击者来说，将会花费 K 份质押金来换取一个延迟的争议期。如果攻击者放下更多的赌注，那么很可能会吸引更多诚实的赌注者。对于攻击者而言，这是一个灾难。

### 优化

可以进行各种优化来减少操作协议所需的链上记账数量，降低链上 gas 成本以及更容易获得针对延迟攻击者的利益获取。 本文档的目的是对 Rollup 协议进行通用和更易接受的概述，因此，我们将不在此处进行对优化的深耕。 但是，如果您担心该实现的成本，我们可以向您保证，EthBridge 监控所有这些的成本将低于您的预期。 有关详细信息，您可以查阅 [Arbitrum Rollup 代码](https://github.com/offchainlabs/arbitrum)。

## 更正式的协议描述

现在我们用一种更正式的方式来描述这个协议。

### 断言和子断言

首先，我们注意到在实践中，未来树不是二元的，而是四元的。 这是因为每个断言实际上包含三个子断言：inbox-top 子断言，消息子断言和执行子断言。 因此，每个 DA 都会引发四个后继节点：（0）所有子断言都是正确的，（1）inbox-top 子断言是错误的，（2）inbox-top 子断言是正确的，但消息子断言是错误的，（3）inbox-top 子断言和消息子断言是正确的，但是执行子断言是错的。 在树的每个点上，质押者都必须确定要支持的四种可能性中的哪一种

### 协议的状态

从逻辑上讲，协议的状态包括：

- LatestConfirmed 节点：rollup 树的节点，它是已在 L1 链上完全确认并执行的最新节点；
- 以 LatestConfirmed 节点为根的节点树，以及诱发该树的 DA ，包括这些 DA 的截止日期；
- 质押人集合，以及每个质押人在树的哪个节点上质押； 
- 和当前正在进行的一系列挑战。

### 挑战期

每个质押者一次最多只能面对一个挑战。 在任何时候，任何人都可以通过指向 DA 和两个质押者来发起挑战，使得（a）当前没有一个质押者正处在挑战期，并且（b）两个质押者直接或间接地放到 DA 的不同子代上，并且（ c）两位质押者最初在 DA 的截止日期之前进行了质押。 （只要满足上述三个条件，挑战就可以在相关 DA 的截止日期之前或之后开始。）

一旦开始挑战，挑战协议将最终宣布两个质押者之一成为挑战的失败者。 失败者将失去他们的押金，并且将不再是质押者。 （失败者可以稍后再进行质押。）

### 创建质押

尚未成为质押者的任何一方都可以随时在树的任何节点上质押。 系统记录最初何时引入质押者的质押，同时在截止日期之后的质押都将被视为是无效的。

### 移动质押

任何质押者都可以通过将质押向右移动到树中该质押的当前位置的后代的任何节点上。

### 收回质押

如果质押者在 LatestConfirmed 节点上进行质押，或者任何节点树中的节点的高度都小于 LatestConfirmed 节点，则质押者可以收回其质押。押金将返还给质押者，而该方不再是质押者。

在当前的实现中，有几个条件可以用来收回质押，这些条件共同涵盖了质押可能比 LatestConfirmed 更晚或以其他方式提出的所有方式。在实现中，如果满足以下条件，则可以收回质押：质押位于 LatestConfirmed 或它之前，或者质押位于非 LatestConfirmed 且既不是 LastConfirmed 的祖先节点也不是其子孙节点上。 （这样的质押是“未决议的”或无关紧要的。它不能参与挑战，也不会影响哪个节点会最终确认。因此，理性的质押者将让这种质押处于闲置状态，直到它晚于最新确认，然后将其收回。）当前的实现方式允许立即收回此类质押。）

此外，如果该质押位于截止期限已过的后继者的节点上，当前实现允许任何一方强行退还质押。这旨在激励质押者继续质押。如果每个挑战期质押者至少一次对最新状态进行质押，则永远不会陷入这种情况。

### 断言并不断更新树

一个对树的叶子节点进行质押的质押者可以根据该叶子节点的状态进行一次新的争议断言。通过创建四个新节点作为该先前叶子节点的子孙节点来更新树。发起争议断言的质押者将自己的质押向右移动到自认为正确的子孙节点上。

### 确认节点

除非 LatestConfirmed 节点是树的叶子节点，否则 LatestConfirmed 节点上必然会有 DA 。如果该 DA 的截止日期已过，并且如果在 DA 的截止日期之前质押的质押者都质押在该节点的同一个子节点（或其后代）上（并且至少有一个这样的质押者），则可以最终确认该子节点。它成为新的 LatestConfirmed 节点。

如果新确认的子节点对应于正确的 DA ，则在 L1 链上执行 DA 的所有附带效应：付款并发送日志。

### 裁剪树

如果满足以下两个条件，则可以从树中修剪子树：（a）子树的根节点是其截止期限已过的节点，（b）子树中没有在根的截止期限之前进行过的质押。这样的子树本质上是没有意义的，因为这样的子树不存在争议。修剪子树后，任何在其中质押的人都将退还其押金。

该规则的唯一例外是无法修剪 LatestConfirmed 节点。

### 工程优化

EthBridge 需要强制执行此协议的规则。为了节省存储空间，EthBridge 不存储整个树，而是存储叶子节点的 Merkle 哈希（哈希链包括树的内部节点的 Merkle 哈希）以及有关 LatestConfirmed 节点的信息，质押者的名单以及任何正在进行的挑战。

上述状态更改可以由任何一方发起，他们可以向 EthBridge 发出交易调用（txcall）来强制状态更新。调用者提交任何必要的 Merkle 证明以允许 EthBridge 确认节点。 （控制质押的动作当然必须由该质押者发起。）

## 协议的正确性

### 假设一名诚实的质押人的情况下的安全性证明

假设有一个诚实的质押者，其质押是在 LatestConfirmed 节点的截止日期之前发起的（因此必定在 LastConfirmed 节点的任何子孙节点的截止日期之前）发起。

每个节点将只有一个有效的子孙节点，因为之前我们定义了一个节点的四个子节点的有效性条件，因此它们中的一个必须为真。因此，树上将只有一个有效的叶子节点。

诚实的质押者可以通过始终对有效叶节点进行质押来保证安全性。诚实的质押者将能够赢得所有挑战，因此其押金将永远存在。只要仍保留此押金，任何 LatestConfirmed 的无效子孙节点就不能确认，因为诚实的质押者的押金在其他分支上的存在会证明该无效节点确认的条件之一是错误的。

因此，诚实的质押者可以防止无效节点的确认。

### 假设一名诚实的质押者，证明最终确认性

在与之前的证明相同的假设下，我们现在显示诚实的质押者可以确保某个有效的 DA 的最终确认性。

首先，我们知道一个有效的 DA 最终将被添加到 LatestConfirmed 的有效路径中。之所以如此，是因为诚实的质押者可以在有效路径上创建有效的 DA （如果尚不存在）。

现在足以证明诚实的质押者可以确保 LatestConfirmed 节点最终不断更新。这是正确的，因为 LatestConfirmed 节点有一个最终期限，截止期限总会到期。截止日期过后，和诚实的质押者相比，质押在其他分支的质押者数量有限。由于截止日期已过，此类不正确的质押者的数量将不再增加。诚实的质押者可以与这些不正确的质押者一一对应地发起挑战。诚实的质押者由于质押的是有效的分支，将能够赢得这些挑战，从而一个个铲除作恶的质押者。

一旦消除了所有作恶的质押者，诚实的质押者就可以使新节点得到确认。因此，最终将确认新的节点。

### 抑制对不正确分支机构的质押

假设至少有一个质押者合理地最大化了他们的收入，而其他每个质押者都不会去对不正确的分支进行质押。

如果质押者质押在不正确的分支上，则理性的质押者可以通过在正确的分支上质押并向不正确的质押者发起挑战来做出响应。理性的质押者将能够赢得挑战并获得不正确的质押者的押金。

请注意，不正确的质押者无法通过安排与同为不正确的质押者之间的争论来保护自己的押金。因为争议的获胜者仅获得失败者一半的押金，另一半被烧毁，因此产生了相互勾结的作恶方将会失去一半的押金，这会抑制人们参与虚假的争议的可能性。

### 诚实质押者的押金的最终可回收性

一个诚实的质押者将永远能够收回他们的利益。 一个诚实的质押者不会输掉任何挑战，因此，一旦质押了，并且无论押金被转移到哪里，押金始终存在。

而且，如果诚实的质押者停止转移其押金，则 LatestConfirmed 节点最终将赶上并超出诚实的质押者的质押高度。 （如果链上没有任何活动，则质押者可以发布空断言，以确保 LatestConfirmed 的深度最终超过质押高度。）此时，质押者将能够收回其押金。
