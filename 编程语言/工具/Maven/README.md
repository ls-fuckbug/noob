# maven  

Maven就是是专门为Java项目打造的管理和构建工具，它的主要功能有：  
- 提供了一套标准化的项目结构；  
- 提供了一套标准化的构建流程（编译，测试，打包，发布……）；  
- 提供了一套依赖管理机制。  

Maven可以把你从繁琐工作中解放出来，能帮你构建工程，管理jar包，编译代码，还能帮你自动运行单元测试，打包，生成报表，甚至能帮你部署项目，生成Web站点（Maven的一键构成）

## 背景

Maven 的一个核心特性就是依赖管理。当我们涉及到多模块的项目（包含成百个模块或者子项目），管理依赖就变成一项困难的任务。Maven 展示出了它对处理这种情形的高度控制。

传统的 WEB 项目中，我们必须将工程所依赖的 jar 包复制到工程中，导致了工程的变得很大。

而maven工程中不直接将jar包导入到工程中，而是通过在pom.xml文件中添加所需jar 包的坐标，这样就很好的避免了jar直接引入进来，在需要用到jar包的时候，只要查找 pom.xml文件，再通过pom.xml文件中的坐标，到一个专门用于”存放jar包的仓库”(maven仓库)中根据坐标从而找到这些jar包，再把这些jar包拿去运行。

## maven仓库

- 本地仓库：用来存储从远程仓库或中央仓库下载的插件和jar包，项目使用一些插件或jar包，优先从本地仓库查找

- 远程仓库【私服】：如果本地需要插件或者jar包，本地仓库没有，默认去远程仓库下载。远程仓库可以在互联网内也可以在局域网内。远程仓库可以从本地上传jar包，也可以从中央仓库里下载。

- 中央仓库：在maven软件中内置一个远程仓库地 http://repo1.maven.org/maven2 ，它是中央仓库，服务于整个互联网，它是由 Maven 团队自己维护，里面存储了非常全的jar包，它包含了世界上大部分流行的开源项目构件。

### 使用顺序

1. 在默认情况下启动一个maven工程会从本地仓库找jar包，如果本地没有，会从中央仓库中下载jar包。

2. 在公司中，启动一个maven工程，会从本地仓库找jar包，如果本地没有，回去私服（远程仓库）下载jar包。如果私服没有，可以从中央仓库下载，也可以从本地上传。


## pom

每一个 Maven 工程都有一个 pom.xml 文件，位于根目录中，包含项目构建生命周期的详细信息。通过 pom.xml 文件，我们可以定义项目的坐标、项目依赖、项目信息、插件信息等等配置。

### maven坐标

项目中依赖的第三方库以及插件可统称为构件。每一个构件都可以使用 Maven 坐标唯一标识，坐标元素包括：

- groupId(必须): 定义了当前 Maven 项目隶属的组织或公司。groupId 一般分为多段，通常情况下，第一段为域，第二段为公司名称。域又分为 org、com、cn 等，其中 org 为非营利组织，com 为商业组织，cn 表示中国。以 apache 开源社区的 tomcat 项目为例，这个项目的 groupId 是 org.apache，它的域是 org（因为 tomcat 是非营利项目），公司名称是 apache，artifactId 是 tomcat。

- artifactId(必须)：定义了当前 Maven 项目的名称，项目的唯一的标识符，对应项目根目录的名称。

- version(必须)：定义了 Maven 项目当前所处版本。

- packaging（可选）：定义了 Maven 项目的打包方式（比如 jar，war...），默认使用 jar。

- classifier(可选)：常用于区分从同一 POM 构建的具有不同内容的构件，可以是任意的字符串，附加在版本号之后。

只要你提供正确的坐标，就能从 Maven 仓库中找到相应的构件供我们使用。

### properties

用来定义一些配置属性。

### maven依赖 depencies

如果使用 Maven 构建产生的构件（例如 Jar 文件）被其他的项目引用，那么该构件就是其他项目的依赖。

```
<project>
    <dependencies>
        <dependency>
            <groupId></groupId>
            <artifactId></artifactId>
            <version></version>
            <type>...</type>
            <scope>...</scope>
            <optional>...</optional>
            <exclusions>
                <exclusion>
                  <groupId>...</groupId>
                  <artifactId>...</artifactId>
                </exclusion>
          </exclusions>
        </dependency>
      </dependencies>
</project>
```

#### 配置说明：

- dependencies：一个 pom.xml 文件中只能存在一个这样的标签，是用来管理依赖的总标签。  
- dependency：包含在 dependencies 标签中，可以有多个，每一个表示项目的一个依赖。  
- groupId,artifactId,version(必要)：依赖的基本坐标，对于任何一个依赖来说，基本坐标是最重要的，Maven 根据坐标才能找到需要的依赖。我们在上面解释过这些元素的具体意思，这里就不重复提了。  
- type(可选)：依赖的类型，对应于项目坐标定义的 packaging。大部分情况下，该元素不必声明，其默认值是 jar。
- scope(可选)：依赖的范围，默认值是 compile。  
- optional(可选)：标记依赖是否可选  
- exclusions(可选)：用来排除传递性依赖,例如 jar 包冲突  


#### 依赖传递

Maven 的依赖传递机制是指：不管 Maven 项目存在多少间接依赖，POM 中都只需要定义其直接依赖，不必定义任何间接依赖，Maven 会动读取当前项目各个直接依赖的 POM，将那些必要的间接依赖以传递性依赖的形式引入到当前项目中。Maven 的依赖传递机制能够帮助用户一定程度上简化 POM 的配置。

#### 依赖调节

依赖调节遵循以下两条原则：
1. 引入路径短者优先  
2. 先声明者优先  

#### 排除依赖 exclusion

关于 exclusions 元素及排除依赖说明如下：
- 排除依赖是控制当前项目是否使用其直接依赖传递下来的接间依赖；
- exclusions 元素下可以包含若干个 exclusion 子元素，用于排除若干个间接依赖；
- exclusion 元素用来设置具体排除的间接依赖，该元素包含两个子元素：groupId 和 artifactId，用来确定需要排除的间接依赖的坐标信息；
- exclusion 元素中只需要设置 groupId 和 artifactId 就可以确定需要排除的依赖，无需指定版本 version。

#### 可选依赖 optional

关于 optional 元素及可选依赖说明如下：

- 可选依赖用来控制当前依赖是否向下传递成为间接依赖；
- optional 默认值为 false，表示可以向下传递称为间接依赖；
- 若 optional 元素取值为 true，则表示当前依赖不能向下传递成为间接依赖。

#### 本地依赖

我们知道，Maven 是通过仓库对依赖进行管理的，当 Maven 项目需要某个依赖时，只要其 POM 中声明了依赖的坐标信息，Maven 就会自动从仓库中去下载该构件使用。但在实际的开发过程中，经常会遇到一种情况：某一个项目需要依赖于存储在本地的某个 jar 包，该 jar 包无法从任何仓库中下载的，这种依赖被称为外部依赖或本地依赖。那么这种依赖是如何声明的呢？

- scope 表示依赖范围，这里取值必须是 system，即系统。
- systemPath 表示依赖的本地构件的位置。


### maven插件 plugins

Maven 实际上是一个依赖插件执行的框架，它执行的每个任务实际上都由插件完成的。Maven 的核心发布包中并不包含任何 Maven 插件，它们以独立构件的形式存在， 只有在 Maven 需要使用某个插件时，才会去仓库中下载。



## SNAPSHOT

SNAPSHOT（快照）是一种特殊的版本，它表示当前开发进度的副本。与常规版本不同，快照版本的构件在发布时，Maven 会自动为它打上一个时间戳，有了这个时间戳后，当依赖该构件的项目进行构建时，Maven 就能从仓库中找到最新的 SNAPSHOT 版本文件。


## 继承

Maven 在设计时，借鉴了 Java 面向对象中的继承思想，提出了 POM 继承思想。

当一个项目包含多个模块时，可以在该项目中再创建一个父模块，并在其 POM 中声明依赖，其他模块的 POM 可通过继承父模块的 POM 来获得对相关依赖的声明。对于父模块而言，其目的是为了消除子模块 POM 中的重复配置，其中不包含有任何实际代码，因此父模块 POM 的打包类型（packaging）必须是 pom。

## depencyManagement

我们知道，子模块可以通过继承获得父模块中声明的全部依赖，这样虽然避免了在各个子模块 POM 中重复进行依赖声明，但也极有可能造成子模块中引入一些不必要的依赖。为此 Maven 引入了 dependencyManagement 来对依赖进行管理。

Maven 可以通过 dependencyManagement 元素对依赖进行管理，它具有以下 2 大特性：
1. 在该元素下声明的依赖不会实际引入到模块中，只有在 dependencies 元素下同样声明了该依赖，才会引入到模块中。
2. 该元素能够约束 dependencies 下依赖的使用，即 dependencies 声明的依赖若未指定版本，则使用 dependencyManagement 中指定的版本，否则将覆盖 dependencyManagement 中的版本。

由于 dependencyManagement 元素是可以被继承的，因此我们可以在父模块 POM 中使用 dependencyManagement 元素声明所有子模块的依赖，然后在各个子模块 POM 使用 dependencies 元素声明实际用到的依赖即可。这样既可以让子模块能够继承父模块的依赖配置，还能避免将不必要的依赖引入到子模块中。

## pluginManagement

pluginManagement 元素与 dependencyManagement 元素的原理十分相似，在 pluginManagement 元素中可以声明插件及插件配置，但不会发生实际的插件调用行为，只有在 POM 中配置了真正的 plugin 元素，且其 groupId 和 artifactId 与 pluginManagement 元素中配置的插件匹配时，pluginManagement 元素的配置才会影响到实际的插件行为。


## Profile

一个项目通常都会有多个不同的运行环境，例如开发环境，测试环境、生产环境等。而不同环境的构建过程很可能是不同的，例如数据源配置、插件、以及依赖的版本等。每次将项目部署到不同的环境时，都需要修改相应的配置，这样重复的工作，不仅浪费劳动力，还容易出错。为了解决这一问题，Maven 引入了 Profile 的概念，通过它可以为不同的环境定制不同的构建过程。

Maven 通过 profiles 元素来声明一组 Profile 配置，该元素下可以包含多个 profile 子元素，每个 profile 元素表示一个 Profile 配置。每个 profile 元素中通常都要包含一个 id 子元素，该元素是调用当前 Profile 的标识。




## maven标准目录结构

```
src /
  main /
    java/
    resources/
  test/ java
     /
    resources/
pom.xml

```



## command

mvn clean : 清空target目录下的内容  
mvn compile: 编译、生成target目录并将.java文件编译成.class文件
mav package: 对项目、工程、模块进行打包操作，打包后位置为 target 目录下  
mvn test：maven工程的测试命令，执行src/test/java下的单元测试类。当使用mvn test命令时，也同时会执行compile命令  
mvn install：maven工程的安装命令，执行install将maven打成jar包或war包发布到本地仓库。install命令首先会将compile、test、package全部执行，再将该maven发布到本地仓库中  
mvn deploy 部署，会保存到私服仓库中，并自动将项目部署到web容器中  




