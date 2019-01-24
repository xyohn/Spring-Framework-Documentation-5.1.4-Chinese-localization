# Core Technologies 核心技术 
Version 5.1.4.RELEASE

This part of the reference documentation covers all the technologies that are absolutely integral to the Spring Framework.

这部分参考文档涵盖了 Spring Framework 绝对不可或缺的所有技术。

Foremost amongst these is the Spring Framework’s Inversion of Control (IoC) container. A thorough treatment of the Spring Framework’s IoC container is closely followed by comprehensive coverage of Spring’s Aspect-Oriented Programming (AOP) technologies. The Spring Framework has its own AOP framework, which is conceptually easy to understand and which successfully addresses the 80% sweet spot of AOP requirements in Java enterprise programming.

其中最重要的是 Spring Framework 的控制反转（IoC）容器。紧随其后的是 Spring 面向切面编程（AOP）技术。 Spring Framework有自己的AOP框架，它在概念上易于理解，并且成功地解决了 Java企业编程中 AOP 要求的80％最佳点。

Coverage of Spring’s integration with AspectJ (currently the richest — in terms of features — and certainly most mature AOP implementation in the Java enterprise space) is also provided.

除此之外还提供了 Spring 与 AspectJ 集成的覆盖范围（目前最丰富的 - 在功能方面 - 当然也是 Java 企业领域中最成熟的AOP实现）。

## 1. The IoC Container IoC容器

This chapter covers Spring’s Inversion of Control (IoC) container.

本章介绍Spring的控制反转（IoC）容器。

### 1.1. Introduction to the Spring IoC Container and Beans Spring IoC容器和Bean简介

This chapter covers the Spring Framework implementation of the Inversion of Control (IoC) principle. (See [Inversion of Control](https://docs.spring.io/spring/docs/current/spring-framework-reference/overview.html#background-ioc).) IoC is also known as dependency injection (DI). It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.

本章介绍了控制反转（IoC）原理的 Spring Framework 实现。（参见控制反转。）IoC 也称为依赖注入（DI）。这是一个对象定义其依赖关系的过程（即，它们使用的其他对象）。这些对象只能通过构造函数的参数，工厂方法的参数或在从工厂方法构造或返回对象实例后在对象实例上设置的属性。 然后容器在创建 bean 时注入这些依赖项。此过程基本上是 bean本身的逆操作（因此得名控制反转），通过使用类的直接构造或诸如服务定位器模式的机制来控制其依赖关系的实例化或位置。

The `org.springframework.beans` and `org.springframework.context` packages are the basis for Spring Framework’s IoC container. The BeanFactory interface provides an advanced configuration mechanism capable of managing any type of object. ApplicationContext is a sub-interface of BeanFactory. It adds:

`org.springframework.beans` 和 `org.springframework.context` 包是Spring Framework的IoC容器的基础。 BeanFactory 接口提供了一种能够管理任何类型对象的高级配置机制。 ApplicationContext 是BeanFactory的子接口。它补充了：

Easier integration with Spring’s AOP features

更容易与 Spring 的AOP功能集成

Message resource handling (for use in internationalization)

消息资源处理（用于国际化）

Event publication

事件发布

Application-layer specific contexts such as the WebApplicationContext for use in web applications.

特定于应用程序层的上下文，例如WebApplicationContext，用于Web应用程序。

In short, the BeanFactory provides the configuration framework and basic functionality, and the ApplicationContext adds more enterprise-specific functionality. The ApplicationContext is a complete superset of the BeanFactory and is used exclusively in this chapter in descriptions of Spring’s IoC container. For more information on using the BeanFactory instead of the ApplicationContext, see The [BeanFactory](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-beanfactory).

简而言之，BeanFactory 提供配置框架和基本功能，ApplicationContext 添加了更多特定于企业的功能。 ApplicationContext 是 BeanFactory 的完整超集，在本章中关于 Spring IoC容器的描述将只使用 ApplicationContext。有关使用 BeanFactory 而不是 ApplicationContext 的更多信息，请参阅The BeanFactory。

In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.

在 Spring 中，构成应用程序主干并由 Spring IoC 容器管理的对象称为 bean。 bean 是一个由 Spring IoC 容器实例化，组装和管理的对象。换言之，bean 只是应用程序中许多对象之一。 Beans 及其之间的依赖关系反映在容器使用的配置元数据中。


### 1.2. Container Overview 容器概述

The `org.springframework.context.ApplicationContext` interface represents the Spring IoC container and is responsible for instantiating, configuring, and assembling the beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. The configuration metadata is represented in XML, Java annotations, or Java code. It lets you express the objects that compose your application and the rich interdependencies between those objects.

`org.springframework.context.ApplicationContext`接口表示 Spring IoC 容器，负责实例化，配置和组装 bean 。容器通过读取配置元数据获取有关实例化，配置和组装的对象的指令。配置元数据以 XML， Java 注解或 Java 代码表示。它允许您表达组成应用程序的对象以及这些对象之间丰富的相互依赖性。

Several implementations of the ApplicationContext interface are supplied with Spring. In stand-alone applications, it is common to create an instance of ClassPathXmlApplicationContext or FileSystemXmlApplicationContext. While XML has been the traditional format for defining configuration metadata, you can instruct the container to use Java annotations or code as the metadata format by providing a small amount of XML configuration to declaratively enable support for these additional metadata formats.

Spring 提供了 ApplicationContext 接口的几个实现。在独立应用程序中，通常会创建ClassPathXmlApplicationContext 或 FileSystemXmlApplicationContext 的实例。虽然 XML 是定义配置元数据的传统格式，但您可以通过提供少量 XML配置 来声明容器使用 Java注解 或 Java 代码作为元数据格式，以声明方式启用对这些其他元数据格式的支持。

In most application scenarios, explicit user code is not required to instantiate one or more instances of a Spring IoC container. For example, in a web application scenario, a simple eight (or so) lines of boilerplate web descriptor XML in the web.xml file of the application typically suffices (see Convenient ApplicationContext Instantiation for Web Applications). If you use the Spring Tool Suite (an Eclipse-powered development environment), you can easily create this boilerplate configuration with a few mouse clicks or keystrokes.

在大多数应用程序方案中，不需要通过配置显式用户代码来实例化 Spring IoC 容器的一个或多个实例。例如，在Web应用程序场景中，应用程序的 web.xml 文件中的只需简单八行（左右）的Web描述符通常就足够了（请参阅 Web应用程序的便捷 ApplicationContext 实例）。如果您使用Spring Tool Suite（基于Eclipse的开发环境），只需点击几下鼠标或按键即可轻松创建此样板配置。

The following diagram shows a high-level view of how Spring works. Your application classes are combined with configuration metadata so that, after the ApplicationContext is created and initialized, you have a fully configured and executable system or application.

下图显示了Spring如何工作的高级视图。您的应用程序类与配置元数据相结合，以便在创建和初始 化 ApplicationContext 之后，您拥有一个完全配置且可执行的系统或应用程序。


![Figure 1. The Spring IoC container](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/container-magic.png)
Figure 1. The Spring IoC container

#### 1.2.1. Configuration Metadata 配置元数据

As the preceding diagram shows, the Spring IoC container consumes a form of configuration metadata. This configuration metadata represents how you, as an application developer, tell the Spring container to instantiate, configure, and assemble the objects in your application.

如上图所示，Spring IoC 容器使用一种配置元数据的形式。 此配置元数据表示您作为应用程序开发人员如何告诉Spring容器在应用程序中实例化，配置和组装对象。

Configuration metadata is traditionally supplied in a simple and intuitive XML format, which is what most of this chapter uses to convey key concepts and features of the Spring IoC container.

传统上，配置元数据以简单直观的XML格式提供，本章大部分内容用于传达Spring IoC容器的关键概念和功能。

>XML-based metadata is not the only allowed form of configuration metadata. The Spring IoC container itself is totally decoupled from the format in which this configuration metadata is actually written. These days, many developers choose Java-based configuration for their Spring applications.
>基于XML的元数据不是唯一允许的配置元数据形式。 Spring IoC容器本身完全与实际编写此配置元数据的格式分离。 目前，许多开发人员为其Spring应用程序选择基于Java的配置。

For information about using other forms of metadata with the Spring container, see:
有关在Spring容器中使用其他形式的元数据的信息，请参阅：

* Annotation-based configuration: Spring 2.5 introduced support for annotation-based configuration metadata.

基于注释的配置：Spring 2.5引入了对基于注释的配置元数据的支持

* Java-based configuration: Starting with Spring 3.0, many features provided by the Spring JavaConfig project became part of the core Spring Framework. Thus, you can define beans external to your application classes by using Java rather than XML files. To use these new features, see the @Configuration, @Bean, @Import, and @DependsOn annotations.

基于Java的配置：从Spring 3.0开始，Spring JavaConfig 项目提供的许多功能成为核心Spring Framework的一部分。 因此，您可以使用 Java 而不是 XML 文件在应用程序类外部定义 bean。 要使用这些新功能，请参阅@Configuration，@ Bean，@ Import和@DependsOn注释。

These bean definitions correspond to the actual objects that make up your application. Typically, you define service layer objects, data access objects (DAOs), presentation objects such as Struts Action instances, infrastructure objects such as Hibernate SessionFactories, JMS Queues, and so forth. Typically, one does not configure fine-grained domain objects in the container, because it is usually the responsibility of DAOs and business logic to create and load domain objects. However, you can use Spring’s integration with AspectJ to configure objects that have been created outside the control of an IoC container. See Using AspectJ to dependency-inject domain objects with Spring.

这些 bean 定义对应于构成应用程序的实际对象。 通常，您定义服务层对象，数据访问对象（DAO），表示对象（如 Struts Action 实例），基础结构对象（如Hibernate SessionFactories，JMS队列等）。 通常，不会在容器中配置细粒度域对象，因为 DAO 和业务逻辑通常负责创建和加载域对象。 但是，您可以使用 Spring 与 AspectJ 的集成来配置在 IoC 容器控制之外创建的对象。 请参阅使用 AspectJ 使用 Spring 依赖注入域对象。

The following example shows the basic structure of XML-based configuration metadata:
以下示例显示了基于XML的配置元数据的基本结构：

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
	The id attribute is a string that identifies the individual bean definition.
	id属性是一个标识单个bean定义的字符串。
	The class attribute defines the type of the bean and uses the fully qualified classname.
	class属性定义bean的类型并使用classname的全限定名
The value of the id attribute refers to collaborating objects. The XML for referring to collaborating objects is not shown in this example. See Dependencies for more information.
属性id的值指的是协作对象。 在此示例中未显示用于引用协作对象的XML。 有关更多信息，请参阅依赖项

#### 1.2.2. Instantiating a Container 实例化容器

The location path or paths supplied to an ApplicationContext constructor are resource strings that let the container load configuration metadata from a variety of external resources, such as the local file system, the Java CLASSPATH, and so on.
提供给 ApplicationContext 构造函数的位置路径是有关资源的字符串，它允许容器从各种外部资源（如本地文件系统，Java CLASSPATH等）加载配置元数据。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```
>After you learn about Spring’s IoC container, you may want to know more about Spring’s Resource abstraction (as described in Resources), which provides a convenient mechanism for reading an InputStream from locations defined in a URI syntax. In particular, Resource paths are used to construct applications contexts, as described in [Application Contexts and Resource Paths](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#resources-app-ctx).
>在了解了Spring的IoC容器之后，您可能想要了解有关 Spring 的资源抽象的更多信息（如参考资料中所述），它提供了一种从URI语法中定义的位置读取InputStream的便捷机制。 特别是，资源路径用于构建应用程序上下文，如[Application Contexts and Resource Paths]中所述。

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

在前面的例子中，服务层包含```PetStoreServiceImpl```类和两个```JpaAccountDao```和```JpaItemDao```的数据访问对象（基于JPA Object-Relational 映射标准）。 ```property name```元素引用JavaBean属性的名称，```ref```元素引用另一个bean定义的名称。 ```id```和```ref```元素之间的这种联系表达了协作对象之间的依赖关系。 有关配置对象的依赖关系的详细信息，请参阅依赖关系。

##### Composing XML-based Configuration Metadata 编写基于XML的配置元数据

It can be useful to have bean definitions span multiple XML files. Often, each individual XML configuration file represents a logical layer or module in your architecture.

让bean定义跨越多个XML文件会很有用。 通常，每个单独的XML配置文件都代表架构中的逻辑层或模块。

You can use the application context constructor to load bean definitions from all these XML fragments. This constructor takes multiple``` Resource``` locations, as was shown in the previous section. Alternatively, use one or more occurrences of the``` <import/>``` element to load bean definitions from another file or files. The following example shows how to do so:
您可以使用应用程序上下文构造函数从所有这些XML片段加载bean定义。 这个构造函数需要多个```Resource```位置，如上一节所示。 或者，使用一个或多个```<import />```元素来从另一个或多个文件加载bean定义。 以下示例显示了如何执行此操作：

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

在前面的示例中，外部bean定义从三个文件加载：```services.xml```，```messageSource.xml```和```themeSource.xml```。 所有位置路径都与执行导入的定义文件相关，因此services.xml必须与执行导入的文件位于相同的目录或 classpath 路径位置，而```messageSource.xml```和```themeSource.xml ```必须位于导入文件位置下方的资源位置。 如您所见，前导斜杠是可忽略的。 但是，鉴于这些路径是相对的，最好不要使用斜杠。 根据Spring Schema，正在导入的文件的内容（包括顶级<beans />元素）必须是有效的XML bean定义。

>It is possible, but not recommended, to reference files in parent directories using a relative "../" path. Doing so creates a dependency on a file that is outside the current application. In particular, this reference is not recommended for ```classpath```: URLs (for example,```classpath:../services.xml```), where the runtime resolution process chooses the “nearest” classpath root and then looks into its parent directory. Classpath configuration changes may lead to the choice of a different, incorrect directory.
使用相对“../”路径引用父目录中的文件是可以的，但不建议这么做。因为 这样做会对当前应用程序之外的文件创建依赖关系。 特别是，这个引用不建议用于```classpath```：URL（例如，```classpath：../ services.xml```），其中运行时解析过程选择“最近的”类路径根 然后查看其父目录。 类路径配置更改可能导致选择不同的，不正确的目录。
>You can always use fully qualified resource locations instead of relative paths: for example, ```file:C:/config/services.xml ```or ```classpath:/config/services.xml```. However, be aware that you are coupling your application’s configuration to specific absolute locations. It is generally preferable to keep an indirection for such absolute locations — for example, through "${…}" placeholders that are resolved against JVM system properties at runtime.
您始终可以使用完全限定的资源位置而不是相对路径：例如，```file：C：/config/services.xml```或```classpath：/ config / services.xml```。 但是，请注意您将应用程序的配置与特定的绝对位置耦合。 通常最好为这些绝对位置保持间接联系 - 例如，通过在运行时针对JVM系统属性解析的“$ {...}”占位符。

The namespace itself provices the import directive feature. Further configuration features beyond plain bean definitions are available in a selection of XML namespaces provided by Spring — for example, the ```context``` and ```util``` namespaces.
命名空间本身提供了import指令功能。 除了普通bean定义之外的其他配置功能可以在Spring提供的一系列XML命名空间中找到 - 例如，```context```和```util```命名空间。

##### The Groovy Bean Definition DSL

As a further example for externalized configuration metadata, bean definitions can also be expressed in Spring’s Groovy Bean Definition DSL, as known from the Grails framework. Typically, such configuration live in a ".groovy" file with the structure shown in the following example:

作为配置元数据的另一个示例，bean定义也可以在 Spring 的 Groovy Bean 定义DSL中表示，如 Grails 框架中所知。 通常，此类配置位于“.groovy”文件中，其结构如下例所示：

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

此配置样式在很大程度上等同于XML bean定义，甚至支持Spring的XML配置命名空间。 它还允许通过`importBeans`指令导入XML bean定义文件。

#### 1.2.3. Using the Container 使用容器

The `ApplicationContext` is the interface for an advanced factory capable of maintaining a registry of different beans and their dependencies. By using the method `T getBean(String name, Class<T> requiredType)`, you can retrieve instances of your beans.

`ApplicationContext`是高级工厂的接口，能够维护不同bean及其依赖项的注册表。 通过使用方法`T getBean（String name，Class <T> requiredType）`，您可以检索bean的实例。

The `ApplicationContext` lets you read bean definitions and access them, as the following example shows:

`ApplicationContext`允许您读取bean定义并访问它们，如以下示例所示：

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

With Groovy configuration, bootstrapping looks very similar. It has a different context implementation class which is Groovy-aware (but also understands XML bean definitions). The following example shows Groovy configuration:

使用Groovy配置，启动形式看起来非常相似。 它有一个不同的上下文实现类，它是Groovy-aware（但也理解XML bean定义）。 以下示例显示了Groovy配置：

```java
ApplicationContext context = new GenericGroovyApplicationContext("services.groovy", "daos.groovy");
```

The most flexible variant is `GenericApplicationContext` in combination with reader delegates — for example, with `XmlBeanDefinitionReader` for XML files, as the following example shows:

最灵活的变体是`GenericApplicationContext`与读者委托相结合 - 例如，对于XML文件使用`XmlBeanDefinitionReader`，如下例所示：

```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```

You can also use the `GroovyBeanDefinitionReader` for Groovy files, as the following example shows:

您还可以对Groovy文件使用`GroovyBeanDefinitionReader`，如以下示例所示：

```java
GenericApplicationContext context = new GenericApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("services.groovy", "daos.groovy");
context.refresh();
```

You can mix and match such reader delegates on the same `ApplicationContext`, reading bean definitions from diverse configuration sources.

您可以在相同的`ApplicationContext`上混合和匹配这些读者，从不同的配置源读取bean定义。

You can then use `getBean` to retrieve instances of your beans. The `ApplicationContext` interface has a few other methods for retrieving beans, but, ideally, your application code should never use them. Indeed, your application code should have no calls to the `getBean()` method at all and thus have no dependency on Spring APIs at all. For example, Spring’s integration with web frameworks provides dependency injection for various web framework components such as controllers and JSF-managed beans, letting you declare a dependency on a specific bean through metadata (such as an autowiring annotation).

然后，您可以使用`getBean`来检索bean的实例。 `ApplicationContext`接口有一些其他的方法来检索bean，但理想情况下，你的应用程序代码永远不应该使用它们。 实际上，您的应用程序代码根本不应该调用`getBean（）`方法，因此根本不依赖于Spring API。 例如，Spring与Web框架的集成为各种Web框架组件（如控制器和JSF托管bean）提供依赖注入，允许您通过元数据（例如自动装配注释）声明对特定bean的依赖性。

### 1.3. Bean Overview Bean 概述

A Spring IoC container manages one or more beans. These beans are created with the configuration metadata that you supply to the container (for example, in the form of XML `<bean/>` definitions).

Spring IoC容器管理一个或多个bean。 这些bean是使用您提供给容器的配置元数据创建的（例如，以XML` <bean />`定义的形式）。

Within the container itself, these bean definitions are represented as `BeanDefinition` objects, which contain (among other information) the following metadata:

在容器本身内，这些bean定义表示为`BeanDefinition`对象，其中包含（以及其他信息）以下元数据：

- A package-qualified class name: typically, the actual implementation class of the bean being defined.
- 包限定类名：通常是定义的bean的实际实现类。
- Bean behavioral configuration elements, which state how the bean should behave in the container (scope, lifecycle callbacks, and so forth).
- Bean行为配置元素，它说明bean在容器中的行为方式（范围，生命周期回调等）。
- References to other beans that are needed for the bean to do its work. These references are also called collaborators or dependencies.
- 引用bean执行其工作所需的其他bean。 这些引用也称为协作者或依赖项。
- Other configuration settings to set in the newly created object — for example, the size limit of the pool or the number of connections to use in a bean that manages a connection pool.
- 要在新创建的对象中设置的其他配置设置 - 例如，池的大小限制或在管理连接池的Bean中使用的连接数。

This metadata translates to a set of properties that make up each bean definition. The following table describes these properties:

此元数据转换为构成每个bean定义的一组属性。 下表描述了这些属性：

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

>Bean metadata and manually supplied singleton instances need to be registered as early as possible, in order for the container to properly reason about them during autowiring and other introspection steps. While overriding existing metadata and existing singleton instances is supported to some degree, the registration of new beans at runtime (concurrently with live access to the factory) is not officially supported and may lead to concurrent access exceptions, inconsistent state in the bean container, or both.
>