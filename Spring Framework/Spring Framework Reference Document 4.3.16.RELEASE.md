# Part Ⅲ. 核心技术 #

## 7. IoC 容器 ##

## 7.1 Spring IoC 容器和 beans 介绍 ##

## 7.2 容器概述 ##

接口`org.springframework.context.ApplicationContext`表示Spring IoC 容器，并且负责实例化，配置和组装上述beans。

![Alt text](https://docs.spring.io/spring/docs/4.3.16.RELEASE/spring-framework-reference/htmlsingle/images/container-magic.png)

### 7.2.1 配置元数据 ###

如上图所示，Spring Ioc 容器使用配置元数据的形式；这些元数据配置表示你作为一个应用开发者怎样告诉Spring容器在你的应用中实例化，配置和组装对象。

传统上，配置元数据以简单直观的XML格式提供，本章大部分采用这种方法来表达Spring IoC 容器的核心概念和特性。

Spring配置由容器必须管理的至少一个和通常不止一个的bean定义组成。基于XML配置元数据表示如`<bean/>`元素配置必须在顶级`<beans/>`元素内。基于Java配置通常在一个`@Configuration`注解类中使用`@Bean`来注解方法。



下面的示例展示了基于XML配置元数据的基本结构：

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



### 7.2.2 实例化一个容器 ###

实例化一个Spring IoC 容器是很简单的。传给`ApplicationContext`构造函数的一个或多个路径地址实际上是资源字符串，它允许容器从多种外部资源加载元数据，如本地文件系统，Java`CLASSPATH`，等等。

```java
ApplicationContext context = new ClassPathXmlApplication("services.xml", "daos.xml");
```



> Spring 





下面的示例展示了服务层对象`(services.xml)`配置文件：

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

下面的示例展示了数据存储对象`(daos.xml)`文件：

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

在上面的示例中，服务层包含`PetStoreServiceImpl`类，和类型为`JpaAccountDao`和`JpaItemDao`的两个数据存储对象（基于JPA对象/关系映射标准）。

#### 基于XML配置元数据的构成 ####

把bean跨越多个XML文件定义也是可用的。通常，每个单独的XML配置文件表示你架构中的一个逻辑层或模块。

你可以使用应用上下文构造函数从这些XML片段去加载 bean 定义。构造函数使用多个`Resource`路径，如上一节所示。或者，使用一个或多个出现的`<import/>`元素从其他一个或多个文件去加载 bean 定义。比如：

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

在上面的例子中，其他 bean 定义 从三个文件中被加载：`service.xml`，`messageSource.xml`和`themeSource.xml`。

### 7.4.4 懒加载 beans ###

在XML中，这个行为由`<bean/>`元素上的`lazy-init`属性控制，比如：

```xml
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.foo.AnotherBean"/>
```

当一个`ApplicationContext`用了上述配置，这个叫`lazy`的bean在`ApplicationContext`启动的时候就不急于预实例化，而`not.lazy`bean则预实例化。

但是，当一个懒加载bean

## ApplicationContext的附加功能 ##

### 7.15.1 使用MessageSource国际化 ###

`ApplicationContext` 接口继承自一个叫`MessageSource`的接口，因此提供了国际化(i18n)的功能。

- `String getMessage(String code, Object[] args, String default, Local loc)`：从`MessageSource`中检索一条消息的基本方法。如果指定地方没有找到消息，就使用默认消息。任何传过来的参数，都将使用标准库提供的`MessageFormat`功能变成替换值。

- `String getMessage(String code, Object[] args, Locale loc)`：基本和上一个方法一样，但有一点不同：没有指定默认消息，如果没有找到消息，抛出一个`NoSuchMessageException`。

- `String getMessage(MessageSourceResolvable resolvable, Locale locale)`：前面方法使用的所有特性也被封装在一个叫`MessageSourceResolvable`的类中，你可以使用此方法使用。


