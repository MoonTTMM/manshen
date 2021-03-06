---
layout: post
title: Thinking in BlockChain
---

## 区块链的思考
### 基本概念
#### 什么是区块链
区块+链，先以比特币的区块链为例，区块即为一页账本，上面纪录了n条转账纪录，添加这页账目的时间戳，以及这页账本的唯一标志(hash)；链，则更加简单，这页账本上还纪录前一页的唯一标志，就形成了一个链。计算机数据结构上，这就很像一个链表。先不考虑所谓的分布式，这就是一个用链表作为数据结构的数据库。而所谓分布式也很容易理解，刚刚这样的账本在不同地方再给我来一百份，每个都存了一份完整的账本，其中一本账本变了，会向所有其他账本同步自己的变化。到此为止，区块链已经具有了分布式数据库的功能，可是和区块链最常被提到的去中心化和不可篡改，似乎还一点关系没有。（这里的分布式只是技术上的行为）
#### 不可篡改
这就需要提到区块链里面最重要的一个概念，共识机制。先抛开区块链，抛开技术，如果想让一个东西无法被篡改，简单来说就是让大家都知道，就好像所谓的曝光，本来悄咪咪的都好办，一曝光，那就板上钉钉，没发改了，这个就是我理解的共识。区块链就是用技术，在分布式的各个节点，实现这种共识。这个过程我把它分为：产生共识和传播共识。传播共识很好理解，我在原来的账本上加了一页记账纪录，要通过网络告诉所有其它账本。这里面有一个问题，现在网速再快，如果我想要给所有账本做这件同步的事情也是要有过程的。为了解决这个问题，区块链本身控制了产生共识的速度，在比特币的区块链实现里，规定了每10分钟才能产生一个区块（共识）。如果没有这个限制，整个网络里大家各顾各的记账，而有效的记账又必须接在之前的账本上，因为新的一页上必须要有前一页的唯一标志。那大家都忙于同步最新账本，可能根本顾不上记账了，或者存在太多的无效记账。所以共识（区块）的产生是被设立了门槛的，这一部分是区块链机制设计最巧妙的，但同时有趣的是也是区块链最被人诟病的瓶颈。说是瓶颈因为该不分导致的区块链TPS(transaction per second) 的限制，比特币10分钟一个区块，每个区块假设1000笔交易记录，那很容易算，每秒不到两笔交易吧。现在这个门槛的两种常见设计方式，POW（工作量证明）和POS（权益证明）。比特币用的是工作量证明，也就是大家最常听到的挖矿。简单来说就是在记账（产生区块）时，不是要有一个当前区块的唯一标志嘛，让这个标志要满足一定条件，很难被算出来。矿工们谁先算出这个区块，谁就获得一定量的奖励，同时这个区块也成功产生出来，并向整个网络传播。为什么无法篡改，也就落到这个共识机制上了。一旦一个区块产生，想要改变它，必须要在下一个区块产生前改掉它，否则该完它还要再去改刚生成的区块，子子孙孙无穷尽，就越差越多了。但整个网络上的算力都在努力算下一个区块，想要更快就要算力更强一点，也是所谓的51%。在一个参与者众多的生态里，这很难达到，尤其时这个社区越火，就越难达到。
#### 去中心化
我理解无法篡改其实很大程度上就已经是去中心化了，因为没有任何单一的机构或个体可以控制数据。为了保证可以实施，区块链项目应该是开源的，任何人可以拿这一部分代码，同步网络上的其它账本，然后加入。但这种去中心化并不是从头到尾的完全自治，在开始的时候依然是要有一个发起者，制定规则，但是一旦规则制定出来，加入的人越来越多，规则制定者是没有特权的。
#### 比特币
本身区块链可以说是比特币的底层技术，也是因为比特币而为大家所熟知，所以这里简单介绍一下。在比特币的实现里面，所有比特币最初都是挖矿产生，在创世区块里，中本聪自己挖出了50个比特币。这个比特币放在哪里呢？比特币钱包，通过非对称加密技术用来标示参与者的身份，这里不再介绍非对称加密技术。简单来说就是你有了一串字符（xxoo），作为你自己的代表，你还有一串密码，当你发起一笔 xxoo转给ooxx 50 个比特币，你用这串密码加密，确认这确实是由你发起的。钱包仅仅是有这一串字符和一个密码。至于怎么知道你有多少钱，其实是通过把所有之前的交易记录都遍历一遍，自然知道谁谁有多少钱。技术实现上，当然不会全部遍历，而是有优化的实现，但原理上就是这样。
### 区块链的意义
#### 虚拟货币的意义
现在比较流行的，比特币、以太币，我只说这两个有代表意义的。被炒出如此高的价格来，首先，一定是有泡沫的，其次，抛开泡沫和投机，它们本身也一定是有意义的，但是不一样的意义。比特币的意义，我理解它是有更多的机缘巧合的，最主要的是它是第一个出现的去中心化的虚拟货币，也就是第一个可以真正不需要中介，可以点对点转账的，比特币钱包，刚刚解释过，单纯是技术上实现的一对公私钥加密码，可以不具备任何现实世界中的意义。这本身就有足够的意义了，可以填补很多原来的空白，无论是黑市上的应用，还是跨国间的转账等等。而这也是从一些geek的使用慢慢发酵起来的，流传甚广的是早起比特币论坛上100个比特币买了一个披萨，据说是第一笔比特币和现实世界的交易。中本聪的那篇论文就叫做：一种点对点的电子现金系统，比特币本身是这片论文的一个验证实现。所以我认为比特币的意义更多还是在于其货币属性上，打通了原来的一些繁琐的价值交换通道。接下来想单纯像比特币这样发币，要么有国家在背后撑腰，要么是完全解决了比特币的几个瓶颈，要不我觉得是完全没有意义的。因为和现实世界的货币不同，虚拟货币没有界限，只是作为价值交换媒介又完全没有差异性，所以是一个赢者通吃的局面，一波投机过后，使用者完全没有动力去使用一个小众虚拟货币。
#### 区块链的意义
先举一些例子：基于区块链的不可篡改和去中心化，一些场景就可以用上了。显而易见的，跨国支付，可以解决现在银行、国家间的各种清洁算问题；水滴筹这种，必须经过一个并不了解的中介进行转账；互助保险，这里面就有智能合约的概念了；大选投票等等。但到此为止，大家能感觉到区块链有用，但也只是作为一种新技术新工具，而更高屋建瓴的首先要有一个大帽子，要不牛逼吹不响。或者说像刚刚那样case by case的吹肯定不行，要从方法论吹。
1. 解决了广义上的通过网络进行价值（权益）转移的问题。这种价值转移不是说比特币转账，而是任何价值（权益）。互联网解决的是信息的传递、共享，但没有解决权益转移，现在看起来移动支付很方便，但其实必须要有微信、支付宝这些中介；包括之前说的捐款、保险、投票等等，其实都是我们在转移我们自己的权益，而所有这些都要经过一个权威机构或中介。所以现在会有区块链+的说法，在区块链上建立应用，怎么建立应用呢？之前介绍了比特币，每个区块里都存了一堆转账记录，所以它是一个点对点现金转账系统，但很显然，区块里不只可以存转账记录，根据业务我们存其他的东西，用更丰富的数据结构，它就可以做其它事情了，可能还有小伙伴听过智能合约，这个听起来高级也没什么，区块里不但存了更发杂的数据结构，还存了些脚本，里面有一堆if-then，满足条件自动触发执行，就成了智能合约，再吹一吹一些书上就把它写成可编程社会了。它的工作模式大概是这样的，比如我们想要建立一个基于区块链的电子病历系统，所以每一个人的用药、病史都记录在案，这样没有某一个机构拥有这些数据，而是我们自己拥有属于我们的那份数据，但一旦写入我们也没发篡改了。让它无法篡改，我们需要矿工帮忙维持，矿工帮忙维持就可以获得token（也是我们说的虚拟货币，但这里我不准备用货币形容它，它只是一份证明，证明我为这个区块链系统工作下去出力了），这个区块链电子病历系统如果本身是有用的，解决了问题的，那么证明我为这个系统工作起来出过力的token也就有意义了。所以在这种模式下将不再是为了发币而发币。这就像发电一样，电本身没用还会电死人，但电支持了一个公司的运转，这个公司赚到钱，自然会去把电费交了。所以互联网会继续承担信息传递的功能，淘宝它还是个平台，上面大家继续分享各自货的信息；区块链承担价值传递的功能，不要在由淘宝担保，钱先付给它，它再转给店家，而是智能合约就把这件事做了。
2. 解决生产关系问题，生产力是生产中人与自然直接的关系，生产力在生产过程中人与人的关系。 马克思指出：“手推磨产生的是封建主为首的社会，蒸汽磨产生的是工业资本家为首的社会。”这个我觉得牛逼就吹大了，解决价值传递问题可以防止一些权威机构自己制定规则，同时执行规则；也可以减少一些交易成本。但生产关系是什么？老板他还是我老板。