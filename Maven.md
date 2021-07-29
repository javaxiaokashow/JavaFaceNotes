## Maven

#### 1.Maven 是什么？

Maven 主要服务于基于 Java 平台的项目构建、依赖管理和项目信息管理。

Maven 的主要功能主要分为 5 点：

- 依赖管理系统
- 多模块构建
- 一致的项目结构
- 一致的构建模型和插件机制

#### 2.什么选用 Maven 进行构建？

- 首先，Maven 是一个优秀的项目构建工具。使用 maven，可以很方便的对项目进行分模块构建，这样在开发和测试打包部署时，效率会提高很多。
- 其次，Maven 可以进行依赖的管理。使用 Maven ，可以将不同系统的依赖进行统一管理，并且可以进行依赖之间的传递和继承。

#### 3. Maven 规约是什么？

- `/src/main/java/` ：Java 源码。
- `/src/main/resource` ：Java 配置文件，资源文件。
- `/src/test/java/` ：Java 测试代码。
- `/src/test/resource` ：Java 测试配置文件，资源文件。
- `/target` ：文件编译过程中生成的 `.class` 文件、jar、war 等等。
- `pom.xml` ：配置文件

Maven 要负责项目的自动化构建，以编译为例，Maven 要想自动进行编译，那么它必须知道 Java 的源文件保存在哪里，这样约定之后，不用我们手动指定位置，Maven 能知道位置，从而帮我们完成自动编译。

遵循 **“约定>>> 配置 >>> 编码”**。即能进行配置的不要去编码指定，能事先约定规则的不要去进行配置。这样既减轻了劳动力，也能防止出错。

#### 4.Maven 常用命令

- `mvn archetype：create` ：创建 Maven 项目。
- `mvn compile` ：编译源代码。
- `mvn deploy` ：发布项目。
- `mvn test-compile` ：编译测试源代码。
- `mvn test` ：运行应用程序中的单元测试。
- `mvn site` ：生成项目相关信息的网站。
- `mvn clean` ：清除项目目录中的生成结果。
- `mvn package` ：根据项目生成的 jar/war 等。
- `mvn install` ：在本地 Repository 中安装 jar 。
- `mvn eclipse:eclipse` ：生成 Eclipse 项目文件。
- `mvn jetty:run` 启动 Jetty 服务。
- `mvn tomcat:run` ：启动 Tomcat 服务。
- `mvn clean package -Dmaven.test.skip=true` ：清除以前的包后重新打包，跳过测试类。

用到最多的命令

- `mvn eclipse:clean` ：清除 Project 中以前的编译的东西，重新再来。
- `mvn eclipse:eclipse` ：开始编译 Maven 的 Project 。
- `mvn clean package` ：清除以前的包后重新打包。

#### 5.Maven 有哪些优点和缺点

##### 1）优点

- 简化了项目依赖管理。

  当年，多少人被 SSH 整合搞死搞活，很多时候，是因为依赖不完整，或者版本不正确。自从 Maven 出来后，终于可以无痛了~ 当然，也有一部分功劳是 Spring Boot ，这是后话。

- 易于上手，对于新手可能一个 `mvn clean package` 命令就可能满足我们的工作。

- 便于与持续集成工具 (Jenkins) 整合。

- 便于项目升级，无论是项目本身升级还是项目使用的依赖升级。

- 有助于多模块项目的开发，一个模块开发好后，发布到仓库，依赖该模块时可以直接从仓库更新，而不用自己去编译。

- Maven 有很多插件，便于功能扩展，比如生产站点，自动发布版本等。

##### 2）缺点

- Maven 是一个庞大的构建系统，学习难度大。

  这里的学习，更多指的完整学习。如果基本使用，并不会存在该问题。

- Maven 采用约定优于配置的策略 (convention over configuration)，虽然上手容易，但是一旦出了问题，难于调试。

  这个确实，略微痛苦。

- 当依赖很多时，m2eclipse 老是搞得 Eclipse 很卡。

  使用 IDEA ，而不是 Eclipse ，完美解决。

- 中国的网络环境差，很多 repository 无法访问，比如 Google Code、 JBoss 仓库无法访问等。

  这个也好解决，在 `<mirrors>` 中增加阿里巴巴的 Maven 私服，具体可以参见 [《提高 Maven 速度 —— Maven 仓库修改成国内阿里巴巴地址》](https://my.oschina.net/af8991/blog/833513) 文章。

#### 6.什么是Maven的坐标

Maven的坐标通过groupId，artifactId，version唯一标志一个构件。groupId通常为公司或组织名字，artifactId通常为项目名称，versionId为版本号。

#### 7.通过坐标如何定位地址

加上groupId为org.codehaus.mojo,artifactId为myproject，versionId为v1.0.0，则对应地址为：仓库目录（.m2）/org/codehaus/mojo/myproject/v1.0.0

#### 8.Maven的依赖范围有哪些（在scope中指定）

compile：默认范围，如果未指定任何范围，则使用该范围。编译依赖项在所有（编译，测试，运行）类路径中都可用。此外，这些依赖关系会传播到依赖的项目

provided：这很像compile，但表示您希望JDK或容器在运行时提供它。它只在编译和测试类路径上可用，不可传递。

runtime：此范围表示编译不需要依赖项，但需要执行依赖项。它在运行时和测试类路径中，但不在编译类路径中。（servlet-api）

test：表示应用程序的正常使用不需要依赖项，并且仅在测试编译和执行阶段可用。它不是传递的。（jdbc）

system：系统依赖范围。该依赖与三种classpath的关系和provided依赖范围完全一致。但是，使用system范围的依赖时必须通过systemPath元素显式地指定依赖文件的路径。由于此类依赖不是通过Maven仓库解析的，而且往往与本机系统绑定，可能造成构建的不可移植。

#### 9.Maven生命周期

　有三套什么周期，分别为clean，default，site

　　　clean：

　　　　此生命周期旨在给工程做清理工作，它主要包含以下阶段：

　　　　pre-clean - 执行项目清理前所需要的工作。

　　　　clean - 清理上一次build项目生成的文件。

　　　　post-clean - 执行完成项目清理所需的工作。

　　　default：

　　　　validate - 验证项目是否正确且所有必要的信息都可用。

　　　　initialize - 初始化构建工作，如：设置参数，创建目录等。

　　　　generate-sources - 为包含在编译范围内的代码生成源代码.

　　　　process-sources - 处理源代码, 如过滤值.

　　　　generate-resources -

　　　　process-resources - 复制并处理资源文件，至目标目录，准备打包。

　　　　compile - 编译项目中的源代码.

　　　　process-classes - 为编译生成的文件做后期工作, 例如做Java类的字节码增强.

　　　　generate-test-sources - 为编译内容生成测试源代码.

　　　　process-test-sources - 处理测试源代码。

　　　　generate-test-resources -

　　　　process-test-resources - 复制并处理资源文件，至目标测试目录。

　　　　test-compile - 将需测试源代码编译到路径。一般来说，是编译/src/test/java目录下的java文件至目标输出的测试classpath目录中。

　　　　process-test-classes -

　　　　test - 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。

　　　　prepare-package -

　　　　package - 接受编译好的代码，打包成可发布的格式，如 JAR 。

　　　　pre-integration-test -

　　　　integration-test - 按需求将发布包部署到运行环境。

　　　　post-integration-test -

　　　　verify -

　　　　install -将包安装到本地仓库，给其他本地引用提供依赖。

　　　　deploy -完成集成和发布工作，将最终包复制到远程仓库以便分享给其他开发人员。

　　　site：

　　　　pre-site - 执行一些生成项目站点前的准备工作。

　　　　site - 生成项目站点的文档。

　　　　post-site - 执行需完成站点生成的工作，如站点部署的准备工作。

　　　　site-deploy - 向制定的web服务器部署站点生成文件。

#### 10.Maven命令

　mvn archetype:generate 创建Maven项目

　　　mvn compile 编译源代码

　　　mvn deploy 发布项目

　　　mvn test-compile 编译测试源代码

　　　mvn test 运行应用程序中的单元测试

　　　mvn site 生成项目相关信息的网站

　　　mvn clean 清除项目目录中的生成结果

　　　mvn package 根据项目生成的jar

　　　mvn install 在本地Repository中安装jar

　　　mvn eclipse:eclipse 生成eclipse项目文件

　　　mvn[jetty](https://baike.baidu.com/item/jetty):run 启动jetty服务

　　　mvn[tomcat](https://baike.baidu.com/item/tomcat):run 启动tomcat服务

　　　mvn clean package -Dmaven.test.skip=true:清除以前的包后重新打包，跳过测试

#### 11.依赖的解析机制

当依赖的范围是 system 的时候，Maven 直接从本地文件系统中解析构件。

根据依赖坐标计算仓库路径，尝试直接从本地仓库寻找构件，如果发现对应的构件，就解析成功。

如果在本地仓库不存在相应的构件，就遍历所有的远程仓库，发现后，下载并解析使用。

如果依赖的版本是 RELEASE 或 LATEST，就基于更新策略读取所有远程仓库的元数据文件（groupId/artifactId/maven-metadata.xml），将其与本地仓库的对应元合并后，计算出 RELEASE 或者 LATEST 真实的值，然后基于该值检查本地仓库，或者从远程仓库下载。

如果依赖的版本是 SNAPSHOT，就基于更新策略读取所有远程仓库的元数据文件，将它与本地仓库对应的元数据合并，得到最新快照版本的值，然后根据该值检查本地仓库，或从远程仓库下载。

如果最后解析得到的构件版本包含有时间戳，先将该文件下载下来，再将文件名中时间戳信息删除，剩下 SNAPSHOT 并使用（以非时间戳的形式使用）。

#### 12.创建Maven的普通Java项目

  mvn archetype:create -DgroupId=packageName -DartifactId=projectName

#### 13.创建 Maven 的 Web 项目

 mvn archetype:create -DgroupId=packageName -DartifactId=webappName -DarchetypeArtifactId=maven-archetype-webapp

#### 14.反向生成 maven 项目的骨架

  mvn artifacttype:generate

#### 15.编译源代码

  mvn compile

#### 16.编译测试代码

  mvn test-compile

#### 17.运行测试

  mvn test

#### 18.产生 site

  mvn site

#### 19.打包

  mvn package

#### 20.在本地 Repository 中安装 jar

  mvn install（例：installing D:\xxx\xx.jar to D:\xx\xxxx）

#### 21.清除产生的项目

  mvn clean

#### 22.生成 Eclipse 项目/idea项目

eclipse项目 

 mvn eclipse:eclipse

 idea 项目

 mvn idea:idea

#### 23.组合使用 goal 命令，如只打包不测试

  mvn -Dtest package

#### 24.编译测试的内容

  mvn test-compile

#### 25.只打 jar 包

  mvn jar:jar

#### 26.只测试而不编译，也不测试编译

  mvn test -skipping compile -skipping test-compile

#### 27.清除 eclipse 的一些系统设置

  mvn eclipse:clean

#### 28.查找当前项目已被解析的依赖

  mvn dependency:list

#### 29.上传到私服

  mvn deploy

#### 30.强制检查更新，由于快照版本的更新策略（一天更新几次、隔断时间更新一次）存在，如果想强制更新就会用到此命令

  mvn clean install-U

#### 31.源码打包

  mvn source:jar

  或

  mvn source:jar-no-fork

#### 参考

https://blog.csdn.net/a303549861/article/details/93752178

https://www.cnblogs.com/lin0/p/14153982.html
