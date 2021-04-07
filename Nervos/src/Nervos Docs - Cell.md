# Nervos Docs - Cell

Nervos CKB（通用知识库）是一个 layer 1 的区块链，一个去中心化的为网络提供公共知识保管的安全层。公共知识是指通过全球共识达成一致的**状态**。

**Cell 是 CKB 中的主要状态单位**，是用户拥有的资产。它们必须遵循脚本指定的关联验证规则。在比特币中，金钱是存储在比特币账本中的典型常识。但是， Nervos CKB 更进一步，可以存储任意的常识。我们从比特币的一般体系结构开始，并通过从 UTXO 模型进行泛化来创建 Cell 模型，同时保留比特币的一致性和简单性。

## 数据结构

例子：

```json
{
  "capacity": "0x19995d0ccf",
  "lock": {
    "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
    "args": "0x0a486fb8f6fe60f76f001d6372da41be91172259",
    "hash_type": "type"
  },
  "type": null
}
```

一个 Cell 具有三个字段：

- `容量（capacity）`：容量服务器有 2 个用途：一方面，它表示存储在 Cell 中的 CKB token 的数量；另一方面，它还设置了对 Cell 可以存储的信息量的限制。容量的基本单位是 `shannon`，也可以使用更大的 `CKByte` 单位，或者仅使用 `CKB` 。 1 CKB 等于 10 ^8 shannon，1 CKB 也意味着该 Cell 可以存储 1 字节的信息。有关如何计算 Cell 的总信息大小，请参见下文。
- `锁定脚本（lock script）`：用于保护 Cell 的脚本：当指定的 Cell 用作交易中的输入 Cell 时，将执行 Cell 中包含的锁定脚本。当锁脚本执行失败时，该交易将被拒绝。锁定脚本的一种典型用例是表示 Cell 的所有权，这意味着签名验证阶段通常包含在 Cell 中。
- `类型脚本（type script）`：用于验证 Cell 结构的脚本。当 Cell 作为输入 Cell 以及将 Cell 创建为输出 Cell 时，都将执行该 Cell 的类型脚本。由于这种性质，类型脚本通常用于验证 dapp 逻辑，例如创建 UDT 。

每个 Cell 必须具有一个锁定脚本，而类型脚本是可选的，可以省略。有关锁定和键入脚本的实际格式，请参考[脚本](https://docs.nervos.org/docs/reference/script)。

### Cell 数据

除上述字段外，每个 Cell 还包含一个 Cell 数据字段。Cell 数据只是一系列未格式化的二进制数据。取决于每个 dapp ，任何内容都可以存储在Cell 格数据部分中：

- 脚本代码，如[脚本](https://docs.nervos.org/docs/reference/script)中所述。
- 用户定义 token（UDT） Cell 的 token 数量。
- 链上范特西游戏的最新游戏统计信息。

为了将来的发展，不能将 Cell 数据直接存储在 Cell 中。它直接保存在[交易](＃data-structure)中。您可能会在每个交易中找到一个名为 `outputs_data` 的字段。此数组应与 `outputs` 具有相同的长度。在每个索引位置，可以为交易中每个创建的输出 Cell 定位对应的 Cell 数据。从概念上讲，我们仍将 Cell 数据视为每个输出 Cell 的一部分。

### Cell 信息大小计算

Nervos CKB 上的每个 Cell 的容量不得低于该 Cell 中存储的信息总大小。Cell 的信息大小由以下字段的总和计算得出：

1. Cell 容量字段为 8 个字节。
2. 锁定脚本中用于代码哈希的 32 个字节。
3. 锁定脚本中哈希类型的 1 个字节。
4. 锁定脚本中 args 字段的实际字节。
5. 如果存在类型脚本，则类型脚本中用于代码哈希的 32 个字节。
6. 如果存在类型脚本，则类型脚本中的哈希类型为 1 个字节。
7. 如果存在类型脚本，则类型脚本中 args 字段的实际字节。
8. Cell 数据的实际字节。

通过汇总以上所有字段，我们可以得出 Cell 所需信息的总大小。Cell 容量（以 `CKBytes` 为单位）表示可以保存的最大信息大小，这意味着有效的 Cell 必须确保存储的 CKBytes 等于或大于信息的总大小。

## Live Cell

Live Cell 是指 CKB 中未使用的Cell 。它类似于比特币术语中的 [UTXO](https://en.wikipedia.org/wiki/Unspent_transaction_output) 的概念。 CKB 中完整的 Live Cell 集被认为是该特定时间点下 CKB 的完整状态。 CKB 上的任何交易都将在提交之前消耗一些作为 Live Cell 的 Cell，并创建新的 Cell ，将其视为提交后的 Live Cell 。

## 索引查询组装模式

Nervos CKB 是基于 Cell 的概念而设计的。交易本质上实际上只是消耗一些 Cell ，并创建另一组 Cell 。结果，定位和转换 Cell 的能力在构建任何 CKB dapp 中起着至关重要的作用，这导致了 `索引查询组装` 模式：

- 索引：将新块提交给 CKB 时，dapp 应该能够将相关 Cell 索引到其自己的存储中以供以后使用。
- 查询：当请求用户操作时，将从 dapp 存储中查询满足特定条件的 Cell 。
- 组装：基于查询的 Cell ，将构建一个新的交易来满足用户请求。

我们相信，按照这种模式，所有 CKB dapp 都可以分解为单独的动作。这里有些例子：

- 在普通的 CKB 钱包中，应基于锁定脚本对 Cell 进行索引。传输操作将首先查询来自发送方的 Cell ，并组装一个将 CKBytes 传输至接收方的交易。
- NervosDAO 管理器可能仅索引与 NervosDAO 相关的 Cell 。然后，用户可能会选择一个 NervosDAO Cell 并执行 withdraw 操作，即使只有一个相关的 Cell ，我们仍然可以将其视为从 NervosDAO 管理器查询的 Cell ，并且还将组装一个交易来执行实际的 withdraw 操作。
- 基于状态的 dapp 可能选择将最新状态存储在 CKB Cell 中。 dapp 仍然需要跟踪最新的 Live Cell ，也可以将其视为索引操作，对该状态的任何操作都将导致查询最新的 Live Cell ，将其组合成一个交易，然后由 CKB 接受并带有新的输出包含更新状态的 Cell 。

### 工具

索引和查询在任何 CKB dapp 中都扮演着核心角色。在大多数情况下，您不必从头开始构建索引器。有几种现有工具可以用来完成这项工作：

#### lumos

我们的 dapp 框架 [lumos](https://github.com/nervosnetwork/lumos) 已经包含一个现成的索引器。当您使用 lumos 时，很可能已经设置了索引器供您使用。请参阅我们的实验室以了解如何设置发光二极管。

#### ckb-indexer

独立的 [ckb-indexer](https://github.com/quake/ckb-indexer) 还可处理索引 Cell 的工作。它提供了一种 RPC 机制，可用于查询相关 Cell 。有关更多详细信息，请参考 ckb-indexer 的文档。

#### perkins-tent

如果您正在寻找一站式解决方案，则[perkins-tent](https://github.com/xxuejie/perkins-tent)提供了一个 docker 镜像，该镜像在一个 docker 实例中同时启动 CKB 和 ckb-indexer 。使用单个命令，您应该能够启动 CKB 实例，并准备好使用随附的 ckb-indexer 来查询任务。