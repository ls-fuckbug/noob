语言的流行通常需要一个杀手级的应用，Spring 就是 Java 生态的一个杀手级的应用框架。

# 什么是Spring全家桶？

当提到"Spring全家桶"时，通常指的是Spring框架及其相关项目的整体生态系统。
Spring全家桶包括了多个项目，每个项目都专注于不同的方面，共同提供全面的解决方案。

- Spring Framework（Spring框架）

Spring Framework 就是我们通常所说的 Spring 框架，它是一个软件设计架构层面的框架，为基于 Java 的企业级应用程序提供了一套标准流程和配置模型，可部署在任何类型的平台上。Spring 优势在于为开发者提供了应用级别的基础结构支持，实现应用层面的解耦合，允许开发者自主选择相关组件，开发者只需专注于业务逻辑的开发，不需要关注特定的部署环境。

- Spring Web MVC

Spring Web MVC（官方名称）就是我们通常所说的 Spring MVC，它是 Spring Framework 中的一个模块，是 Spring Framework 在 Web 领域实现 MVC 设计模式的具体方案，主要是基于 DispatcherServer 的前端路由处理和 ViewResolver 视图解析器来简化开发者的工作效率。


- Spring Boot

Spring Boot 是目前 Spring 全家桶系列中最流行的一个产品，在 Spring 官网的介绍排在第一位，可见对其重视程度，Spring 官方对 Spring Boot 的描述是“build anything”，翻译过来是构建任何事物，这样一个非常简单的描述将 Spring Boot 的特点展现的淋漓尽致，即通过 Spring Boot 可以快速构建一个基于 Spring 的独立生存级别的应用程序，开发者直接运行程序即可，无需处理各种繁琐的配置文件。简单理解，Spring Boot 就是为了让开发者快速启动和运行 Spring 应用程序而设计的。

- Spring Cloud

Spring 官方对 Spring Cloud 的描述是“coordinate anything”，翻译过来是协调任何事物，通过这个描述可以明确 Spring Cloud 并不是为了实现某个业务模块而存在的，它是一个集大成者，将分布式系统开发中常用的模块进行整合，如服务注册、服务发现、配置管理、熔断器、控制总线等，基于 Spring Boot 形成一套框架体系，开箱即用，使得开发者可以快速实现分布式、微服务应用。


- Spring Data

Spring Data 是 Spring 提供的持久层产品，主要功能是为应用程序中的数据访问提供统一的开发模型，同时保留不同数据存储的特殊性，并且这套开发模式是基于 Spring 的。根据不同类型的数据存储类型又可分为 Spring Data JDBC、Spring Data JPA、Spring Data Redis、Spring Data MongoDB 等，适用于关系型数据库和非关系型数据库。


- Spring Security

Spring Security 是 Spring 提供的一个功能强大的安全框架，为 Java 应用程序提供授权功能，通过定制身份验证来实现对于访问权限的控制，Spring Security 的特点在于扩展性好，可以根据具体的业务需求来实现定制验证服务。
 

# Spring Framework

我们一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发，比如说 Spring 支持 IoC（Inversion of Control:控制反转） 和 AOP(Aspect-Oriented Programming:面向切面编程)、可以很方便地对数据库进行访问、可以很方便地集成第三方组件（电子邮件，任务，调度，缓存等等）、对单元测试支持比较好、支持 RESTful Java 应用程序的开发。

## 组件

### Core Container

Spring 框架的核心模块，也可以说是基础模块，主要提供 IoC 依赖注入功能的支持。Spring 其他所有的功能基本都需要依赖于该模块，我们从上面那张 Spring 各个模块的依赖关系图就可以看出来。

spring-core：Spring 框架基本的核心工具类。
spring-beans：提供对 bean 的创建、配置和管理等功能的支持。
spring-context：提供对国际化、事件传播、资源加载等功能的支持。
spring-expression：提供对表达式语言（Spring Expression Language） SpEL 的支持，只依赖于 core 模块，不依赖于其他模块，可以单独使用。

### AOP

spring-aspects：该模块为与 AspectJ 的集成提供支持。  
spring-aop：提供了面向切面的编程实现。  
spring-instrument：提供了为 JVM 添加代理（agent）的功能。 具体来讲，它为 Tomcat 提供了一个织入代理，能够为 Tomcat 传递类文 件，就像这些文件是被类加载器加载的一样。没有理解也没关系，这个模块的使用场景非常有限。  

### Data Access/Integration

spring-jdbc：提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响。  
spring-tx：提供对事务的支持。  
spring-orm：提供对 Hibernate、JPA、iBatis 等 ORM 框架的支持。  
spring-oxm：提供一个抽象层支撑 OXM(Object-to-XML-Mapping)，例如：JAXB、Castor、XMLBeans、JiBX 和 XStream 等。  
spring-jms : 消息服务。自 Spring Framework 4.1 以后，它还提供了对 spring-messaging 模块的继承。  

### Spring Web

spring-web：对 Web 功能的实现提供一些最基础的支持。  
spring-webmvc：提供对 Spring MVC 的实现。  
spring-websocket：提供了对 WebSocket 的支持，WebSocket 可以让客户端和服务端进行双向通信。  
spring-webflux：提供对 WebFlux 的支持。WebFlux 是 Spring Framework 5.0 中引入的新的响应式框架。与 Spring MVC 不同，它不需要 Servlet API，是完全异步。  

### Messaging

spring-messaging 是从 Spring4.0 开始新加入的一个模块，主要职责是为 Spring 框架集成一些基础的报文传送应用。

### Spring Test

Spring 团队提倡测试驱动开发（TDD）。有了控制反转 (IoC)的帮助，单元测试和集成测试变得更简单。

Spring 的测试模块对 JUnit（单元测试框架）、TestNG（类似 JUnit）、Mockito（主要用来 Mock 对象）、PowerMock（解决 Mockito 的问题比如无法模拟 final, static， private 方法）等等常用的测试框架支持的都比较好。


# Spring, Spring MVC, Spring Boot之间什么关系？

Spring 包含了多个功能模块（上面刚刚提到过），其中最重要的是 Spring-Core（主要提供 IoC 依赖注入功能的支持） 模块， Spring 中的其他模块（比如 Spring MVC）的功能实现基本都需要依赖于该模块。

Spring MVC 是 Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力。MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

Spring 旨在简化 J2EE 企业应用程序开发。Spring Boot 旨在简化 Spring 开发（减少配置文件，开箱即用！）。
Spring Boot 只是简化了配置，如果你需要构建 MVC 架构的 Web 程序，你还是需要使用 Spring MVC 作为 MVC 框架，只是说 Spring Boot 帮你简化了 Spring MVC 的很多配置，真正做到开箱即用！


# Spring IOC

IoC（Inversion of Control:控制反转） 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。不过， IoC 并非 Spring 特有，在其他语言中也有应用。

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。

在实际项目中一个 Service 类可能依赖了很多其他的类，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IoC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。

在 Spring 中， IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value），Map 中存放的是各种对象。

Spring 时代我们一般通过 XML 文件来配置 Bean，后来开发人员觉得 XML 文件来配置不太好，于是 SpringBoot 注解配置就慢慢开始流行起来。




# Spring Bean

简单来说，Bean 代指的就是那些被 IoC 容器所管理的对象。

我们需要告诉 IoC 容器帮助我们管理哪些对象，这个是通过配置元数据来定义的。配置元数据可以是 XML 文件、注解或者 Java 配置类。

## 将一个类声明为 Bean 的注解有哪些?

- @Component：通用的注解，可标注任意类为 Spring 组件。如果一个 Bean 不知道属于哪个层，可以使用@Component 注解标注。
- @Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。
- @Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- @Controller : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。

## @Component 和 @Bean 的区别是什么？

@Component 注解作用于类，而@Bean注解作用于方法。

@Component通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean,@Bean告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。

@Bean 注解比 @Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。



# Spring AOP

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。



# Spring MVC

MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。


## 核心组件

- DispatcherServlet：核心的中央处理器，负责接收请求、分发，并给予客户端响应。

- HandlerMapping：处理器映射器，根据 URL 去匹配查找能处理的 Handler ，并会将请求涉及到的拦截器和 Handler 一起封装。

- HandlerAdapter：处理器适配器，根据 HandlerMapping 找到的 Handler ，适配执行对应的 Handler；

- Handler：请求处理器，处理实际请求的处理器。

- ViewResolver：视图解析器，根据 Handler 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 DispatcherServlet 响应客户端


## 工作原理

1. 客户端（浏览器）发送请求， DispatcherServlet拦截请求。
2. DispatcherServlet 根据请求信息调用 HandlerMapping 。HandlerMapping 根据 URL 去匹配查找能处理的 Handler（也就是我们平常说的 Controller 控制器） ，并会将请求涉及到的拦截器和 Handler 一起封装。
3. DispatcherServlet 调用 HandlerAdapter适配器执行 Handler 。
4. Handler 完成对用户请求的处理后，会返回一个 ModelAndView 对象给DispatcherServlet，ModelAndView 顾名思义，包含了数据模型以及相应的视图的信息。Model 是返回的数据对象，View 是个逻辑上的 View。
5. ViewResolver 会根据逻辑 View 查找实际的 View。
6. DispaterServlet 把返回的 Model 传给 View（视图渲染）。
7. 把 View 返回给请求者（浏览器）


## 统一异常处理怎么做？

推荐使用注解的方式统一异常处理，具体会使用到 @ControllerAdvice + @ExceptionHandler 这两个注解 。





