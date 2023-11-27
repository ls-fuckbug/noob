# maven  

Maven就是是专门为Java项目打造的管理和构建工具，它的主要功能有：  
- 提供了一套标准化的项目结构；  
- 提供了一套标准化的构建流程（编译，测试，打包，发布……）；  
- 提供了一套依赖管理机制。  

## pom

每一个 Maven 工程都有一个 pom.xml 文件，位于根目录中，包含项目构建生命周期的详细信息。通过 pom.xml 文件，我们可以定义项目的坐标、项目依赖、项目信息、插件信息等等配置。

## maven坐标

项目中依赖的第三方库以及插件可统称为构件。每一个构件都可以使用 Maven 坐标唯一标识，坐标元素包括：

- groupId(必须): 定义了当前 Maven 项目隶属的组织或公司。groupId 一般分为多段，通常情况下，第一段为域，第二段为公司名称。域又分为 org、com、cn 等，其中 org 为非营利组织，com 为商业组织，cn 表示中国。以 apache 开源社区的 tomcat 项目为例，这个项目的 groupId 是 org.apache，它的域是 org（因为 tomcat 是非营利项目），公司名称是 apache，artifactId 是 tomcat。

- artifactId(必须)：定义了当前 Maven 项目的名称，项目的唯一的标识符，对应项目根目录的名称。

- version(必须)：定义了 Maven 项目当前所处版本。

- packaging（可选）：定义了 Maven 项目的打包方式（比如 jar，war...），默认使用 jar。

- classifier(可选)：常用于区分从同一 POM 构建的具有不同内容的构件，可以是任意的字符串，附加在版本号之后。

只要你提供正确的坐标，就能从 Maven 仓库中找到相应的构件供我们使用。

## maven依赖

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

### 配置说明：

- dependencies：一个 pom.xml 文件中只能存在一个这样的标签，是用来管理依赖的总标签。  
- dependency：包含在 dependencies 标签中，可以有多个，每一个表示项目的一个依赖。  
- groupId,artifactId,version(必要)：依赖的基本坐标，对于任何一个依赖来说，基本坐标是最重要的，Maven 根据坐标才能找到需要的依赖。我们在上面解释过这些元素的具体意思，这里就不重复提了。  
- type(可选)：依赖的类型，对应于项目坐标定义的 packaging。大部分情况下，该元素不必声明，其默认值是 jar。
- scope(可选)：依赖的范围，默认值是 compile。  
- optional(可选)：标记依赖是否可选  
- exclusions(可选)：用来排除传递性依赖,例如 jar 包冲突  

### 依赖范围

classpath 用于指定 .class 文件存放的位置，类加载器会从该路径中加载所需的 .class 文件到内存中。


### maven标准目录结构

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






# anaconda

- 查看conda版本 conda --version  
- 更新conda版本 conda update conda  
- 查看都安装了那些依赖库 conda list  
- 创建新的python环境 conda create --name myenv  
- 并且还可以指定python的版本 conda create -n myenv python=3.7  
- 创建新环境并指定包含的库 conda create -n myenv scipy  
- 并且还可以指定库的版本 conda create -n myenv scipy=0.15.0  
- 复制环境 conda create --name myclone --clone myenv  
- 查看是不是复制成功了 conda info --envs  
- 激活、进入某个环境 source activate myenv  
- 退出环境 source deactivate  
- 删除环境 conda remove --name myenv --all  
- 查看当前的环境列表 conda info --envs or $ conda env list  
- 查看某个环境下安装的库 conda list -n myenv  
- 查找包 conda search XXX  
- 安装包 conda install XXX  
- 更新包 conda update XXX  
- 删除包 conda remove XXX  
- 安装到指定环境 conda install -n myenv XXX  
- 生成依赖文件  conda env export --name <环境名称> --file environment.yml  



