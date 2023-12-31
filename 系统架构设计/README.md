# 高并发系统如何设计？

1. 需求分析

高并发系统设计的第一步是对业务需求进行全面分析和理解。这包括对系统的用户数量、并发请求量、请求类型和响应时间等方面的预估和测算。同时，需要对系统的扩展性需求和预算做出明确规划。只有深入了解需求，才能为后续的架构设计提供准确的依据。

2. 架构设计

分布式架构：高并发系统通常采用分布式架构，将系统拆分为多个子系统或模块，以实现负载均衡和高可用性。常见的分布式架构包括微服务架构和分布式消息队列。

异步处理：采用异步处理方式可以将用户请求与实际业务逻辑解耦，提高系统的响应速度和并发处理能力。消息队列是实现异步处理的常见工具。

 缓存机制：合理使用缓存可以大幅度减轻数据库压力，提高系统的读取性能。常见的缓存策略包括分布式缓存和页面静态化。

 3. 负载均衡

 负载均衡是保障高并发系统稳定性和性能的关键措施。通过将请求分发到不同的服务器节点，可以避免单点故障，并确保系统的资源利用率达到最优。常见的负载均衡策略包括轮询、最小连接数和基于性能的负载均衡算法。


4. 缓存策略

缓存是高并发系统中重要的性能优化手段。适当地引入缓存可以大幅度减少数据库的访问次数，降低系统响应时间。但缓存也带来了数据一致性和过期失效等问题，需要慎重设计和管理。常见的缓存策略有全局缓存、本地缓存和分布式缓存。


5. 合理的数据库设计

在高并发系统中，数据库的设计至关重要。合理的数据库选择、数据表设计、索引优化和读写分离等措施可以显著提高数据库的性能和稳定性。此外，采用分库分表和数据库主从复制等手段也是应对高并发访问的有效方法。

6. 故障容错

高并发系统在长时间运行中难免会遇到故障，如硬件故障、网络问题或程序错误。为了保障系统的稳定性和可用性，需要实施故障容错机制，包括监控系统、自动报警、自动恢复和灾备方案等。

7. 服务降级

熔断降级是保护系统的一种手段。在分布式系统中偶尔会出现某个基础服务不可用，最终导致整个系统不可用的情况, 这种现象被称为服务雪崩效应。
保证设计的系统能应对高并发场景，那肯定要考虑熔断降级。
可降级的多级读服务：比如服务调用降级为只读本地缓存、只读分布式缓存、只读默认降级数据

8. 限流

限流的目的是防止恶意请求流量、恶意攻击，或者防止流量超出系统峰值。


# 高并发系统如何限流

## 几大限流算法

1. 固定窗口算法

在一个固定长度的时间窗口内限制请求数量，每来一个请求，请求次数加一，如果请求数量超过最大限制，就拒绝该请求。

限流不够平滑， 无法处理窗口边界问题。


2. 滑动窗口算法

维护一个时间窗口，计算这个窗口内的请求数量。如果请求数量超过限制就拒绝请求，否则就处理请求，并记录请求的时间戳。另外还需要一个任务清理过期的时间戳。

解决了窗口边界问题，但限流不够平滑。

3. 漏桶算法

一个固定容量的漏桶，按照固定速率出水（处理请求）；
当流入水（请求数量）的速度过大会直接溢出（请求数量超过限制则直接拒绝）。
桶里的水（请求）不够则无法出水（桶内没有请求则不处理）。

平滑流量，但无法处理突发流量

4. 令牌桶算法

系统以固定的速率向桶中添加令牌；
当有请求到来时，会尝试从桶中移除一个令牌，如果桶中有足够的令牌，则请求可以被处理或数据包可以被发送；
如果桶中没有令牌，那么请求将被拒绝；
桶中的令牌数不能超过桶的容量，如果新生成的令牌超过了桶的容量，那么新的令牌会被丢弃。

可以处理突发流量，限制平均速率，但需要额外空间存储令牌，实现稍复杂。



# 微服务设计

微服务架构是一种软件设计模式，它将一个大型应用程序拆分成一组较小、相对独立的服务。每个服务都运行在其自己的进程中，与其他服务通信使用轻量级的通信机制（如HTTP API, GPRC）。这种模式使得开发人员能够更快、更可靠地构建和维护应用程序。

## 微服务设计原则

1. 单一服务内部功能高内聚低耦合

也就是说每个服务只完成自己职责内的任务，对于不是自己职责的功能交给其它服务来完成。

2. 闭包原则

微服务的闭包原则就是当我们需要改变一个微服务的时候，所有依赖都在这个微服务的组件内，不需要修改其他微服务。

3. 服务自治，接口隔离原则

尽量消除对其他服务的强依赖，这样可以降低沟通成本，提升服务稳定性。服务通过标准的接口隔离，隐藏内部实现细节。这使得服务可以独立开发、测试、部署、运行，以服务为单位持续交付。

4. 持续演进原则

非必要情况，应逐步划分，持续演进，避免服务数量的爆炸性增长

5. 拆分的过程避免影响产品的日常迭代

也就是说要一边做产品功能迭代，一边完成服务化拆分。比如优先剥离比较独立的边界服务（如短信服务等），从非核心的服务出发减少拆分对现有业务的影响

6. 服务接口的定义要具备可扩展性

服务拆分之后，由于服务是以独立进程的方式部署，所以服务之间通信就不再是进程内部的方法调用而是跨进程的网络通信了。在这种通信模型下服务接口的定义要具备可扩展性，否则在服务变更时会造成意想不到的错误。比如微服务的接口因为升级把之前的三个参数改成了四个，上线后导致调用方大量报错，推荐做法服务接口的参数类型最好是封装类，这样如果增加参数就不必变更接口的签名，而只需要在类中添加字段就可以了

7. 避免环形依赖与双向依赖

尽量不要有服务之间的环形依赖或双向依赖，原因是存在这种情况说明我们的功能边界没有化分清楚或者有通用的功能没有下沉下来。

8. 阶段性合并

随着你对业务领域理解的逐渐深入或者业务本身逻辑发生了比较大的变化，亦或者之前的拆分没有考虑的很清楚，导致拆分后的服务边界变得越来越混乱，这时就要重新梳理领域边界，不断纠正拆分的合理性。


### 弓箭原理

平衡微服务拆分粒度可以从两方面进行权衡，一是业务发展的复杂度，二是团队规模的人数。如上图，它就像弓箭一样，只有当业务复杂度和团队人数足够大的时候，射出的服务拆分粒度这把剑才会飞的更远，发挥出最大的威力。

### 三个火枪手原则

为什么说是三个人分配一个服务是比较理性的？而不是 4 个，也不是 2 个呢？

1. 首先，从系统规模来讲，3 个人负责开发一个系统，系统的复杂度刚好达到每个人都能全面理解整个系统，又能够进行分工的粒度；如果是 2 个人开发一个系统，系统的复杂度不够，开发人员可能觉得无法体现自己的技术实力；如果是 4 个甚至更多人开发一个系统，系统复杂度又会无法让开发人员对系统的细节都了解很深。

2. 其次，从团队管理来说，3 个人可以形成一个稳定的备份，即使 1 个人休假或者调配到其他系统，剩余 2 个人还可以支撑；如果是 2 个人，抽调 1 个后剩余的 1 个人压力很大；如果是 1 个人，这就是单点了，团队没有备份，某些情况下是很危险的，假如这个人休假了，系统出问题了怎么办？

3. 最后，从技术提升的角度来讲，3 个人的技术小组既能够形成有效的讨论，又能够快速达成一致意见；如果是 2 个人，可能会出现互相坚持自己的意见，或者 2 个人经验都不足导致设计缺陷；如果是 1 个人，由于没有人跟他进行技术讨论，很可能陷入思维盲区导致重大问题；如果是 4 个人或者更多，可能有的参与的人员并没有认真参与，只是完成任务而已。


### 微服务拆分策略有哪些？

#### 功能维度

功能维度主要是划分清楚业务边界

#### 非功能维度

- 扩展性

区分系统中变与不变的部分，不变的部分一般是成熟的、通用的服务功能，变的部分一般是改动比较多、满足业务迭代扩展性需要的功能，我们可以将不变的部分拆分出来，作为共用的服务，将变的部分独立出来满足个性化扩展需要。

- 复用性

不同的业务里或服务里经常会出现重复的功能，比如每个服务都有鉴权、限流、安全及日志监控等功能，可以将这些通过的功能拆分出来形成独立的服务，也就是微服务里面的 API 网关

- 高性能

将性能要求高或者性能压力大的模块拆分出来，避免性能压力大的服务影响其它服务。

- 高可用

将可靠性要求高的核心服务和可靠性要求低的非核心服务拆分开来，然后重点保证核心服务的高可用。




## 微服务设计方案

- 拆分服务：在设计微服务架构时，首先需要确定哪些功能应该被拆分成服务。这个决定应该基于不同的团队或开发者可以独立开发和维护的功能领域。

- API 设计：定义每个服务的 API 是至关重要的，因为这决定了如何与其他服务通信。API 应该是轻量级和易于使用的，最好是RESTful风格的。

- 数据管理：将数据管理作为一个服务也是很有用的。这个服务可以管理数据的存储和检索，并提供查询接口。这个服务可以被其他服务使用，以避免每个服务都要直接连接到数据库。

- 服务发现：微服务架构中的服务数量会很快增加，因此必须使用服务发现机制来管理这些服务。服务发现机制可以帮助服务找到其他服务，并根据需要建立连接。

- 负载均衡：由于服务数量的增加，负载均衡也变得很重要。负载均衡器可以根据负载情况将请求分配给多个服务实例，以提高应用程序的性能和可靠性。

- 安全性：在微服务架构中，每个服务都需要独立处理其安全性。这包括身份验证、授权和加密传输。

- 监控和日志记录：为了保持对微服务架构的控制，必须对每个服务进行监控和日志记录。这将帮助您了解服务的行为，并及时解决任何问题。
自动化部署：使用自动化部署工具可以极大地简化部署过程，并减少人为错误的可能性。


## 微服务好处

- 从开发视角来看：每个服务的功能更内聚，可以在微服务内设计和扩展功能，并且采用不同的开发语言及开发工具；

- 从运维视角来看：在微服务化后，每个服务都在独立的进程里，可以自运维；更重要的是，微服务化是单一变更的基础，迭代速度更快，上线风险更小；

- 从组织管理视角来看：将团队安装微服务切分成小组取代服务大组也有利于敏捷开发。

## 微服务缺点

- 服务数量变多，业务本身的规模和复杂度并没有变少，反而变多。

- 在分布式系统中，网络可靠性、通信安全、网络时延、网络拓扑变化等都成了我们要关注的内容。

- 微服务机制带来了大量的工作，比如服务如何请求目标服务，需要引入服务发现和负载均衡等，以及对跨进程的分布式调用栈进行分布式调用链路追踪


# 服务网格

业界比较认同的是William Morgan关于服务网关（Service Mesh）的一段定义，这里提取和解释该定义中的几个关键字来讲解服务网格的特点：

- 基础设施：服务网格是一种处理服务间通信的基础设施层。

- 云原生：服务网格尤其适用于在云原生场景下帮助应用程序在复杂的服务拓扑间可靠地传递请求。

- 网络代理：在实际使用用，服务网格一般是通过一组轻量级网络代理来执行治理逻辑的。

- 对应用透明：轻量网络代理与应用程序部署在一起，但应用感知不到代理的存在，还是使用原来的方式工作。


在云原生时代，随着采用各种语言开发的服务剧增，应用间的访问拓扑更加复杂，治理需求也越来越多。原来的那种嵌入在应用中的治理功能无论是从形态、动态性还是可扩展性来说都不能满足需求，迫切需要一种具备云原生动态、弹性特定的应用治理基础设施。

如果用一句话来解释什么是服务网格，可以将它比作是应用程序或者说微服务间的 TCP/IP，负责服务之间的网络调用、限流、熔断和监控。对于编写应用程序来说一般无须关心 TCP/IP 这一层（比如通过 HTTP 协议的 RESTful 应用），同样使用服务网格也就无须关系服务之间的那些原来是通过应用程序或者其他框架实现的事情，比如 Spring Cloud、OSS，现在只要交给服务网格就可以了。


## 服务网格特点

- 应用程序间通讯的中间层

- 轻量级网络代理

- 应用程序无感知

- 解耦应用程序的重试/超时、监控、追踪和服务发现


## SideCar

服务网格（Service Mesh）是处理服务间通信的基础设施层。它负责构成现代云原生应用程序的复杂服务拓扑来可靠地交付请求。在实践中，Service Mesh 通常以轻量级网络代理阵列的形式实现，这些代理与应用程序代码部署在一起，对应用程序来说无需感知代理的存在。

服务网格会在每个 pod 中注入一个 sidecar 代理，该代理对应用程序来说是透明，所有应用程序间的流量都会通过它，所以对应用程序流量的控制都可以在服务网格中实现。

SideCar 的主要职责就是负责各个微服务之间的通信，承载了原本上一代微服务架构中的服务发现、调用容错、服务治理等功能。使得微服务基础能力和业务逻辑迭代彻底解耦。


## 服务网格如何工作

1. 控制平面将整个网格中的服务配置推送到所有节点的 sidecar 代理中。  
2. Sidecar 代理将服务请求路由到目的地址，根据中的参数判断是到生产环境、测试环境还是 staging 环境中的服务（服务可能同时部署在这三个环境中），是路由到本地环境还是公有云环境？所有的这些路由信息可以动态配置，可以是全局配置也可以为某些服务单独配置。
3. 当 sidecar 确认了目的地址后，将流量发送到相应服务发现端点，在 Kubernetes 中是 service，然后 service 会将服务转发给后端的实例。
4. Sidecar 根据它观测到最近请求的延迟时间，选择出所有应用程序的实例中响应最快的实例。
5. Sidecar 将请求发送给该实例，同时记录响应类型和延迟数据。
6. 如果该实例挂了、不响应了或者进程不工作了，sidecar 将把请求发送到其他实例上重试。
7. 如果该实例持续返回 error，sidecar 会将该实例从负载均衡池中移除，稍后再周期性得重试。
8. 如果请求的截止时间已过，sidecar 主动失败该请求，而不是再次尝试添加负载。
7. Sidecar 以 metric 和分布式追踪的形式捕获上述行为的各个方面，这些追踪信息将发送到集中 metric 系统。



## lstio

Istio 是一个与Kubernetes紧密结合的适用于云原生场景的Service Mesh形态的用于服务治理的开放平台。

根据Istio官方的介绍，服务治理涉及连接（Connect）、安全（Secure）、策略执行（Control）和可观察性（Observe）。

1. 连接：Istio通过集中配置的流量规则控制服务间的流量和调用，实现负载均衡、熔断、故障注入、重试、重定向等服务治理功能。

2. 安全：Istio提供透明的认证机制、通道加密、服务访问授权等安全能力，可增强服务访问的安全性。

3. 策略执行：Istio通过可动态插拔、可扩展的策略实现访问控制、速率限制、配额管理、服务计费等能力。

4. 可观察性：动态获取服务运行数据和输出，提供强大的调用链、监控和调用日志收集输出的能力。配合可视化工具，可方便运维人员了解服务的运行状态，发现并解决问题。


## lstio 与 k8s

Kubernetes的Service基于每个Pod的kube-proxy从kube-apiserver上获取Service和Endpoint的信息，并将对Service的请求经过负载均衡转发到对应的Endpoint上。

但Kubernetes只提供了4层负载均衡能力，无法基于应用层的信息进行负载均衡，更不能提供应用层的流量管理，在服务运行管理上也只提供了基本的探针机制，并不能提供服务访问的指标和调用链追踪这种应用的服务运行诊断能力。

Istio复用了Kubernetes Service的定义，在实现上进行了更细粒度的控制。

Istio的服务发现就是从kube-apiserver中获取Service和Endpoint，然后将其转换成Istio服务模型的Service和ServiceInstance，但是其数据面组件不再是kube-proxy，而是在每个Pod里部署的Sidecar，也可以将其看作每个服务实例的Proxy。

通过拦截Pod的Inbound流量和Outbound流量，并在Sidecar上解析各种应用层协议，Istio可以提供真正的应用层治理、监控和安全等能力。

总之，Istio和Kubernetes从设计理念、使用体验、系统架构甚至代码风格等小细节来看，关系都非常紧密，甚至有人认为Istio就是Kubernetes团队开发的Kubernetes可插拔的增强特性。




# 库存扣减问题解决方案

1. 加锁 （悲观锁）将并发变成串行

2. 加锁（乐观锁） 扣减完成后检查

3. 消息队列

4. 重试幂等可以考虑加唯一标识token

## 基于mysql解决库存扣减的问题

1、用数据库扣减库存的方式，扣减库存的操作必须在一条语句中执行，不能先selec在update，这样在并发下会出现超扣的情况。（MySQL 默认RR隔离级别）

2、MySQL自身对于高并发的处理性能就会出现问题，一般来说，MySQL的处理性能会随着并发thread上升而上升，但是到了一定的并发度之后会出现明显的拐点，之后一路下降，最终甚至会比单thread的性能还要差。

3、当减库存和高并发碰到一起的时候，由于操作的库存数目在同一行，就会出现争抢InnoDB行锁的问题，导致出现互相等待甚至死锁，从而大大降低MySQL的处理性能，最终导致前端页面出现超时异常。

## 基于redis

### 库存放到缓存

将库存放到缓存，利用redis的incrby特性来扣减库存，解决了超扣和性能问题。但是一旦缓存丢失需要考虑恢复方案。

实现

1. 使用redis的lua脚本来实现扣减库存

2. 由于是分布式环境下所以还需要一个分布式锁来控制只能有一个服务去初始化库存

3. 需要提供一个回调函数，在初始化库存的时候去调用这个函数获取初始化库存


### 分布式锁（悲观锁）

设计一个分布式锁保证串行



# 支付系统如何解决重复支付的问题

我们可以在发起支付的时候，订单还在支付中的情况下，允许用户发起多笔支付，在支付回调的时候，检查用户是否已经有成功流水，对后来的流水进行退款处理。

也可以查询三方支付记录，对重复支付的用户进行手动退款处理。


# 设计一个秒杀抢购服务

## 页面静态化

活动页面是用户流量的第一入口，所以是并发量最大的地方。

可以将活动页面做成静态化页面， 内容固定，同时使用CDN(内容分发网络)保证用户访问的速度

## 秒杀按钮

在静态页面中如何控制秒杀按钮，只在秒杀时间点时才点亮呢？

为了性能考虑，一般会将css、js和图片等静态资源文件提前缓存到CDN上，让用户能够就近访问秒杀页面。

当秒杀开始的时候系统会生成一个新的js文件，此时标志为true，并且随机参数生成一个新值，然后同步给CDN。由于有了这个随机参数，CDN不会缓存数据，每次都能从CDN中获取最新的js代码。

前端还可以加一个定时器，控制比如：10秒之内，只允许发起一次请求。如果用户点击了一次秒杀按钮，则在10秒之内置灰，不允许再次点击，等到过了时间限制，又允许重新点击该按钮。


## 库存问题

### mysql扣减库存

基于数据库的乐观锁， 在扣减的时候判断库存是否大于0

但需要频繁访问数据库， 高并发的情况下可能会导致系统雪崩


### redis扣减库存

redis配合lua脚本保证原子性  

获取该商品id的库存，判断库存如果是-1，则直接返回，表示不限制库存。如果库存大于0，则扣减库存。如果库存等于0，是直接返回，表示库存不足。


## 分布式锁

在秒杀的时候，需要先从缓存中查商品是否存在，如果不存在，则会从数据库中查商品。如果数据库中，则将该商品放入缓存中，然后返回。如果数据库中没有，则直接返回失败。
大家试想一下，如果在高并发下，有大量的请求都去查一个缓存中不存在的商品，这些请求都会直接打到数据库。数据库由于承受不住压力，而直接挂掉。

可以加一个分布式锁

## mq异步处理

真实的秒杀场景中，有三个核心流程： 秒杀->下单->支付

而这三个核心流程中，真正并发量大的是秒杀功能，下单和支付功能实际并发量很小。所以，我们在设计秒杀系统时，有必要把下单和支付功能从秒杀的主流程中拆分出来，特别是下单功能要做成mq异步处理的。

### 消息丢失问题

在生产者发送mq消息之前，先把该条消息写入消息发送表，初始状态是待处理，然后再发送mq消息。消费者消费消息时，处理完业务逻辑之后，再回调生产者的一个接口，修改消息状态为已处理。

如果生产者把消息写入消息发送表之后，再发送mq消息到mq服务端的过程中失败了，造成了消息丢失。这时候，要如何处理呢？

答：使用job，增加重试机制。用job每隔一段时间去查询消息发送表中状态为待处理的数据，然后重新发送mq消息。

### 重复消费问题

本来消费者消费消息时，在ack应答的时候，如果网络超时，本身就可能会消费重复的消息。但由于消息发送者增加了重试机制，会导致消费者重复消息的概率增大。

那么，如何解决重复消息问题呢？

答：加一张消息处理表。

消费者读到消息之后，先判断一下消息处理表，是否存在该消息，如果存在，表示是重复消费，则直接返回。如果不存在，则进行下单操作，接着将该消息写入消息处理表中，再返回。

有个比较关键的点是：下单和写消息处理表，要放在同一个事务中，保证原子操作。

### 垃圾消息问题

由于某些原因，消息消费者下单一直失败，一直不能回调状态变更接口，这样job会不停的重试发消息。最后，会产生大量的垃圾消息。

每次在job重试时，需要先判断一下消息发送表中该消息的发送次数是否达到最大限制，如果达到了，则直接返回。如果没有达到，则将次数加1，然后发送消息。

这样如果出现异常，只会产生少量的垃圾消息，不会影响到正常的业务。

### 延迟消费问题

- 定时job

- 延迟队列

rocket mq自带延迟队列功能

## 如何限流

### 合法性限流

- 基于redis对用户限流

- 基于ip进行限流

- 对接口进行限流

- 加验证码

### 负载限流

- nginx限流

- 控制连接数

- 令牌桶限流

- 消息队列限流

- 缓存限流

- 做好监控， 服务降级






# 服务发现系统设计

## 服务间调用模式

### 客户端发现模式

由客户端负责向服务发现系统（可以认为是一个数据库，存储了所有服务提供者的所有节点位置信息）询问某个服务提供者的所有实例的 ip、port 信息，并采用某种负载均衡策略，直接发起对服务实例的访问。

这种模式去除了对中心化单点(API Gateway or Load Balancer)的依赖，可以避开单点造成的性能瓶颈与故障问题，同时由于负载均衡的逻辑在客户端，它可以根据自身的配置选择负载均衡算法，比如一致性 Hash 算法。不过这种模式也存在缺陷，由于客户端的负载均衡逻辑是分布式的，各自为政，没有全局统一视角，在某些情景下会因为客户端的高度竞争而导致后端服务提供者节点的负载不均衡。同时客户端的业务逻辑和服务发现的逻辑耦合在一起,不同的服务使用了不同的编程语言，那么就需要有不同语言的 SDK，如果未来某天服务发现的逻辑变更了，也需要重新发布所有的客户端节点。


### 服务端发现模式

把原本客户端执行的服务列表拉取 &负载均衡 &熔断 &故障转移这部分逻辑抽象变成一个专属的服务。不过跟传统的 load balancer 不大一样的地方是: 这个的 load balancer 会跟服务发现系统密切的配合，实时订阅服务发现系统中服务提供者节点列表信息，扮演反向代理的角色，将请求分发到合适的 Endpoint。


这种模式对于客户端来说是透明的，所有细节都被隔离在 load balancer 跟服务发现系统之间, 因此也沒有前面跨语言等相关问题，更新相关逻辑也只要統一部署 load balancer & service registry 就足够了。很明显，这种模式下服务的架构等于多了一层转发，延迟事件会增加；整个系统也多了一个故障点，整体系統的运维难度会提高；另外这个 load balancer 也可能会成为性能瓶颈。


## 健康检查模式

### 客户端心跳

客户端每隔一定时间主动发送“心跳”的方式来向服务端表明自己的服务状态正常，心跳可以是 TCP 的形式，也可以是 HTTP 的形式。

也可以通过维持客户端和服务端的一个 socket 长连接自己实现一个客户端心跳的方式。

但是客户端心跳中，长连接的维持和客户端的主动心跳都只是表明链路上的正常，不一定是服务状态正常。

### 服务端主动探测

服务端调用服务发布者某个 HTTP 接口来完成健康检查。

对于没有提供 HTTP 服务的 RPC 应用，服务端调用服务发布者的接口来完成健康检查。

可以通过执行某个脚本的形式来进行综合检查。


服务端主动调用服务进行健康检查是一个较为准确的方式，返回结果成功表明服务状态确实正常。但是服务端主动探测也存在问题。服务注册中心主动调用 RPC 服务的某个接口无法做到通用性；在很多场景下服务注册中心到服务发布者的网络是不通的，服务端无法主动发起健康检查，那么往往需要在宿主机器上部署一个 agent 来代替服务端的接口探测

服务端主动探测也存在判断异常的情况


## 消费端的订阅机制

- 定时轮询拉去注册表信息，保证注册表信息是最新的。

- push推送 （socket长连接的推送和http长轮询）

- 推拉结合， 定时长连接


## 容灾与高可用

### 服务端

服务节点信息原本是分布式存储的，少数节点挂了，不会影响整体可用性。

当大多数节点挂了的时候，如果是强一致的系统此时会进入只读不可写的模式（比如 Zookeeper 和开启了 stale read 的 consul。如果是最终一致的系统，此时客户端 sdk 会自动重试并切换到正常节点上去，读和写都不受影响。（缺少后括号，但不知道在哪加）。

当服务端所有的节点都挂了时候，此时需要服务端能够持久化存储之前注册的 Provider 节点信息，并在重启之后进入保护模式一段时间，在此期间先不剔除不健康的 Provider 节点（因为宕机过程中心跳没办法成功上报），否则可能会导致在一个 ttl 内大量 Provider 节点失效。

网络闪断保护，监测到大面积出现服务提供者节点心跳没有上报，则自动进入保护模式，该模式下不会剔除因为心跳上报失败的服务提供者节点


### 客户端

客户端 SDK 需要有不可用节点剔除能力，当服务端某个节点不可用的时候，能够立即切换到下一个节点尝试(切换的时候随机 sleep 0-3s 防止重试风暴打垮某个节点)。这里要注意客户端 SDK 每次请求的超时时间是否设置正确，我们发现部分服务发现官方 SDK 的默认超时时间过长，比如 java 的 consul sdk 中默认超时是 10 分钟，在生产实践中如果发生了网络闪断导致 response 包回不来就会导致 sdk 的心跳请求一致阻塞住，没办法进行下次的心跳上报，从而导致节点从注册中心中异常下线。

当所有的服务端节点都不可用的时候，SDK 能够使用内存中的缓存继续提供服务

如果客户端重启了，内存中的数据不存在了，则走本地配置降级。


# 如何保证系统的稳定性？

## 雪崩、熔断、降级

### 服务雪崩

在微服务中，假如一个或者多个服务出现故障，如果这时候，依赖的服务还在不断发起请求，或者重试，那么这些请求的压力会不断在下游堆积，导致下游服务的负载急剧增加。不断累计之下，可能会导致故障的进一步加剧，可能会导致级联式的失败，甚至导致整个系统崩溃，这就叫服务雪崩。


### 服务熔断

服务熔断是微服务架构中的容错机制，用于保护系统免受服务故障或异常的影响。当某个服务出现故障或异常时，服务熔断可以快速隔离该服务，确保系统稳定可用。
它通过监控服务的调用情况，当错误率或响应时间超过阈值时，触发熔断机制，后续请求将返回默认值或错误信息，避免资源浪费和系统崩溃。


### 服务降级

服务降级是也是一种微服务架构中的容错机制，用于在系统资源紧张或服务故障时保证核心功能的可用性。
当系统出现异常情况时，服务降级会主动屏蔽一些非核心或可选的功能，而只提供最基本的功能，以确保系统的稳定运行。通过减少对资源的依赖，服务降级可以保证系统的可用性和性能。






























