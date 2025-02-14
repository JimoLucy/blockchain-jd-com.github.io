## 总体目标
区块链是一种新型分布式架构，以密码学和分布式技术为核心，无需借助“第三方” 就能在多个业务方之间进行安全、可信、直接的信息和价值交换。在这种点对点的信息和价值的交换中，区块链起到了“协议”的作用。基于这一视角，`JD Chain`的目标是实现一个面向企业应用场景的通用区块链框架系统，能够作为企业级基础设施，为业务创新提供高效、灵活和安全的解决方案。

## 设计原则
设计原则是系统设计和实现的第一价值观，从根本上指导技术产品的发展方向。京东区块链在技术规划和系统架构设计上，遵循以下设计原则：
![rule](http://ledger.jd.com/images/636143efds-rule.png)

1. 面向业务

“企业级区块链”的目标定位决定了系统的功能设计必须要从实际的业务场景出发，分析抽象不同业务领域的共性需求。京东的区块链应用实践案例涉及包括金融、供应链、电子存证、医疗、政务、公益慈善等众多领域，从中获得丰富的应用实践经验，这能够为京东区块链获得良好通用性提供设计输入和业务验证。

2. 模块化

企业应用场景的多样性和复杂性要求系统有良好的扩展性，遵循模块化的设计原则，可以在确保系统核心逻辑稳定的同时，对外提供最小的扩展边界，实现系统的高内聚低耦合。

3. 安全可审计

区块链的可信任需要在系统设计和实现上遵循安全原则，数据可审计原则，以及满足不同地区和场景的标准与合规要求，保障信息处理可满足机密性、完整性、可控性、可用性和不可否认性等要求。

4. 简洁与效率

简洁即高效，从设计到编码都力求遵循这一原则。采用简洁的系统模型可以提升易用性并降低分布式系统的实现风险。此外，在追求提升系统性能的同时，也注重提升应用开发和方案落地的效能。

5. 标准化

区块链作为一种点对点的信息和价值交换的“桥梁”，通过定义一套标准的操作接口和数据结构，能够提升多方业务对接的效率，降低应用落地的复杂度。遵循标准化原则，要求在系统设计时数据模型及操作模型独立于系统实现，让数据“系于链却独于链”，可在链下被独立地验证和运用，更好地支持企业进行数据治理，提升区块链系统的灵活性和通用性。

## 设计核心

区块链的核心可以归结为两点：运用密码算法保障信息的完整性与不可否认性；在上述基础上，运用共识协议使信息复制保存到不同业务方，实现多方对信息共同背书。基于这两点可定义出区块链的5个核心部分：密码算法、共识协议、数据账本模型、数据存储、`API`（应用编程接口Application Programing Interface，以下简称`API`）。围绕总体设计原则，`JD Chain`的设计思路如下：

1. 密码算法 

密码算法的选择需要满足安全和合规的要求，同时面临源自实际业务场景的多样性要求。`JD Chain`在密码方面的关键任务是设计可插拔的密码框架，定义标准的`SPI`（服务提供者接口Service Provider Interface, 以下简称`SPI`）。系统默认支持国密算法以满足合规要求。基于密码SPI可以快速适配其它的密码算法实现，支持多密码体系。`JD Chain`将提供具有隐私保护功能密码算法和安全协议，来满足具体应用与业务的需求。

2. 共识协议 

共识协议的核心任务是保障区块链网络中有效节点的状态一致性。另外在选择共识协议时，还需要考虑业务场景中的安全性要求、时效性要求和节点规模等诸多因素。`JD Chain`在共识协议方面的关键任务是设计可插拔的共识框架，解耦共识协议与数据账本模型，定义标准的共识协议`SPI`，以满足业务场景的多样化需求。

3. 数据账本模型

数据账本的核心任务是对数据进行有效地组织和管理，因此，需要定义数据的结构和数据处理的操作模型。`JD Chain`的数据账本模型以“键值”结构来组织业务数据，定义标准的读写操作，记录数据变更历史，维护数据完整性与不可否认性，管理数据的存在性证明。

4. 数据存储

数据存储的核心任务是把数据账本高效地读写到持久化介质中。`JD Chain`把数据账本模型映射为“键值”结构，为数据的存储提供更好的伸缩性。另外，还定义了标准的持久化服务`SPI`，能够适配不同的数据库引擎，更好地复用企业现有的IT基础设施，满足企业的多样化需求。

5. API 

`JD Chain`的`API`设计需要提供标准化的操作接口，考虑通讯协议和编程语言的广泛性，支持端到端的离线密码计算，向企业提供更安全可信和易用的编程接口。

## 功能模块
JD Chain按功能层次分为4个部分：网关服务、共识服务、数据账本和工具包。![stru-1](http://ledger.jd.com/images/38d35f2fsys-stru-1.png)

1. 网关服务

`JD Chain`的网关服务是应用的接入层，提供终端接入、私钥托管、安全隐私和协议转换等功能。

终端接入是`JD Chain`网关的基本功能，在确认终端身份的同时提供连接节点、转发消息和隔离共识节点与客户端等服务。网关确认客户端的合法身份，接收并验证交易；网关根据初始配置文件与对应的共识节点建立连接，并转发交易数据。

私钥托管功能使共识节点可以将私钥等秘密信息以密文的形式托管在网关内，为有权限的共识节点提供私钥恢复、签名生成等服务。

安全隐私，一方面是网关借助具有隐私保护功能的密码算法和协议，来进行隐藏端到端身份信息，脱敏处理数据信息，防止无权限客户端访问数据信息等操作；另一方面，网关的隔离作用使外部实体无法干预内部共识过程，保证共识和业务之间的独立性。

网关中的协议转换功能提供了轻量化的HTTP Restful Service，能够适配区块链节点的`API`，实现各节点在不同协议之间的互操作。

数据浏览功能提供对链上数据可视化展示的能力。

2. 共识服务

共识服务是`JD Chain`的核心实现层，包括共识网络、身份管理、安全权限、交易处理、智能合约和数据检索等功能，来保证各节点间账本信息的一致性。

`JD Chain`的共识网络采用多种可插拔共识协议，并加以优化，来提供确定性交易执行、拜占庭容错和动态调整节点等功能，进而满足企业级应用场景需求。按照模块化的设计思路，将共识协议的各阶段进行封装，抽象出可扩展的接口，方便节点调用。共识节点之间使用`P2P`网络作为传输通道来执行共识协议。

身份管理功能使`JD Chain`网络能够通过公钥信息来辨识并认证节点，为访问控制、权限管理提供基础身份服务。

安全权限指的是，根据具体应用和业务场景，为节点设置多种权限形式，实现指定的安全管理，契合应用和业务场景。

交易处理是共识节点根据具体的协议来对交易信息进行排序、验证、共识和结块等处理操作，使全局共享相同的账本信息的功能。

智能合约是`JD Chain`中一种能够自动执行的链上编码逻辑，来更改账本和账户的状态信息。合约内容包括业务逻辑、节点的准入退出和系统配置的变更等。此外，`JD Chain`采用相应的合约引擎来保证智能合约能够安全高效地执行，降低开发难度并增加扩展性。开发者可以使用该合约引擎进行开发和测试，并通过接口进行部署和调用。

数据检索能够为协助节点检索接口，来查询区块、交易、合约、账本等相关信息。

3. 数据账本

数据账本为各参与方提供区块链底层服务功能，包括区块、账户、配置和存储等。

区块是`JD Chain`账本主要组成部分，包含交易信息和交易执行状态的数据快照哈希值，但不存储具体的交易操作和状态数据。`JD Chain`将账本状态和合约进行分离，并约束合约对账本状态的访问，来实现数据与逻辑分离，提供无状态逻辑抽象。

`JD Chain`通过细化账户分类、分级分类授权的方式，对区块链系统中的账户进行管理，达到逻辑清晰化、隔离业务和保护相关数据内容的目的。

配置文件包括密钥信息，存储信息以及共享的参与者身份信息等内容，使`JD Chain`系统中各节点能够执行诸如连接其他节点、验证信息、存储并更新账本等操作。

存储格式采用简洁的`KV`数据类型， 使用较为成熟的`NoSQL`数据库来实现账本的持久化存储，使区块链系统能够支持海量交易。

4. 工具包

节点可以使用`JD Chain`中提供的工具包获取上述三个层级的功能服务，并响应相关应用和业务。工具包贯穿整个区块链系统，使用者只需调用特定的接口即可使用对应工具。工具包包括数据管理、开发包（`SDK`）、安装部署和服务监控等。

上述三个功能层级都有对应的开发包，以接口形式提供给使用者，这些开发包包括密码算法、智能合约、数据检索的`SPI`等。

数据管理是对数据信息进行管理操作的工具包，这些管理操作包括备份、转移、导出、校验、回溯，以及多链情况下的数据合并、拆分等操作。

安装部署类工具包括密钥生成、数据存储等辅助功能，帮助各节点执行区块链系统。

服务监控工具能够帮组使用者获取即时吞吐量、节点状态、数据内容等系统运行信息，实现运维管理和实时监控。

## 部署模型

在企业的实际使用过程中，应用场景随着业务的不同往往是千差万别，不同的场景下如何选择部署模型，如何进行部署，往往是每个企业都会面临的实际问题。面对复杂多样的应用场景，`JD Chain`从易用性方面考虑，为企业应用提供了一套行之有效的部署模型解决方案。
![deploy](http://ledger.jd.com/images/825c0933locate-530.png)
`JD Chain`通过节点实现信息之间的交互，不同类型的节点可以在同一物理服务器上部署运行。`JD Chain`中定义了三种不同类型的节点：

客户端节点（`Client`）：通过`JD Chain`的`SDK`进行区块链操作的上层应用；
网关节点（`Gateway`）：提供网关服务的节点，用于连接客户端和共识节点；
共识节点（`Peer`）：共识协议参与方，会产生一致性账本。
不同企业规模的应用，部署方案会有较大区别，`JD Chain`根据实际应用的不同规模，提供了面向中小型企业和大型企业的两种不同部署模型。

1. 中小企业应用部署模型

`JD Chain`在不同的应用环境中可采用不同的部署模型。它的最简部署模型是`JD Chain`可正常运行的最低配置，在硬件条件满足的情况下，可以支持亿级交易，通常用于`Demo`实验或小型应用。

最简部署模型需要部署一个客户端节点、一个网关节点和多个共识节点（共识节点数量依赖于共识算法），如下所示：
![deploy-1](http://ledger.jd.com/images/c2b07193locate-521.png)

此外，`JD Chain`提供了数据服务功能作为可选项，通过安装数据服务组件可以进行数据的检索、汇总等功能。数据服务组件与共识节点部署在相同或不同服务器均可，由此最简部署可演化为如下图所示模型：
![deploy-2](http://ledger.jd.com/images/c3bfa6ablocate-522.png)

随着应用级别的提升，对数据存储需求越来越大，每个共识节点可采用数据库集群的存储方式实现存储的平行化扩展。在这种方式下，可支持交易级别达到十亿（乃至更多），如下图所示：
![deploy-3](http://ledger.jd.com/images/6881ea94locate-523.png)

在某些中型（乃至大型）实际应用中，共识节点会由不同的业务方安装部署，从安全性和扩展性角度考虑，更推荐使用共识节点集群：
![deploy-4](http://ledger.jd.com/images/63cf646flocate-524.png)

2. 大型企业应用部署模型

在一些大型的企业应用中，实际的业务方关系、应用场景将非常复杂。在这类应用中，可能由很多不同类型的参与方、不同类型的终端接入区块链网络，甚至这些终端可以从任意授权的网关节点接入，它们之间也会存在各种复杂的逻辑关系。 下图是一个比较常见的大型企业应用部署模型，在这个模型中涉及到多种类型的参与方，不同的终端，不同的接入方式。
![deploy-5](http://ledger.jd.com/images/a2c9069blocate-529.png)

## 交易流程
本文档描述了溯源数据状态转换及共享的交易机制。该方案包括四个参与方，`A`、`B`、`C`、`D`。他们每个参与方都有一部分基础溯源数据，换句话说，每个参与方对应了溯源数据的不同流程，但是有序的流程。基本的流程可以暂时认为包括：生产、加工、运输、收货。
![tx-1](http://ledger.jd.com/images/ec052606tx-1.png)

### 准备工作

1. 用户准备

每个参与方在实际的`JD Chain`使用过程中都对应了一个用户，参与方之间实际是通过对应用户进行交易处理。

`JD Chain`利用用户的公私钥来标识用户，使用者可通过公私钥工具（`keygen.sh`） 生成一个用户：
![tx-2](http://ledger.jd.com/images/b8d0437dtx-2.png)

2. 账本初始化

所有参与方根据安装部署脚本达成共识，建立一条专门用户溯源数据的状态转换和数据共享的链（一个账本）。

四个参与方分别对应了整个溯源的四个环节，分别按照`A -> B -> C -> D`顺序进行数据状态变更及数据更新，每个参与方依赖于前一个参与方的数据状态对其进行更新和共享。

### 交易过程

每个参与方在实际处理过程都需要使用`Client`作为交易处理的载体，每个`Client`中都有该参与方在该链对应的用户。

以参与方A为示例对交流过程进行说明。每个参与方都是在其他参与方交易基础上进行数据共享和状态转换，这得益于`K-V-Version`的存储方式，可以将状态以`Version`的方式标记。

1. 启动交易

参与方A通过`Client`发送A所持有的生产数据。`Client`为实际业务系统，通过调用`JD Chain`提供的`SDK`（目前支持`Java`）将某溯源产品的生产数据封装为交易。该交易附有业务系统的签名信息，`SDK`将该交易封装成正确的请求格式，通过`Http`将其发送至`Gateway`节点。
![tx-3](http://ledger.jd.com/images/fe8cd6e8tx-3.png)

2. 验证交易

`Gateway`节点收到`Client`发送的交易后，首先对交易进行验证（包括交易的格式及签名信息），验证通过后`Gateway`节点会在原交易基础上附加自己的签名信息，然后将交易发送至`Peer`节点群进行共识。
![tx-4](http://ledger.jd.com/images/cfeed468tx-4.png)

3. 交易共识

Peer节点群使用BFTSmart共识算法对提交的交易进行共识，该算法需要节点数为：`N = 3f + 1`（其中`N`为节点总数，`f`为可支持作恶节点数），因此共识过程至少需要`4`个节点参与。

`Peer`节点群对交易共识成功后会输出排序后的交易列表，该交易列表对于每个`Peer`节点而言是平等的，换句话说，每个`Peer`节点都会接收到完全一致的按顺序的交易列表。
![tx-5](http://ledger.jd.com/images/c57b9bcdtx-5.png)

4. 交易应答

`Peer`节点收到排序后交易后对其进行检查（包括合法性验证、验签等），检查通过后会根据实际结块规则生成区块并更新账本。然后将结块的信息作为应答发送至`Gateway`节点，进而发送至`Client`节点。
![tx-6](http://ledger.jd.com/images/4ebd5fdbtx-6.png)

### 时序图
一种事务流程如下：

1. `Client`调用`SDK`生成对应的交易请求，并通过`SDK`将该交易发送至`Gateway`（`HTTP`协议）。

2. `Gateway`会对交易进行合法性验证，并进行验签，然后使用自身节点的用户进行签名，再将交易以自定义协议（`TCP`）的方式发送至`Peer`节点群进行共识。

3. `Peer`节点群接收连续的交易并进行共识，共识通过后会输出排序后的交易列表至每个`Peer`节点。

4. 每个`Peer`节点对接收的交易列表进行合法性检查、验签，验证通过后会执行对应的交易并根据结块规则生成区块，更新账本。完成后会将应答（含结块信息）发送至对应`Gateway`，进而发送至`Client`。
![tx-7](http://ledger.jd.com/images/ce3576fctx-7.png)