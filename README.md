# CosmosChain技术白皮书V0.1




### 区块链平台
我们可以将区块链分为基础平台层和智能合约两层。智能合约或者DApp关心的是提供具体的功能，比如互联网金融平台资产跟踪应用，汽车供应链金融应用等。而基础平台层关心的则是区块链本身的安全性、速度、带宽等。 以太坊或者EOS都是一个典型的平台+应用的模式，而CosmosChain也提供了基础平台层和智能合约层两部分。

区块链平台根本上来说，像一个分布式操作系统，其上可以运行任何（分布式）App。但这个操作系统和我们熟悉的安卓或者Windows，却有个根本性的不同 - 它并不运行在一个单台的PC或者iPhone手机上，而是背后有无数个节点在支撑这个操作系统。这个结构性的不同正是区块链优势的来源 - 许许多多台电脑确保了即使其中一些宕机了或者损坏了，操作系统及上面的应用依旧安然无恙，所有的数据也都持续的保存。同样的，当新的电脑加入这个操作系统，他们也自动的和网络上其他节点进行同步。

### 共识机制：
#### 共识背景知识说明：
区块链系统首先是分布式系统，分布式领域最为基础的问题就是一致性问题。所谓一致性，是指对于分布式系统中的多个节点，给定一系列操作，在约定协议的保障下，它们对处理结果达成认同。分布式环境里要求多点数据是一致的，即数据要完整、要同步。当多个主机通过异步通讯方式组成网络集群时，这种异步网络默认是不可靠的，那么在这些不可靠主机之间复制状态需要采取一种机制，以保证每个主机的状态最终达成相同一致性状态，取得共识。 根据FLP不可能原理，Impossibility of Distributed Consensus with One Faulty Process一文提出：在一个异步系统中我们不可能确切知道任何一台主机是否死机了，因为我们无法分清楚主机或网络的性能减慢与主机死机的区别，也就是说我们无法可靠地侦测到失败错误。但是，我们还必须确保安全可靠。
在传统的数据库和分布式系统领域，对数据一致性的研究已经非常多，但在区块链出现之前，很少有系统有上万个节点要同步，同时在传统的分布式网络中，各个节点也不会因为贪图利益故意伪造信息，很多情况下是由于网络的原因而掉线或发送错误消息。因此，可根据要解决的问题是普通错误还是拜占庭将军问题，将共识算法分为CFT（Crash Fault Tolerance）和BFT（Byzantine Fault Tolerance)。BFT是在区块链系统中常用的共识算法，分为PBFT（Practical Byzantine Fault Tolerance）为代表的确定性系列算法和工作量证明（PoW）为代表的概率算法。对于确定性算法，一旦达成对某个结果的共识就不可逆转，即共识是最终结果；而对于概率性算法，共识结果则是临时的，随着时间推移或某种强化，共识结果被推翻的概率越来越小，成为事实上的最终结果。 
 
#### CosmosChain 共识机制：（*需讨论确认*）
区块链的去中⼼化特性，以及交易透明性、⾃治性和不可篡改特性，对于加密货币来说是⾄关重要的，这些特性为此类系统定下了基调。然⽽，早期设计的加密货币，如⽐特币和以太坊，由于采用了PoW共识，在交易效率⽅⾯已被⼴泛认为是不可扩展的，⽽且在经济上也不可⾏。而EOS等区块链产品的DPoS共识虽然想效率更高，但是却带来了中心化、投票积极性低、对于坏节点处理存在诸多困难等问题。而以PBFT为代表的拜占庭容错协议虽然在一定条件下可以有较高的性能，但是它们通常被设计于私有场合使用，即所有节点需要在协议开始前知道相互的身份(公钥等)，并且节点不能自由出入网络。这使得BFT协议自身难以适用于区块链的场景。
随着在现实世界中使⽤公链上的应⽤程序和平台的需求不断增长，众多的去中心化应用对性能是有要求的，这就要求有新的区块链基础设施，同时满足两个条件，首先要比原有的基于PoW的公链有明显的性能提升，同时保证去中心化和低参与或部署成本。混合共识(Hybrid Consensus)是目前看来同时满足性能要求和公链要求的几个最具有前景的设计方案之一。自提出至今，它在学术界已经有了比较扎实的理论基础，同时有比较强的可实施性。介我们的方案又是公链和联盟链的结合，于此我们可以把区块链分层，这样达到去中心、并发效率和权限控制的平衡，所以我们采用了POS+PBFT的方式作为共识机制。这样CosmosChain的通讯效率足以支持10,000-100,000 TPS（每秒交易处理量），可以保证多个智能合约或商业应用同时处理交易时全链通讯不受到阻塞，账本按时间戳先后顺序准确记录交易。

### 匿名性：
在大部分有记录的历史中，交易都是隐秘并且匿名的。交易信息只会对支付方和收取方披露。但是近年来由于绝大部分金融交易都需要通过信息技术完成，因此维护金融财务信息的隐私变得越来越困难。时常发生的大型金融机构内部因违规行为而导致个人和金融信息严重泄漏，一个更具保护金融隐私的需求已经变得更加迫切。比特币或者以太坊等数字货币缺乏隐私性，还有政府和私营部门组织利用大量数据集和机器学习来识别与此类交易相关的个人身份。
除了金融交易外，在大数据概念热潮的今天，我们看到了另一种关注隐私为主要出发点的新思潮：隐私是人权的基本部分。从斯诺等事件出现后，整个西方世界对于隐私，特别是数字隐私，加速了觉醒。不管是Telegram短短时间几亿用户增长，到Whatsapp十亿多用户全部被切换到新的端到端加密通讯，都是很好的证明。然而，今天几乎所有的互联网社交媒体和数字身份，还是中心化的。微信登录、Facebook账户，Google帐号、邮件等等都是被服务商牢牢掌握身份和行为数据。
许多加密货币都试图解决这个隐私问题。不幸的是，大部分通过混淆或TOR 节点实现匿名的链上交易系统仍然能够通过各种技术被攻陷。2014年，麻省理工 大 学 的 研究人员 在 一 项 突 破 性 研 究 论 文 中讨论了“zero-knowledge non-interactive arguments of knowledge”中提出了zk-SNARKs。值得注意的是，实施 zk-SNARKs 的加密货币允许进行隐秘式交易 - 资金完全匿名且无交易或地址余额出现在分类帐上。而为了保证机构数据、用户数据，资产数据和交易数据的匿名性，CosmosChain也使用了 zk-SNARKs技术。

我们当前使用的 zkSNARKs 包含 4 个主要的部分，
A) 编码成一个多项式问题
把需要验证的程序编写成一个二次的多项式方程：t(x) h(x) = w(x) v(x)，当且仅当程序的计算结果正确时这个等式才成立。证明者需要说服验证者这个等式成立。
B) 简单随机抽样
验证者会选择一个私密评估点 s 来将多项式乘法和验证多项式函数相等的问题简化成简单乘法和验证等式 t(s)h(s) = w(s)v(s) 的问题。
这样做不但可以减少证明量，还可以大量的减小验证所需的时间。
C) 同态（Homomorphic）编码 / 加密
我们使用一个拥有一些同态属性的（并不是完全同态的，至少在实际使用中有一些不是同态的）编码 / 加密函数 E。这个函数允许证明者在不知道 s 的情况下计算 E(t(s)), E(h(s)), E(w(s)), E(v(s))，她只知道 E(s) 和一些其他有用的加密信息。
D) 零知识
证明者通过乘以一个数来替换 E(t(s)), E(h(s)), E(w(s)), E(v(s)) 的值，这样验证者就可以在不知道真实的编码值的情况下验证他们正确的结构了。
有一个粗糙的想法是这样的，因为校验 t(s)h(s) = w(s)v(s) 和校验 t(s)h(s) k = w(s)v(s) k（对于一个不等于 0 的私密的随机数 k 来说）几乎是完全一样的，而不同的地方在于如果你只接收到了 (t(s)h(s) k) 和 (w(s)v(s) k) 那么从中获取到 t(s)h(s) 或者 w(s)v(s) 的值就几乎是不可能了。

### 数据存储
诸如比特币、以太坊等主流区块链产品在链上都只能记录少量内容，通常我们只能用来存储hash值，具体内容还需要放在中心化的数据库中，hash值只是用来校验。CosmosChain有机的结合了区块链、哈希值和IFPS（Inter Planetary File System），我们将在IPFS上存储大量的文本、图像、资料等内容，达到了全数字资产上链的目的。

### 自主进化
目前已有的区块链产品无法自主进化，而必须依赖"硬分叉"。区块链平台像一个生命体，它需要不断地自我适应和升级。然而今天的大部分区块链没有任何自我变更的能力，唯一的方式是硬分叉 - 也就是启用一个全新的网络并让所有人大规模迁移。然而，在比特币和以太坊的经验，我们已经看到每一次的硬分叉升级都面临一系列的社区分化风险，甚至面临各种僵持不下的局面，迟迟不能推进网络进化。
而CosmosChain采用了POS+PBFT的共识机制，决定了只要社区达成共识，那么CosmosChain不会硬分叉，这样也使得CosmosChain可以进行无缝进行进化

### 发行分解
永久线性增长模型降低了在比特币中出现的财富过于集中的风险，并且给予了活在当下和将来的人公平的机会去获取货币，同时保持了对获取和持有CosmosChain币的激励，因为长期来看“货币供应增长率”是趋于零的。我们还推断，随着时间流逝总会发生因为粗心和死亡等原因带来的币的遗失，假设币的遗失是每年货币供应量的一个固定比例，则最终总的流通中的货币供应量会稳定在一个等于年货币发行量除以遗失率的值上（例如，当遗失率为1%时，当供应量达到30x时，每年有0.3x被挖出同时有0.3x丢失，达到一个均衡）。

### 扩展性
扩展性问题是CosmosChain常被关注的地方，与比特币一样，CosmosChain也遭受着每个交易都需要网络中的每个节点处理这一困境的折磨。比特币的当前区块链大小约为20GB，以每小时1MB的速度增长。如果比特币网络处理Visa级的2000tps的交易，它将以每三秒1MB的速度增长（1GB每小时，8TB每年）。CosmosChain可能也会经历相似的甚至更糟的增长模式，因为在CosmosChain区块链之上还有很多应用，而不是像比特币只是简单的货币，但CosmosChain全节点只需存储状态而不是完整的区块链历史这一事实让情况得到了改善。
CosmosChain会使用两个附加的策略以应对此问题。首先，因为基于区块链的挖矿算法，至少每个矿工会被迫成为一个全节点，这保证了一定数量的全节点。其次，更重要的是，处理完每笔交易后，我们会把一个中间状态树的根包含进区块链。即使区块验证是中心化的，只要有一个诚实的验证节点存在，中心化的问题就可以通过一个验证协议避免。如果一个矿工发布了一个不正确的区块，这区块要么是格式错，要么状态S[n]是错的。因为S[0]是正确的，必然有第一个错误状态S[i]但S[i-1]是正确的，验证节点将提供索引i，一起提供的还有处理APPLY(S[i-1],TX[i]) -> S[i]所需的帕特里夏树节点的子集。这些节点将受命进行这部分计算，看产生的S[i]与先前提供的值是否一致。
另外，更复杂的是恶意矿工发布不完整区块进行攻击，造成没有足够的信息去确定区块是否正确。解决方案是质疑-回应协议：验证节点对目标交易索引发起质疑，接受到质疑信息的轻节点会对相应的区块取消信任，直到另外一个矿工或者验证者提供一个帕特里夏节点子集作为正确的证据。

### 代币Token合约标准
Ethereum 是基于区块链技术实现的、开源的、公共维护的分布式计算底层系统，它提供了去中心化的图灵完备的虚拟机来支持智能合约的运行。Ethereum 作为是市面上最成熟的支持智能合约的平台，我们基于以太坊实现了诸多特性来支持 CosmosChainToken 的运转，其中包
括通过智能合约进行"网贷平台资产上链"、"汽车供应链资产上链"等。ERC20 Standard 是得到以太坊社区共识的账户合约标准, CosmosChainToken正是采用了这一合约标准。
CosmosChain团队将Token资产CosmosChainToken在区块链上进行应用登记，确保资产一旦通过智能合约被确认后，所有数据公开、透明、不可篡改。所以使用CosmosChainToken 进行进行交易的是完全可靠的数据，不会出现虚假资产、交易的情况。

### 时间进度表 (待完善修改)
*2018年6月至2018年8月* 
网贷平台资产上链项目、CosmosChainToken智能合约发布
*2018年9月至2018年12月* 
网贷平台联盟资产上链，网贷平台联盟区块链风控系统，网贷平台联盟Token商城系统，CosmosChain网络层、共识层开发
*2019年1月至2019年6月* 
网贷平台联盟Token商城2.0，基于区块链的汽车供应链金融系统，CosmosChain公链与联盟链对接部分开发，优化DPOS与PBFT算法。
*2019年7月至2019年12月* 
CosmosChainv0.1版本发布并公开，并将更多系统作为DAPP加入CosmosChain
*2020年1月至2020年6月* 
CosmosChain链测试链上线，接入更多第三方DAPP并测试完善后CosmosChain主网上线。



References:
[1]Satoshi Nakamoto (2008) "Bitcoin: A peer-to-peer electronic
cash system."

[2] David Schwartz, Noah Youngs,  Arthur Britto (2014) "The Ripple Protocol Consensus Algorithm"

[3] Michael Backes, Manuel Barbosa, Dario Fiore, Raphael M. Reischuk (2015) "ADSNARK: nearly practical and privacy-preserving proofs on authenticated data", IEEE Symposium on Security and Privacy (Oakland)

[4]Eli Ben Sasson et al. (2014) Zerocash: Decentralized Anonymous Payments from Bitcoin

[5]MJ Fischer et al. (1985) Impossibility of Distributed Consensus with One Faulty Process

[6] G. Bracha. (1987)  Asynchronous byzantine agreement protocols. Information and Computation.

[7] C. Cachin and S. Tessaro.(2005)  Asynchronous verifiable information dispersal. In Reliable Distributed Systems, 2005. 

[8] M. Ben-Or, B. Kelmer, and T. Rabin. Asynchronous secure computations with optimal resilience. In Proceedings of the thirteenth annual ACM symposium on Principles of distributed computing1994.

[9] A. Mostefaoui, H. Moumen, and M. Raynal. (2014) Signature-free asynchronous byzantine consensus with t< n/3 and o (n 2) messages. In Proceedings of the 2014 ACM symposium on Principles of distributed computing .

[10] Martin, J-P., and Lorenzo Alvisi.  (2006) “Fast byzantine
consensus.” Dependable and Secure Computing

[11] Alchieri, Eduardo AP, et al. （2008） “Byzantine consensus
with unknown participants.” Principles of Distributed
Systems. Springer Berlin Heidelberg.

