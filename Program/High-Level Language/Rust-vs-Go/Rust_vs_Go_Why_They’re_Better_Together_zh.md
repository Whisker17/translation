# Rust vs. Go：为什么它们在一起更好

原文链接：[Rust vs. Go: Why They’re Better Together](https://thenewstack.io/rust-vs-go-why-theyre-better-together/)

原文作者：Steve Francia

虽然其他人可能认为 [Rust](https://www.rust-lang.org/) 和 [Go](https://go.dev/) 是有竞争力的编程语言，但 Rust 和 Go 团队他们自己都不这么认为。恰恰相反，我们的团队非常尊重其他人正在做的事情，并将这些语言视为对整个软件开发行业现代化状态的共同愿景的赞美。

在本文中，我们将讨论 Rust 和 Go 的优点和缺点，同时讨论它们如何相互补充和支持，以及我们对这两种语言在什么场景下最合适的建议。

许多公司正在发现采用这两种语言的价值，以及它们的互补价值。为了从我们的观点转向实际的用户体验，我们与 [Dropbox](https://www.dropbox.com/) 、[Fastly](https://www.fastly.com/) 和 [Cloudflare](https://www.cloudflare.com/) 这三家这样的公司讨论了他们同时使用 Go 和 Rust 的经验。在这篇文章中，我们将引用他们的语录，以提供进一步的视角。

## 编程语言的对比

| 语言                                                         | Go                                                           | Rust                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建日                                                       | 2009                                                         | 2010                                                         |
| 创建人                                                       | Google                                                       | Mozilla                                                      |
| Notable software written in language                         | Kubernetes, Docker, Github CLI, Hugo, Caddy, Drone, Ethereum, Syncthing, Terraform | Firefox, ripgrep, alacritty, deno, Habitat                   |
| Key workloads                                                | APIs, Web Apps, CLI apps, DevOps, Networking, Data Processing, cloud apps | IoT, processing engines, security-sensitive apps, system components, cloud apps |
| [开发者使用度](https://insights.stackoverflow.com/survey/2020%23technology-programming-scripting-and-markup-languages-all-respondents) | 8.8% (#12)                                                   | 5.1% (#19)                                                   |
| [喜爱度](https://insights.stackoverflow.com/survey/2020%23technology-programming-scripting-and-markup-languages-all-respondents) | 62.3% (#5)                                                   | 86.1% (#1)                                                   |
| [开发者期望使用程度](https://insights.stackoverflow.com/survey/2020%23technology-most-loved-dreaded-and-wanted-languages-wanted) | 17.9% (#3)                                                   | 14.6% (#5)                                                   |

## 相同点

Go 和 Rust 有很多共同之处。这两种现代软件语言都是出于为影响软件开发的问题提供安全且可扩展的解决方案的需要而诞生的。这两种语言都是针对业界现有编程语言的缺陷而创建的，尤其是在开发人员的生产力、可扩展性、安全性和并发性方面的缺陷。

今天大多数流行的语言都是 30 多年前设计的。当这些语言被设计出来的时候，它们与现在的语言有五个主要的区别:

1. 摩尔定律被认为是永远正确的。
2. 大多数软件项目都是由小团队编写的，他们经常亲自一起工作。
3. 大多数软件都有相对较少的依赖，大多数是专有的。
4. 安全性是次要考虑的，或者根本不考虑。
5. 软件通常是为单一平台编写的。

相比之下，Rust 和 Go 都是为现今环境设计的，并且通常采用类似的方法来设计适合当今开发需要的语言。

### 1. 性能与并发度

Go 和 Rust 都是专注于生成高效代码的编译语言。它们还提供了对当今机器的多处理器的方便访问，使它们成为编写高效并行代码的理想语言。

“借用 Go 使得 MercadoLibre  将用于这项服务的服务器数量减少到原来的 1/8 （从 32 台服务器减少到 4 台），而且每个服务器可以用更少的电力运行（原来是 4 个 CPU 核，现在减少到 2 个 CPU 核)。有了 Go ，MercadoLibre  减少了 88% 的服务器，并在剩下的服务器上削减了一半的 CPU ，节省了大量成本。—— *[MercadoLibre Grows with Go](https://go.dev/solutions/mercadolibre/)*

“在我们运行 Go 代码的严格管理环境中，我们看到 CPU 减少了大约 10% （vs c++），代码更干净、更易于维护。” ——*[Bala Natarajan, Paypal](https://go.dev/solutions/paypal/)*

“在 AWS 的环境中，我们也喜欢 Rust ，因为它帮助 AWS 编写高性能、安全的基础设施级网络和其他系统软件。亚马逊的第一个用 Rust 构建的引人注目的产品是 Firecracker，于 2018 年公开发布，并提供了为 AWS Lambda 和其他 serverless 产品提供支持的开源虚拟化技术。但同时我们也使用 Rust 提供服务，如亚马逊简单存储服务（Amazon S3）、亚马逊弹性计算云（Amazon EC2）、亚马逊 CloudFront 、亚马逊 Route 53 等。最近我们推出了一个用 Rust 编写的基于 linux 的容器操作系统 Bottlerocket 。” —— *[Matt Asay, Amazon Web Services](https://aws.amazon.com/blogs/opensource/why-aws-loves-rust-and-how-wed-like-to-help/)*

我们 “看到速度惊人地提高了1200-1500% ！我们从 300-450ms 的 Scala 发布模式，实现更少的解析规则，到 25-30ms 的 Rust 模式，同时实现更多的解析规则！” ——*[Josh Hannaford, IBM](https://developer.ibm.com/technologies/web-development/articles/why-webassembly-and-rust-together-improve-nodejs-performance/)*

### 2. 团队可扩展性 —— 可审查

如今的软件开发是由不断成长和扩展的团队构建的，通常以分布式的方式协作源码控制。Go 和 Rust 都是为团队的工作方式而设计的，通过消除诸如格式、安全性和复杂组织等不必要的问题来改进代码审查。这两种语言都需要相对较少的上下文来理解代码在做什么，从而允许 reviewer 更快地处理其他人写的代码，并评审团队成员和团队之外的开源开发人员贡献的代码。

“在我的早期职业生涯中，我有过 Java 和 Ruby 的背景，构建 Go 和 Rust 代码感觉像是肩上卸下了一个不可能的重量。当我在谷歌的时候，遇到用 Go 编写的服务是一种解脱，因为我知道它很容易构建和运行。这也适用于 Rust ，尽管我只在一个小得多的规模上研究过它。我希望无限可配置构建系统的时代已经一去不复返了，所有的语言都带有自己的专用构建工具，可以开箱即用。” —— *[Sam Rose, CV Partner](https://bitfieldconsulting.com/golang/rust-vs-go)*

“在使用 Go 编写服务时，我往往会松一口气，因为与动态语言相比，它具有非常简单，易于推理的静态类型系统，并发是一类公民，Go 的标准库令人难以置信的完善和强大，这也是很重要的一点。采用一个标准的 Go 安装，加入一个 grpc 库和一个数据库连接器，只需要这些你就可以做到在服务器端进行一定的开发，每个工程师都可以阅读代码并理解库。当在 Rust 中编写一个模块时，Dropbox 的工程师们感到 Rust 在服务器端的成长之痛，但在 2019 年 async -await 稳定下来之后，*crates* 开始使用它，我们从中获得了异步模式与无限制的并发性的好处。” —— *Daniel Reiter Horn, Dropbox*

### 3. 开源意识

今天，平均的软件项目所使用的依赖库的数量是惊人的。在现代开发中，软件重用的目标已经实现了几十年，今天的软件是使用上百个项目构建的。为了做到这一点，开发人员使用软件存储库，它越来越多地成为跨广泛应用程序的软件开发的主要内容。开发人员所包含的每个包依次都有自己的依赖项。当今编程环境的语言需要毫不费力地处理这种复杂性。

而 Go 和 Rust 有自己的包管理系统，使开发人员可以简单列出要构建的程序包，语言工具会自动为他们获取和维护这些程序包，以便开发人员可以将更多精力放在自己的代码上，而不必在意其他代码的管理。

### 4. 安全性

Go 和 Rust 都很好地解决了当今应用程序的安全问题，它们确保了内置在语言中的代码在运行时不会给用户带来各种经典的安全漏洞，如缓存溢出、释放后重用（UAF）等。通过消除这些问题，开发人员可以专注于手头的问题，并构建默认情况下更安全的应用程序。

“当你处理错误的时候，Rust 编译器真的会帮助你。这可以让您专注于您的业务目标，而不是寻找 bug 或破译神秘的消息。” —— *[Josh Hannaford, IBM](https://developer.ibm.com/technologies/web-development/articles/why-webassembly-and-rust-together-improve-nodejs-performance/)*

“简而言之，Rust 的灵活性、安全性超过了必须遵守严格的生命周期、借用和其他编译器规则，甚至缺乏垃圾收集器带来的不便。这些特性是云软件项目急需的附加功能，将有助于规避在这些项目中许多常见的 bug 。” —— *[Taylor Thomas, Sr., Microsoft](https://msrc-blog.microsoft.com/2020/04/29/the-safety-boat-kubernetes-and-rust/)*

”Go 是强静态类型的，没有隐式转换的一门语言，但语法开销仍然小得惊人。这是通过在赋值中使用简单的类型推断以及无类型的数字常量来实现的。这比 Java （具有隐式转换）提供了更强的类型安全性，但代码读起来更像 Python （具有无类型变量）。” —— *[Stefan Nilsson, computer science professor](https://yourbasic.org/golang/advantages-over-java-python/)*

“当我们在 Dropbox 开发用于存储块数据的 Brotli 压缩库时，我们将自己限制在 Rust 的安全子集中，进一步涉及到核心库（no-stdlib），同时将分配器指定为通用分配器。通过这种方式使用 Rust 的子集，可以很容易地在客户端用 Rust 调用 Rust- brotli 库，并在服务器端同时使用 Python 和 Go 中的 C FFI 。这种编译模式也提供了[实质性的安全保障](https://dropbox.tech/infrastructure/lossless-compression-with-brotli)。经过一些调优，Rust Brotli 的实现做到了 100% 安全、数组边界检查代码，但仍然比 C 的实现中相应的原生 Brotli 代码要快。” —— *Daniel Reiter Horn, Dropbox*

### 5. 真正的便携度

在 Go 和 Rust 中，编写一个运行在许多不同操作系统和架构上的软件都是轻而易举的。“只需要写一次代码，就能做到在任何地方编译。此外，Go 和 Rust 在本地都支持交叉编译，消除了通常与旧编译语言相关的“构建农场”的需要。

“Golang 在生产优化方面具有出色的品质，比如内存占用很小，这支持它在大型项目中构建模块的能力，以及轻松地对其他架构进行开箱即用的交叉编译。因为 Go 代码被编译成单一的静态二进制文件，因此它可以轻易做到容器化，而且我们可以通过扩展，将它部署到任何高可用性的环境中，如 Kubernetes ，几乎是小事一件。” —— *[Dewet Diener, Curve](https://jaxenter.com/golang-curve-163187.html)*

“当你着眼于基于云的基础设施时，你通常会使用 Docker 容器之类的东西来部署工作负载。使用你在 Go 中构建的静态二进制文件，你可以有一个10、11、12兆字节的 Docker 文件，而不是引入整个 Node.js 生态系统，或 Python ，或 Java ，在那里你有几百兆字节大小的 Docker 文件。所以，发行这种微小的二进制文件是非常棒的。” —— *[Brian Ketelsen, Microsoft](https://cloudblogs.microsoft.com/opensource/2018/02/21/go-lang-brian-ketelsen-explains-fast-growth/)*

“有了 Rust ，我们将拥有一个能够轻松在 Mac、iOS、Linux、Android 和 Windows 上运行的高性能便携平台。” —— *[Matt Ronge, Astropad](https://blog.astropad.com/why-rust/)*

## 差异性

在设计中，总是需要权衡取舍。尽管 Go 和 Rust 基于相同的目标诞生于相同的时代，但由于他们有时会面临决策，因此他们选择了不同的权衡方法，以不同的方式将语言带到不同的发展方向。

### 1. 性能

Go具有开箱即用的出色性能。 根据设计，没有旋钮或杠杆可用来挤压 Go 的更多性能。 Rust 旨在使您能够从代码中压榨出所有的性能； 在这方面，您确实找不到比 Rust 更快的语言。 但是，Rust 的性能提高是以增加复杂性为代价的。

“值得注意的是，在编写 Rust 版本时，我们在优化时只采用了最基本的思想。 即使仅进行基本优化，Rust 仍能胜过超手工调整的 Go 。 与我们用 Go 进行的深入研究相比，这充分证明了用 Rust 编写高效的程序是多么容易。” —— *[Jesse Howarth, Discord](https://blog.discord.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f)*

“通过将逐行 Python 代码移植到 Go 中，Dropbox 工程师经常看到 5 倍的性能和延迟改进，并且与 Python 相比，由于没有 GIL（全局解释器锁） 并且内存数量可能减少，因此内存使用率通常会急剧下降。 但是，当我们的内存受到限制时，例如在桌面客户端软件或某些服务器进程中，我们将转移到 Rust 上，因为 Rust 中的手动内存管理比 Go GC 效率更高。”

### 2. 适应性/交互性

Go 的快速迭代优势使开发人员可以快速尝试想法并专注于解决手头任务的工作代码。 通常来说这就这足够了，并可以使开发人员有更多时间着手其他任务。 另一方面，与 Go 相比，Rust 的编译时间更长，导致迭代时间更慢。 这使得 Go 在更快的周转时间允许开发人员适应不断变化的需求的情况下可以更好地工作，而 Rust 更适合在花更多的时间进行更精致和高性能的实现。

“Go 类型系统的天才之处在于，调用者可以定义接口，从而允许库返回扩展的结构，但需要窄接口。 Rust 类型系统的天才之处在于将匹配语法与 `Result <>` 相结合，您可以在其中确定所有可能的事件都可以静态处理，而不必发明空值来满足未使用的返回参数。” —— *Daniel Reiter Horn, Dropbox*

“如果您的开发离用户层面越近，就越容易受到需求变化的影响，那么这种情况下 Go 会更好，因为持续重构的成本要便宜得多。 表达新要求并进行尝试的速度非常快。” —— *Peter Bourgon, Fastly*

### 3. 易学性

简而言之，与 Go 相比，确实没有比这更容易上手的语言了。 一个团队能够选择 Go 并在几周内将 Go 服务/应用程序投入生产，这样的故事有很多。 此外，Go 在各种语言中相对独特，因为它的语言设计和做法在其 10 年以上的生命周期中都非常一致。 因此，花在学习 Go 上的时间可以长期保持其价值。 相比之下，由于 Rust 的复杂性，它被认为是一种难以学习的语言。 通常，需要花费几个月的时间来学习 Rust 以适应它，但是这种额外的复杂性带来了精确的控制和更高的性能。

“当时，没有一个团队成员会 Go ，但不到一个月，所有人都在用 Go 开发。” ——  *[Jaime Garcia, Capital One](https://medium.com/capital-one-tech/a-serverless-and-go-journey-credit-offers-api-74ef1f9fde7f)*

“使 Go 与其他编程语言不同的是认知负担。 您可以用更少的代码来做更多的事情，这使您更容易推理和理解最终编写的代码。 大多数 Go 代码最终看起来都非常相似，因此，即使您使用的是全新的代码库，您也可以快速上手并运行。” —— *Glen Balliet Engineering Director of loyalty platforms at American Express [American Express Uses Go for Payments & Rewards](https://go.dev/solutions/americanexpress/)*

“但是，不同于其他编程语言，Go 的创建是为了最大程度地提高用户效率。 因此，具有 Java 或 PHP 背景的开发人员和工程师可以在几周内提高使用 Go 的技能和培训——并且根据我们的经验，其中许多人最终更喜欢它。” —— *[Dewet Diener, Curve](https://jaxenter.com/golang-curve-163187.html)*

### 4. 精准控制

Rust 的最大优势之一可能是开发人员对内存的管理方式，如何使用机器的可用资源，如何优化代码以及如何制定问题解决方案的控制权。 与 Go 相比，这有很大的复杂性成本，Go 的设计目的是减少此类精确操作的时间，而为更快的勘探时间和更快的周转时间而设计。

“随着我们在 Rust 方面的经验的增长，它在其他两个方面都显示出优势：作为一种具有强大的内存安全性的语言，它是在边缘进行处理的一个很好的选择，作为一种有着巨大热情的语言，它成为了一种在新生组件中流行的语言。”  —— *John Graham-Cumming, Cloudflare*

## 总结/要点

Go 的简单性，性能和开发人员的生产力使 Go 成为创建面向用户的应用程序和服务的理想语言。快速迭代使团队可以迅速进行调整以满足用户不断变化的需求，从而使团队可以将精力集中在灵活性上。

Rust 更好的控制可以实现更高的精度，从而使 Rust 成为不易更改的底层操作的理想语言，并且可以从与 Go 相比略有改善的性能中受益，尤其是在大规模部署时。

Rust 的优势最接近 metal 。 Go 的优势在于最贴近用户。这并不是说任何一方都不能在对方的领域中工作，但是这样做会增加摩擦。随着您的需求从灵活性到效率的转变，在 Rust 中重写库的情况就更强了。

尽管 Go 和 Rust 的设计差异很大，但它们的设计发挥了兼容的优势，并且在一起使用时具有很高的灵活性和性能。

## 推荐建议

对于大多数公司和用户，Go 是正确的默认选项。 它的性能强大，Go 易于采用，并且 Go 的高度模块化特性使其特别适合于需求不断变化或发展的情况。

随着您产品的成熟和需求的稳定，可能会有机会从性能的小幅提高中获得丰硕的成果。 在这些情况下，使用 Rust 来最大化性能可能很值得进行初始投资。