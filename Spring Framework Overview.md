# Spring Framework Overview 概述  
Version 5.1.4.RELEASE
---
Spring makes it easy to create Java enterprise applications. It provides everything you need to embrace the Java language in an enterprise environment, with support for Groovy and Kotlin as alternative languages on the JVM, and with the flexibility to create many kinds of architectures depending on an application’s needs. As of Spring Framework 5.0, Spring requires JDK 8+ (Java SE 8+) and provides out-of-the-box support for JDK 9 already.

Spring 可以轻松创建 Java 企业应用程序。它提供了 Java 语言在企业环境中所需的一切，并支持 Groovy 和 Kotlin 作为 JVM 上的替代语言。Spring 可根据应用程序的需要灵活地创建多种体系结构。从 Spring Framework 5.0 开始，Spring需要 JDK 8+（Java SE 8+），并且已经为 JDK 9 提供了开箱即用的支持。

Spring supports a wide range of application scenarios. In a large enterprise, applications often exist for a long time and have to run on a JDK and application server whose upgrade cycle is beyond developer control. Others may run as a single jar with the server embedded, possibly in a cloud environment. Yet others may be standalone applications (such as batch or integration workloads) that do not need a server.

Spring 支持广泛的应用场景。在大型企业中，应用程序通常已经存在很长时间，并且必须在升级周期超出开发人员控制范围的 JDK 和应用程序服务器上运行。其他人可能在嵌入服务器的情况下作为单个 jar 运行，也可能在云环境中运行。还有一些可能是不需要服务器的独立应用程序（例如批处理或集成工作负载）。

Spring is open source. It has a large and active community that provides continuous feedback based on a diverse range of real-world use cases. This has helped Spring to successfully evolve over a very long time.

Spring 是开源的。它拥有一个庞大而活跃的社区，可根据各种各样的实际用例提供持续的反馈。这有助于 Spring 在很长一段时间内成功发展。


## 1. What We Mean by "Spring" “Spring”的含义

The term "Spring" means different things in different contexts. It can be used to refer to the Spring Framework project itself, which is where it all started. Over time, other Spring projects have been built on top of the Spring Framework. Most often, when people say "Spring", they mean the entire family of projects. This reference documentation focuses on the foundation: the Spring Framework itself.

"Spring" 在不同的背景下意味着不同的东西。它可以用来表示 Spring Framework 项目本身，这是一切开始的地方。随着时间的推移，其他 Spring 项目已经构建在 Spring Framework 之上。大多数情况下，当人们说起 “Spring” 时，他们意味着整个项目系列。而本参考文档更侧重于 Spring Framework 本身。

The Spring Framework is divided into modules. Applications can choose which modules they need. At the heart are the modules of the core container, including a configuration model and a dependency injection mechanism. Beyond that, the Spring Framework provides foundational support for different application architectures, including messaging, transactional data and persistence, and web. It also includes the Servlet-based Spring MVC web framework and, in parallel, the Spring WebFlux reactive web framework.

Spring Framework 分为几个模块。应用程序可以选择所需的模块。Spring Framework 的核心是核心容器的模块（ Core ），包括配置模型和依赖注入机制。除此之外，Spring Framework 还为不同的应用程序体系结构提供了基础支持，包括消息传递，事务数据和持久性以及Web。它还包括基于 Servlet 的 Spring MVC Web 框架，以及 Spring WebFlux 响应式 Web 框架。

A note about modules: Spring’s framework jars allow for deployment to JDK 9’s module path ("Jigsaw"). For use in Jigsaw-enabled applications, the Spring Framework 5 jars come with "Automatic-Module-Name" manifest entries which define stable language-level module names ("spring.core", "spring.context" etc) independent from jar artifact names (the jars follow the same naming pattern with "-" instead of ".", e.g. "spring-core" and "spring-context"). Of course, Spring’s framework jars keep working fine on the classpath on both JDK 8 and 9.

关于模块的说明：Spring Framework 的 jar 允许部署到 JDK 9 的模块路径（“Jigsaw”）。为了在支持 Jigsaw的应用程序中使用，Spring Framework 5 jar带有 “Automatic-Module-Name” ，它们定义了独立于 jar 工件的稳定语言级模块名称（“spring.core”，“spring.context”等）名称（ jar 使用相同的命名模式“-”，而不是“.”，例如“spring-core”和“spring-context”）。当然，Spring Framework 的 jar 在 JDK 8 和 9 上的 classpath 路径上都能正常工作。  


## 2. History of Spring and the Spring Framework Spring 和 Spring Framework 的历史

Spring came into being in 2003 as a response to the complexity of the early J2EE specifications. While some consider Java EE and Spring to be in competition, Spring is, in fact, complementary to Java EE. The Spring programming model does not embrace the Java EE platform specification; rather, it integrates with carefully selected individual specifications from the EE umbrella:  

Spring诞生于2003年，是对早期 J2EE 规范复杂性的回应。虽然有些人认为 Java EE 和 Spring 处于竞争中，但 Spring 实际上是对 Java EE 的补充。 Spring 编程模型不包含 Java EE 平台规范;相反，它集成了 从 Java EE 中精心挑选的部分规范：  

Servlet API (JSR 340)

WebSocket API (JSR 356)

Concurrency Utilities (JSR 236)

JSON Binding API (JSR 367)

Bean Validation (JSR 303)

JPA (JSR 338)

JMS (JSR 914)

as well as JTA/JCA setups for transaction coordination, if necessary.

以及必要时用于事务协调的JTA / JCA设置。

The Spring Framework also supports the Dependency Injection (JSR 330) and Common Annotations (JSR 250) specifications, which application developers may choose to use instead of the Spring-specific mechanisms provided by the Spring Framework.

Spring Framework还支持依赖注入（DI）（JSR 330）和 Common Annotations（JSR 250）规范，应用程序开发人员可以选择使用这些规范，而不是Spring Framework提供的Spring特定机制。

As of Spring Framework 5.0, Spring requires the Java EE 7 level (e.g. Servlet 3.1+, JPA 2.1+) as a minimum - while at the same time providing out-of-the-box integration with newer APIs at the Java EE 8 level (e.g. Servlet 4.0, JSON Binding API) when encountered at runtime. This keeps Spring fully compatible with e.g. Tomcat 8 and 9, WebSphere 9, and JBoss EAP 7.

从 Spring Framework 5.0 开始，Spring 至少需要 Java EE 7 级别（例如Servlet 3.1 +，JPA 2.1+） - 同时在 Java EE 8 级别提供在运行时与新 API 的开箱即用集成（例如，Servlet 4.0，JSON绑定API）。这使得Spring完全兼容 Tomcat 8 和 9 ， WebSphere 9 和 JBoss EAP 7。

Over time, the role of Java EE in application development has evolved. In the early days of Java EE and Spring, applications were created to be deployed to an application server. Today, with the help of Spring Boot, applications are created in a devops- and cloud-friendly way, with the Servlet container embedded and trivial to change. As of Spring Framework 5, a WebFlux application does not even use the Servlet API directly and can run on servers (such as Netty) that are not Servlet containers.

随着时间的推移，Java EE在应用程序开发中的作用也在不断发展。在 Java EE 和 Spring 的早期，应用程序被创建并部署到应用程序服务器上。今天，在 Spring Boot 的帮助下，应用程序以 devops 和 cloud-friendly 的方式创建，Servlet 容器嵌入及修改变得简单。从 Spring Framework 5 开始，WebFlux 应用程序甚至不直接使用Servlet API，并且可以在不是Servlet容器的服务器（例如Netty）上运行。

Spring continues to innovate and to evolve. Beyond the Spring Framework, there are other projects, such as Spring Boot, Spring Security, Spring Data, Spring Cloud, Spring Batch, among others. It’s important to remember that each project has its own source code repository, issue tracker, and release cadence. See spring.io/projects for the complete list of Spring projects.

Spring继续创新并不断发展。除了Spring Framework之外，还有其他项目，例如 Spring Boot，Spring Security，Spring Data，Spring Cloud，Spring Batch 等。每个项目都有自己的源代码存储库，问题追踪和发布节奏。有关Spring项目的完整列表，请参见spring.io/projects 。

## 3. Design Philosophy 设计理念

When you learn about a framework, it’s important to know not only what it does but what principles it follows. Here are the guiding principles of the Spring Framework:

当您了解框架时，重要的是不仅要知道它的作用，还要了解它遵循的原则。以下是 Spring Framework 的指导原则：

Provide choice at every level. Spring lets you defer design decisions as late as possible. For example, you can switch persistence providers through configuration without changing your code. The same is true for many other infrastructure concerns and integration with third-party APIs.

提供各个层面的选择。 Spring 允许您尽可能晚地推迟设计决策。例如，您可以通过配置切换持久性生产者，而无需更改代码。许多其他基础架构问题以及与第三方 API 的集成也是如此。

Accommodate diverse perspectives. Spring embraces flexibility and is not opinionated about how things should be done. It supports a wide range of application needs with different perspectives.

适应不同的场景。 Spring拥抱灵活性，并不认为应该如何做。它以不同的视角支持广泛的应用需求。

Maintain strong backward compatibility. Spring’s evolution has been carefully managed to force few breaking changes between versions. Spring supports a carefully chosen range of JDK versions and third-party libraries to facilitate maintenance of applications and libraries that depend on Spring.

保持强大的向后兼容性。 Spring 的演变经过精心设计，可以在版本之间进行一些重大改变。 Spring支持精心挑选的 JDK 版本和第三方库，以便于维护依赖于 Spring 的应用程序和库。

Care about API design. The Spring team puts a lot of thought and time into making APIs that are intuitive and that hold up across many versions and many years.

关心API设计。 Spring 团队花了很多心思和时间来制作直观的 API，这些 API 在很多版本和多年中都有用。

Set high standards for code quality. The Spring Framework puts a strong emphasis on meaningful, current, and accurate javadoc. It is one of very few projects that can claim clean code structure with no circular dependencies between packages.

为代码质量设定高标准。 Spring Framework 非常强调有意义的，最新的和准确的 javadoc 文档。它是可以声称干净的代码结构，包之间没有循环依赖的极少数项目之一。

## 4.Feedback and Contributions 反馈和贡献

For how-to questions or diagnosing or debugging issues, we suggest using StackOverflow, and we have a [questions page](https://spring.io/questions) that lists the suggested tags to use. If you’re fairly certain that there is a problem in the Spring Framework or would like to suggest a feature, please use the [JIRA issue tracker](https://jira.spring.io/browse/spr).

对于操作的问题或诊断、调试的问题，我们建议使用 StackOverflow ，我们有一个问题页面列出了要使用的建议标签。如果您非常确定 Spring Framework 中存在问题或想要建议功能，请使用 JIRA问题跟踪器。

If you have a solution in mind or a suggested fix, you can submit a pull request on Github. However, please keep in mind that, for all but the most trivial issues, we expect a ticket to be filed in the issue tracker, where discussions take place and leave a record for future reference.

如果您有解决方案或建议的修复，您可以在Github上提交pull request。但是，请记住，对于除了最微不足道的问题以外的所有问题，我们希望在问题跟踪器中提交一张票据，在那里进行讨论并留下记录以供将来参考。

For more details see the guidelines at the [CONTRIBUTING](https://github.com/spring-projects/spring-framework/blob/master/CONTRIBUTING.md), top-level project page.

有关更多详细信息，请参阅“贡献”顶级项目页面上的指南。

## 5.Getting Started 开始使用

If you are just getting started with Spring, you may want to begin using the Spring Framework by creating a Spring Boot-based application. Spring Boot provides a quick (and opinionated) way to create a production-ready Spring-based application. It is based on the Spring Framework, favors convention over configuration, and is designed to get you up and running as quickly as possible.

如果您刚刚开始使用Spring，您可能希望通过创建基于 Spring Boot 的应用程序来开始使用 Spring Framework 。 Spring Boot提供了一种快速（和固定）的方式来创建一个生产就绪的基于Spring的应用程序。它基于 Spring Framework，倡导约定大于配置，旨在帮助您尽快启动和运行。

You can use [start.spring.io](https://start.spring.io/) to generate a basic project or follow one of the "Getting Started" guides, such as Getting Started Building a RESTful Web Service. As well as being easier to digest, these guides are very task focused, and most of them are based on Spring Boot. They also cover other projects from the Spring portfolio that you might want to consider when solving a particular problem.

您可以使用 start.spring.io 生成基本项目，也可以按照“入门”指南之一进行操作，例如“入门构建RESTful Web 服务”。除了更容易理解之外，这些指南非常注重任务，而且大多数都基于Spring Boot。它们还涵盖了 Spring 组合中您在解决特定问题时可能需要考虑的其他项目。