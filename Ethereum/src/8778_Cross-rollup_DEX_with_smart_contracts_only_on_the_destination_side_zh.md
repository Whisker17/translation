# 仅在目标端部署智能合约的跨 Rollup DEX

原作者：Vitalik Buterin

原文链接：[Cross-rollup DEX with smart contracts only on the destination side](https://ethresear.ch/t/cross-rollup-dex-with-smart-contracts-only-on-the-destination-side/8778/2)

假设我们有两种 rollup 解决方案 A 和 B，Alice 想要用 rollup A 上一定数量的 token 来换取 rollup B 上相同的 token 。目前已经有人提出[方案](https://hop.exchange/whitepaper.pdf)解决这个问题了，如果 rollup A 和 rollup B 都是完全支持智能合约时，那么就可以去中心化地实现这个假设。而这篇文章提出的是，当仅有 rollup B 完全地支持智能合约时 (且 rollup A 只能处理简单交易) 如何实现跨 rollup 转账。

我们假设 rollup A 上的交易有某种形式的“备注字段”；如果没有备注的话，可以使用该交易值的低位数字作为备注发送。

## 提案

假设存在一个交易所中介 Ivan (在现实实现中我们有许多中介可供选择)。Ivan 在 rollup A 中拥有一个 (完全由他控制的账户) `IVAN_A`。同时，Ivan 还在 rollup B 的智能合约 `IVAN_B` 中存了一些资金。

智能合约 `IVAN_B` 具有以下规则：

-  如果任意用户发送了一笔 `TRADE_VALUE` 个 token 到账户 `IVAN_A` 的交易 ，交易中还存在一个目的地址 `DESTINATION` 作为备注，然后在 `MIN_REDEMPTION_DELAY` 个区块之后，该用户就可以发送一笔交易至账户 `IVAN_B` 中 (其中包括之前的转账证明)，这笔交易会排队等候提款 `TRADE_VALUE` 个 token 至地址 `DESTINATION` 中。
-  等待一定的延迟 (例如一天) 后，按照转账打包进 rollup A 的批次和索引顺序处理提款。
-  当 Ivan 发现其账户 `IVAN_A` 收到款项时，他就可以亲自发送 `TRADE_VALUE * (1 - fee)` 个 token 至 `DESTINATION` 中。他可以用 `IVAN_B` 中提供的方法发送交易来完成上述操作，这个方法保存了一个记录，防止合约中的自动发送条款触发该交易。

预期的行为很简单：

- Alice 发送一笔交易至账户 `IVAN_A` 中 (包含 N  token  和一个备注 `ALICE_B`)
- Ivan 通过 `IVAN_B` 发送 `TRADE_VALUE * (1 - fee)` 个 token 到 `ALICE_B` 中

第二笔交易紧接着第一笔交易发生。如果 Ivan 可以证明第一笔交易和第二笔交易之间的时间戳差非常小，那么合约甚至允许提高费用 fee 。

最糟糕的情况是，Ivan 没有如他所期望那样向 `ALICE_B` 发送 token 。遇到这种情况，Alice 可以等待 rollup A 上的交易确认之后，在 rollup B 上找到其他获取 token 的替代路径来支付 fee ，然后就可以自己认领其资金。

## 资本成本

该方案的主要限制是，`IVAN_B` 需要持有大量的资金，以确保所有交易发送者都能得到支付。尤其是，假设出现以下情况：

- 我们将交易上限设置为 `TRADE_LIMIT` (所以当发送至 `IVAN_A` 的交易超出限额 `value > TRADE_LIMIT` 时，这笔交易是无效的)
-  每个 rollup 批次最多可以包含 `TXS_PER_BATCH` 笔交易

Alice 可以自查 rollup A 下一批需要处理的交易之前，还有多少未处理的交易，用她在合约 `IVAN_B` 中的资金减去这些交易的总值，并检查剩余的金额是否足够。由于提款是按序处理的 (这是上述的排列机制的目的)，Alice 不需要担心合约先处理其他提款申请，再处理她的提款交易申请。

在每个批次中最大交易额为 `TRADE_LIMIT * TXS_PER_BATCH` ，因此 `IVAN_B` 合约中至少需要有这么多的 ETH，还需要额外的足够资金包含未处理的交易。举个例子，假设 `TRADE_LIMIT` = 0.1 ETH (交易上限可以设得比较低，因为一笔大额交易可以分成几笔小交易完成)，并且 `TXS_PER_BATCH = 1000`。那么，合约 `IVAN_B` 需要持有 100 ETH。

注意，这个设计中还包括隐含的费用，因为**交易额超过 0.1 ETH 的任意用户都需要浪费区块空间**。这与资本要求相权衡，也就是说，如果用户的区块浪费要求减半，那么其资本要求将翻倍，反之亦然。如果想要获得合适的平衡，那么隐含的 fee 要比市场上明确的费用少几倍。

如果我们想要减少或者消除这种浪费，可以这样设计 rollup A：让序列器发送一个已签名的信息，该信息证明了 Alice 在该批次的所有交易。然后 Alice 就会知道在她之前没有交易 (尽管恶意的序列器可以以高昂的成本欺骗 Alice)。

## 备注

上述设计基于一个假设：Rollup A 上的交易有一个备注字段，Alice 可以通过该备注指定 `ALICE_B` 作为她接收 token 的目的地址。如果 rollup 没有这种特性，那么我们可以使用以下解决方案。Alice 可以在 rollup B 上的一个以序列登记的合约上注册账号 `ALICE_B` ，并获得一个按序分配的 ID (因此 Alice 的 ID 等于在她之前注册的用户数量)。设置用户数的最大值 `MAX_USER_COUNT` ；如果有必要，这个值可以随时间向上调整。则 Alice 可以确保  `TRADE_VALUE % MAX_USER_COUNT` 等于 (Alice 的 ID)，使用 `TRADE_VALUE` 的低位数字 (这个数字是这笔交易的一个无关紧要的值) 来表示她想交易的 token 数量。

## 从 Rollup B 到 Rollup A 的交易

如果 Alice 把 Rollup B 上的 token 转移到 Rollup A，她可以使用相同的机制，只是角色颠倒了：

- Alice 将 token 发送给 `IVAN_B`
-  经过一段时间的延迟后，她将获得取回 token 的权利
-  如果 Ivan 可以向 `IVAN_B` 证明，他在 Rollup A 上给 Alice 发送了 token ，Alice 就失去了这个权利