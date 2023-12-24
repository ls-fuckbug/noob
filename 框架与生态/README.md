# SSM

## 什么是SSM

SSM全称Spring+SpringMVC+MyBatis，是spring、spring MVC 、和mybatis框架的整合，为标准的MVC模式，是目前比较主流的Java EE企业级框架，适用于搭建各种大型的企业级应用系统。

## SSM框架

标准的SSM框架有四层，分别是dao（mapper）层，service层，controller层和View层。使用spring实现业务对象管理，使用spring MVC负责请求的转发和视图管理，mybatis作为数据对象的持久化引擎。


## SSM的组成

### Spring

Spring是一个开源的轻量级Java框架，它的主要目的是为了简化企业级Java应用的开发。Spring提供了诸如依赖注入（DI）和面向切面编程（AOP）等核心功能，以及在Web开发、安全性、数据访问等方面提供了丰富的支持。Spring 的核心是一个容器（IoC容器），它管理Java类之间的依赖关系和对象的生命周期。Spring 提供了各种集成包和插件，使其易于集成到其他开发框架中。

#### 优点

1. 低耦合：通过IoC容器实现对象的松散耦合，使代码更加灵活、可维护和可测试。

2. AOP支持：Spring对AOP的支持使得编写复杂的系统更加容易。

3. 事务支持：Spring的事务管理可以使得在使用Java数据库API时实现事务管理变得更加简单。

4. 面向切面编程：Spring提供了面向切面编程的支持，使开发人员能够对业务逻辑进行更好的组织和管理。

5. Web框架支持：Spring支持各种Web框架，如Struts、JSF和Spring MVC等，使得开发Web应用程序变得更加容易。

6. 易于集成：Spring框架易于与其他框架进行集成，例如Hibernate、MyBatis和Apache Struts等。


### Spring MVC

SpringMVC是Spring框架中的一个模块，它是一个Web框架，用于构建基于MVC模式的Web应用程序。它通过使用Java编程语言为Web应用程序提供了一种简单的方式，以便于在Web环境中使用Spring框架。

SpringMVC基于前端控制器模式设计，其中一个Servlet充当控制器，负责接收和处理客户端请求，并决定如何响应。SpringMVC提供了很多有用的功能，如：处理HTTP请求，将请求参数绑定到方法参数中，拦截请求和响应，使用视图技术渲染输出等。

SpringMVC使用注解驱动的方式，简化了开发人员的工作，提高了代码的可读性和可维护性。它还支持多种视图技术，如JSP、FreeMarker、Velocity等。


### MyBatis

MyBatis是一种开源的持久化框架，它通过XML或注解配置SQL映射，将Java对象映射到关系型数据库中的表。MyBatis可以将数据查询结果映射成Java对象，也可以将Java对象插入、更新、删除到数据库中。







