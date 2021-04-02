# 基于密码学的证明系统的寒武纪大爆发

本文是写给有密码学背景的人的。它调查了不断发展中的密码学证明系统以及其中的对称 STARK 的作用。本文是基于 2019 年 11 月在旧金山发表的演讲。

## 1. 引言

35亿年来，地球上的生命由原始的单细胞生物组成。然后，在所谓的寒武纪爆炸期间，几乎所有我们今天认识到的所有动物种类都出现了。

以此类推，我们目前在计算完整性（CI）密码学证明领域经历寒武纪爆炸，其中包括**零知识证明**。几年前，每年大约有 1-3 个新系统的出现，但是发展的速度非常之快，以至于今天我们看到的每月（甚至可能是每周）就会有同样数量的新系统出现。在 2019 年，我们了解到的新的零知识证明结构，例如有 [Libra](https://eprint.iacr.org/2019/317.pdf)、[Sonic](https://eprint.iacr.org/2019/099)、[SuperSonic](https://eprint.iacr.org/2019/1229)、[PLONK](https://eprint.iacr.org/2019/953)、[SLONK](https://ethresear.ch/t/slonk-a-simple-universal-snark/6420)、[Halo](https://eprint.iacr.org/2019/1021)、[Marlin](https://eprint.iacr.org/2019/1047)、[Fractal](https://eprint.iacr.org/2019/1076)、[Spartan](https://eprint.iacr.org/2019/550)、[简洁 Aurora](https://eprint.iacr.org/2018/828)，以及 [OpenZKP](https://github.com/0xProject/OpenZKP)、[Hodor](https://github.com/matter-labs/hodor) 和 [GenSTARK](https://github.com/GuildOfWeavers/genSTARK) 等实现。哦，好吧，在差不多写完这篇文章的时候，[RedShift](https://eprint.iacr.org/2019/1400) 以及 [AirAssembly](https://github.com/GuildOfWeavers/AirAssembly/tree/master/specs) 也已经出现。

如何理解所有这些奇妙的创新？这篇文章的目的是确定代码实现的所有 CI 系统的共同点，并讨论一些差异化因素。

请注意，由于本文具有一点技术性，所以希望读者有一定的密码学背景！尽管如此，对于那些感兴趣的非密码学读者来说，这也是值得略读一下，以了解该领域中使用的术语。因此我们的描述将是简短的，并且会摒弃一些数学相关的东西。这篇文章的另一个主要目的是解释为什么我们的公司 StarkWare 在科学，工程学和产品方面将其所有芯片都放置在 CI-verse 的一个特定子族中，之后我们会称此子族为对称 STARK 。

### 共性

计算完整性证明系统可以帮助解决困扰去中心化区块链的两个基本问题：**隐私**和**可扩展性**。零知识证明（ZKPs [1] ） 通过屏蔽计算的某些输入而不会损害完整性，同时提供了隐私。简约的可验证的 CI 系统通过指数级压缩大批量交易完整性所需的计算量，以此提供可扩展性。

所有已经用代码实现的 CI 系统都有两个共同点：它们都使用被称为 Arithmetization 的东西，并且全部引入了一个密码学概念，称为 “低阶顺应性” （LDC）[2]。

**Arithmetization** 是指通过证明算法来减少计算语句。你可以从这样的概念性陈述开始：

> **I know the keys that allow me to spend a shielded Zcash transaction**（我知道允许我花费一笔 shielded Zcash 交易的密钥）

然后将它转换为包含一组有界次多项式的代数陈述，如：

> **“I know four polynomials A(X), B(X), C(X), D(X), each of degree less than 1,000, such that this equality holds: A(X) * B²(X) - C(X) = (X¹⁰⁰⁰–1) * D(X)”** （我知道四个多项式 A(X)，B(X)，C(X)，D(X)，它们每个阶数都小于 1000，所以这个等式成立：A(X) * B²(X) - C(X) = (X¹⁰⁰⁰–1) * D(X)）

**低阶顺应性（LDC）**是指使用密码学来确保 prover 实际上选择低阶多项式 [3]，并在 verifier 提供的随机点上评估这些多项式。在上面的例子中（本文会持续提及），一个好的 LDC 解决方案向我们保证，当询问有关 x₀ 的问题时，它将回答 x₀ 在 A、B、C 和 D 上的值 a₀、b₀、c₀、d₀ 来作为正确答案。棘手的部分是，prover 可能在看到查询 x₀ 后选择 A、B、C 和 D，或者可能用自己编造的任意的 a₀、b₀、c₀、d₀ 来回答，以糊弄 verifier，并且与预先选择的低阶多项式的任何求值都不对应。所以，所有的密码学都是为了防止这种攻击向量。（需要 prover 发送完整 A、B、C 和 D 的简单解决方案，既不提供可扩展性，也不提供隐私性。）

有鉴于此，CI-verse 可根据以下三种方式绘制出来：（i）用于强制 LDC 的密码学原语，（ii）用这些原语构建的特定 LDC 解决方案以及（iii）这些选择所允许的 Arithmetization 。

## 2. 比较维度

### 2.1 密码学假设

从 30000 英尺的高空来看，不同 CI 系统之间最大的理论区别因素，**就在于它们的安全性需要对称密码学原语还是非对称密码学原语**（见图 1）。典型的对称原语有 SHA2、Keccak （SHA3）或 Blake，我们假设它们是抗碰撞哈希（CRH）函数，它们是伪随机的，行为类似于随机预言机。非对称假设包括求解模素数、RSA 模或在椭圆曲线群中，计算 RSA 环的乘法组的大小的难度，以及类似于 “指数知识” 假设，“自适应根” 假设这样的问题。

![*图一：密码学假设家族树*](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210301231750.png)

CI 系统之间的这种对称 / 不对称划分，会带来很多结果，其中包括：

#### A. 计算效率

今天在代码 [4] 中实现的非对称密码学原语的安全性，要求在大型代数域上对 LDC 问题进行运算与求解：大型素数域和它们上面的大型椭圆曲线，其中每个域 / 组元素都有数百（bit）位长，或整数环中每个元素都有数千（bit）位长。相比之下，仅依赖对称假设的构造对包含 smooth[5] 子群的任何代数域（环域或有限域）进行算术运算并执行 LDC，包括非常小的二进制域和 2 个 smooth 素数域（64 位或更少），其中算术运算速度是很快的。

**结论：对称 CI 系统可以对任何字段进行算术运算，从而提高效率。**

#### B. 后量子安全性

一旦出现一台具有足够大状态（以量子位测量）的量子计算机时，当前在 CI-verse 中使用的所有非对称密码学原语将被破坏。而另一方面，对称密码学原语似乎是后量子安全的。

**结论：只有对称系统才是合理的后量子安全系统。**

![*图 2：密码学假设及由它们支撑的经济价值*](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210301233415.png)

#### C. 经得起考验

[Lindy 效应理论](https://en.wikipedia.org/wiki/Lindy_effect)提出：

> 「一些不易腐烂的东西，比如一项技术或一个想法，其未来预期寿命与其当前年龄成正比。」

或者用通俗的话来说，旧东西要比新东西存活的时间要更长。在密码学领域，这可以被翻译成这样一种说法，即依赖于较老的、经实战测试的原语系统，要比那些经受考验较少的较新假设要更安全，也更容易存活下来（见图 2）。从这个角度来看，非对称密码学假设的新变体，如未知阶群、一般群模型和指数假设的知识，要比较旧的假设（如用于数字签名、基于身份的加密和 SSH 初始化的更标准的 DLP 和 RSA 假设）更年轻。这些假设，相较于对称假设（如存在抗碰撞的哈希算法）要更经不起未来的考验，因为后一种假设（甚至是特定的哈希函数）是用来保护计算机、网络、互联网和电子商务而存在的。

此外，这些假设之间有严格的数学层次。CRH 假设在这个层次中是主宰的，因为如果这个假设被破坏（意味着没有安全的密码哈希函数被发现），那么，特别是 RSA 和 DLP 假设也会被破坏，这些假设的前提是存在一个好的 CRH !同样，DLP 假设支配着指数知识（KoE）假设，因为如果前者（DLP）假设不能成立，那么后者（KoE）也不能成立。类似地，RSA 假设支配着未知阶群（GoUO）假设，因为如果 RSA 被破坏，那么 GoUO 也会被破坏。

**结论：新的不对称密码学假设，对于金融基础设施而言，会是具有更高风险的基础；**

#### D. 论证长度

以上各点都有利于对称 CI 结构而不是非对称 CI 结构。但在一个方面，非对称结构会表现得更好。与之相关联的通信复杂性（或论证长度）会小 1-3 个数量级（尼尔森定律 [6] ）。比如最著名的， [Groth16](https://eprint.iacr.org/2016/260) SNARK 在 128 位安全级别上的证明预计小于 200 字节，而今天存在的所有对称结构需要几十 KB 才能有相同的安全级别。需要注意的是，并不是所有的非对称结构都会有 200 字节那么简洁。Groth16 结构通过（i）消除了对可信设置的需求（透明性）和（ii）处理通用电路（Groth16 要求每个电路有一个可信设置）已经得到了改进。但这些较新的结构有更长的参数，其大小在 0.5 KB （如 PLONK）到两位数 KB 之间，接近对称结构的论证长度。

**结论：非对称电路专用系统（Groth16）最短，它要比所有非对称通用系统和所有对称系统都要短。**

然后重申下上面的要点：

1. 对称 CI 系统可以在任意域上进行算术运算，从而提高效率；
2. 只有对称系统才是合理的后量子安全系统；
3. 新的不对称假设，对于金融基础设施而言，会是具有更高风险的基础；
4. 非对称电路专用系统（Groth16）最短，它要比所有非对称通用系统和所有对称系统都要短。

### 2.2 低阶顺应性（LDC）方案

实现低阶顺应性的主要方法有两种：（i）隐藏查询，（ii）承诺方案（见图 3）。让我们来讨论一下差异。

![*图 3: 隐藏查询 & 承诺方案*](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210301233439.png)

**隐藏查询（Hiding Queries）**

[这种方法](https://eprint.iacr.org/2012/718)是 Zcash 类型的 SNARKs，如 Pinocchio、libSNARK、Groth16 和依托他们构建的其他系统，例如 Zcash 的 Sapling，以太坊的 Zokrates 等。为了让 prover 正确回答，我们使用[同态加密](https://en.wikipedia.org/wiki/Homomorphic_encryption)隐藏或加密 x₀，并提供足够的信息，以便 prover 能够计算 A、B、C 和 D 在 x₀ 上的值。

实际上，给 prover 的是一个 x₀ 幂的加密序列（即，x₀¹ , x₀², … x₀¹⁰⁰⁰ 的加密值），这样 prover 就可以计算任意1000 阶以内的多项式。粗略地说，系统是安全的，因为 prover 不知道 x₀ 的值，而这个 x₀ 是随机（预先）选择的，因此如果 prover 试图欺骗，那么它们很有可能会暴露。这里需要一个可信的预处理设置阶段来采样 x₀，并加密上面的幂序列（以及附加信息），从而得到至少与被证明的计算电路一样大的证明密钥（还有一个更短的验证密钥）。一旦设置完成并释放了密钥，每个证明都是一个单一的、简洁的、非交互的知识论证（简称 SNARK）。请注意，这个系统确实需要某种形式的交互，以预处理阶段的形式，因为理论上这是不可避免的。还请注意，该系统是不透明的，这意味着用于对 x₀ 进行采样和加密的熵，不能仅仅是公开的随机 coin，因为任何知道 x₀ 的一方，都可以破坏该系统并证明其错误性。**因此，在不暴露 x₀ 的情况下生成 x₀ 及其幂的加密，是构成潜在单点故障的一个安全问题。**

**承诺方案（Commitment Scheme）**

这种方法要求 prover 通过向 verifier 发送一些加密构造的[承诺](https://en.wikipedia.org/wiki/Commitment_scheme)消息来提交一组低阶多项式（在上面的示例中是 A、B、C 和 D）。有了这个承诺，verifier 现在对随机选择的 x₀ 取样并询问 prover，现在 prover 用 a₀、b₀、c₀和 d₀，以及使 verifier 确信由 prover 返回的四个值以及其附加的密码学信息符合先前发送给 verifier 的承诺。

这种方案是天然交互的，而且许多方案都是透明的（verifier 生成的所有消息都只是公开的随机 coin）。透明性允许人们通过 Fiat-Shamir 启发式（它将 SHA 2/3 这样的伪随机函数视为提供「公共」随机性的随机预言机）将协议压缩为非交互式协议，或者使用其他随机性公共源（如区块头）。最流行的透明承诺方案是通过 Merkle 树，这种方法看似是后量子安全的，但会导致在许多对称系统中出现较大的论证长度（因为所有认证路径都需要被揭示并附上每个 prover 的答案）。这是大多数 STARK （如 libSTARK 和简洁 Aurora）以及简洁证明系统（如 ZKBoo、Ligero、Aurora 和 Fractal）使用的方法（即使这些系统不满足 STARK 的正式可扩展性定义）。

特别是，我们在 StarkWare 建造的 STARKs （比如我们即将部署的 [StarkDEX alpha](https://www.starkdex.io/) 版本和 [StarkExchange](https://medium.com/starkware/starkexchange-8045695b798)）就属于这一类。人们也可以使用非对称密码学原语来构造承诺方案，例如基于椭圆曲线组上离散对数问题的难度（这是 BulletProof 和 Halo 所采用的方法）以及未知阶假设群（如 DARK 和 SuperSonic 采用的）。**使用非对称承诺方案具有前面提到的优点和缺点：证明较短，但计算时间较长，易受量子计算影响，具有较新（且较少研究）的假设，以及在某些情况下会有透明度的损失。**

#### 2.3 Arithmetization

密码学假设和 LDC 方法的选择，也以三种明显的方式影响 arithmetization 可能性的范围（见图 4）：

![*图 4:Arithmetization 效应*](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210301233500.png)

**A. NP (电路） vs. NEXP （程序）**

大多数已实现的 CI 系统**将计算问题简化为算术电路，然后将其转换为一组约束**（一般是下面我们将讨论的 R1CS 约束）。此方法允许特定于电路的优化，但要求 verifier 或可信方执行与正在验证的计算（电路）一样大的计算。对于 Zcash 的 Sapling 电路这样的多用途电路来说，这种算法就足够了。但是，像 libSTARK、简洁 Aurora 和 StarkWare 正在构建的可扩展且透明（没有可信设置）的系统而言，必须要使用简洁的计算表示，这种表示类似于一般的计算机程序，并且其描述要比被验证的计算要小指数级。现有的两种实现：（i） libSTARK、genSTARK 以及 StaskWS 系统所使用代数中间表示（AIR）方法，以及（ii）简洁 Aurora 的简洁 R1CS，这两者被描述为最好的通用计算机程序（与电路相反）的 arithmetization。这些简洁表示足以捕捉非确定性指数时间（NEXP）的复杂类，它比由电路描述的非确定性多项式时间（NP）类更具有指数表现力，也更强大。

**B. Alphabet 大小和类型**

如上所述，所使用的密码学假设在很大程度上也决定了哪些代数域可用作我们进行算术运算的 alphabet。例如，如果我们使用双线性配对，那么我们将使用一个椭圆曲线点的循环群作为 arithmetization 的 alphabet，这个群的最大值必须是一个大素数，这意味着我们需要对这个域进行算术运算。再举一个例子，SuperSonic 系统（在其某个版本中）使用了 RSA 整数，在这种情况下，Alphabet 将是一个大的素数域。相比之下，当使用 Merkle 树时，Alphabet 的大小可以是任意的，允许在任何有限域上进行算术运算。这包括上面的例子，也包括任意素数域，小素数域的扩展，如二进制域。域的类型是很重要的，因为域越小，证明和验证时间就越快。

**C. R1CS vs. 一般多项式约束**

Zcash 型 SNARKs 利用椭圆曲线上的双线性配对来计算约束。双线性配对的这种特殊使用 [7]，限制了二次秩 1 约束系统（R1CS）的门运算。R1CS 的简单性和普遍性使得许多其他系统对电路使用这种算术形式，尽管可以使用更普遍的约束形式，如任意秩二次型（ arbitrary rank quadratic form）或更高阶的约束。

### 三、 STARK vs. SNARK

下面我们来理清一下 STARKs 和 SNARKs 之间的差异。这两个术语都有具体的数学定义，某些结构可以实例化为 STARKs 或 SNARKs，或者两者兼而有之。不同的术语强调证明系统的不同性质。下面让我们深入了解一下。（参见图 5）。

![*图 5:STARK vs. SNARK*](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210301233541.png)

#### STARK

这里的 **S** 代表**可扩展性**，这意味着随着批处理大小 n 的增加，证明时间大小与 n 是呈准线性关系的，同时验证时间大小与 n 是呈多对数 [8] 关系的。

STARK 中的 **T** 代表**透明性**，这意味着所有 verifier 消息都是公共随机 coin (没有可信设置)。根据这个定义，如果有任何预处理设置，它必须简洁（多对数级别），并且必须仅包括抽样公共随机 coin。

#### SNARK

这里的 **S** 代表**简洁性**，这意味着在 n （不要求准线性证明时间）中验证时间大小是多对数级别的，而 **N** 表示**非交互**，这意味着在预处理阶段（可能是非透明的）之后，证明系统不能允许任何进一步的交互。注意，根据这个定义，允许非简洁的可信设置阶段，一般来说，系统不需要透明，但必须是非交互的（在完成不可避免的预处理阶段之后）。

纵观 CI-verse （见图 5），有人注意到它的一些成员是 STARKs，其它一些成员则是 SNARKs，有些则是两者都是，而其它成员则都不是（例如，验证时间大小与 n 的关系要比多对数要差的方案）。如果你对隐私（ZKP）应用是感兴趣的，那么 ZK-SNARKs 和 ZK-STARKs，甚至是那些不具备 STARK 可扩展性也没有 SNARK 的的（弱）简洁性的系统，都可以很好地工作，例如 Monero 使用的 Bulletproofs 就是这样一个显著的例子，**其中验证时间与电路大小成线性关系。**而当谈到代码成熟度时， SNARKs 会拥有优势，因为有很多好的开源库可供构建。但是，如果你对可扩展性应用感兴趣，那么我们会建议使用对称 STARKs，因为在编写本文时，它们有最快的证明时间，并且保证验证过程（或建立系统）的任何部分都不需要超过多对数级别的处理时间。如果你想建立具有最小信任假设的系统，那么，同样，你也会想要使用对称 STARK，因为唯一需要的成分是一些 CRH 和公共随机性的来源。

### 四、总结

![*图 6: 总结*](https://raw.githubusercontent.com/Whisker17/ImageStoreService/main/img/20210301233601.png)

我们很幸运地经历了计算完整性密码学证明系统宇宙的惊人寒武纪爆发，我们认为系统和创新的扩散，将继续以越来越快的速度进行。

此外，随着未来新的见解及结构的出现，以上描述的 CI-verse 扩展及变化的尝试很可能会过时。话虽如此，纵观当今的 CI 领域，我们所看到的最大分界线是 (i) **需要非对称密码假设的系统，它们会导致证明时间更短，但证明成本更高，它们具有较新的假设，这些假设是易受量子计算影响的，其中有很多是不透明的**，以及（ii）**只依赖对称假设的系统，这使得它们在计算上是高效、透明，且可能是后量子安全的**，而根据 Lindy 效应来看，它们也可能是最经得起未来考验的。

关于使用哪个证明系统的争论远未结束，但在 StarkWare 看来，我们会说：对于简短论证，可以使用 Groth16/PLONK SNARKs，而对于其它的情况，则使用对称 STARKs。

> 脚注
>
> [1]. 术语 ZKP 经常被误用来指代所有的 CI 系统，甚至是一些非正式 ZKP 系统。为了避免这种混淆，我们使用了定义松散的术语「密码学证明」和「计算完整性（CI）证明」；
> [2]. 你可以在这里读到 STARK 算术化和低阶顺应性：
> 	- **Arithmetization**：blog [[1](https://medium.com/starkware/arithmetization-i-15c046390862), [2](https://medium.com/starkware/arithmetization-ii-403c3b3f4355)]，[slides](https://cyber.biu.ac.il/wp-content/uploads/2019/02/3-arithmetization_BarIlanFeb2019.pdf)，[video](https://www.youtube.com/watch?v=9VuZvdxFZQo)
> 	- **Low-degreeneess**: [blog](https://medium.com/starkware/low-degree-testing-f7614f5172db)
> [3]. 一元多项式的使用可以被广泛地推广到多元多项式和代数几何码，但是为了简单起见，我们坚持使用最简单的一元情况；
> [4]. 我们特别将基于格的构造排除在讨论之外，因为它们还没有代码基础。这种结构是非对称的，而且似乎也是后量子安全的，并且通常使用小（素数）域。
> [5]. 如果一个域包含一个子群（乘法或加法），且其所有素因子都不超过 k，则该域是 k-smooth 的。例如，所有的二元域都是 2-smooth，因此 q 大小的域 q-1 可以被 2 的大幂（power）除；
> [6]. Nielsen 的[互联网带宽定律](https://www.nngroup.com/articles/law-of-bandwidth/)指出，用户带宽每年会增长 50%，该定律适用于 1983 年至 2019 年的数据；
> [7]. 其他系统（如 PLONK）仅使用配对来获得（多项式）承诺方案案，而不用于算术化 arithmetization。在这种情况下，算术化可能导致任何低阶约束；
> [8]. 形式上，「n 中的准线性」表示 O(n logᴼ⁽¹⁾ n) ，「n 中的多对数」表示为 logᴼ⁽¹⁾ n。