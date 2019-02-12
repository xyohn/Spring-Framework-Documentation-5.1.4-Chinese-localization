# Core Technologies 核心技术 

Version 5.1.4.RELEASE

This part of the reference documentation covers all the technologies that are absolutely integral to the Spring Framework.

这部分参考文档涵盖了 Spring Framework 绝对不可或缺的所有技术。

Foremost amongst these is the Spring Framework’s Inversion of Control (IoC) container. A thorough treatment of the Spring Framework’s IoC container is closely followed by comprehensive coverage of Spring’s Aspect-Oriented Programming (AOP) technologies. The Spring Framework has its own AOP framework, which is conceptually easy to understand and which successfully addresses the 80% sweet spot of AOP requirements in Java enterprise programming.

其中最重要的是 Spring Framework 的控制反转（IoC）容器。紧随其后的是 Spring 面向切面编程（AOP）技术。 Spring Framework 有自己的 AOP 框架，它在概念上易于理解，并且成功地解决了 Java 企业编程中 AOP 要求的 80％最佳点。

Coverage of Spring’s integration with AspectJ (currently the richest — in terms of features — and certainly most mature AOP implementation in the Java enterprise space) is also provided.

除此之外还提供了 Spring 与 AspectJ 集成的覆盖范围（目前最丰富的 - 在功能方面 - 当然也是 Java 企业领域中最成熟的 AOP 实现）。

## 1. The IoC Container IoC 容器

This chapter covers Spring’s Inversion of Control (IoC) container.

本章介绍 Spring 的控制反转（IoC）容器。

### 1.1. Introduction to the Spring IoC Container and Beans Spring IoC 容器和 Bean 简介

This chapter covers the Spring Framework implementation of the Inversion of Control (IoC) principle. (See [Inversion of Control](https://docs.spring.io/spring/docs/current/spring-framework-reference/overview.html#background-ioc).) IoC is also known as dependency injection (DI). It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.

本章介绍了控制反转（IoC）原理的 Spring Framework 实现。（参见控制反转）IoC 也称为依赖注入（DI）。这是一个对象定义其依赖关系的过程（即，它们使用的其他对象）。这些对象只能通过构造函数的参数，工厂方法的参数或在从工厂方法构造或返回对象实例后在对象实例上设置的属性。 然后容器在创建 bean 时注入这些依赖项。此过程基本上是 bean 本身的逆操作（因此得名控制反转），通过使用类的直接构造或诸如服务定位器模式的机制来控制其依赖关系的实例化或位置。

The `org.springframework.beans` and `org.springframework.context` packages are the basis for Spring Framework’s IoC container. The BeanFactory interface provides an advanced configuration mechanism capable of managing any type of object. ApplicationContext is a sub-interface of BeanFactory. It adds:

`org.springframework.beans` 和 `org.springframework.context` 包是 Spring Framework 的 IoC 容器的基础。 BeanFactory 接口提供了一种能够管理任何类型对象的高级配置机制。 ApplicationContext 是 BeanFactory 的子接口。它补充了：

Easier integration with Spring’s AOP features

更容易与 Spring 的 AOP 功能集成

Message resource handling (for use in internationalization)

消息资源处理（用于国际化）

Event publication

事件发布

Application-layer specific contexts such as the WebApplicationContext for use in web applications.

特定于应用程序层的上下文，例如 WebApplicationContext，用于 Web 应用程序。

In short, the BeanFactory provides the configuration framework and basic functionality, and the ApplicationContext adds more enterprise-specific functionality. The ApplicationContext is a complete superset of the BeanFactory and is used exclusively in this chapter in descriptions of Spring’s IoC container. For more information on using the BeanFactory instead of the ApplicationContext, see The [BeanFactory](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-beanfactory).

简而言之，BeanFactory 提供配置框架和基本功能，ApplicationContext 添加了更多特定于企业的功能。 ApplicationContext 是 BeanFactory 的完整超集，在本章中关于 Spring IoC 容器的描述将只使用 ApplicationContext。有关使用 BeanFactory 而不是 ApplicationContext 的更多信息，请参阅 The BeanFactory。

In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.

在 Spring 中，构成应用程序主干并由 Spring IoC 容器管理的对象称为 bean。 bean 是一个由 Spring IoC 容器实例化，组装和管理的对象。换言之，bean 只是应用程序中许多对象之一。 Beans 及其之间的依赖关系反映在容器使用的配置元数据中。

### 1.2. Container Overview 容器概述

The `org.springframework.context.ApplicationContext` interface represents the Spring IoC container and is responsible for instantiating, configuring, and assembling the beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. The configuration metadata is represented in XML, Java annotations, or Java code. It lets you express the objects that compose your application and the rich interdependencies between those objects.

`org.springframework.context.ApplicationContext`接口表示 Spring IoC 容器，负责实例化，配置和组装 bean 。容器通过读取配置元数据获取有关实例化，配置和组装的对象的指令。配置元数据以 XML， Java 注解或 Java 代码表示。它允许您表达组成应用程序的对象以及这些对象之间丰富的相互依赖性。

Several implementations of the ApplicationContext interface are supplied with Spring. In stand-alone applications, it is common to create an instance of ClassPathXmlApplicationContext or FileSystemXmlApplicationContext. While XML has been the traditional format for defining configuration metadata, you can instruct the container to use Java annotations or code as the metadata format by providing a small amount of XML configuration to declaratively enable support for these additional metadata formats.

Spring 提供了 ApplicationContext 接口的几个实现。在独立应用程序中，通常会创建 ClassPathXmlApplicationContext 或 FileSystemXmlApplicationContext 的实例。虽然 XML 是定义配置元数据的传统格式，但您可以通过提供少量 XML 配置 来声明容器使用 Java 注解 或 Java 代码作为元数据格式，以声明方式启用对这些其他元数据格式的支持。

In most application scenarios, explicit user code is not required to instantiate one or more instances of a Spring IoC container. For example, in a web application scenario, a simple eight (or so) lines of boilerplate web descriptor XML in the web.xml file of the application typically suffices (see Convenient ApplicationContext Instantiation for Web Applications). If you use the Spring Tool Suite (an Eclipse-powered development environment), you can easily create this boilerplate configuration with a few mouse clicks or keystrokes.

在大多数应用程序方案中，不需要通过配置显式用户代码来实例化 Spring IoC 容器的一个或多个实例。例如，在 Web 应用程序场景中，应用程序的 web.xml 文件中的只需简单八行（左右）的 Web 描述符通常就足够了（请参阅 Web 应用程序的便捷 ApplicationContext 实例）。如果您使用 Spring Tool Suite（基于 Eclipse 的开发环境），只需点击几下鼠标或按键即可轻松创建此样板配置。

The following diagram shows a high-level view of how Spring works. Your application classes are combined with configuration metadata so that, after the ApplicationContext is created and initialized, you have a fully configured and executable system or application.

下图显示了 Spring 如何工作的高级视图。您的应用程序类与配置元数据相结合，以便在创建和初始 化 ApplicationContext 之后，您拥有一个完全配置且可执行的系统或应用程序。

![Figure 1. The Spring IoC container](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/container-magic.png)  
Figure 1. The Spring IoC container

#### 1.2.1. Configuration Metadata 配置元数据

As the preceding diagram shows, the Spring IoC container consumes a form of configuration metadata. This configuration metadata represents how you, as an application developer, tell the Spring container to instantiate, configure, and assemble the objects in your application.

如上图所示，Spring IoC 容器使用一种配置元数据的形式。 此配置元数据表示您作为应用程序开发人员如何告诉 Spring 容器在应用程序中实例化，配置和组装对象。

Configuration metadata is traditionally supplied in a simple and intuitive XML format, which is what most of this chapter uses to convey key concepts and features of the Spring IoC container.

传统上，配置元数据以简单直观的 XML 格式提供，本章大部分内容用于传达 Spring IoC 容器的关键概念和功能。

> XML-based metadata is not the only allowed form of configuration metadata. The Spring IoC container itself is totally decoupled from the format in which this configuration metadata is actually written. These days, many developers choose Java-based configuration for their Spring applications.
> 基于 XML 的元数据不是唯一允许的配置元数据形式。 Spring IoC 容器本身完全与实际编写此配置元数据的格式分离。 目前，许多开发人员为其 Spring 应用程序选择基于 Java 的配置。

For information about using other forms of metadata with the Spring container, see:
有关在 Spring 容器中使用其他形式的元数据的信息，请参阅：

- Annotation-based configuration: Spring 2.5 introduced support for annotation-based configuration metadata.

基于注释的配置：Spring 2.5 引入了对基于注释的配置元数据的支持

- Java-based configuration: Starting with Spring 3.0, many features provided by the Spring JavaConfig project became part of the core Spring Framework. Thus, you can define beans external to your application classes by using Java rather than XML files. To use these new features, see the @Configuration, @Bean, @Import, and @DependsOn annotations.

基于 Java 的配置：从 Spring 3.0 开始，Spring JavaConfig 项目提供的许多功能成为核心 Spring Framework 的一部分。 因此，您可以使用 Java 而不是 XML 文件在应用程序类外部定义 bean。 要使用这些新功能，请参阅@Configuration，@ Bean，@ Import 和@DependsOn 注释。

These bean definitions correspond to the actual objects that make up your application. Typically, you define service layer objects, data access objects (DAOs), presentation objects such as Struts Action instances, infrastructure objects such as Hibernate SessionFactories, JMS Queues, and so forth. Typically, one does not configure fine-grained domain objects in the container, because it is usually the responsibility of DAOs and business logic to create and load domain objects. However, you can use Spring’s integration with AspectJ to configure objects that have been created outside the control of an IoC container. See Using AspectJ to dependency-inject domain objects with Spring.

这些 bean 定义对应于构成应用程序的实际对象。 通常，您定义服务层对象，数据访问对象（DAO），表示对象（如 Struts Action 实例），基础结构对象（如 Hibernate SessionFactories，JMS 队列等）。 通常，不会在容器中配置细粒度域对象，因为 DAO 和业务逻辑通常负责创建和加载域对象。 但是，您可以使用 Spring 与 AspectJ 的集成来配置在 IoC 容器控制之外创建的对象。 请参阅使用 AspectJ 使用 Spring 依赖注入域对象。

The following example shows the basic structure of XML-based configuration metadata:  
以下示例显示了基于 XML 的配置元数据的基本结构：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">   
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->
</beans>
```

```
The id attribute is a string that identifies the individual bean definition.
id 属性是一个标识单个 bean 定义的字符串。
The class attribute defines the type of the bean and uses the fully qualified classname.
class 属性定义 bean 的类型并使用 classname 的全限定名
```

The value of the id attribute refers to collaborating objects. The XML for referring to collaborating objects is not shown in this example. See Dependencies for more information.    
属性 id 的值指的是协作对象。 在此示例中未显示用于引用协作对象的 XML。 有关更多信息，请参阅依赖项

#### 1.2.2. Instantiating a Container 实例化容器

The location path or paths supplied to an ApplicationContext constructor are resource strings that let the container load configuration metadata from a variety of external resources, such as the local file system, the Java CLASSPATH, and so on.  
提供给 ApplicationContext 构造函数的位置路径是有关资源的字符串，它允许容器从各种外部资源（如本地文件系统，Java CLASSPATH 等）加载配置元数据。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

> After you learn about Spring’s IoC container, you may want to know more about Spring’s Resource abstraction (as described in Resources), which provides a convenient mechanism for reading an InputStream from locations defined in a URI syntax. In particular, Resource paths are used to construct applications contexts, as described in [Application Contexts and Resource Paths](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#resources-app-ctx).
> 在了解了 Spring 的 IoC 容器之后，您可能想要了解有关 Spring 的资源抽象的更多信息（如参考资料中所述），它提供了一种从 URI 语法中定义的位置读取 InputStream 的便捷机制。 特别是，资源路径用于构建应用程序上下文，如[Application Contexts and Resource Paths] 中所述。

The following example shows the service layer objects (```services.xml```) configuration file:  
以下示例显示了服务层对象（```services.xml```）配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

The following example shows the data access objects ```daos.xml``` file:  
以下示例显示了数据访问对象```daos.xml```文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```

In the preceding example, the service layer consists of the ```PetStoreServiceImpl``` class and two data access objects of the types ```JpaAccountDao``` and ```JpaItemDao ```(based on the JPA Object-Relational Mapping standard). The ```property name``` element refers to the name of the JavaBean property, and the``` ref``` element refers to the name of another bean definition. This linkage between ```id``` and ```ref``` elements expresses the dependency between collaborating objects. For details of configuring an object’s dependencies, see Dependencies.

在前面的例子中，服务层包含```PetStoreServiceImpl```类和两个```JpaAccountDao```和```JpaItemDao```的数据访问对象（基于 JPA Object-Relational 映射标准）。 ```property name```元素引用 JavaBean 属性的名称，```ref```元素引用另一个 bean 定义的名称。 ```id```和```ref```元素之间的这种联系表达了协作对象之间的依赖关系。 有关配置对象的依赖关系的详细信息，请参阅依赖关系。

##### Composing XML-based Configuration Metadata 编写基于 XML 的配置元数据

It can be useful to have bean definitions span multiple XML files. Often, each individual XML configuration file represents a logical layer or module in your architecture.

让 bean 定义跨越多个 XML 文件会很有用。 通常，每个单独的 XML 配置文件都代表架构中的逻辑层或模块。

You can use the application context constructor to load bean definitions from all these XML fragments. This constructor takes multiple``` Resource``` locations, as was shown in the previous section. Alternatively, use one or more occurrences of the``` <import/>``` element to load bean definitions from another file or files. The following example shows how to do so:  
您可以使用应用程序上下文构造函数从所有这些 XML 片段加载 bean 定义。 这个构造函数需要多个```Resource```位置，如上一节所示。 或者，使用一个或多个```<import />```元素来从另一个或多个文件加载 bean 定义。 以下示例显示了如何执行此操作：

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

In the preceding example, external bean definitions are loaded from three files: ```services.xml```, ```messageSource.xml```, and ```themeSource.xml```. All location paths are relative to the definition file doing the importing, so services.xml must be in the same directory or classpath location as the file doing the importing, while ```messageSource.xml``` and ```themeSource.xml``` must be in a resources location below the location of the importing file. As you can see, a leading slash is ignored. However, given that these paths are relative, it is better form not to use the slash at all. The contents of the files being imported, including the top level <beans/> element, must be valid XML bean definitions, according to the Spring Schema.

在前面的示例中，外部 bean 定义从三个文件加载：```services.xml```，```messageSource.xml```和```themeSource.xml```。 所有位置路径都与执行导入的定义文件相关，因此 services.xml 必须与执行导入的文件位于相同的目录或 classpath 路径位置，而```messageSource.xml```和```themeSource.xml ```必须位于导入文件位置下方的资源位置。 如您所见，前导斜杠是可忽略的。 但是，鉴于这些路径是相对的，最好不要使用斜杠。 根据 Spring Schema，正在导入的文件的内容（包括顶级 <beans /> 元素）必须是有效的 XML bean 定义。

> It is possible, but not recommended, to reference files in parent directories using a relative "../" path. Doing so creates a dependency on a file that is outside the current application. In particular, this reference is not recommended for ```classpath```: URLs (for example,```classpath:../services.xml```), where the runtime resolution process chooses the “nearest” classpath root and then looks into its parent directory. Classpath configuration changes may lead to the choice of a different, incorrect directory.
> 使用相对“../”路径引用父目录中的文件是可以的，但不建议这么做。因为 这样做会对当前应用程序之外的文件创建依赖关系。 特别是，这个引用不建议用于```classpath```：URL（例如，```classpath：../ services.xml```），其中运行时解析过程选择“最近的”类路径根 然后查看其父目录。 类路径配置更改可能导致选择不同的，不正确的目录。

> You can always use fully qualified resource locations instead of relative paths: for example, ```file:C:/config/services.xml ```or ```classpath:/config/services.xml```. However, be aware that you are coupling your application’s configuration to specific absolute locations. It is generally preferable to keep an indirection for such absolute locations — for example, through "${…}" placeholders that are resolved against JVM system properties at runtime.
> 您始终可以使用完全限定的资源位置而不是相对路径：例如，```file：C：/config/services.xml```或```classpath：/ config / services.xml```。 但是，请注意您将应用程序的配置与特定的绝对位置耦合。 通常最好为这些绝对位置保持间接联系 - 例如，通过在运行时针对 JVM 系统属性解析的“$ {...}”占位符。

The namespace itself provices the import directive feature. Further configuration features beyond plain bean definitions are available in a selection of XML namespaces provided by Spring — for example, the ```context``` and ```util``` namespaces.
命名空间本身提供了 import 指令功能。 除了普通 bean 定义之外的其他配置功能可以在 Spring 提供的一系列 XML 命名空间中找到 - 例如，```context```和```util```命名空间。

##### The Groovy Bean Definition DSL

As a further example for externalized configuration metadata, bean definitions can also be expressed in Spring’s Groovy Bean Definition DSL, as known from the Grails framework. Typically, such configuration live in a ".groovy" file with the structure shown in the following example:

作为配置元数据的另一个示例，bean 定义也可以在 Spring 的 Groovy Bean 定义 DSL 中表示，如 Grails 框架中所知。 通常，此类配置位于“.groovy”文件中，其结构如下例所示：

```java
beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

This configuration style is largely equivalent to XML bean definitions and even supports Spring’s XML configuration namespaces. It also allows for importing XML bean definition files through an `importBeans` directive.

此配置样式在很大程度上等同于 XML bean 定义，甚至支持 Spring 的 XML 配置命名空间。 它还允许通过`importBeans`指令导入 XML bean 定义文件。

#### 1.2.3. Using the Container 使用容器

The `ApplicationContext` is the interface for an advanced factory capable of maintaining a registry of different beans and their dependencies. By using the method `T getBean(String name, Class<T> requiredType)`, you can retrieve instances of your beans.

`ApplicationContext`是高级工厂的接口，能够维护不同 bean 及其依赖项的注册表。 通过使用方法`T getBean（String name，Class <T> requiredType）`，您可以检索 bean 的实例。

The `ApplicationContext` lets you read bean definitions and access them, as the following example shows:

`ApplicationContext`允许您读取 bean 定义并访问它们，如以下示例所示：

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

With Groovy configuration, bootstrapping looks very similar. It has a different context implementation class which is Groovy-aware (but also understands XML bean definitions). The following example shows Groovy configuration:

使用 Groovy 配置，启动形式看起来非常相似。 它有一个不同的上下文实现类，它是 Groovy-aware（但也理解 XML bean 定义）。 以下示例显示了 Groovy 配置：

```java
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");

```

The most flexible variant is `GenericApplicationContext` in combination with reader delegates — for example, with `XmlBeanDefinitionReader` for XML files, as the following example shows:

最灵活的变体是`GenericApplicationContext`与读者委托相结合 - 例如，对于 XML 文件使用`XmlBeanDefinitionReader`，如下例所示：

```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();

```

You can also use the `GroovyBeanDefinitionReader` for Groovy files, as the following example shows:

您还可以对 Groovy 文件使用`GroovyBeanDefinitionReader`，如以下示例所示：

```java
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();

```

You can mix and match such reader delegates on the same `ApplicationContext`, reading bean definitions from diverse configuration sources.

您可以在相同的`ApplicationContext`上混合和匹配这些读者，从不同的配置源读取 bean 定义。

You can then use `getBean` to retrieve instances of your beans. The `ApplicationContext` interface has a few other methods for retrieving beans, but, ideally, your application code should never use them. Indeed, your application code should have no calls to the `getBean()` method at all and thus have no dependency on Spring APIs at all. For example, Spring’s integration with web frameworks provides dependency injection for various web framework components such as controllers and JSF-managed beans, letting you declare a dependency on a specific bean through metadata (such as an autowiring annotation).

然后，您可以使用`getBean`来检索 bean 的实例。 `ApplicationContext`接口有一些其他的方法来检索 bean，但理想情况下，你的应用程序代码永远不应该使用它们。 实际上，您的应用程序代码根本不应该调用`getBean（）`方法，因此根本不依赖于 Spring API。 例如，Spring 与 Web 框架的集成为各种 Web 框架组件（如控制器和 JSF 托管 bean）提供依赖注入，允许您通过元数据（例如自动装配注释）声明对特定 bean 的依赖性。

### 1.3. Bean Overview Bean 概述

A Spring IoC container manages one or more beans. These beans are created with the configuration metadata that you supply to the container (for example, in the form of XML `<bean/>` definitions).

Spring IoC 容器管理一个或多个 bean 。 这些 bean 是使用您提供给容器的配置元数据创建的（例如，以 XML` <bean />`定义的形式）。

Within the container itself, these bean definitions are represented as `BeanDefinition` objects, which contain (among other information) the following metadata:

在容器本身内，这些 bean 定义表示为`BeanDefinition`对象，其中包含（以及其他信息）以下元数据：

- A package-qualified class name: typically, the actual implementation class of the bean being defined.
- 包限定类名：通常是定义的 bean 的实际实现类。
- Bean behavioral configuration elements, which state how the bean should behave in the container (scope, lifecycle callbacks, and so forth).
- Bean 行为配置元素，它说明 bean 在容器中的行为方式（范围，生命周期回调等）。
- References to other beans that are needed for the bean to do its work. These references are also called collaborators or dependencies.
- 引用 bean 执行其工作所需的其他 bean。 这些引用也称为协作者或依赖项。
- Other configuration settings to set in the newly created object — for example, the size limit of the pool or the number of connections to use in a bean that manages a connection pool.
- 要在新创建的对象中设置的其他配置设置 - 例如，池的大小限制或在管理连接池的 Bean 中使用的连接数。

This metadata translates to a set of properties that make up each bean definition. The following table describes these properties:

此元数据转换为构成每个 bean 定义的一组属性。 下表描述了这些属性：

|              Property               |                        Explained in…                         |
| :---------------------------------: | :----------------------------------------------------------: |
|                Class                | [Instantiating Beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-class) |
|                Name                 | [Naming Beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-beanname) |
|             Scope 范围              | [Bean Scopes](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes) |
| Constructor arguments 构造函数参数  | [Dependency Injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators) |
|           Properties 属性           | [Dependency Injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators) |
|      Autowiring mode 自动装配       | [Autowiring Collaborators](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-autowire) |
| Lazy initialization mode 延迟初始化 | [Lazy-initialized Beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lazy-init) |
|  Initialization method 初始化方法   | [Initialization Callbacks](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean) |
|     Destruction method 销毁方法     | [Destruction Callbacks](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-disposablebean) |

In addition to bean definitions that contain information on how to create a specific bean, the `ApplicationContext`implementations also permit the registration of existing objects that are created outside the container (by users). This is done by accessing the ApplicationContext’s BeanFactory through the `getBeanFactory()` method, which returns the BeanFactory `DefaultListableBeanFactory` implementation. `DefaultListableBeanFactory` supports this registration through the `registerSingleton(..)` and `registerBeanDefinition(..)` methods. However, typical applications work solely with beans defined through regular bean definition metadata.

除了包含有关如何创建特定 bean 的信息的 bean 定义之外，`ApplicationContext` 还允许注册在容器外部（由用户创建）的现有对象。 这是通过`getBeanFactory（）`方法访问 ApplicationContext 的 BeanFactory 来完成的，该方法返回 BeanFactory 的 `DefaultListableBeanFactory` 实现。 `DefaultListableBeanFactory`通过`registerSingleton（..）`和`registerBeanDefinition（..）`方法支持这种注册。 但是，典型的应用程序仅用于通过常规 bean 定义元数据定义的 bean。

> Bean metadata and manually supplied singleton instances need to be registered as early as possible, in order for the container to properly reason about them during autowiring and other introspection steps. While overriding existing metadata and existing singleton instances is supported to some degree, the registration of new beans at runtime (concurrently with live access to the factory) is not officially supported and may lead to concurrent access exceptions, inconsistent state in the bean container, or both.
>
> 需要尽早注册 Bean 元数据和手动提供的单例实例，以便容器在自动装配和其他内省步骤期间正确推理它们。 虽然在某种程度上支持覆盖现有元数据和现有单例实例，但是在运行时注册新 bean（与对工厂的实时访问同时）并未得到官方支持，并且可能导致并发访问异常，bean 容器中的状态不一致，或者两者都发生。



#### 1.3.1. Naming Beans 命名 beans

Every bean has one or more identifiers. These identifiers must be unique within the container that hosts the bean. A bean usually has only one identifier. However, if it requires more than one, the extra ones can be considered aliases.

每个 bean 都有一个或多个标识符。 这些标识符在托管 bean 的容器中必须是唯一的。 bean 通常只有一个标识符。 但是，如果它需要多个，则额外的可以被视为别名。

In XML-based configuration metadata, you use the `id` attribute, the `name` attribute, or both to specify the bean identifiers. The `id` attribute lets you specify exactly one id. Conventionally, these names are alphanumeric ('myBean', 'someService', etc.), but they can contain special characters as well. If you want to introduce other aliases for the bean, you can also specify them in the `name` attribute, separated by a comma (`,`), semicolon (`;`), or white space. As a historical note, in versions prior to Spring 3.1, the `id` attribute was defined as an `xsd:ID` type, which constrained possible characters. As of 3.1, it is defined as an `xsd:string`type. Note that bean `id` uniqueness is still enforced by the container, though no longer by XML parsers.

在基于 XML 的元数据配置中，你可以使用 `id` 属性，`name ` 属性或两者来指定 bean 标识符。 `id` 属性允许您指定一个 id。 通常，这些名称是字母数字（'myBean'，'someService' 等），但它们也可以包含特殊字符。 如果要为 bean 引入其他别名，还可以在 `name` 属性中指定它们，用逗号（`，`），分号（`;`）或空格分隔。 作为历史记录，在 Spring 3.1 之前的版本中，`id`属性被定义为`xsd：ID`类型，它约束了可能的字符。 从 3.1 开始，它被定义为`xsd：string`类型。 请注意，bean 的“id”唯一性仍由容器强制执行，但不再由 XML 解析器强制执行。

You are not required to supply a `name` or an `id` for a bean. If you do not supply a `name` or `id` explicitly, the container generates a unique name for that bean. However, if you want to refer to that bean by name, through the use of the `ref` element or a[Service Locator](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-servicelocator) style lookup, you must provide a name. Motivations for not supplying a name are related to using [inner beans](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-inner-beans)and [autowiring collaborators](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-autowire).

你不是一定需要为 bean 提供`name`或`id`。 如果没有显式提供`name`或`id`，容器会为该 bean 生成一个唯一的名称。 但是，如果你想通过名称引用那个 bean ，可以通过使用`ref`元素或[Service Locator](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-servicelocator) 样式查找，您必须提供名称。 不提供名称的将作为 [内部 bean](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-inner-beans) 和 [自动装配 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-autowire)

```
Bean Naming Conventions Bean 命名约定
The convention is to use the standard Java convention for instance field names when naming beans. That is, bean names start with a lowercase letter and are camel-cased from there. Examples of such names include accountManager, accountService, userDao, loginController, and so forth.
命名的约定是在命名 bean 时使用标准 Java 约定作为实例字段名称。 也就是说，bean 名称以小写字母开头，并从那里开始驼峰。 示例包括 accountManager，accountService，userDao，loginController 等。
Naming beans consistently makes your configuration easier to read and understand. Also, if you use Spring AOP, it helps a lot when applying advice to a set of beans related by name.
命名 bean 始终使您的配置更易于阅读和理解。 此外，如果您使用 Spring AOP，那么在将建议应用于与名称相关的一组 bean 时，它会有很大帮助。

```

```
With component scanning in the classpath, Spring generates bean names for unnamed components, following the rules described earlier: essentially, taking the simple class name and turning its initial character to lower-case. However, in the (unusual) special case when there is more than one character and both the first and second characters are upper case, the original casing gets preserved. These are the same rules as defined by `java.beans.Introspector.decapitalize` (which Spring uses here).
通过类路径中的组件扫描，Spring 按照前面描述的规则为未命名的组件生成 bean 名称：实质上，采用简单的类名并将其初始字符转换为小写。 但是，在（不常见的）特殊情况下，当有多个字符且第一个和第二个字符都是大写字母时，原始外壳将被保留。 这些规则与`java.beans.Introspector.decapitalize`（Spring 在这里使用）定义的规则相同。

```

##### Aliasing a Bean outside the Bean Definition 为 bean 定义别名

In a bean definition itself, you can supply more than one name for the bean, by using a combination of up to one name specified by the `id` attribute and any number of other names in the `name` attribute. These names can be equivalent aliases to the same bean and are useful for some situations, such as letting each component in an application refer to a common dependency by using a bean name that is specific to that component itself.

在 bean 定义本身中，通过使用 `id` 属性指定的最多一个名称和 `name`  属性中的任意数量的其他名称，可以为 bean 提供多个名称。 这些名称可以是同一个 bean 的等效别名，对某些情况很有用，例如让应用程序中的每个组件通过使用特定于该组件本身的 bean 名称来引用公共依赖项。

Specifying all aliases where the bean is actually defined is not always adequate, however. It is sometimes desirable to introduce an alias for a bean that is defined elsewhere. This is commonly the case in large systems where configuration is split amongst each subsystem, with each subsystem having its own set of object definitions. In XML-based configuration metadata, you can use the `<alias/>` element to accomplish this. The following example shows how to do so:  
但是，指定实际定义 bean 的所有别名并不总是足够的。 有时需要为其他地方定义的 bean 引入别名。 在大型系统中通常就是这种情况，其中配置在每个子系统之间分配，每个子系统具有其自己的一组对象定义。 在基于 XML 的配置元数据中，您可以使用`<alias />`元素来完成此任务。 以下示例显示了如何执行此操作：

```
<alias name="fromName" alias="toName"/>

```

In this case, a bean (in the same container) named `fromName` may also, after the use of this alias definition, be referred to as `toName`.

在使用此别名定义之后，名为`fromName`的 bean（在同一容器中）也可以称为`toName`。

For example, the configuration metadata for subsystem A may refer to a DataSource by the name of `subsystemA-dataSource`. The configuration metadata for subsystem B may refer to a DataSource by the name of `subsystemB-dataSource`. When composing the main application that uses both these subsystems, the main application refers to the DataSource by the name of `myApp-dataSource`. To have all three names refer to the same object, you can add the following alias definitions to the configuration metadata:

例如，子系统 A 的配置元数据可以通过`subsystemA-dataSource`引用 DataSource 。 子系统 B 的配置元数据可以通过`subsystemB-dataSource`来引用 DataSource。 在编写使用这两个子系统的主应用程序时，主应用程序通过名称`myApp-dataSource`引用 DataSource。 要使所有三个名称引用同一对象，可以将以下别名定义添加到配置元数据中：

```
<alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
<alias name="myApp-dataSource" alias="subsystemB-dataSource"/>

```

Now each component and the main application can refer to the dataSource through a name that is unique and guaranteed not to clash with any other definition (effectively creating a namespace), yet they refer to the same bean.

现在，每个组件和主应用程序都可以通过一个唯一的名称引用 dataSource ，并保证不与任何其他定义冲突（有效地创建命名空间），但它们引用相同的 bean 。

```
Java-configuration Java 配置

If you use Javaconfiguration, the `@Bean` annotation can be used to provide aliases. See [Using the `@Bean` Annotation](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-java-bean-annotation) for details.
如果使用 Java 配置，则 @Bean 注释可用于提供别名。 有关详细信息，请参阅使用 @Bean 批注。

```

#### 1.3.2. Instantiating Beans 实例化 bean

A bean definition is essentially a recipe for creating one or more objects. The container looks at the recipe for a named bean when asked and uses the configuration metadata encapsulated by that bean definition to create (or acquire) an actual object.

bean 定义本质上是用于创建一个或多个对象的准则。 容器在被询问时查看命名 bean 的准则，并使用由该 bean 定义封装的配置元数据来创建（或获取）实际对象。

If you use XML-based configuration metadata, you specify the type (or class) of object that is to be instantiated in the `class`attribute of the `<bean/>` element. This `class` attribute (which, internally, is a `Class` property on a `BeanDefinition` instance) is usually mandatory. (For exceptions, see [Instantiation by Using an Instance Factory Method](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-class-instance-factory-method) and [Bean Definition Inheritance](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-child-bean-definitions).) You can use the `Class` property in one of two ways:

如果使用基于 XML 的配置元数据，则指定要在`<bean />`元素的`class`属性中实例化的对象的类型（或类）。 这个 `class` 属性（内部是 `BeanDefinition`实例上的`Class`属性）通常是必需的。 （有关例外，请参阅[使用实例工厂方法实例化 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-class-instance-factory-method)和[Bean Definition Inheritance](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-child-bean-definitions)。）使用 `Class`属性有两种方式：

- Typically, to specify the bean class to be constructed in the case where the container itself directly creates the bean by calling its constructor reflectively, somewhat equivalent to Java code with the `new` operator.

- 通常通过在容器本身进行反射调用其构造函数直接创建 bean ，来指定要构造的 bean 类，稍微等同于使用`new`运算符的 Java 代码。

- To specify the actual class containing the `static` factory method that is invoked to create the object, in the less common case where the container invokes a `static` factory method on a class to create the bean. The object type returned from the invocation of the `static` factory method may be the same class or another class entirely.

- 指定包含被调用以创建对象的`static`工厂方法的实际类，在不常见的情况下，容器在类上调用`static`工厂方法来创建 bean。 从`static`工厂方法的调用返回的对象类型可以完全是同一个类或另一个类。

  ```
  Inner class names 内部类名
  If you want to configure a bean definition for a static nested class, you have to use the binary name of the nested class.
  如果要为静态嵌套类配置 bean 定义，则必须使用嵌套类的二进制名称。
  For example, if you have a class called SomeThing in the com.example package, and this SomeThing class has a static nested class called OtherThing, the value of the class attribute on a bean definition would be com.example.SomeThing$OtherThing.
  例如，如果在 com.example 包中有一个名为 SomeThing 的类，并且此 SomeThing 类具有一个名为 OtherThing 的静态嵌套类，则 bean 定义上的 class 属性值将为 com.example.SomeThing $ OtherThing。
  Notice the use of the $ character in the name to separate the nested class name from the outer class name.
  请注意，在名称中使用$字符可以将嵌套类名与外部类名分开。
  ```

  ##### Instantiation with a Constructor 使用构造函数实例化

  When you create a bean by the constructor approach, all normal classes are usable by and compatible with Spring. That is, the class being developed does not need to implement any specific interfaces or to be coded in a specific fashion. Simply specifying the bean class should suffice. However, depending on what type of IoC you use for that specific bean, you may need a default (empty) constructor.  

  当您通过构造方法创建 bean 时，所有普通类都可以使用并与 Spring 兼容。 也就是说，正在开发的类不需要实现任何特定接口或以特定方式编码。 简单地指定 bean 类就足够了。 但是，根据您为该特定 bean 使用的 IoC 类型，您可能需要一个默认（空）构造函数。  

  The Spring IoC container can manage virtually any class you want it to manage. It is not limited to managing true JavaBeans. Most Spring users prefer actual JavaBeans with only a default (no-argument) constructor and appropriate setters and getters modeled after the properties in the container. You can also have more exotic non-bean-style classes in your container. If, for example, you need to use a legacy connection pool that absolutely does not adhere to the JavaBean specification, Spring can manage it as well.     

  Spring IoC 容器几乎可以管理您希望它管理的任何类。 它不仅限于管理真正的 JavaBeans。 大多数 Spring 用户更喜欢实际的 JavaBeans ，只有一个默认（无参数）构造函数，并且在容器中的属性之后建模了适当的 setter 和 getter 。 您还可以在容器中拥有更多异国情调的非 bean 样式类。 例如，如果您需要使用绝对不符合 JavaBean 规范的旧连接池，那么 Spring 也可以对其进行管理。

  With XML-based configuration metadata you can specify your bean class as follows:  
  使用基于 XML 的配置元数据，您可以按如下方式指定 bean 类：  

```
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>

```

For details about the mechanism for supplying arguments to the constructor (if required) and setting object instance properties after the object is constructed, see [Injecting Dependencies](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-collaborators).

有关为构造函数提供参数的机制（如果需要）以及在构造对象后设置对象实例属性的详细信息，请参阅注入依赖项。

##### Instantiation with a Static Factory Method 使用静态工厂方法实例化

When defining a bean that you create with a static factory method, use the `class` attribute to specify the class that contains the `static` factory method and an attribute named `factory-method` to specify the name of the factory method itself. You should be able to call this method (with optional arguments, as described later) and return a live object, which subsequently is treated as if it had been created through a constructor. One use for such a bean definition is to call `static` factories in legacy code.

在定义使用静态工厂方法创建的 bean 时，使用`class`属性指定包含`static`工厂方法的类和名为`factory-method`的属性，以指定工厂方法本身的名称。 您应该能够调用此方法（使用可选参数，如稍后所述）并返回一个活动对象，随后将其视为通过构造函数创建的对象。 这种 bean 定义的一个用途是在遗留代码中调用`static`工厂。

The following bean definition specifies that the bean be created by calling a factory method. The definition does not specify the type (class) of the returned object, only the class containing the factory method. In this example, the `createInstance()` method must be a static method. The following example shows how to specify a factory method:

以下 bean 定义指定通过调用工厂方法来创建 bean 。 该定义未指定返回对象的类型（类），仅指定包含工厂方法的类。 在此示例中，`createInstance()`方法必须是静态方法。 以下示例显示如何指定工厂方法：

```
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>

```

The following example shows a class that would work with the preceding bean definition:
以下示例显示了一个可以使用前面的 bean 定义的类：

```
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}

```

For details about the mechanism for supplying (optional) arguments to the factory method and setting object instance properties after the object is returned from the factory, see [Dependencies and Configuration in Detail](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-properties-detailed).

有关在工厂返回对象后为工厂方法提供（可选）参数和设置对象实例属性的机制的详细信息，请参阅 [Dependencies and Configuration in Detail](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-properties-detailed).

##### Instantiation by Using an Instance Factory Method 使用实例工厂方法实例化

Similar to instantiation through a [static factory method](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-class-static-factory-method), instantiation with an instance factory method invokes a non-static method of an existing bean from the container to create a new bean. To use this mechanism, leave the `class` attribute empty and, in the `factory-bean` attribute, specify the name of a bean in the current (or parent or ancestor) container that contains the instance method that is to be invoked to create the object. Set the name of the factory method itself with the `factory-method`attribute. The following example shows how to configure such a bean:

类似于通过[静态工厂方法 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-class-static-factory-method) 实例化， 使用实例工厂方法进行实例化从容器调用现有 bean 的非静态方法以创建新 bean。 要使用此机制，请将`class`属性保留为空，并在`factory-bean`属性中指定当前（或父或祖先）容器中 bean 的名称，该容器包含要调用的实例方法。 创建对象。 使用`factory-method`属性设置工厂方法本身的名称。 以下示例显示如何配置此类 bean：

```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

```

The following example shows the corresponding Java class:

以下示例展示了相应的 Java 类：

```
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}

```

One factory class can also hold more than one factory method, as the following example shows:

一个工厂类也可以包含多个工厂方法，如以下示例所示：

```xml
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>

```

The following example shows the corresponding Java class:

以下示例展示了相应的 Java 类：

```
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    private static AccountService accountService = new AccountServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```

This approach shows that the factory bean itself can be managed and configured through dependency injection (DI). See [Dependencies and Configuration in Detail](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-properties-detailed).

这种方法表明工厂 bean 本身可以通过依赖注入（DI）进行管理和配置。 请参阅[详细信息的依赖关系和配置 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-properties-detailed)。

```
	In Spring documentation, “factory bean” refers to a bean that is configured in the Spring container and that creates objects through an instance or static factory method. By contrast, FactoryBean (notice the capitalization) refers to a Spring-specific FactoryBean.
	在 Spring 文档中，“factory bean”是指在 Spring 容器中配置并通过实例或静态工厂方法创建对象的 bean。 相比之下，FactoryBean（注意大小写）是指特定于 Spring 的 FactoryBean。
```

### 1.4. Dependencies 依赖关系

A typical enterprise application does not consist of a single object (or bean in the Spring parlance). Even the simplest application has a few objects that work together to present what the end-user sees as a coherent application. This next section explains how you go from defining a number of bean definitions that stand alone to a fully realized application where objects collaborate to achieve a goal.

典型的企业应用程序不包含单个对象（或 Spring 用法中的 bean）。 即使是最简单的应用程序也有一些对象可以协同工作，以呈现最终用户所看到的连贯应用程序。 下一节将介绍如何定义多个独立的 bean 定义，以及对象协作实现目标的完全实现的应用程序。

#### 1.4.1. Dependency Injection 依赖注入

Dependency injection (DI) is a process whereby objects define their dependencies (that is, the other objects with which they work) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies on its own by using direct construction of classes or the Service Locator pattern.

依赖注入（DI）是一个过程，通过这个过程，对象只能通过构造函数参数，工厂方法的参数或在构造对象实例后在对象实例上设置的属性 或工厂方法返回的实例来定义它们的依赖关系（即它们使用的其他对象）。 然后容器在创建 bean 时注入这些依赖项。 这个过程基本上是 bean 本身的反向（因此名称，控制反转），它通过使用类的直接构造或服务定位器模式来控制其依赖项的实例化或位置。

Code is cleaner with the DI principle, and decoupling is more effective when objects are provided with their dependencies. The object does not look up its dependencies and does not know the location or class of the dependencies. As a result, your classes become easier to test, particularly when the dependencies are on interfaces or abstract base classes, which allow for stub or mock implementations to be used in unit tests.

使用 DI 原则的代码更清晰，当对象提供其依赖项时，解耦更有效。 该对象不查找其依赖项，也不知道依赖项的位置或类。 因此，您的类变得更容易测试，特别是当依赖关系在接口或抽象基类上时，这允许在单元测试中使用存根或模拟实现。

DI exists in two major variants: [Constructor-based dependency injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-constructor-injection) and [Setter-based dependency injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-setter-injection).

DI 存在两个主要变体：[基于构造函数的依赖注入 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-constructor-injection) 和[基于 Setter 依赖注入 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-setter-injection)。

##### Constructor-based Dependency Injection 基于构造函数的依赖注入

Constructor-based DI is accomplished by the container invoking a constructor with a number of arguments, each representing a dependency. Calling a `static` factory method with specific arguments to construct the bean is nearly equivalent, and this discussion treats arguments to a constructor and to a `static` factory method similarly. The following example shows a class that can only be dependency-injected with constructor injection:

基于构造函数的依赖注入由容器调用具有多个参数的构造函数来完成，每个参数表示一个依赖项。 调用带有特定参数的`static`工厂方法来构造 bean 几乎是等价的，本节讨论同样处理构造函数和`static`工厂方法的参数。 以下示例显示了一个只能通过构造函数注入进行依赖注入的类：

```
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}
```

Notice that there is nothing special about this class. It is a POJO that has no dependencies on container specific interfaces, base classes or annotations.

请注意，这个类没有什么特别之处。 它是一个 POJO ，它不依赖于容器特定的接口，基类或注释。

###### Constructor Argument Resolution 构造函数参数解析

Constructor argument resolution matching occurs by using the argument’s type. If no potential ambiguity exists in the constructor arguments of a bean definition, the order in which the constructor arguments are defined in a bean definition is the order in which those arguments are supplied to the appropriate constructor when the bean is being instantiated. Consider the following class:

通过使用参数的类型进行构造函数参数解析匹配。 如果 bean 定义的构造函数参数中不存在潜在的歧义，那么在 bean 定义中定义构造函数参数的顺序是在实例化 bean 时将这些参数提供给适当的构造函数的顺序。 考虑以下类：

```
package x.y;

public class ThingOne {

    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) {
        // ...
    }
}
```

Assuming that `ThingTwo` and `ThingThree` classes are not related by inheritance, no potential ambiguity exists. Thus, the following configuration works fine, and you do not need to specify the constructor argument indexes or types explicitly in the `<constructor-arg/>` element.

假 设 ThingTwo 和 ThingThree 类与继承无关，则不存在潜在的歧义。 因此，以下配置工作正常，您无需在`<constructor-arg />`元素中显式指定构造函数参数索引或类型。

```
<beans>
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg ref="beanTwo"/>
        <constructor-arg ref="beanThree"/>
    </bean>

    <bean id="beanTwo" class="x.y.ThingTwo"/>

    <bean id="beanThree" class="x.y.ThingThree"/>
</beans>
```

When another bean is referenced, the type is known, and matching can occur (as was the case with the preceding example). When a simple type is used, such as `<value>true</value>`, Spring cannot determine the type of the value, and so cannot match by type without help. Consider the following class:

当引用另一个 bean 时，类型是已知的，并且可以发生匹配（与前面的示例一样）。 当使用简单类型时，例如 <value> true </ value>，Spring 无法确定值的类型，因此无法在没有帮助的情况下按类型进行匹配。 考虑以下类：

```
package examples;

public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private int years;

    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

Constructor argument type matching 构造函数参数类型匹配

In the preceding scenario, the container can use type matching with simple types if you explicitly specify the type of the constructor argument by using the `type` attribute. as the following example shows:  

在前面的场景中，如果使用`type`属性显式指定构造函数参数的类型，则容器可以使用与简单类型匹配的类型。 如下例所示：  

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

Constructor argument index 构造函数参数索引

You can use the `index` attribute to specify explicitly the index of constructor arguments, as the following example shows:

您可以使用 index 属性显式指定构造函数参数的索引，如以下示例所示：

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

In addition to resolving the ambiguity of multiple simple values, specifying an index resolves ambiguity where a constructor has two arguments of the same type.

除了解决多个简单值的歧义之外，指定索引还可以解决构造函数具有相同类型的两个参数的歧义。

```
The index is 0-based.
索引从 0 开始
```

Constructor argument name 构造函数参数名称

You can also use the constructor parameter name for value disambiguation, as the following example shows:

您还可以使用构造函数参数名称进行值消除歧义，如以下示例所示：

```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

Keep in mind that, to make this work out of the box, your code must be compiled with the debug flag enabled so that Spring can look up the parameter name from the constructor. If you cannot or do not want to compile your code with the debug flag, you can use @ConstructorProperties JDK annotation to explicitly name your constructor arguments. The sample class would then have to look as follows:

请记住，为了使这项工作开箱即用，必须在启用调试标志的情况下编译代码，以便 Spring 可以从构造函数中查找参数名称。 如果您不能或不想使用 debug 标志编译代码，则可以使用@ConstructorProperties JDK 批注显式命名构造函数参数。 然后，示例类必须如下所示：

```
package examples;

public class ExampleBean {

    // Fields omitted

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}

```

##### Setter-based Dependency Injection 基于 Setter 的依赖注入

Setter-based DI is accomplished by the container calling setter methods on your beans after invoking a no-argument constructor or a no-argument static factory method to instantiate your bean.  

基于 setter 的依赖注入是在调用无参数构造函数或无参数静态工厂方法来实例化 bean 之后，通过容器调用 bean 上的 setter 方法来完成的。

The following example shows a class that can only be dependency-injected by using pure setter injection. This class is conventional Java. It is a POJO that has no dependencies on container specific interfaces, base classes, or annotations.

以下示例显示了一个只能通过使用纯 setter 注入进行依赖注入的类。 这个类是传统的 Java。 它是一个 POJO，它不依赖于容器特定的接口，基类或注释。

```
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on the MovieFinder
    private MovieFinder movieFinder;

    // a setter method so that the Spring container can inject a MovieFinder
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}

```

The `ApplicationContext` supports constructor-based and setter-based DI for the beans it manages. It also supports setter-based DI after some dependencies have already been injected through the constructor approach. You configure the dependencies in the form of a `BeanDefinition`, which you use in conjunction with `PropertyEditor` instances to convert properties from one format to another. However, most Spring users do not work with these classes directly (that is, programmatically) but rather with XML `bean` definitions, annotated components (that is, classes annotated with `@Component`,`@Controller`, and so forth), or `@Bean` methods in Java-based `@Configuration` classes. These sources are then converted internally into instances of `BeanDefinition` and used to load an entire Spring IoC container instance.

ApplicationContext 支持它管理的 bean 的基于构造函数和基于 setter 的依赖注入。 在通过构造函数方法注入了一些依赖项之后，它还支持基于 setter 的依赖注入。 您可以以 BeanDefinition 的形式配置依赖项，并将其与 PropertyEditor 实例结合使用，以将属性从一种格式转换为另一种格式。 但是，大多数 Spring 用户不直接使用这些类（即以编程方式），而是使用 XML bean 定义，带注释的组件（即使用@Component，@Controller 等注释的类）或者@Bean 方法。 基于 Java 的@Configuration 类。 然后，这些源在内部转换为 BeanDefinition 的实例，并用于加载整个 Spring IoC 容器实例。

```
Constructor-based or setter-based DI? 使用基于构造函数还是基于 setter 注入的依赖注入？
Since you can mix constructor-based and setter-based DI, it is a good rule of thumb to use constructors for mandatory dependencies and setter methods or configuration methods for optional dependencies. Note that use of the @Required annotation on a setter method can be used to make the property be a required dependency.
由于您可以混合基于构造函数和基于 setter 的依赖注入，因此将构造函数用于强制依赖项，setter 方法用于可选依赖项的配置方法是一个很好的经验法则。 请注意，在 setter 方法上使用 @Required 注释可用于使属性成为必需的依赖项。
The Spring team generally advocates constructor injection, as it lets you implement application components as immutable objects and ensures that required dependencies are not null. Furthermore, constructor-injected components are always returned to the client (calling) code in a fully initialized state. As a side note, a large number of constructor arguments is a bad code smell, implying that the class likely has too many responsibilities and should be refactored to better address proper separation of concerns.
Spring 团队通常提倡构造函数注入，因为它允许您将应用程序组件实现为不可变对象，并确保所需的依赖项不为 null。 此外，构造函数注入的组件始终以完全初始化的状态返回到客户端（调用）代码。 作为旁注，大量的构造函数参数是一个糟糕的代码，暗示该类可能有太多的责任，应该重构以更好地解决关注点的正确分离。

Setter injection should primarily only be used for optional dependencies that can be assigned reasonable default values within the class. Otherwise, not-null checks must be performed everywhere the code uses the dependency. One benefit of setter injection is that setter methods make objects of that class amenable to reconfiguration or re-injection later. Management through JMX MBeans is therefore a compelling use case for setter injection.
Setter 注入应主要仅用于可在类中指定合理默认值的可选依赖项。 否则，必须在代码使用依赖项的任何位置执行非空检查。 setter 注入的一个好处是 setter 方法使该类的对象可以在以后重新配置或重新注入。 因此，通过 JMX MBean 进行管理是二次注入的一个引人注目的用例。

Use the DI style that makes the most sense for a particular class. Sometimes, when dealing with third-party classes for which you do not have the source, the choice is made for you. For example, if a third-party class does not expose any setter methods, then constructor injection may be the only available form of DI.
使用对特定类最有意义的依赖注入形式。 有时，在处理您没有源的第三方类时，选择权不在您手中。 例如，如果第三方类没有公开任何 setter 方法，那么构造函数注入可能是唯一可用的 DI 形式。

```

##### Dependency Resolution Process 依赖性解决过程

The container performs bean dependency resolution as follows:

容器执行 bean 依赖性解析，如下所示：

- The `ApplicationContext` is created and initialized with configuration metadata that describes all the beans. Configuration metadata can be specified by XML, Java code, or annotations.
- 使用描述所有 bean 的配置元数据创建并初始化`ApplicationContext`。 配置元数据可以由 XML，Java 代码或注释指定。
- For each bean, its dependencies are expressed in the form of properties, constructor arguments, or arguments to the static-factory method( if you use that instead of a normal constructor). These dependencies are provided to the bean, when the bean is actually created.
- 对于每个 bean，它的依赖关系以属性，构造函数参数或 static-factory 方法的参数的形式表示（如果使用它而不是普通的构造函数）。 实际创建 bean 时，会将这些依赖项提供给 bean。
- Each property or constructor argument is an actual definition of the value to set, or a reference to another bean in the container.
- 每个属性或构造函数参数都是要设置的值的实际定义，或者是对容器中另一个 bean 的引用。
- Each property or constructor argument that is a value is converted from its specified format to the actual type of that property or constructor argument. By default, Spring can convert a value supplied in string format to all built-in types, such as `int`, `long`, `String`, `boolean`, and so forth.
- 作为值的每个属性或构造函数参数都从其指定的格式转换为该属性或构造函数参数的实际类型。 默认情况下，Spring 可以将以字符串格式提供的值转换为所有内置类型，例如`int`，`long`，`String`，`boolean`等等。

The Spring container validates the configuration of each bean as the container is created. However, the bean properties themselves are not set until the bean is actually created. Beans that are singleton-scoped and set to be pre-instantiated (the default) are created when the container is created. Scopes are defined in [Bean Scopes](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes). Otherwise, the bean is created only when it is requested. Creation of a bean potentially causes a graph of beans to be created, as the bean’s dependencies and its dependencies' dependencies (and so on) are created and assigned. Note that resolution mismatches among those dependencies may show up late — that is, on first creation of the affected bean.

Spring 容器在创建容器时验证每个 bean 的配置。 但是，在实际创建 bean 之前，不会设置 bean 属性本身。 创建容器时会创建单例作用域并设置为预先实例化（默认值）的 Bean。 范围在[Bean Scopes](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes) 中定义。 否则，仅在请求时才创建 bean。 创建 bean 可能会导致创建 bean 的图，因为 bean 的依赖关系及其依赖关系（依此类推）被创建和分配。 请注意，这些依赖项之间的解决方案不匹配可能会显示较晚 - 也就是说，首次创建受影响的 bean 时（才会体现）。

```
Circular dependencies 循环依赖
If you use predominantly constructor injection, it is possible to create an unresolvable circular dependency scenario.
如果您主要使用构造函数注入，则可能创建无法解析的循环依赖关系场景。

For example: Class A requires an instance of class B through constructor injection, and class B requires an instance of class A through constructor injection. If you configure beans for classes A and B to be injected into each other, the Spring IoC container detects this circular reference at runtime, and throws a BeanCurrentlyInCreationException.
例如：类 A 通过构造函数注入需要类 B 的实例，而类 B 通过构造函数注入需要类 A 的实例。 如果为 A 类和 B 类配置 bean 以便相互注入，则 Spring IoC 容器会在运行时检测此循环引用，并抛出 BeanCurrentlyInCreationException。

One possible solution is to edit the source code of some classes to be configured by setters rather than constructors. Alternatively, avoid constructor injection and use setter injection only. In other words, although it is not recommended, you can configure circular dependencies with setter injection.
一种可能的解决方案是编辑由 setter 而不是构造函数配置的某些类的源代码。 或者，避免构造函数注入并仅使用 setter 注入。 换句话说，尽管不推荐使用，但您可以使用 setter 注入配置循环依赖项。

Unlike the typical case (with no circular dependencies), a circular dependency between bean A and bean B forces one of the beans to be injected into the other prior to being fully initialized itself (a classic chicken-and-egg scenario).
与典型情况（没有循环依赖）不同，bean A 和 bean B 之间的循环依赖强制其中一个 bean 在完全初始化之前被注入另一个 bean（一个经典的鸡与鸡蛋场景）。
```

You can generally trust Spring to do the right thing. It detects configuration problems, such as references to non-existent beans and circular dependencies, at container load-time. Spring sets properties and resolves dependencies as late as possible, when the bean is actually created. This means that a Spring container that has loaded correctly can later generate an exception when you request an object if there is a problem creating that object or one of its dependencies — for example, the bean throws an exception as a result of a missing or invalid property. This potentially delayed visibility of some configuration issues is why `ApplicationContext` implementations by default pre-instantiate singleton beans. At the cost of some upfront time and memory to create these beans before they are actually needed, you discover configuration issues when the `ApplicationContext` is created, not later. You can still override this default behavior so that singleton beans initialize lazily, rather than being pre-instantiated.

你通常可以相信 Spring 做正确的事。它在容器加载时检测配置问题，例如对不存在的 bean 和循环依赖关系的引用。当实际创建 bean 时，Spring 会尽可能晚地设置属性并解析依赖关系。这意味着，如果在创建该对象或其中一个依赖项时出现问题，则在请求对象时，正确加载的 Spring 容器可以在以后生成异常 - 例如，bean 因缺失或无效而抛出异常属性。这可能会延迟一些配置问题的可见性，这就是为什么默认情况下`ApplicationContext`实现预先实例化单例 bean。以实际需要之前创建这些 bean 的一些前期时间和内存为代价，在创建`ApplicationContext`时发现配置问题，而不是以后。您仍然可以覆盖此默认行为，以便单例 bean 可以懒惰地初始化，而不是预先实例化。

If no circular dependencies exist, when one or more collaborating beans are being injected into a dependent bean, each collaborating bean is totally configured prior to being injected into the dependent bean. This means that, if bean A has a dependency on bean B, the Spring IoC container completely configures bean B prior to invoking the setter method on bean A. In other words, the bean is instantiated (if it is not a pre-instantiated singleton), its dependencies are set, and the relevant lifecycle methods (such as a [configured init method](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean) or the [InitializingBean callback method](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean)) are invoked.

如果不存在循环依赖关系，当一个或多个协作 bean 被注入依赖 bean 时，每个协作 bean 在被注入依赖 bean 之前完全配置。 这意味着，如果 bean A 依赖于 bean B，则 Spring IoC 容器在调用 bean A 上的 setter 方法之前完全配置 bean B.换句话说，bean 被实例化（如果它不是预先实例化的单例 ），它的依赖关系，以及相关的生命周期方法（例如[配置的 init 方法 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean) 或[InitializingBean 回调方法 ](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lifecycle-initializingbean)）被调用。

##### Examples of Dependency Injection 依赖注入的示例

The following example uses XML-based configuration metadata for setter-based DI. A small part of a Spring XML configuration file specifies some bean definitions as follows:

以下示例将基于 XML 的配置元数据用于基于 setter 的依赖注入。 Spring XML 配置文件的一小部分指定了一些 bean 定义，如下所示：

```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

The following example shows the corresponding `ExampleBean` class:

以下示例显示了相应的 ExampleBean 类：

```
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

In the preceding example, setters are declared to match against the properties specified in the XML file. The following example uses constructor-based DI:

在前面的示例中，声明 setter 与 XML 文件中指定的属性匹配。 以下示例使用基于构造函数的依赖注入：

```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- constructor injection using the nested ref element -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
    </constructor-arg>

    <!-- constructor injection using the neater ref attribute -->
    <constructor-arg ref="yetAnotherBean"/>

    <constructor-arg type="int" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

The following example shows the corresponding `ExampleBean` class:

以下示例显示了相应的 ExampleBean 类：

```
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public ExampleBean(
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        this.beanOne = anotherBean;
        this.beanTwo = yetAnotherBean;
        this.i = i;
    }
}
```

The constructor arguments specified in the bean definition are used as arguments to the constructor of the `ExampleBean`.

bean 定义中指定的构造函数参数用作 ExampleBean 的构造函数的参数。

Now consider a variant of this example, where, instead of using a constructor, Spring is told to call a `static` factory method to return an instance of the object:

现在考虑这个例子的变体，其中，不是使用构造函数，而是告诉 Spring 调用静态工厂方法来返回对象的实例：

```
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

The following example shows the corresponding `ExampleBean` class:

以下示例显示了相应的 ExampleBean 类：

```
public class ExampleBean {

    // a private constructor
    private ExampleBean(...) {
        ...
    }

    // a static factory method; the arguments to this method can be
    // considered the dependencies of the bean that is returned,
    // regardless of how those arguments are actually used.
    public static ExampleBean createInstance (
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {

        ExampleBean eb = new ExampleBean (...);
        // some other operations...
        return eb;
    }
```

Arguments to the `static` factory method are supplied by `<constructor-arg/>` elements, exactly the same as if a constructor had actually been used. The type of the class being returned by the factory method does not have to be of the same type as the class that contains the `static` factory method (although, in this example, it is). An instance (non-static) factory method can be used in an essentially identical fashion (aside from the use of the `factory-bean` attribute instead of the `class` attribute), so we do not discuss those details here.

静态工厂方法的参数由 <constructor-arg /> 元素提供，与实际使用的构造函数完全相同。 工厂方法返回的类的类型不必与包含静态工厂方法的类相同（尽管在本例中，它是）。 实例（非静态）工厂方法可以以基本相同的方式使用（除了使用 factory-bean 属性而不是 class 属性），因此我们不在这里讨论这些细节。