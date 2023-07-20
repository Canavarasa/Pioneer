> * 原文链接： https://rainandcoffee.substack.com/p/the-modular-world
> * 译文出自：[登链翻译计划](https://github.com/lbc-team/Pioneer)
> * 译者：[翻译小组](https://learnblockchain.cn/people/412)  校对：[Tiny 熊](https://learnblockchain.cn/people/15)
> * 本文永久链接：[learnblockchain.cn/article…](https://learnblockchain.cn/article/1)

# 模块化的世界

> 阐述 Celestia 的新功能和模块化世界的未来



## 前言

早在2019年，当我们（Maven11）投资LazyLedger（现在叫Celestia）时，**模块化**一词在区块链设计方面还没有普及。在过去的一年里，它已经被像polynya这样的人、大量的L2团队、无数的其他人，当然还有Celestia Labs团队(他们在LazyLedger的第一篇博文中创造了这个词， 表示分离共识和执行) 大量推广和普及。

正因为如此，我们很高兴能阐述我们对  Celestia 投资的思考。这里将提供对我们所设想的模块化世界的见解，在这样一个生态系统中的各个层次和协议，以及为什么我们对它提供的潜在功能如此兴奋。

## 架构

目前（本文发表于 2022 年 4 月），大多数正在运行的公共区块链都是单体区块链（monolithic）。所谓单体，我们指的是一条链，它可以自行处理数据可用性、结算和执行。现在，有一些单体链的变化，特别是关于以太坊上的 Rollup 和Avalanche上的子网，有模块化的组件。然而，这些并不是最真正意义上的模块化区块链。

让我们定义一下我们说的'模块化'是什么意思，这样就不会有误解了。当我们说模块化时，我们指的是可组合的层，他们是解耦。那么这意味着什么呢？它意味着链的三个组成部分是解耦的，所以要么是执行、共识或数据可用性。这意味着你可以把模块化这个词放在 Rollup 上，因为它们只处理执行。而以太坊处理所有的一切，对应单体区块链。 

对于 Celestia ，我们可以把模块化这个词放在它身上，因为它只处理数据可用性和共识。而它将结算和执行委托给其他层。这些层也是模块化的，因为它们只处理部分组件本身。而以太坊，我们不能称其为模块化区块链，因为组件的外包仅适用于其当前用于执行的 Rollup 。尽管如此，以太坊仍然能够自行处理执行，同时也允许 Rollup 在链外批量交易。因此，在其目前的实施中，以太坊仍然是一个单体链。尽管以太坊仍然是理想的结算层，同时也是最去中心化和最安全的智能合约链。

现在，你可能会说那 Polkadot 或 Avalanche 呢？对于 Avalanche ，它不是模块化的，而只是拆分的网络，他们都能处理区块链的所有组件。这意味着它们不是模块化的扩展，而是通过横向利用其他单体链进行扩展。Polkadot 的平行链处理执行，类似于 Rollups，他们将区块发送到中继链以达成共识和数据可用性。然而，中继链依旧在确保交易的有效性。

随着时间的推移，随着单体链的增长，它会导致巨大的拥堵和低效率。如果我们想让更多的人加入进来，局限于使用一个单一的链来达到所有目的是根本不可行的。因为它给终端用户带来了极高的费用和延误。这正是我们看到越来越多的链决定走向分片的原因。我们都知道以太坊的TheMerge，它将把以太坊转移到一个Proof-of-Stake链。然而，他们也计划最终转向分片。分片是指你将区块链横向分割成几块。这些分片将纯粹地处理数据可用性。

这与Rollups一起是以太坊社区计划解决其可扩展性问题的方式。现在，也有其他方法吗？当然有--我们也看到Avalanche通过Subnets走向了一个轻微的模块化未来，然而，正如前面解释的那样，我们不会把它归类为完全的模块化。

为了更好地理解各种 "模块化" 架构的功能，让我们试着把它们画出来，以便我们能更好地了解它们的区别。

## 不同架构的比较

### 以太坊

首先，让我们来看看现存最大的智能合约区块链，**以太坊**。让我们看看他们目前的架构是怎样的，以及未来启用分片后的架构是怎样的。

![当前的以太坊架构与 Rollup ](https://img.learnblockchain.cn/2023/07/11/34927.jpeg)

>  当前的以太坊架构与 Rollup 

目前，以太坊处理区块链的所有组件。然而，它也将一些执行转移到 L2 Rollup，然后在以太坊上进行批量交易结算。在未来，随着分片的出现，该架构将看起来有点像这样：



![分片后的以太坊](https://img.learnblockchain.cn/2023/07/11/58426.jpeg)



>  分片后的以太坊

这将使以太坊变成一个统一的结算层，而分片将处理数据的可用性。这意味着，分片将只是数据可用性（后面简称 DA ） 环境，供 Rollup 者提交数据。在分片上，验证者只需要为他们正在验证的分片存储数据，而不是整个网络。分片最终会让你在轻节点上运行以太坊，类似于Celestia。

### Avalanche

对于**Avalanche**来说，他们的主要扩展主张是通过可以轻松创建的独特的区块链--他们的子网。Avalanche的架构看起来有点像这样：

![有子网的雪崩](https://img.learnblockchain.cn/2023/07/11/25507.jpeg)



> 带子网的 Avalanche

子网是一组验证区块链的新验证者。每个区块链正好由一个子网来验证。所有雪崩子网都会自行处理共识、数据可用性和执行。每个子网也将有自己的Gas代币，由验证者指定。目前已上线的子网的一个例子是DefiKingdoms子网，它使用JEWEL作为其Gas代币。

### Cosmos

在我们继续看Celestia的架构之前，让我们先看看 **Cosmos**。Celestia 在很大程度上借鉴了Cosmos，并将通过IBC与之进行大量互动，因为它也是用Cosmos SDK和Tendermint的一个版本 -- Optimint构建的。Cosmos架构与前面其他架构有很大不同，因为它使dApps成为区块链应用本身，而不是提供一个虚拟机。这意味着一个主权的Cosmos SDK链只需要定义它所需要的交易类型和状态转换，同时依靠Tendermint作为其共识引擎。Cosmos链拆分了区块链的应用部分，并使用ABCI 将其连接到网络（p2p）和共识。ABCI是将区块链的应用部分连接到提供共识和网络机制的Tendermint状态复制引擎的接口。它的架构通常是这样描述的：

![Cosmos架构](https://img.learnblockchain.cn/2023/07/11/58390.png)



>  Cosmos 架构

现在让我们来看看，一旦生态系统开始建立，**Celestia的** 架构将是怎样的呢？

![早期Celestia生态系统](https://img.learnblockchain.cn/2023/07/11/83634.png)



>  早期 Celestia 生态系统

这就是Celestia的早期生态系统的样子。Celestia将作为所有在模块化技术栈内运行的各种类型的 Rollup 之间的共享共识和数据可用性层。结算层的存在是为了促进它上面的各种 Rollup 之间的衔接和流动性。虽然你很可能也会看到主权 Rollup 独立运行，不依靠结算层。

现在我们已经确定了不同程度的模块化，它们是如何运作的，以及它们的看起来是什么样的，让我们来看看像Celestia这样的纯模块化区块链的一些独特能力和功能。

## 共享安全

单体区块链的一个美丽之处在于，所有利用它的用户、应用程序和 Rollup 都能从底层获得安全。那么，这在模块化技术栈的设置中是如何运作的呢？

其实很简单--Celestia提供了链上建立共享安全所需的基本功能--数据可用性。这是因为每一个使用Celestia的层都需要将他们所有的交易数据传到数据可用性层，以证明数据确实是可用的。这意味着这些链就可以毫不费力地相互连接、观察和互操作。通过始终拥有底层DA层的安全性，使得硬分叉和软分叉也变得非常容易，这一点我们将在后面讨论。

同样，Celestia允许各种类型的实验执行层同时运行，甚至不依赖结算层，同时仍然具有共享数据可用性层的优势。这意味着迭代的速度将变得更快，因为它可能会随着用户数量的增加而线性扩展。因此，我们的论点是，随着时间的推移，这将导致执行层的指数改进，因为我们不受具有中心化执行层的单体实体的限制，因为执行和数据可用性是解耦的。模块化的无许可性质允许进行实验，并给开发者以选择的灵活性。

## 数据可用性采样和区块验证

Celestia的区块验证工作与目前其他区块链有很大不同，因为区块可以在次线性（sub-linear）时间内被验证。这意味着，与成本的线性增长相比，吞吐量会随着成本的次线性增长而增加。他们对比是这样的。



![次线性与线性](https://img.learnblockchain.cn/2023/07/11/41394.jpeg)



> 线性与次线性

这是可能的，因为 Celestia 的轻客户端不验证交易，他们只检查每个区块是否有共识，以及区块数据是否在网络中可用。

![区块验证](https://img.learnblockchain.cn/2023/07/11/85381.jpeg)



> 在Celestia上进行块状验证

Celestia 消除了检查交易有效性的需要，因为它只检查区块是否有共识和数据的可用性，如上所述。

Celestia 轻节点不需要下载整个区块，而只是从区块中随机下载一些数据样本。如果所有的样本都是可用的，那么这就可以证明整个区块是可用的。基本上，通过从一个区块中随机抽取数据，你可以概率地验证该区块确实是完整的。

这意味着Celestia将区块验证的问题简化为数据可用性验证，我们知道如何使用数据可用性抽样以次线性成本有效地完成这一验证。

![数据可用性证明](https://img.learnblockchain.cn/2023/07/11/41940.jpeg)

>  数据可用性证明

数据可用性证明是指当你要求正在发送的区块"擦除编码(erasure encoding)"的时候。这意味着原来的区块数据现在被扩大了一倍，而新的数据则被编码为冗余数据。Celestia的擦除编码将块的大小扩大了4倍，其中25%的块是原始数据，而75%是复制的数据。因此，若一个排序器想要作恶（或类似的进行欺诈的恶意行为），就必须扣留超过75%的块的数据。

因此，它允许轻型客户以非常高的概率检查一个区块的所有数据是否已经发布，只需下载该区块的一小部分（DA采样）。每一轮采样都会降低数据不可用的概率，直到确定所有数据都可用。这是非常有效的，因为不是每一个节点都下载每一个区块，而是有许多轻量级的节点下载每一个区块的一小部分，但安全保证与以前一样。这意味着，只要有足够的节点对数据的可用性进行采样，就有可能提高吞吐量，因为采样节点的数量在增加。你可能在日常生活中熟悉这种类型的网络（DA证明），即使你没有使用过区块链，例如你使用的BitTorrent等协议。

## 可扩展性

当我们谈论可扩展性时，大多数人想到的第一个想法通常是每秒的交易量。然而，这不应该是围绕可扩展性进行的实际讨论。当谈论专业DA层的可扩展性时，应该是Mb/s，而不是每秒交易量，这应该是需要克服的主要障碍。Mb/s成为衡量一个链的能力的客观标准，而不是tp，因为交易的大小是不同的。Celestia在这方面做得很好，因为它利用数据可用性采样来提高系统可以处理的mb/s数量。

我们的意思是，一个区块链能够处理多少交易的真正限制是基于其输入和输出的。因此，通过将数据可用性与输入和输出转换过程解耦， Rollup 将处理转换过程，Celestia将能够产生比单体区块链高得多的每秒字节数。

这一切都源于数据可用性的问题。排序器或类似的角色可以验证的数据数量（一个拟提议的区块），同时受限于底层DA层的数据吞吐量。对于利用全节点的单体区块链，解决这个问题的正常步骤是增加全节点的硬件要求。然而，如果你这样做，全节点将减少，网络的去中心化将随之动摇。

因此，通过利用我们之前在区块验证部分提到的技术，我们可以在不增加节点要求的情况下提高扩展性，通过DA采样使全节点等同于轻节点。这反过来又会使节点的增长能支持更多的吞吐量，因为DA采样也会随添加的轻节点的数量成比例增长（次线性增长）。在单体区块链设计中，块大小的增加同样会增加验证网络的成本，但在Celestia上，情况并非如此。



虽然，以太坊也希望通过 [EIP-4844](https://eips.ethereum.org/EIPS/eip-4844) 来解决一些可扩展性问题，这将使一个新的交易类型--blob交易成为可能。其将包含大量不能被EVM执行的数据，但仍然能够被以太坊访问。之所以这样做，是因为目前以太坊上的Rollup 依靠很小数据量的 calldata 来行使其交易。当然分片也会有帮助，但仍然相当遥远，当分片发布时，应该为每个区块的 Rollup 提供约16MB的数据空间。然而，对 blob 交易空间的争夺将变得多么激烈，还有待观察。虽然，一旦你解决了其中一个可扩展性的难题，另一个可能会冒出来。因此，通过向模块化层发展，可以让技术栈的各个部分专注于自己的可利用最多的特定资源。



在大多数情况下，当硬分叉发生在单体区块链链上时，你会失去底层的安全性，因为执行环境不共享相同的（原有的）安全性。这意味着通常硬分叉是不可行的，也是不受欢迎的，因为新的分叉会丢失数据可用性和共识层的安全。当我们说你可以提交对区块链代码的修改，但你必须说服所有人同意你的修改，就是这样。以比特币为例。比特币的代码是很容易改变的，然而，让每个人都同意改变是困难的部分。如果你想硬分叉一个单体区块链，你还需要分叉共识层，这意味着你失去了原始链的安全性。损失的安全量取决于不验证新分叉链的矿工或验证者的数量。然而，如果所有验证者都升级到同一个分叉，那么就不会失去安全性。

而在模块化区块链上，不是这样的，因为如果你想分叉一个结算或执行层，你仍然会有底层共识层的安全性。在此案例中，分叉是可行的，因为执行环境都共享相同的安全。虽然，这不适用于结算层 Rollup ，因为结算层作为新增区块的信任来源。

![以Celestia为DA层的硬分叉](https://img.learnblockchain.cn/2023/07/11/84947.png)



>  以Celestia为 DA / 共识层的硬分叉

对于执行环境来说，硬分叉是不受限制的，而且很容易实现，这是因为野生的想法可以被测试和尝试。同时，在不失去基础层安全的情况下，在别人的工作之上开展工作也是可行的。如果你考虑一下自由市场的想法（有些人可能不同意这一点），它往往可以创造出竞争性的实施方案，可以得到更好的结果。

##  模块化技术栈（Modular Stack）

模块化技术栈是 Celestia 特有的一个概念。它指的是将通常区块链的所有不同层解耦为独立的层。因此，当我们说技术栈时，指的是所有的层一起运作。

那么存在哪些层呢？毋庸置疑，有共识和数据可用性层 Celestia，但也有其他层。指的是结算层，它是一个链，让各 Rollup 有一个信任最小化的桥，用于统一流动性和在各 Rollup 之间进行桥接。这些结算层可以是各种类型的。例如，你可以有受限的结算层，它只允许简单的桥接和为其上的执行Rollup准备的决议合约(resolution contract)。不过，你也可以在结算层有自己的应用程序，也有 Rollup 。尽管也存在其他类型的 Rollup ，它们不依赖于结算层，而是仅靠 Celestia 自己的功能 -- 这些被称为主权 Rollup ，我们将在[下一节](#主权 Rollup)讨论它。

现在，有可能出现这样的技术栈，执行层不直接向结算层发布块数据，而是直接向 Celestia 发布。在这种情况下，执行层只是将他们的区块头发布到结算层，然后结算层会检查某个区块的所有数据是否包含在 DA 层中。这是通过结算层的一个合约完成的，该合约从 Celestia 接收交易数据的Merkle树。它就是我们所说的数据证明。

![模块化技术栈](https://img.learnblockchain.cn/2023/07/11/65194.png)

> 模块化技术栈

模块化技术栈的另一个巨大优势是其主权性。在模块化技术栈中，治理可以被划分到特定的应用程序或某层，而不会与其他应用程序重叠。如果遇到问题，治理可以修复它，而不干扰集群中的其他应用程序。

## 主权 Rollup



主权 Rollup 是一个独立于任何结算层的 Rollup。这意味着它不依赖于具有智能合约功能的结算层（结算层合约通常提供状态更新和证明），而是纯粹通过Celestia 上的命名空间发挥作用。通常情况下，Rollup 是在一个生态系统中运作，比如以太坊，它有一个Rollup智能合约（决议合约）。这个 Rollup 智能合约也在结算层和 Rollup 之间提供信任最小化的桥。然而，在以太坊上，所有的Rollup都在争夺珍贵的调用数据（Calldata）。这就是为什么EIP-4844正在推进，EIP-4844 将提供一个新的交易类型--blob交易。这也会增加区块大小。然而，即使有了blob交易，很可能仍然会有激烈的结算竞争。

大多数单体区块链都有能力处理智能合约。以以太坊为例，在链上有一个智能合约，用来处理状态根，也就是 Rollup 的当前状态的Merkle根。这个合约不断检查之前的状态根是否符合其当前根的 Rollup 交易批次。如果是这样的话，那么就会创建一个新的状态根。然而，在Celestia上，不是这样的，因为 Celestia 不会处理智能合约。

相反，在Celestia上，主权 Rollup 直接向Celestia发布他们的数据。这里的数据不会被计算或结算，而只是存储在区块头中。区块头是识别区块链上一个特定区块的东西，每个区块都是独一无二的。在这个区块头中，存在一个Merkle根，它是由所有的哈希交易组成的。

那么，它是如何工作的呢？ Rollup 有自己的点对点网络，全节点和轻节点都从这里下载区块。然而，节点也通过 Merkle 树验证所有的 Rollup 区块数据在Celestia 上的发送和排序（因此被称为数据可用性）--我们在前面已经看到了例子。因此，链的“权威"历史是由本地节点设定的，这些节点验证 Rollup 的交易是正确的。其含义是，主权 Rollup 需要在数据可用性层上发布每一个交易，这样任何节点都可以跟踪正确的状态。因此，作为 Rollup 命名空间的观察者的全节点（把命名空间看作是Rollup的智能合约）也可以为轻节点提供安全。这是因为，在 Celestia 上，轻节点几乎等同于全节点。

详述一下命名空间：在Celestia上，Merkle树是按命名空间排序的，这使得 Celestia 上的任何 Rollup 只下载与他们的链相关的数据，而忽略其他 Rollup 的数据。命名空间Merkle树（NMTs）使 Rollup 节点能够检索他们需要查询的所有 Rollup 数据，而无需解析整个 Celestia 或 Rollup 链。此外，它们还允许验证器节点证明所有的数据都已正确地包含在 Celestia 中。

那么，为什么主权Rollup 会有一个独特的前景？因为之前的Rollup实现，比如在以太坊上的实现是有限制的，因为以太坊是单体区块链，所以需要存储执行相关的状态。然而，在模块化设计中，我们可以有用于各种目的专门的节点，这应该使网络的运行成本大大降低。因此，运行网络的成本与轻节点的成本而不是全节点的成本成比例，因为正如我们前面解释的那样--轻节点=全节点。

让我们来看看一些 Rollup 实现如何作为主权 Rollup 的功能。

首先，有必要澄清各种 Rollup 证明系统在Celestia上是如何运作的。

### Celestia 上的 乐观 Rollup

**乐观 Rollup** 依赖于欺诈证明。欺诈证明将通过 Rollup 的全节点和轻节点在客户之间进行点对点的传播。我们将进一步研究这一点的实现。主权 Rollup 改变了欺诈证明的分布方式。他们现在不是在结算层合约上进行验证，而是在Rollup 点对点网络中分发，并由本地节点进行验证。通过 Celestia 上的主权乐观  Rollups，我们也可能将挑战期降到最低，这是解决当前OR的主要障碍之一，因为目前在以太坊上的争议窗口是非常保守的。在以太坊上这是需要的，因为目前所有的欺诈交互都发生在以太坊这个高度竞争的区块空间的链上，这导致最终处理变得旷日持久。然而，在主权Rollup上，任何轻量级节点如果与诚实的全量级节点相连，就有全量级节点的安全性，因此欺诈交互应该会快得多。



### Celestia 上的 ZK Rollup

**ZK Rollup **依赖于有效性证明（例如zksnarks）。作为主权 Rollup 的ZK Rollup 的功能与目前的实现方式相当类似。然而，它不是向智能合约发送ZK证明，而是在分布式点对点网络的 Rollup 上，供节点验证。主权 ZKRollup 与统一结算层上的 ZKRollup 一样，允许各种执行运行时作为主权链在其之上运行，因为他们的交易不被Celestia解释。在这里，ZKRollup之上的运行时可以以各种方式运作。可以有一个保护隐私的运行时，一个特定应用的运行时，以及更多。这就是所谓的分形扩容(Fractal Scaling)。

现在我们已经建立了主权 Rollup 的概念，并对它们在 Celestia 上的实现有了初步理解，让我们看看两个不同的 Rollup 的架构是怎样的：

![Sovereign Rollups on Celestia](https://img.learnblockchain.cn/2023/07/11/81133.jpeg)



>  Celestia 上的主权 Rollup

那么他们为什么需要Celestia？乐观 Rollup 需要 DA，这样用于欺诈证明检测，ZK Rollup 需要 DA，这样可以知道 Rollup 链的状态。

当你看一个东西的时候，始终保持逆向思维也很重要。因为如果不这样做，你往往会被自己的信念所蒙蔽。在本文中，我将尝试解释主权 Rollup 的一些负面因素。

主权 Rollup 将在很大程度上依赖于在其上建立的新生态系统，类似于经常吹捧的 L1 一样，他们需要 dApps 等。然而，如果 Rollup 有一个已经有很多开发多的虚拟机实现，并且dApps是开源的，那么实现就变得更加容易做到。尽管这样，流动性仍然是需要克服的主要问题。流动性通常会被分割到主权 Rollup 和它的运行时。因此， Rollup 将在很大程度上依赖于安全的、信任最小化的与其他层的桥，如其他主权 Rollup 或结算层。我们将在后面看一些可能的实现方式。此外，主权Rollup的实现在很大程度上取决于能够支持其各种功能的基础设施的建设。

### 乐观 Rollup 实现

在这一节中，我们将尝试解释一个可能的主权乐观 Rollup 的实现方式。这一部分主要借鉴了Ertem Nusret Tas、Dionysis Zindros、Lei Yang和Davis Tse撰写的[Light Clients for Lazy Blockchains](https://arxiv.org/pdf/2203.15968.pdf) 研究论文。

为OR提供欺诈证明的独特方式之一是在 有全节点和轻节点 Rollup 上进行一个对分游戏（bisection game）。对分游戏是在两个节点之间进行的，一个是挑战者（challenger），一个是响应者（responder）。挑战者将通过作为验证者（verifier）的第三个节点向响应者发送一个查询。响应者对该查询的答复将通过同一通道进行。在收到挑战后，验证者将把查询转发给响应者，响应者将产生一个响应，并被送回给验证者和挑战者。验证者将持续进行检查，以确保两者之间不存在不匹配，而且不是恶意的。验证者的作用是确保响应者没有发送错误的Merkle树，而挑战者的作用是确保响应者遵循正确的根。如果响应者能够为自己辩护，那么游戏就会照常进行。由于这个对分游戏的结果，诚实的挑战者总是会赢，诚实的回应者总是会赢。

![乐观 Rollup 的双节游戏](https://img.learnblockchain.cn/2023/07/11/61686.jpeg)



>  乐观Rollup的对分游戏

## 基于 Celestia 的数据可用性，在 X 上的结算。

还有另一个可能性：既不使用与Celestia连接的结算层进行桥接，结算也不作为主权 Rollup 的功能。因为 Celestia 只是提供了具有共享安全性的底层DA层，只要Celestia 能够向结算层合约发送可用交易数据的Merkle根，任何结算层都可以使用。这意味着，如果他们愿意的话，任何结算层都可以用于 Rollup 。那么他们为什么要这样做呢？许多现有的结算层，如以太坊，有一个已经存在并蓬勃发展的生态系统。因此，已经有流动性，和用户可以利用。这对于那些不想依靠从头开始建立整个生态系统的 Rollup 来说，是特别有利的。同样，这并不纯粹限于以太坊作为结算层。例如，你也可以利用Mina作为ZK的 Rollup 。这意味着你可以将你的交易数据发送到Celestia，同时将状态更新和zk-proofs发送到Mina。通过这样做，你已经有了一个默认的有效性证明的结算层。



如果你是一个 Rollup 运营商，并想利用其他区块链的流动性和的用户，那么这种类型的解决方案对你来说是非常有吸引力的。在某种程度上也有可能成为一个即插即用型的 Rollup 运营商。你可以让不同的排序器插入到不同的结算层。例如，一个ZK Rollup 排序器可以连接到Mina并提供状态更新和有效性证明。而另一个不同的ZK-Rollup上的排序器可以连接到以太坊，通过Quantum桥（传说中的Celestiums）进行结算。他们的共同点是，他们将把所有的交易数据发送到Celestia，然后Celestia将在结算层操作一个智能合约或类似的东西，在那里它将发送一个可用数据的Merkle树（证明）。



让我们以ZK Rollup 为例，看看这在架构上会是怎样的：

![X上的结算，Celestia上的数据可用性。](https://img.learnblockchain.cn/2023/07/11/14356.jpeg)



>  基于 X 结算，Celestia上的数据可用性

## 价值累积

Celestia 本身的收入来源将是来自各种 Rollup 提交的交易批次的交易费。Celestia 的交易费用将与以太坊目前的 EIP-1559 的运作方式相当类似，所以是一种燃烧式的机制。这意味着将有一个动态的基础费用被烧掉，以及给验证者的 "小费"，以更快地纳入某个交易，这些验证者也将出块的代币发行中获得价值。然而，这是从 Celestia 的验证者的角度来看的，那么从用户的角度来看会是怎样的呢？让我们首先根据你所使用的层数，说明各种费用会是什么样子，然后我们可以得出用户体验的结论。

执行层 Rollup 的收费构成将主要是运营成本 + DA发布成本。也可能会有一个管理费用，以便使 Rollup 获得利润。这意味着对于用户来说，你可能会支付一个包含这三个方面的费用+一个拥堵费 -- 由于拥堵减少，这个费用可能会低很多。

结算层的收入来源是结算合约费用， Rollup 需要支付以便能够在其上进行结算。此外，还将通过结算层在 Rollup 之间建立信任最小化的桥，所以它也能够收取桥接费。

那么，在没有结算层的情况下运作的主权 Rollup 呢？在主权Rollup上，用户将不得不支付一笔Gas费用来访问 Rollup 上的计算。 Rollup 将设置一个费用，很可能是由治理决定的，然后你可能会有一个拥堵费需要支付。 Rollup 的这些费用将涵盖向 Celestia 发布数据的费用，以及 Rollup 验证者的少量开销。主权Rollup 将没有结算费用，这可能会给终端用户带来极低的费用。

因此，最后，我们可以创建一个收费结构图，说明各种费用对终端用户来说是怎样的。模块化技术栈的终端用户可能会得到3个经常性的费用，有可能是4个。他们是 DA 发布费，结算合约费和 Rollup 执行费。第四种可能的费用是交易过载期间的拥堵费。用户只需在执行层支付一笔费用，这笔费用将包含模块化技术栈中所有层的成本。因此，让我们看看从用户的角度来看，收费结构会是什么样子：

![收费结构](https://img.learnblockchain.cn/2023/07/11/66645.jpeg)



> 费用结构

那么这对未来意味着什么？

好吧，如果 Celestia 被证明是一个更便宜和更快的数据可用性层，同时仍然提供去中心化和共享的安全，那么你可以看到越来越多的 Rollup 公司使用它做数据可用层。如果我们对比目前 Rollups 使用以太坊的安全而支付的费用有[多少](https://l2fees.info/l1-fees)，那么 Celestia 上的 Rollups 支付的费用会少得多。当然，也有一些升级措施即将到来，以解决以太坊上的拥堵问题，主要是blob交易、分片等。

另外，对于 MEV 呢？目前，Rollups 通过排序器在mempool中收集和排序用户的交易，然后再执行并发布到DA层。这是一个关于MEV的问题，因为在目前的实现中，排序器主要是中心化的，因此不具备抗审查能力。目前解决这个问题的方法是将排序器去中心化，目前的很多 Rollup 计划都是这样做的，尽管这带来了它自己的一系列问题。另一个以某种形式解决这个问题的方法是将验证器和交易列表的排序分开(如果你有兴趣，可以看看Vitalik的[论文](https://notes.ethereum.org/@vbuterin/pbs_censorship_resistance))。



总而言之，模块化技术栈的各层通过交易价值获得收入。用户从每个层上的交易中获得价值（从其交易包含在一个层上获得的价值），从而也需要支付费用。

## 桥

正如我们之前所讨论的，如果一个 Rollup 有一个结算层，与其他 Rollup 的信任最小化的桥接是通过结算层的。但是，如果是主权 Rollup ，或者它想桥接另一个集群（网络），会发生什么？让我们来看看跨集群的通信。

在两个主权 Rollup 想要通信的情况下，他们实际上可以利用轻客户端技术，就像 IBC 的功能一样。轻客户端将通过 P2P 网络接收来自两个 Rollup 的区块头以及 Rollup 所使用的证明。这既可以通过锁和铸币机制工作，如IBC，也可以通过中继器的验证器。使用Cosmos SDK构建的链和那些利用Tendermint或Optimint桥接的链变得更加无缝，因为你可以完全利用ICS的IBC。然而，这需要两个链包含彼此的状态机，并让桥接链的验证者注销（write off）交易。其他的桥接方式也可以存在。例如，我们可以设想有第三条链，它的功能是某种轻型客户端。在这个链上，想要桥接的两个链可以流转他们的区块头，然后作为两个链的结算层来运作。或者你可以依靠一个Cosmos链来充当 "集群间 Rollup 中心"，链上的验证者可以通过遵循 Rollup 的条件来操作桥接。还存在各种各样的桥即服务的链，如Axelar，以及其他许多链。

然而，到目前为止，促进桥接的最简单的方法，是让执行Rollup使用相同的结算层，因为他们会在上面有信任最小化的桥接合约。

各层之间的桥之所以如此重要，是因为它可以实现统一的流动性。其次，通过允许协议和层通过共享状态相互组合，我们解锁了新的互操作性水平。状态共享是指一个链对另一个链进行调用的能力。这里特别值得关注的是，特别是与[ICS-27](https://github.com/cosmos/ibc/blob/master/spec/app/ics-027-interchain-accounts/README.md)的链间账户的能力。

因此，我们可以得出结论，轻客户端在IBC等互操作性标准中是至关重要的。Celestia 轻客户端的结果将使不同集群中的链之间的互操作性更加安全。关于Celestia与IBC的连接，那么他们正计划使用治理白名单来对某些链与Celestia 的连接，以限制状态的膨胀。

## 终端用户验证

虽然过去几年的各种单体区块链和模块化设计方法都是创新的，而且构建这些方法的人才数量也是惊人的。在各种权衡之下，有一个基本问题在我们的领域里已经存在了相当长的一段时间。我们认为关键是终端用户的验证及其需要。

终端用户有可验证的可能性是否重要？很多设计上的权衡（例如区块大小）都是围绕着运行一个全节点的便利性进行的，而DAS使轻型客户机成为 "一等公民"，可以与全节点相媲美。

这样想的基本假设是，用户根本不关心成为"一等公民"的问题。用户可以通过运行轻量级客户端/全节点轻松地验证链，但这并不意味着他们会这样做，但他们会重视拥有这样做的能力。

支持这个论点的论据是相当直接的。如果用户不关心验证，你还不如运行一个中心的数据库。它总是更有效率，因为去中心化往往是以牺牲效率为代价的。因此，我们为什么要建立加密协议，就是因为终端用户能够验证计算结果。

反对的论点是，只要网络足够分散，终端用户验证本身并不重要。只要用户体验好，用户就不会关心它。终端用户验证有多重要，目前还没有明确的答案。然而，我们认为终端用户能够验证链是一个值得追逐的目标，也是许多人在这个领域建设的原因。

## 模块化技术栈的未来

本节将作为一种方式来设想建立在Celestia之上的模块化技术栈在未来会是什么样子。我们看一下模块化技术栈的宏观架构，以及可能看到什么样的层。



下面是许多可能的层的图示，这些层可以在模块化技术栈中发挥作用。它们都有一个共同点，那就是它们都在使用Celestia来提供数据。我们可能会看到各种主权Rollup，包括 Optimism 和ZKRollup，它们将在没有结算层的情况下运作。我们也有可能看到 Rollup 利用Cevmos作为结算层，同时还有各种应用链。我们也有可能看到其他类型的结算层。这些结算层可能是受限制的，这意味着他们将有预先设定的合约（或依靠治理来白名单合约），用于桥接和 Rollup 。

![模块化技术栈的未来](https://img.learnblockchain.cn/2023/07/11/42761.jpeg)



> 模块化技术栈的未来

在图的右边是其他非原生的结算链，它们同样可以有 Rollup ，利用它们进行流动性和结算，同时依靠 Celestia 向结算层提供交易数据的证明。

所有这些集群都将通过各种桥接服务连接，包括新的桥和旧的桥。

你没有看到的是，还将建立所有的基础设施，以方便访问 Celestia 的各种功能，如RPC端点、API和更多。



---

本翻译由 [DeCert.me](https://decert.me/) 协助支持， 来DeCert码一个未来， 支持每一位开发者构建自己的可信履历。