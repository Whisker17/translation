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

Go 和 Rust 有很多共同之处。这两种现代软件语言都是出于为影响软件开发的问题提供安全且可伸缩的解决方案的需要而诞生的。这两种语言都是针对业界现有语言的缺陷而创建的，尤其是在开发人员的生产率、可伸缩性、安全性和并发性方面的缺陷。

今天大多数流行的语言都是30多年前设计的。当这些语言被设计出来的时候，它们与今天有五个主要的区别:

1. 摩尔定律被认为是永远正确的。
2. 大多数软件项目都是由小团队编写的，他们经常亲自一起工作。
3. 大多数软件都有相对较少的依赖项，大多数是专有的。
4. 安全是次要考虑的，或者根本不考虑。
5. 软件通常是为单一平台编写的。

相比之下，Rust和Go都是为当今世界编写的，并且通常采用类似的方法来设计适合当今开发需要的语言。

### 1. 性能与并发度

Go和Rust都是专注于生成高效代码的编译语言。它们还提供了对当今机器的多处理器的方便访问，使它们成为编写高效并行代码的理想语言。

“使用Go允许自由市场将用于这项服务的服务器数量减少到原来的1 / 8(从32台服务器减少到4台)，而且每个服务器可以用更少的电力运行(原来是4个CPU核，现在减少到2个CPU核)。有了Go，该公司淘汰了88%的服务器，并在剩下的服务器上削减了一半的CPU，节省了大量成本。—— *[MercadoLibre Grows with Go](https://go.dev/solutions/mercadolibre/)*

“在我们运行Go代码的严格管理环境中，我们看到CPU减少了大约10% (vs c++)，代码更干净、更易于维护。” ——*[Bala Natarajan, Paypal](https://go.dev/solutions/paypal/)*

“在AWS，我们也喜欢Rust，因为它帮助AWS编写高性能、安全的基础设施级网络和其他系统软件。
亚马逊的第一个引人注目的产品是用Rust构建的爆竹，于2018年公开发布，并提供了为AWS Lambda和其他无服务器产品提供支持的开源虚拟化技术。
但我们也使用Rust提供服务，如亚马逊简单存储服务(Amazon S3)、亚马逊弹性计算云(Amazon EC2)、亚马逊CloudFront、亚马逊Route 53等。
最近我们推出了一个用Rust编写的基于linux的容器操作系统Bottlerocket。” —— *[Matt Asay, Amazon Web Services](https://aws.amazon.com/blogs/opensource/why-aws-loves-rust-and-how-wed-like-to-help/)*

我们“看到速度惊人地提高了1200-1500% !我们从300-450ms的Scala发布模式，实现更少的解析规则，到25-30ms的Rust模式，实现更多的解析规则!” ——*[Josh Hannaford, IBM](https://developer.ibm.com/technologies/web-development/articles/why-webassembly-and-rust-together-improve-nodejs-performance/)*

### 2. 团队可扩展性 —— 可审查

如今的软件开发是由不断成长和扩展的团队构建的，通常使用源代码控制以分布式的方式协作。Go和Rust都是为团队的工作方式而设计的，通过消除诸如格式、安全性和复杂组织等不必要的问题来改进代码审查。这两种语言都需要相对较少的上下文来理解代码在做什么，从而允许评审者更快地处理其他人编写的代码，并评审团队成员和团队之外的开源开发人员贡献的代码。

“在我的早期职业生涯中，我有过Java和Ruby的背景，构建Go和Rust代码感觉像是肩上卸下了一个不可能的重量。当我在谷歌的时候，遇到用Go编写的服务是一种解脱，因为我知道它很容易构建和运行。这也适用于《Rust》，尽管我只在一个小得多的规模上研究过它。我希望无限可配置构建系统的时代已经一去不复返了，所有的语言都带有自己的专用构建工具，可以开箱即用。” —— *[Sam Rose, CV Partner](https://bitfieldconsulting.com/golang/rust-vs-go)*

“我倾向于松一口气当写作服务,因为它有一个非常简单,容易推断,静态类型系统和动态语言相比,并发性是一等公民,去的标准库是难以置信的抛光和强大,但也要点。
采用一个标准的Go安装，加入一个grpc库和一个数据库连接器，在服务器端构建任何东西都不需要什么其他东西，每个工程师都可以阅读代码并理解库。
当在Rust中编写一个模块时，Dropbox的工程师们感到Rust在服务器端的成长之痛，但在2019年async -await稳定下来之前，板条箱开始使用它，我们从中获得了异步模式与无所畏惧的并发性的好处。” —— *Daniel Reiter Horn, Dropbox*

### 3. 开源意识

今天，平均的软件项目所使用的依赖项的数量是惊人的。在现代开发中，软件重用的目标已经实现了几十年，今天的软件是使用上百个项目构建的。为了做到这一点，开发人员使用软件存储库，它越来越多地成为跨越广泛应用程序的软件开发的主要内容。开发人员所包含的每个包依次都有自己的依赖项。当今编程环境的语言需要毫不费力地处理这种复杂性。

去锈都包管理系统,允许开发人员做一个简单的列表,他们想建造的包,和语言工具自动获取和维护这些包,以便开发人员可以将更多的注意力放在自己的代码,减少对别人的管理。

### 4. 安全性

Go和Rust都很好地解决了当今应用程序的安全问题，它们确保了内置在语言中的代码在运行时不会暴露给用户各种经典的安全漏洞，如缓冲区溢出、免后使用等。通过消除这些问题，开发人员可以专注于手头的问题，并构建默认情况下更安全的应用程序。

“当你处理错误的时候，(生锈的)编译器真的会帮助你。这可以让您专注于您的业务目标，而不是寻找bug或破译神秘的消息。” —— *[Josh Hannaford, IBM](https://developer.ibm.com/technologies/web-development/articles/why-webassembly-and-rust-together-improve-nodejs-performance/)*

简而言之，Rust的灵活性、安全性和安全性超过了必须遵守严格的生存期、借用和其他编译器规则，甚至缺乏垃圾收集器带来的不便。这些特性是云软件项目急需的附加功能，将有助于避免在这些项目中常见的许多bug。” —— *[Taylor Thomas, Sr., Microsoft](https://msrc-blog.microsoft.com/2020/04/29/the-safety-boat-kubernetes-and-rust/)*

Go是强静态类型的，没有隐式转换，但语法开销仍然小得惊人。这是通过在赋值中使用简单的类型推断以及无类型的数字常量来实现的。这比Java(具有隐式转换)提供了更强的类型安全性，但代码读起来更像Python(具有无类型变量)。” —— *[Stefan Nilsson, computer science professor](https://yourbasic.org/golang/advantages-over-java-python/)*

“当我们在Dropbox构建用于存储块数据的Brotli压缩库时，我们将自己限制在Rust的安全子集，以及核心库(no-stdlib)，以及指定为通用分分器。
通过这种方式使用Rust的子集，可以很容易地在客户端从Rust调用Rust- brotli库，并在服务器端同时使用Python和Go中的C FFI。
这种编译模式也提供了[实质性的安全保障](https://dropbox.tech/infrastructure/lossless-compression-with-brotli)。经过一些调优，Rust Brotli实现尽管是100%安全的、数组边界检查代码，但仍然比c中相应的原生Brotli代码要快。” —— *Daniel Reiter Horn, Dropbox*

### 5. 真正的便携度

在Go和Rust中，编写一个运行在许多不同操作系统和架构上的软件都是微不足道的。“写一次，在任何地方编译。此外，Go和Rust在本地都支持交叉编译，消除了通常与旧编译语言相关的“构建农场”的需要。

“Golang在生产优化方面具有出色的品质，比如内存占用很小，这支持它在大型项目中构建模块的能力，以及轻松地对其他架构进行开箱即用的交叉编译。
因为Go代码被编译成单一的静态二进制文件，它允许很容易的容器化，而且，通过扩展，将它部署到任何高可用性的环境中，如Kubernetes，几乎是小事一件。” —— *[Dewet Diener, Curve](https://jaxenter.com/golang-curve-163187.html)*

“当你着眼于基于云的基础设施时，你通常会使用Docker容器之类的东西来部署工作负载。使用你在Go中构建的静态二进制文件，你可以有一个10、11、12兆字节的Docker文件，而不是引入整个Node.js生态系统，或Python，或Java，在那里你有几百兆字节大小的Docker文件。所以，发行这种微小的二进制文件是非常棒的。” —— *[Brian Ketelsen, Microsoft](https://cloudblogs.microsoft.com/opensource/2018/02/21/go-lang-brian-ketelsen-explains-fast-growth/)*

“有了《Rust》，我们将拥有一个能够轻松在Mac、iOS、Linux、Android和Windows上运行的高性能便携平台。” —— *[Matt Ronge, Astropad](https://blog.astropad.com/why-rust/)*

