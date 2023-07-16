# ssm框架

SSM（SpringMVC+Spring+MyBatis）是目前市场上最流行的开发web项目的框架，它由SpringMVC、Spring、MyBatis整合而成。SpringMVC框架负责接收浏览器发送的请求，并响应浏览器数据；Spring框架使用其核心IOC思想管理服务器中各个组件，使用AOP思想面向切面编程，在不改变源码的基础上实现功能增强；MyBatis框架封装JDBC，负责访问数据库，完成持久化操作

# spring框架

## Spring Framework特性 

非侵入式：使用 Spring Framework 开发应用程序时，Spring 对应用程序本身的结构影响非常 小。对领域模型可以做到零污染；对功能性组件也只需要使用几个简单的注解进行标记，完全不会 破坏原有结构，反而能将组件结构进一步简化。这就使得基于 Spring Framework 开发应用程序 时结构清晰、简洁优雅。

 控制反转：IOC——Inversion of Control，翻转资源获取方向。把自己创建资源、向环境索取资源 变成环境将资源准备好，我们享受资源注入。 

面向切面编程：AOP——Aspect Oriented Programming，在不修改源代码的基础上增强代码功 能。

 容器：Spring IOC 是一个容器，因为它包含并且管理组件对象的生命周期。组件享受到了容器化 的管理，替程序员屏蔽了组件创建过程中的大量细节，极大的降低了使用门槛，大幅度提高了开发 效率。

 组件化：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用 XML 和 Java 注解组合这些对象。这使得我们可以基于一个个功能明确、边界清晰的组件有条不紊的搭 建超大型复杂应用系统。

 声明式：很多以前需要编写代码才能实现的功能，现在只需要声明需求即可由框架代为实现。 

一站式：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库。而且 Spring 旗下的项目已经覆盖了广泛领域，很多方面的功能性需求可以在 Spring Framework 的基 础上全部使用 Spring 来实现。

## Spring Framework五大功能模块 

| 功能模块                | 功能介绍                                                    |
| ----------------------- | ----------------------------------------------------------- |
| Core Container          | 核心容器，在 Spring 环境下使用任何功能都必须基于 IOC 容器。 |
| AOP&Aspects             | 面向切面编程                                                |
| Testing                 | 提供了对 junit 或 TestNG 测试框架的整合。                   |
| Data Access/Integration | 提供了对数据访问/集成的功能。                               |
| Spring MVC              | 提供了面向Web应用程序的集成功能。                           |

# IOC依赖注入

## 什么是IOc

IOC是指  Inverse Of Control控制反转，在使用spring模块之前，对象都是自己在代码中去new创建，而使用了spring之后，对象的创建交给了spring容器。

注意**：所有要是用spring功能的对象都需要交给spring容器来创建。**

## 什么是DI

DI指Dependency Injection 。就是依赖注入

依赖：A要去完成某个功能，在这个过程中需要B的存在，如果没有B，A就无法实现功能。A对于B来说就是A依赖B

注入：注入就是赋值操作

依赖注入：就是给依赖的对象进行赋值操作。

在Spring中依赖注入，可以通过xml配置文件，以及注解的形式进行操作。

## IOC实例

创建spring工程在lib文件夹中导入下图jar包

![](D:\桌面\java\笔记\spring\assest\spring需要导入的jar包.png)

创建applicationContext.xml文件如果不导入spring包xml文件创建不了

![](D:\桌面\java\笔记\spring\assest\创建springxml.png)

创建建实体类

```
public class Person {
    private Integer id;
    private Integer age;
    private String name;
    private String phone;
    set,get方法
    }
```

编写applicationContext.xml文件

```
<!--
bean标签表示配置一个Bean对象，class属性表示配置Bean全类名(对象所在的路径)id唯一标识
property表示给bean中属性进行配值，name代表属性名，value代表赋的值
-->
    <bean class="com.atguigu.pojo.Person" id="p1">
        <property name="id" value="1"></property>
        <property name="name" value="haha"></property>
    </bean>
```

```
测试编写代码
@Test
    public void test(){
//        在使用spring的时候，需要现有spring容器（spring IOC容器或IOC容器）对象
//        在spring中有一个接口表示了这个容器对象ApplicationContext使用ClassPathXmlApplicationContext（"applicationContext.xml"）；
//        ClassPathXmlApplicationContext表示从类路径下加载xml配置文件创建spring容器对象
        ApplicationContext  applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
//        从fileSystemXMLApplicationContext从文件系统中读取xml文件生成spring容器。
ApplicationContext fileSystemXmlApplicationContext = new FileSystemXmlApplicationContext("src/applicationContext.xml");

//        applicationContext.getBean();从容器中获取指定的对象根据 id获取
        Person p1 = (Person) applicationContext.getBean("p1");
        System.out.println(p1.toString());
         Person bean = applicationContext.getBean(Person.class);

    }
```



### 有参

| 类型名                          | 简介                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| ClassPathXmlApplicationContext  | 通过读取类路径下的 XML 格式的配置文件创建 IOC 容器 对象      |
| FileSystemXmlApplicationContext | 通过文件系统路径读取 XML 格式的配置文件创建 IOC 容 器对象    |
| ConfigurableApplicationContext  | ApplicationContext 的子接口，包含一些扩展方法 refresh() 和 close() ，让 ApplicationContext 具有启动、 关闭和刷新上下文的能力。 |
| WebApplicationContext           | 专门为 Web 应用准备，基于 Web 环境创建 IOC 容器对 象，并将对象引入存入 ServletContext 域中。 |

### 构造创建实例

```
<!--使用construction-arg标签构造实体类有参构造器，使用有参构造器赋值根据函数参数定-->
    <bean class="com.atguigu.pojo.Person" id="conP1">
        <constructor-arg name="age" value="1" ></constructor-arg>
        <constructor-arg name="id" value="1"></constructor-arg>
        <constructor-arg name="name" value="1"></constructor-arg>
        <constructor-arg name="phone" value="1"></constructor-arg>
    </bean>

```

其余不变

#### bean标签中属性

```
<!--
bean标签表示配置一个Bean对象，
class属性表示配置Bean全类名(对象所在的路径)id唯一标识

property表示给bean中属性进行配值，name代表属性名，value代表赋的值
-->
<!--使用construction-arg标签构造实体类有参构造器，使用有参构造器赋值根据函数参数定
，name代表属性名，value代表赋的值
index属性表示参数的索引顺序。0表示第一个参数，1代表第二个参数，以此类推n表示n+1个参数
type指定参数的数据格式
-->
```

### 根据构造器参数类型注入选择构造器赋值

```
<bean class="com.atguigu.pojo.Person" id="p3">
        <constructor-arg  value="" index="0" type="java.lang.Integer"></constructor-arg>
        <constructor-arg  value="" index="1" type="java.lang.Integer"></constructor-arg>
        <constructor-arg  value="" index="2" type="java.lang.String"></constructor-arg>
        <constructor-arg  value="" index="3" type="java.lang.String"></constructor-arg>
 </bean>
```

### IOC的P命名空间

p命名空间可以以简短的形式通过set方法给属性赋值

使用p进行赋值系统自动回添加头文件

p名称空间书写规则：

P:属性名=“值”

```
<bean id="p6" class="com.atguigu.pojo.Person" p:id="6" p:age="7" p:name="18" p:phone="11"/>
```

### 测试null值的使用

```
<bean id="p7" class="com.atguigu.pojo.Person">
        <property name="name" ><null></null></property>
 </bean>
```

在属性标签内添加<null>标签便会赋值空值

### IOC子对象赋值（重点）

```
<!--    先在外面定义要赋值的实体类,然后在实体对象使用ref调用id名相同的被调用对象-->
    <bean id="b1" class="com.atguigu.pojo.Book" p:type="english" p:name="高一英语"></bean>
    <bean id="p8" class="com.atguigu.pojo.Person">
        <property name="book" ref="b1"></property>
    </bean>
```

#### 子对象内部Bean方式进行赋值

内部bean只能赋值使用，无法通过spring容器直接获取到（根据id获取不到），除非使用外层对象实例的get方法获取。内部属性bean可以不写。

```
 <bean id="p9" class="com.atguigu.pojo.Person">
        <property name="book">
<!--            内部bean-->
            <bean class="com.atguigu.pojo.Book" id="book">
                <property name="name" value="11"></property>
                <property name="type" value="22"></property>
            </bean>
        </property>
    </bean>
```

### IOC对List集合进行赋值

```
<!--bean对list集合进行赋值-->
    <bean class="com.atguigu.pojo.Person" id="p10">
        <property name="friend">
            <list>
<!--                list标签-->
                <value>ai</value>
                <value>a2</value>
            </list>
        </property>
    </bean>
```

### IOC对Map集合进行赋值

```
<bean class="com.atguigu.pojo.Person" id="p11">
        <property name="family">
<!--            map标签表示赋值类型是map集合，entry标签代表一个组键值对其中key属性和value属性不在解释-->
            <map>
                <entry key="" value="">11</entry>
            </map>
        </property>
    </bean>
```

### IOC对properties属性进行赋值

```
  <bean class="com.atguigu.pojo.Person" id="p12">
        <property name="prop">
<!--            props标签表示赋值的类型是properties，prop表示一个键值对，key表示键，标签中的内容表示值-->
            <props>
                <prop key="11" >ss</prop>
            </props>
        </property>
    </bean>
```

### IOC使用utils名称空间

util名称空间可以用来定义一些外部的集合，方便对象去引用。同时还可以通过容器的方式获取该集合

```
<!--    定义一个集合去引用-->
    <util:list id="list01">
        <value>01</value>
        <value>02</value>
        <value>03</value>
    </util:list>
    <bean class="com.atguigu.pojo.Person" id="p13">
        <property name="friend" ref="list01">
            
        </property>
    </bean>
```

### IOC对级联属性进行赋值

比如人的实体类中有book这实体类，book实体类中有name属性。便是通过book这实体类对book.name进行赋值。

```
<!--    使用级联属性进行赋值需要先进行创建实体类对象然后进行赋值，不然会报空指针异常-->
    <bean class="com.atguigu.pojo.Person" id="p14">
        <property name="book" ref="b1"></property>
        <property name="book.name" value="match"></property>
    </bean>
```

### IOC静态工厂方法创建对象

```
工厂类
public class PersonFactory {
    public static Person create(){
        return new Person();
    }
}
```

```
<!--    class属性+factory-method属性（使用create创建对象） 是静态工厂方法-->
    <bean class="com.atguigu.factory.PersonFactory" id="p15" factory-method="create"></bean>
```

### IOC非静态工厂实例

```
//    非静态工厂实例
   public Person create2(){
        return new Person();
    }
```

```
xml中创建
<!--    配置工厂实例-->
    <bean id="personFactory" class="com.atguigu.factory.PersonFactory"></bean>
<!--    factory-bean （根据工厂的id进行查找），factory-method这是工厂实例方法创建Bean对象-->
    <bean factory-bean="personFactory" factory-method="create2" id="p16"></bean>
```

### IOC的FactoryBean接口方式创建对象

步骤：

1、编写一个类去实现工厂接口

2、实现接口方法

3、到application.xml中配置

xml启动时会自动调用PersonFactoryBean中的getBean()方法

```
public class PersonFactoryBean implements FactoryBean<Person> {
//    创建bean对象
    @Override
    public Person getObject() throws Exception {
        return new Person();
    }

    @Override
    public Class<?> getObjectType() {
        return Person.class;
    }
//是否是单例
    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

```
<bean id="p17" class="com.atguigu.factory.PersonFactoryBean"></bean>
```

### bean配置信息的继承

```
  <bean id="parent" class="com.atguigu.pojo.Person">
        <property name="id" value="100"></property>
        <property name="name" value="person实例"></property>
        <property name="age" value="100"></property>
    </bean>
<!--    bean标签中parent属性可以按id查询用来指定继承那些Bean的配置信息，如果子类的信息与父类不相同时可以再子类中重新赋值-->
    <bean id="p18" class="com.atguigu.pojo.Person" parent="parent">
        <property name="id" value="1"></property>
    </bean>
```

### IOC的bean配置信息的抽象化

```
<!--abstract=true表示当前的配置信息，只能用于继承不能实例化-->
    <bean id="parent" class="com.atguigu.pojo.Person" abstract="true">
        <property name="id" value="100"></property>
        <property name="name" value="person实例"></property>
        <property name="age" value="100"></property>
    </bean>
    <bean id="p18" class="com.atguigu.pojo.Person" parent="parent">
        <property name="id" value="1"></property>
    </bean>
```

### scope属性决定单例、多例

scope属性决定Bean对象的作用域。

prototype多例：当spring容器对象创建的时候，多例的对象不会随之创建。多次调用getBean（）方法都会创建一个新对象返回。

```
<bean id="p19" class="com.atguigu.pojo.Person" scope="prototype"></bean>
```

singleton单例（默认单例）：默认情况下，当spring容器对象创建的时候，容器中的所有单例的对象也会随之创建。多次调用getBean（）方法返回的都是同一个对象。

request：同一个请求下，多次调用getBean（）方法返回同一个对象。applicationContext.getBean（“xxx”）;底层原理是：

```
object bean=request.getAttribute（"xxx"）;
if(bean==null){
bean=反射创建对象；
request.setAttribute（"xxx"，bean）;
}
return Bean；
```

session：同一会话下，多次调用getBean（）方法返回的同一个对象

```
object bean=session.getAttribute（"xxx"）;
if(bean==null){
bean=反射创建对象；
session.setAttribute（"xxx"，bean）;
}
return Bean；
```

### 基于xml配置文件的自动注入

xml放配置实现自动注入是指，通过在xml配置文件中以配置的方式对配置的bean对象的子对象自动复制

```
<!--    autowire配置自动为子对象赋值
            no：默认值，不配置子对象，子对象就没有值
            byName:是指按子对象的属性名，作为id到spring容器中查找Bean对象斌进行赋值
            1、如果找到就直接赋值。2、如果找不到就null值
            ByType:根据子对象的类型到spring容器中查找，并赋值
            1、找到一个便赋值。2、如果找不到对象赋值为null。3、如果找到多个便报错
            constructor:按构造器参数进行赋值
            1、先按构造器（构造器中必须含有该实体属性）参数的类型来进行查找并赋值。
            2、如果找不到就调用其他构造器，该属性不存在便为null。
            3、如果找到多个实体，就会按照参数的名称作为id继续到spring容器中查找并赋值。（先byType后ByName）

-->
    <bean class="com.atguigu.pojo.Person" id="p20" autowire="byName">
        
    </bean>
```

### IOC中bean的生命周期

bean对象创建（调用无参构造器） 

给bean对象设置属性

 bean对象初始化之前操作（由bean的后置处理器负责）

 bean对象初始化（需在配置bean时指定初始化方法） 

bean对象初始化之后操作（由bean的后置处理器负责） 

bean对象就绪可以使用 

bean对象销毁（需在配置bean时指定销毁方法）

 IOC容器关闭

```
<!--  init-method="init"配置初始化方法名，初始化会在对象创建之后自动调用。
        destroy-method="destroy"配置销毁方法，
        调用applicationContext.close()时是容器对象关闭时 ，也会销毁容器中的对象，对象的销毁方法也会调用。
        销毁方法只对单例有效，因为单例是交给spring管理的，多例容器只负责创建不负责管理
          -->
    <bean class="com.atguigu.pojo.Person" id="p22" init-method="init" destroy-method="destroy">
        <property name="id" value="22"></property>
    </bean>
```

### Bean的后置处理器BeanPostProcessor

后置处理器，可以在对象**创建初始化**的前后，对对象做一些扩展，增强等操作

bean的后置处理器处理使用步骤：

1、先编写一个类去实现后置处理器接口

2、实现后置处理器的方法

3、去容器中配置后置处理器

```
public class MyBeanProcessor implements BeanPostProcessor {
//    该方法在初始化之前使用，bean正在创建的对象，beanName对象的唯一表示标识，返回正在初始化（创建）的对象
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return null;
    }
//初始化之后执行，，bean正在创建的对象，beanName对象的唯一表示标识，返回正在初始化（创建）的对象
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return null;
    }
}
	<!--   系统自动调 调用-->
    <bean class="com.atguigu.process.MyBeanProcessor"></bean>
```

### spring管理数据库连接池（重点）

```
<!--    前面已经添加链接jar和数据库池jar包使用spring创建德鲁伊数据库连接池-->
    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">
        <property name="username" value="root" ></property>
        <property name="password" value="root" ></property>
        <property name="driverClassName" value="com.mysql.jdbc.Driver" ></property>
        <property name="url" value="jdbc:mysql://localhost:3306/college" ></property>
    </bean>
    
```

```
public void DataSource(){
        ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
        DataSource dataSource =(DataSource) applicationContext.getBean("dataSource");
        try {
            Connection connection = dataSource.getConnection();
            System.out.println(connection);
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }
```

#### 读取外部文件

正确头部配置信息

```
xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
               http://www.springframework.org/schema/context/spring-context-4.1.xsd
           http://www.springframework.org/schema/util
       https://www.springframework.org/schema/util/spring-util.xsd
```

```
   <!--   spring中有一个Context名称空间，它可以用来做很多扫描操作其中一个是
context:property-placeholder用来扫描加载properties属性配置文件
location属性就是指定你要加载的properties属性配置文件路径classpath：jdbc.properties表示从类路径下开始查找jdbc.properties文件
-->
    <context:property-placeholder location="classpath:jdbc.properties"/>

<!--    前面已经添加链接jar和数据库池jar包使用spring创建德鲁伊数据库连接池，注意获取username值时应该换个名字应为这里的username是windows系统的名字-->
    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">
        <property name="username" value="${user}" ></property>
        <property name="password" value="${password}" ></property>
        <property name="driverClassName" value="${driverClassName}" ></property>
        <property name="url" value="${url}" ></property>
    </bean>
```



### 为数组类型属性赋值 

修改Student类 在Student类中添加以下代码：

```
private String[] hobbies;
public String[] getHobbies() {
return hobbies;
}
public void setHobbies(String[] hobbies) {
this.hobbies = hobbies;
}

```

```
<bean id="studentFour" class="com.atguigu.spring.bean.Student">
<property name="id" value="1004"></property>
<property name="name" value="赵六"></property>
<property name="age" value="26"></property>
<property name="sex" value="女"></property>
<!-- ref属性：引用IOC容器中某个bean的id，将所对应的bean为属性赋值 -->
<property name="clazz" ref="clazzOne"></property>
<property name="hobbies">
<array>
<value>抽烟</value>
<value>喝酒</value>
<value>烫头</value>
</array>
</property>
</bean>
```



### 问题

1、在spring中Bean是什么时候创建的？在创建ApplicationContext对象是一起创建bean对象（默认），创建顺序按照applicationContext.xml文件编写的实体类调用。

2、如果调用多次同一个对象getBean（），会创建几个？默认创建同一个。 

3、默认情况下，spring容器|spring IOC容器|IOC容器 容器中对象创建的顺序是在xml中从上到下的顺序。修改bean中depends-on属性来进行修改。方式depends-on=“b”如果想要创建本对象不许先创建b，属性中可以有多个id值用逗号隔开从左到右一次创建

# springEl表达式

# 基于注解管理bean

## 标记与扫描

### 1、注解

和 XML 配置文件一样，注解本身并不能执行，注解本身仅仅只是**做一个标记**，具体的功能是框架检测 到注解标记的位置，然后针对这个位置按照注解标记的功能来执行具体操作。 本质上：所有一切的操作都是Java代码来完成的，XML和注解只是告诉框架中的Java代码如何执行。

### 2、扫描

Spring 为了知道程序员在**哪些地方标记了什么注解**，就需要**通过扫描**的方式，**来进行检测**。然后**根据注 解进**行后续操作。

### 3、实现

1、创建maven工程

导入依赖

```
<dependencies>
<!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-context</artifactId>
<version>5.3.1</version>
</dependency>
<!-- junit测试 -->
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
</dependency>
</dependencies>
```

2、创建spring配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

### 4、常用的spring组件

注解需要加到实现类上，并不能加到

@component：将类标识为普通组件（将作为一个bean被IOC进行管理）

@controller：将类标识为控制层组件

@Service：将类标识为业务层组件

@Repository：将类标识为持久层组件

问：以上四个注解有什么区别

答：@Controller、@Service、@Repository这三个注解只是在@Component注解 的基础上起了三个新的名字。 对于Spring使用IOC容器管理这些组件来说没有区别。所以@Controller、@Service、@Repository这 三个注解只是给开发人员看的，让我们能够便于分辨组件的作用。 注意：虽然它们本质上一样，但是为了代码的可读性，为了程序结构严谨我们肯定不能随便胡乱标记。

### 5、扫描组件

在spring配置文件里配置

```
<context:component-scan base-package="com.atguigu.spring"></context:component-scan>
```

### 6、测试

```
@Test
    public void test(){
        ApplicationContext ioc=new ClassPathXmlApplicationContext("spring-ioc-annotation.xml");
        UserController bean = ioc.getBean(UserController.class);
        System.out.println(bean);
        userDaoImpl bean1 = ioc.getBean(userDaoImpl.class);
        System.out.println(bean1);
        userService bean2 = ioc.getBean(userServiceImp.class);
        System.out.println(bean2);
    }
```

## 扫描组件

```
<context:component-scan base-package="com.atguigu.spring">
<!--    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>-->
        <context:include-filter type="assignable"expression="com.atguigu.spring.controller.UserController"/>
        
    </context:component-scan>
```

context:exclude-filter :排除扫描（使用的多）

​		type：设置排除扫描的方式的方式

​		type=“annotation|assignable”，annotation：根据注解的类型进行排除，expression需要设置排除的注解的全类名

​		type=“annotation|assignable”，assignable：根据类的类型进行排除，expression需要设置排除的类的全类名

Context：include-filter：扫描规定的组件（包含扫描）

注意： 在Context：component-scan标签中还需要添加use-default-filters="false"（所设置的包下所有的类都不需要扫描，此时可以使用包含扫描），如果设置为true（默认）所设置的包下所有的类都需要扫描，此时可以使用排除扫描

## bean的id

通过注解+扫描所配置的id，默认值为类的小驼峰，即类名为小写的结果

```
 ApplicationContext ioc=new ClassPathXmlApplicationContext("spring-ioc-annotation.xml");
 UserController bean = ioc.getBean("userController",UserController.class);
 System.out.println(bean);
```

```
@Service("service")
public class userServiceImp implements userService {
}
userService bean2 = ioc.getBean("service",userServiceImp.class);
        System.out.println(bean2);
```

通过标识组件的注解的value属性值设置bean的自定义的id比如@Service("service")

## 基于注解的自动装配@autowired

自动装配就是会通过Spring的上下文为你找出相应依赖项的类，通俗的说就是Spring会在上下文中自动查找，并自动给Bean装配与其相关的属性！

@AutoWired：实现自动装配功能的注解

1、@autoWired注解能过标识的位置

①标识在成员变量上，此时不需要设置成员变量的set方法

②标识的在set方法上

③为当前成员变量赋值的有参构造上

2、@AutoWired注解的原理

①默认通过byType的方式，在IOC容器中通过类型匹配某个bean为属性赋值

②特殊情况下若有多个类型匹配的bean，此时会自动转换为byName的实现方式实现自动装配的效果。即将要赋值的属性的属性名作为bean的id匹配某个bean为属性赋值

③若byType和byName的方式都无妨实现自动装配，即IOC容器中有多个类型匹配的bean且这些bean的id和要赋值的属性的属性名都不一致，此时抛异常NoUniqueBeanDefinitionException

④接第三步，此时可以再要赋值的属性上，添加一个注解@Qualifier（“userDao”）,通过该注解的value属性值，指定某个bean的id，将这个bean为属性赋值（这时无法通过byType实现因为有两个相同的属性实体，只有通过byName属性确认实体）

注意：若IOC容器中没有任何一个类型匹配的bean，此时抛出异常：NoSuchBeanDefinitionException在@Autowired注解中有个属性required，默认值为true，要求必须完成自动装配，可以将required设置为false，此时能装配则装配，无法装配则使用属性的默认值，如果required修改为false后还是报空指针异常便检查类中是否添加普通注解。

```
@Component("haha")
@Data
public class ha {
    @Autowired
    hao hao1;
    private Integer id;
    private String name;

    public ha(Integer id, String name) {
        this.id = id;
        this.name = name;
    }

    public ha() {
    }
    public void hhhhhh(){
        hao1.hao1();
        System.out.println(111);
    }
}
@Component
class hao{
    public void hao1(){
        System.out.println("hao");
    }
}

@Test
    public void  test(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("aop-annotation.xml");
        ha service = classPathXmlApplicationContext.getBean("haha",ha.class);
        service.hhhhhh();
    }
    
    比如我们的test测试文件夹下做单元测试的时候，可能会用到有关于Spring容器的有关测试，会因为Test包导入的时候导错了包而导致无法正确使用对应的测试标签 以及我们在使用一些监听器的时候，比如之前我们整合监听器的时候会因为监听Spring容器的配置文件的监听类写错而导致无法正确的引入IoC容器从而无法获取到容器中的Bean实例而导致无法自动注入。使用自动注入后便不能new对象。
    解决方案：
    //指定当前测试类在spring的测试环境中执行，此时就可以通过注入的方式直接获取IOC容器中bean
	@RunWith(SpringJUnit4ClassRunner.class)

```



# AOP

## 场景

1、声明计算器接口Calculator，包含加减乘除的抽象方法

```
public interface Calculator {
    int add(int i, int j);
    int sub(int i, int j);
    int mul(int i, int j);
    int div(int i, int j);
}

```

2、创建实现类

```
public class CalculatorPureImpl implements Calculator {
    @Override
    public int add(int i, int j) {
        int result = i + j;
        System.out.println("方法内部 result = " + result);
        return result;
    }
    @Override
        public int sub(int i, int j) {
        int result = i - j;
        System.out.println("方法内部 result = " + result);
        return result;
    }
    @Override
    public int mul(int i, int j) {
        int result = i * j;
        System.out.println("方法内部 result = " + result);
        return result;
    }
    @Override
    public int div(int i, int j) {
        int result = i / j;
        System.out.println("方法内部 result = " + result);
        return result;
    }
}

```

3、创建带日志功能的实现类

```
public class CalculatorLogImpl implements Calculator {
    @Override
    public int add(int i, int j) {
        System.out.println("[日志] add 方法开始了，参数是：" + i + "," + j);
        int result = i + j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] add 方法结束了，结果是：" + result);
        return result;
    }
    @Override
    public int sub(int i, int j) {
        System.out.println("[日志] sub 方法开始了，参数是：" + i + "," + j);
        int result = i - j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] sub 方法结束了，结果是：" + result);
        return result;
    }
    @Override
    public int mul(int i, int j) {
        System.out.println("[日志] mul 方法开始了，参数是：" + i + "," + j);
        int result = i * j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] mul 方法结束了，结果是：" + result);
        return result;
    }
    @Override
    public int div(int i, int j) {
        System.out.println("[日志] div 方法开始了，参数是：" + i + "," + j);
        int result = i / j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] div 方法结束了，结果是：" + result);
        return result;
    }
}

```

### 提出问题

①现有代码缺陷 针对带日志功能的实现类，我们发现有如下缺陷：

 对核心业务功能有干扰，导致程序员在开发核心业务功能时分散了精力

 附加功能分散在各个业务功能方法中，不利于统一维护

②解决思路 解决这两个问题，核心就是：

解耦。我们需要把附加功能从业务功能代码中抽取出来。 

③困难 解决问题的困难：要抽取的代码在方法内部，靠以前把子类中的重复代码抽取到父类的方式没法解决。 所以需要引入新的技术。

## 代理模式

二十三种设计模式中的一种，属于结构型模式。它的作用就是通过提供一个代理类，让我们在调用目标 方法的时候，不再是直接对目标方法进行调用，而是通过代理类间接调用。让不属于目标方法核心逻辑 的代码从目标方法中剥离出来——解耦。调用目标方法时**先调用代理对象**的方法，减少对目标方法的调 用和打扰，同时让附加功能能够集中在一起也有利于统一维护。

②生活中的代理 

广告商找大明星拍广告需要经过经纪人

 合作伙伴找大老板谈合作要约见面时间需要经过秘书 

房产中介是买卖双方的代理 

③相关术语 代理：

将非核心逻辑剥离出来以后，封装这些非核心逻辑的类、对象、方法。 

目标：被代理“套用”了非核心逻辑代码的类、对象、方法。

## 静态代理实现

在核心代码实现之前，核心代码执行之后、catch、finally等处

```
public class CalculatorPureProxy implements Calculator {
//    核心功能实体类
    private CalculatorPureImpl target;

    public CalculatorPureProxy(CalculatorPureImpl target) {
        this.target = target;
    }

    @Override
    public int add(int i, int j) {
        System.out.println("添加日志功能");
        int add = target.add(i, j);
        System.out.println("添加非核心功能");
        return add;
    }
    
    
    public class testt {
    @Test
    public void test(){
        CalculatorPureProxy calculatorPureProxy=new CalculatorPureProxy(new CalculatorPureImpl());
        calculatorPureProxy.add(1,2);
    }
}

```

静态代理确实实现了解耦，但代码都是写死了，完全不具备灵活性。就拿日志功能来 说，将来其他地方也需要附加日志，那还得再声明更多个静态代理类，那就产生了大量重复的代 码，日志功能还是分散的，没有统一管理。 

提出进一步的需求：将日志功能集中到一个代理类中，将来有任何日志需求，都通过这一个代理 类来实现。这就需要使用动态代理技术了。

## 动态代理

主要是为了代理不同的类,所以称之为动态代理

动态代理有两种：

jdk动态代理：要求必须有接口，最终生成的代理类和目标类实现相同的接口，在com.sum.proxy包下类名为$proxy2

cglib动态代理：最终生成的代理类会继承目标类，并且和目标类在相同的报下

```
public class ProxyFactory {
//    1.创建目标类
    private Object tatget;

    public ProxyFactory(Object tatget) {
        this.tatget = tatget;
    }
    public Object getProxy(){
//        使用jdk动态代理
//        jdk动态代理类中有三个参数如下所示
//ClassLoader loader:指定加载动态生成的代理类的类加载器，①根类加载器②扩展类加载器③应用类加载器
//Class<?>[] interfaces：获取目标对象实现的所有接口的class对象的数组
//InvocationHandler h：设置代理类中的抽象方法如何重写
        ClassLoader classLoader = ProxyFactory.class.getClassLoader();
        Class<?>[] interfaces = tatget.getClass().getInterfaces();
        InvocationHandler h1=new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("方法执行之前的多余方法");
//                proxy:表示代理对象，method：表示要执行的方法，args：表示要执行的方法到的参数列表
                Object result = method.invoke(tatget, args);
                System.out.println("目标对象方法执行后的步骤");
                return result;
            }
        };

        return Proxy.newProxyInstance(classLoader,interfaces,h1);
    }
}

 @Test
    public void testPorxy(){
        ProxyFactory proxyFactory=new ProxyFactory(new CalculatorPureImpl());
        Calculator proxy=(Calculator) proxyFactory.getProxy();
        proxy.add(1,2);
    }
```

## AOP

### 概念

AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面 向对象编程的一种补充和完善，它以通过预编译方式和运行期动态代理方式实现在不修改源代码的情况 下给程序动态统一添加额外功能的一种技术。

### 相关术语

①横切关注点

从每个方法中**抽取**出来的同一类非核心业务。**在同一个项目中**，我们可以使用多个横切关注点对相关方 法进行多个不同方面的增强。 这个概念不是语法层面天然存在的，而是根据附加功能的逻辑上的需要：有十个附加功能，就有十个横 切关注点。

②通知

每一个横切关注点上要做的事情都需要写一个方法来实现，这样的方法就叫通知方法。

 前置通知：在被代理的目标方法前执行

 返回通知：在被代理的目标方法成功结束后执行（寿终正寝）

 异常通知：在被代理的目标方法异常结束后执行（死于非命）

 后置通知：在被代理的目标方法最终结束后执行（盖棺定论）

 环绕通知：使用try...catch...finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所 有位置

③切面

封装通知方法的类

④目标

被代理的目标对象

⑤代理

向目标对象应用通知后创建的代理对象

⑥连接点

这也是一个纯逻辑概念，不是语法定义的。 把方法排成一排，每一个横切位置看成x轴方向，把方法从上到下执行的顺序看成y轴，x轴和y轴的交叉 点就是连接点。

⑦切入点

定位连接点的方式

每个类的方法中都包含多个连接点，所以连接点是类中客观存在的事物（从逻辑上来说）。

 如果把连接点看作数据库中的记录，那么切入点就是查询记录的 SQL 语句。

 Spring 的 AOP 技术可以通过切入点定位到特定的连接点。

 切点通过 org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条件。

### 作用

1、简化代码：把方法中固定的位置的重复代码抽出来，让被抽取的方法更转专注于自己的核心功能，提高内聚性

2、代码增强：把特定的功能封装到切面类中，看哪里有需要，就往哪里套，被套用了切面逻辑的方法就被切面给增强了

## 基于注解的AOP

### 技术说明

动态代理（InvocationHandler）：JDK原生的实现方式，需要被代理的目标类必须实现接口。因 为这个技术要求代理对象和目标对象实现同样的接口（兄弟两个拜把子模式）。

 cglib：通过继承被代理的目标类（认干爹模式）实现代理，所以不需要目标类实现接口。

 AspectJ：本质上是静态代理，将代理逻辑“织入”被代理的目标类编译得到的字节码文件，所以最 终效果是动态的。weaver就是织入器。Spring只是借用了AspectJ中的注解。

aop是思想，aspectJ是spring实现方式

### 准备工作

1、创建maven工程，并添加依赖

```
<dependencies>
        <!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.3.1</version>
        </dependency>
    </dependencies>
```

2、使用的还是aop的场景

3、代码

```
@Component
//将当前组件标识为切面
@Aspect
public class LoggerAspect {
//    在切面中，需要通过指定的注解将方法标识为通知方法
//    @before：前置通知，在目标对象方法执行之前执行
    @Before("execution(public int com.atguigu.spring.aop.annotaation.CalculatorPureImpl.add(int,int))")
    public void beforeAdviceMethod(){
        System.out.println("loggerAspect,前置通知");
    }

}

```

```
spring配置文件
<!--
    AOP的注意事项：
    切面类和目标类都需要交给IOC容器管理
    切面类必须通过@Aspect注解标识为一个切面
    在spring配置文件中使用<aop:aspectj-autoproxy>开基于注解的aop

-->
    <context:component-scan base-package="com.atguigu.spring.aop.annotaation"></context:component-scan>
<!--    开启基于注解的aop功能-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

```
@Test
    public void annotation(){
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("aop-annotation.xml");
        通过调用目标类的接口的方法操作，实现动态代理
        Calculator bean = classPathXmlApplicationContext.getBean(Calculator.class);
        bean.add(1,1);
    }
```

### 切入点表达式(注解中execution（）的值)

切入点表达式：设置在标识通知的注解value属性中

用*号代替“权限修饰符”和“返回值”部分表示“权限修饰符”和“返回值”不限*

 在包名的部分，一个“**”号只能代表包的层次结构中的一层，表示这一层是任意的。*

​	例如：*.Hello匹配com.Hello，不匹配com.atguigu.Hello 

在包名的部分，使用“**..”表示包名任意、包的层次深度任意 *

在类名的部分，类名部分整体用**号代替，表示类名任意 *

在类名的部分，可以使用*号代替类名的一部分 *

​	例如：*Service匹配所有名称以Service结尾的类或接口 *

在方法名部分，可以使用*号表示方法名任意 

在方法名部分，可以使用*号代替方法名的一部分*

​	 例如：Operation匹配所有方法名以Operation结尾的方法 

在方法参数列表部分，使用(..)表示参数列表任意*

在方法参数列表部分，使用(int,..)表示参数列表以一个int类型的参数开头 

在方法参数列表部分，基本数据类型和对应的包装类型是不一样的

​	切入点表达式中使用 int 和实际方法中 Integer 是不匹配的

在方法返回值部分，如果想要明确指定一个返回值类型，那么必须同时写明权限修饰符

​	例如：execution(public int ..Service.*(.., int)) 正确 *

​	例如：execution(* int ..Service.*(.., int)) 错误

真正的使用：@Before("execution(* com.atguigu.spring.aop.annotaation.CalculatorPureImpl.*(..))")

​	第一个“*”表示方法的权限（所有权限的方法都可以使用），即任意的访问修饰符合返回值类型

​	包名和方法名也可以使用“*”表，包内所有的类、类中所有的方法都可以

​	*表示类中任意的方法

​	（..）表示任意参数列表

### 获取切面编程中的方法名和参数

```
 @Before("execution(public int com.atguigu.spring.aop.annotaation.CalculatorPureImpl.add(int,int))")
    public void beforeAdviceMethod(JoinPoint joinPoint){
//        获取连接点所对应方法的签名信息
        Signature signature = joinPoint.getSignature();
//        获取连接点所对应方法的参数
        Object[] args = joinPoint.getArgs();
        System.out.println("loggerAspect,前置通知.方法名："+signature.getName()+",参数："+ Arrays.toString(args));
    }
```

### 获取连接点的信息

在通知方法的参数位置，设置joinPoint类型的参数，就可以获取父节点所对应的方法的信息

```
//        获取连接点所对应方法的签名信息（方法的声明信息）
        Signature signature = joinPoint.getSignature();
        //        获取连接点所对应方法的参数
        Object[] args = joinPoint.getArgs();
        获取方法名
        signature.getName()
```

### 切入点表达式的重用

@pointCut声明一个公共的切入点表达式

使用方法：先使用@pointCut注解并添加方法名

然后在@after或@before等各种通知中使用value添加pointCut的方法名

```
//    切入点表达式的重用,切点表达式
    @Pointcut("execution(* com.atguigu.spring.aop.annotaation.*.*(..))")
    public void pointCut(){
    }

    @After("pointCut()")
    public void afterAdvice(){

    }
```

### aop各种通知的使用

@before：前置通知，在目标对象方法只想之前执行

@after：后置通知，在目标对象方法的finally字句中执行，如果没有finally 语句便在方法执行后执行

@AfterReturning：返回通知，在目标方法返回值之后执行，在后置通知之前执行，如果有异常报错打断程序便不会执行返回通知

@AfterThrowing：异常通知，在目标对象方法的catch子句中执行

@Around：环绕通知，方法需要设置返回值。

1、后置通知

```
@After("pointCut()")
    public void afterAdvice(JoinPoint joinPoint){
        Signature signature = joinPoint.getSignature();
        Object[] args = joinPoint.getArgs();
        System.out.println("loggerAspect,方法："+signature.getName()+"执行完毕");

    }
```

2、返回通知

```
//    要想在返回通知中获取目标对象方法的返回值，只需要通过@AfterReturning注解的Returning属性值，
//    然后在方法的参数列表中添加属性名和Returning属性值一样的参数名便可以调用
    @AfterReturning(value = "pointCut()",returning = "result")
    public void afterReturningAdviceMethod(JoinPoint joinPoint,Object result){
        Signature signature = joinPoint.getSignature();

        System.out.println("loggerAspect,返回通知.方法名："+signature.getName()+",结果："+result);
    }
```

3、异常通知

```
//在异常通知中若要获取目标对象方法的异常，只需要通过@AfterThrowing注解的throwing属性，
// 就可以将通知方法的某个参数指定为接收目标出现的异常参数
    @AfterThrowing(value = "pointCut()",throwing = "exception")
    public void afterThrowingAdviceMethod(JoinPoint joinPoint,Exception exception){
        Signature signature = joinPoint.getSignature();
        System.out.println("loggerAspect,异常通知。方法名："+signature.getName()+",异常："+exception);
    }
```

4、返回通知

```
//    环绕通知的方法的返回值一定要和目标对象方法的返回值一致，环绕通知只能增强代码并不能改变代码执行。
    @Around("pointCut()")
    public Object aroundAdviceMethod(ProceedingJoinPoint joinPoint){
        Object result=null;

        try {
            System.out.println("环绕通知中的前置通知");
            //        表示目标对象方法的执行
            result= joinPoint.proceed();
            System.out.println("环绕通知中的返回通知");
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("环绕通知中的异常通知");
        }finally {
            System.out.println("环绕通知中的后置通知");
        }
        return result;
    }
```

各种通知的执行顺序：

 Spring版本5.3.x以前：

 前置通知 目标操作 后置通知 返回通知或异常通知 

Spring版本5.3.x以后： 

前置通知 目标操作 返回通知或异常通知 后置通知

### 切面的优先级

可以通过@order注解的value属性设置aop动态代理优先级，默认值为integer的最大值，**value值越小优先级越高**

```
@Aspect
@Component
//可以通过@order注解的value属性设置aop动态代理优先级，默认值为integer的最大值，value值越小优先级越高
@Order(1)
public class validateAspect {
    @Pointcut("execution(* com.atguigu.spring.aop.annotaation.*.*(..))")
    public void pointcut(){
    }
    @Before("pointcut()")
    public void beforeMethod(){
        System.out.println("validateAspect->前置通知");
    }
}

```

## 基于xml的aop

1、使用场景导入Calculator类和CalculatorPureImpl类

2、编写LoggerAspect类编写切入点方法

```
@Component
//将当前组件标识为切面
//@Aspect
public class LoggerAspect {
//    在切面中，需要通过指定的注解将方法标识为通知方法
//    @before：前置通知，在目标对象方法执行之前执行
//    @Before("execution(* com.atguigu.spring.aop.annotaation.CalculatorPureImpl.*(..))")
//    @Before("execution(public int com.atguigu.spring.aop.annotaation.CalculatorPureImpl.add(int,int))")
    public void beforeAdviceMethod(JoinPoint joinPoint){
//        获取连接点所对应方法的方法名
        Signature signature = joinPoint.getSignature();
//        获取连接点所对应方法的参数
        Object[] args = joinPoint.getArgs();
        System.out.println("loggerAspect,前置通知.方法名："+signature.getName()+",参数："+ Arrays.toString(args));
    }
//    切入点表达式的重用,切点表达式
//    @Pointcut("execution(* com.atguigu.spring.aop.annotaation.*.*(..))")
//    public void pointCut(){
//    }

//    @After("pointCut()")
    public void afterAdvice(JoinPoint joinPoint){
        Signature signature = joinPoint.getSignature();
        Object[] args = joinPoint.getArgs();
        System.out.println("loggerAspect,方法："+signature.getName()+"执行完毕");

    }

//    要想在返回通知中获取目标对象方法的返回值，只需要通过@AfterReturning注解的Returning属性值，
//    然后在方法的参数列表中添加属性名和Returning属性值一样的参数名便可以调用
//    @AfterReturning(value = "pointCut()",returning = "result")
    public void afterReturningAdviceMethod(JoinPoint joinPoint,Object result){
        Signature signature = joinPoint.getSignature();

        System.out.println("loggerAspect,返回通知.方法名："+signature.getName()+",结果："+result);
    }

//在异常通知中若要获取目标对象方法的异常，只需要通过@AfterThrowing注解的throwing属性，
// 就可以将通知方法的某个参数指定为接收目标出现的异常参数
//    @AfterThrowing(value = "pointCut()",throwing = "exception")
    public void afterThrowingAdviceMethod(JoinPoint joinPoint,Exception exception){
        Signature signature = joinPoint.getSignature();
        System.out.println("loggerAspect,异常通知。方法名："+signature.getName()+",异常："+exception);
    }

//    环绕通知的方法的返回值一定要和目标对象方法的返回值一致
//    @Around("pointCut()")
    public Object aroundAdviceMethod(ProceedingJoinPoint joinPoint){
        Object result=null;

        try {
            System.out.println("环绕通知中的前置通知");
            //        表示目标对象方法的执行
            result= joinPoint.proceed();
            System.out.println("环绕通知中的返回通知");
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println("环绕通知中的异常通知");
        }finally {
            System.out.println("环绕通知中的后置通知");
        }
        return result;
    }

}

```

3、编写spring的xml文件

```
<!--基于xml配置aop的实现-->
    <!--扫描组件-->
    <context:component-scan base-package="com.atguigu.spring.aop.xml"></context:component-scan>
  <aop:config>
<!--      设置一个公共的切入点表达式-->
 <aop:pointcut id="pointCut" expression="execution(* com.atguigu.spring.aop.xml.CalculatorPureImpl.*(..))"/>
<!--      将ioc容器中的某个bean设置为切面-->
      <aop:aspect ref="loggerAspect">
            <aop:before method="beforeAdviceMethod" pointcut-ref="pointCut"></aop:before>
          <aop:after method="afterAdvice" pointcut-ref="pointCut"></aop:after>
          <aop:after-returning method="afterReturningAdviceMethod" returning="result" pointcut-ref="pointCut"></aop:after-returning>
          <aop:after-throwing method="afterThrowingAdviceMethod" pointcut-ref="pointCut" throwing="exception"></aop:after-throwing>

      </aop:aspect>

  </aop:config>
```

4、编写测试类

```
 @Test
    public void tesst(){
        ClassPathXmlApplicationContext application = new ClassPathXmlApplicationContext("aop-xml.xml");
        Calculator bean = application.getBean(Calculator.class);
        bean.add(1,1);
    }
```

# 声明式事务

在service层添加事务管理

## JDBCTemplate

### 简介

spring框架对JDBC进行封装，使用jdbcTemplate方便实现对数据库操作

### 准备工作

1、创建maven工程

2、导入jar包

```
<dependencies>
<!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-context</artifactId>
<version>5.3.1</version>
</dependency>
<!-- Spring 持久化层支持jar包 -->
<!-- Spring 在执行持久化层操作、与持久化层技术进行整合过程中，需要使用orm、jdbc、tx三个
jar包 -->
<!-- 导入 orm 包就可以通过 Maven 的依赖传递性把其他两个也导入 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-orm</artifactId>
<version>5.3.1</version>
</dependency>
<!-- Spring 测试相关 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-test</artifactId>
<version>5.3.1</version>
</dependency>
<!-- junit测试 -->
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
</dependency>
<!-- MySQL驱动 -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>8.0.16</version>
</dependency>
<!-- 数据源 -->
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>druid</artifactId>
<version>1.0.31</version>
</dependency>
</dependencies>
```

3、准备jdbc.properties文件

```
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.username=root
jdbc.password=root
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
```

4、编写spring的xml文件

```
<!--导入数据库连接文件：重点读取不到文件会报错-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
<!--配置数据源-->
    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="password" value="${jdbc.password}"></property>
        <property name="username" value="${jdbc.username}"></property>
    </bean>
<!--    配置jdbcTemplate-->
    <bean class="org.springframework.jdbc.core.JdbcTemplate">
<!--        装配数据源-->
        <property name="dataSource" ref="dataSource">

        </property>
    </bean>
```

5、编写测试类(添加功能)

```
//指定当前测试类在spring的测试环境中执行，此时就可以通过注入的方式直接获取IOC容器中bean
@RunWith(SpringJUnit4ClassRunner.class)
//设置spring测试环境的配置文件
@ContextConfiguration("classpath:jdbc.xml")
public class jdbcTemplate {
    @Autowired
   private JdbcTemplate jdbcTemplate;
    @Test
    public void testInsert(){
        String sql="insert into user values(null,?,?,?,?,?)";
        jdbcTemplate.update(sql,"6","6",6,"6","6");
    }

}
```

6、查询功能

```
	查询单个实体
	@Test
    public void testSelect(){
        String sql="select * from user where id=? ";
//        new BeanPropertyRowMapper<User>(User.class)：用于映射结果集对象 ,queryForObject:查询语句
        User user = jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<User>(User.class), 2);
        System.out.println(user);
    }
    查询实体集合
    @Test
    public void testAllSelect(){
        String sql="select * from user";
        List<User> query = jdbcTemplate.query(sql, new BeanPropertyRowMapper<User>(User.class));
        query.forEach(System.out::println);
    }
    查询单行单列的数据
     @Test
    public void  testGetCount(){
        String sql="select count(*) from user";
        Integer integer = jdbcTemplate.queryForObject(sql, Integer.class);
        System.out.println(integer);
    }
```

## 声明式事务概念

### 编程式事务

事务的功能的相关操作全部通过自己编写的代码实现：

```
Connection conn = ...;
try {
    // 开启事务：关闭事务的自动提交
    conn.setAutoCommit(false);
    // 核心操作
    // 提交事务
    conn.commit();
}catch(Exception e){
    // 回滚事务
    conn.rollBack();
}finally{// 释放数据库连接
	conn.close();
}

```

编程式的实现方式存在缺陷： 

细节没有被屏蔽：具体操作过程中，所有细节都需要程序员自己来完成，比较繁琐。

代码复用性不高：如果没有有效抽取出来，每次实现功能都需要自己编写代码，代码就没有得到复 用。

### 声明式事务

既然事务控制的代码有规律可循，代码的结构基本是确定的，所以框架就可以将固定模式的代码抽取出 来，进行相关的封装。 

封装起来后，我们只需要在配置文件中进行简单的配置即可完成操作。

 	好处1：提高开发效率 

​	好处2：消除了冗余的代码 

​	好处3：框架会综合考虑相关领域中在实际开发环境下有可能遇到的各种问题，进行了健壮性、性 能等各个方面的优化

 所以，我们可以总结下面两个概念：

 	编程式：自己写代码实现功能 

​	声明式：通过配置让框架实现功能

## 基于注解的声明事务

每一个sql语句是**独占一个事务且自动提交**，如果前面两个sql语句执行成功并且最后一个sql执行失败是无法回滚的，解决方案将sql语句放入一个事务当中要么都成功要么都失败。

### 准备工作

1、基本环境使用JDBCTemplate

2、创建新的spring xml文件

```
 <!--扫描组件-->
    <context:component-scan base-package="com.atguigu.spring">
    </context:component-scan>
    <!-- 导入外部属性文件 -->
    <context:property-placeholder location="classpath:jdbc.properties" />
    <!-- 配置数据源 -->
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    <!-- 配置 JdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!-- 装配数据源 -->
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
```

3、在创建数据库表

```
CREATE TABLE `t_book` (
    `book_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
    `book_name` varchar(20) DEFAULT NULL COMMENT '图书名称',
    `price` int(11) DEFAULT NULL COMMENT '价格',
    `stock` int(10) unsigned DEFAULT NULL COMMENT '库存（无符号）',
    PRIMARY KEY (`book_id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
insert into `t_book`(`book_id`,`book_name`,`price`,`stock`) values (1,'斗破苍
穹',80,100),(2,'斗罗大陆',50,100);
CREATE TABLE `t_user` (
    `user_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
    `username` varchar(20) DEFAULT NULL COMMENT '用户名',
    `balance` int(10) unsigned DEFAULT NULL COMMENT '余额（无符号）',
    PRIMARY KEY (`user_id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
insert into `t_user`(`user_id`,`username`,`balance`) values (1,'admin',50);
```

4、controller层

```
@Controller
public class BookController {
    @Autowired
    private BookService bookService;
    public void buyBook(Integer userId,Integer bookId){
        bookService.buyBook(userId,bookId);
    }
}
```

5、service层

```
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    @Override
    public void buyBook(Integer userId, Integer bookId) {
//        查询图书的价格
        Integer price= bookDao.getPriceByBookId(bookId);
 //        更新图书的库存
        bookDao.updateStock(bookId);
//        跟新用户的余额
        bookDao.updateBalance(userId,price);
    }
}
```

6、dao层

```
@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;
//根据图书的id查询图书的价格
    @Override
    public Integer getPriceByBookId(Integer bookId) {
        String sql="select price from t_book where book_id=? ";
        Integer integer = jdbcTemplate.queryForObject(sql, Integer.class, bookId);
        System.out.println("书本价格："+integer);
        return integer;

    }
//跟新图书库存
    @Override
    public void updateStock(Integer bookId) {
        String sql="update t_book set stock=stock-1 where book_id=?";
        jdbcTemplate.update(sql,bookId);
    }
//跟新用户余额
    @Override
    public void updateBalance(Integer userId, Integer price) {
        String sql="update t_user set balance=balance-? where user_id=?";
        jdbcTemplate.update(sql,price,userId);
    }
}
```

7、测试类

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:tx-annotation.xml")
public class TxByAnnotation {
    @Autowired
    private BookController controller;
    @Test
    public void testBuyBook(){
        controller.buyBook(1,1);
    }
}
```

②模拟场景

 用户购买图书，先查询图书的价格，再更新图书的库存和用户的余额 假设用户id为1的用户，购买id为1的图书 用户余额为50，而图书价格为80 购买图书之后，用户的余额为-30，数据库中余额字段设置了无符号，因此无法将-30插入到余额字段 此时执行sql语句会抛出SQLException 。但图书剩余数量已经完成更新，造成损失。因为每一个sql语句是**独占一个事务且自动提交**，如果前面两个sql语句执行成功并且最后一个sql执行失败是无法回滚的，解决方案将sql语句放入一个事务当中要么都成功要么都失败。

③观察结果 因为没有添加事务，图书的库存更新了，但是用户的余额没有更新 显然这样的结果是错误的，购买图书是一个完整的功能，更新库存和更新余额要么都成功要么都失败

### 实现事务的功能

1、在spring配置文件中配置事务管理器

```
 <!--扫描组件-->
    <context:component-scan base-package="com.atguigu.spring">
    </context:component-scan>
    <!-- 导入外部属性文件 -->
    <context:property-placeholder location="classpath:jdbc.properties" />
    <!-- 配置数据源 -->
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    <!-- 配置 JdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!-- 装配数据源 -->
        <property name="dataSource" ref="druidDataSource"/>
    </bean>

<!--    配置事务管理器相当于切面-->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="druidDataSource"></property>
    </bean>
<!--    开启事务的注解驱动相当于通知，注意这里的driven标签以tx结尾-->
<!--    将使用@Transaction注解所标识的方法或类中所有的方法使用事务进行管理
		transaction-manager属性设置事务管理器的id
		若事务管理器的bean的id默认值为transactionmanager，则该属性可以不写
-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
```

2、在service层方法上添夹@transaction注解添加事务

因为service层表示业务逻辑层，一个方法表示一个完成的功能，因此处理事务一般在service层处理 在BookServiceImpl的buybook()添加注解@Transactional

```
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
    @Transactional
    @Override
    public void buyBook(Integer userId, Integer bookId) {
//        查询图书的价格
        Integer price= bookDao.getPriceByBookId(bookId);
 //        更新图书的库存
        bookDao.updateStock(bookId);
//        跟新用户的余额
        bookDao.updateBalance(userId,price);
    }
}
```

声明事务的步骤：

1、在spring的配置文件中配置事务管理器

2、开启事务的注解驱动

只需要被事务管理的方法上，添加@Transaction注解，该方法就会被事务管理

@Transaction注解标识的的位置：

标识在方法上，只会对注解方法进行事务管理

标识在类上，则类中所有的方法都会被事务管理

### 事务属性：只读

①介绍

对一个**查询操作**来说，如果我们把它设置成只读，就能够明确告诉数据库，这个操作不涉及写操作。这 样数据库就能够针对查询操作来进行优化。

```
 @Transactional(readOnly = true)
```

②注意 对**增删改**操作设置只读会抛出下面异常：

 Caused by: java.sql.SQLException: Connection is read-only. Queries leading to data modification are not allowed

### 事务属性：超时

①介绍 

**事务**在执行过程中，有可能因为遇到某些问题，**导致程序卡住**，从而长时间占用数据库资源。而长时间 占用资源，大概率是因为程序运行出现了问题（可能是Java程序或MySQL数据库或网络连接等等）。 此时这个很可能出问题的程序应该被回滚，撤销它已做的操作，事务结束，把资源让出来，让其他正常 程序可以执行。

概括来说就是一句话：**超时回滚，释放资源**。

```
  @Transactional(timeout = 3)；3表示执行时间超过3秒事务便会超时回滚
```

③观察结果 执行过程中抛出异常： 

org.springframework.transaction.**TransactionTimedOutException**: Transaction timed out: deadline was Fri Jun 04 16:25:39 CST 2022

### 事务属性：回滚策略

①介绍 声明式事务**默认**只针对**运行时异常**回滚，**编译时异常**不回滚。 

可以通过@Transactional中相关属性设置回滚策略

 rollbackFor属性：需要设置一个Class类型的对象 

rollbackForClassName属性：需要设置一个字符串类型的全类名

 noRollbackFor属性：需要设置一个Class类型的对象，如果该异常报错时不会造成回滚

 noRollbackForClassName属性：需要设置一个字符串类型的全类名，如果该异常报错时不会造成回滚

```
@Transactional(noRollbackFor = {ArithmeticException.class,Exception.class})
@Transactional(noRollbackForClassName = "java.lang.ArithmeticException")
```

③观察结果 虽然购买图书功能中出现了数学运算异常（ArithmeticException），但是我们设置的回滚策略是，当 出现ArithmeticException不发生回滚，因此购买图书的操作正常执行。

### 事务属性：事务隔离级别

脏读：读取出的数据没有任何意义

①介绍 数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题。一个事 务与其他事务隔离的程度称为隔离级别。SQL标准中规定了多种事务隔离级别，不同隔离级别对应不同 的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱。

 隔离级别一共有四种： 

读未提交：READ UNCOMMITTED 允许Transaction01读取Transaction02未提交的修改。 

读已提交：READ COMMITTED、 要求Transaction01只能读取Transaction02已提交的修改。 

可重复读：REPEATABLE READ 确保Transaction01可以多次从一个字段中读取到相同的值，即Transaction01执行期间禁止其它 事务对这个字段进行更新。

 串行化：SERIALIZABLE 确保Transaction01可以多次从一个表中读取到相同的行，在Transaction01执行期间，禁止其它 事务对这个表进行添加、更新、删除操作。可以避免任何并发问题，但性能十分低下。

 各个隔离级别解决并发问题的能力见下表：

| 隔离级别         | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ---- | ---------- | ---- |
| READ UNCOMMITTED | 有   | 有         | 有   |
| READ COMMITTED   | 无   | 有         | 有   |
| REPEATABLE READ  | 无   | 无         | 有   |
| SERIALIZABLE     | 无   | 无         | 无   |

各种数据库产品对事务隔离级别的支持程度：

| 隔离级别         | Oracle  | MySQL   |
| ---------------- | ------- | ------- |
| READ UNCOMMITTED | ×       | √       |
| READ COMMITTED   | √(默认) | √       |
| REPEATABLE READ  | ×       | √(默认) |
| SERIALIZABLE     | √       | √       |

```
@Transactional(isolation = Isolation.DEFAULT)//使用数据库默认的隔离级别
@Transactional(isolation = Isolation.READ_UNCOMMITTED)//读未提交
@Transactional(isolation = Isolation.READ_COMMITTED)//读已提交
@Transactional(isolation = Isolation.REPEATABLE_READ)//可重复读
@Transactional(isolation = Isolation.SERIALIZABLE)//串行化
```

### 事务属性：事务的传播行为

①介绍 当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有事务中 运行，也可能开启一个新事务，并在自己的事务中运行。

②代码环境（原有代码不变）

checkOutService接口实现类

```
@Service
public class CkeckOutImpl implements CheckOutService {
    @Autowired
    BookService bookService;
    @Override
    @Transactional
    public void checkout(Integer userid, Integer[] args) {
        for (Integer i:args){
            bookService.buyBook(userid,i);
        }

    }
}
```

在bookController中添加方法

```
  @Autowired
    private CheckOutService checkOutService;
    public void checkout(Integer userid,Integer ...args){
        checkOutService.checkout(userid,args);
    }
```

在数据库中将用户的余额修改为100元 

③观察结果

可以通过@Transactional中的propagation属性设置事务传播行为，该属性表示另外开一个线程确保方法执行完成，即购买两本书一本购买成功一本购买失败，购买成功数据不会回滚，购买失败数据进行回滚。所以属性加在buyBook（）方法上而不是加在checkOut（）结账方法上

修改BookServiceImpl中buyBook()上，注解@Transactional的propagation属性 @Transactional(propagation = Propagation.REQUIRED)，默认情况，表示如果当前线程上有已经开 启的事务可用，那么就在这个事务中运行。经过观察，购买图书的方法buyBook()在checkout()中被调 用，checkout()上有事务注解，因此在此事务中执行。所购买的两本图书的价格为80和50，而用户的余 额为100，因此在购买第二本图书时余额不足失败，导致整个checkout()回滚，即只要有一本书买不 了，就都买不了

@Transactional(propagation = Propagation.REQUIRES_NEW)，表示不管当前线程上是否有已经开启 的事务，都要开启新事务。同样的场景，每次购买图书都是在buyBook()的事务中执行，因此第一本图 书购买成功，事务结束，第二本图书购买失败，只在第二次的buyBook()中回滚，购买第一本图书不受 影响，即能买几本就买几本

## 基于xml进行事务管理

1、环境使用注解的声明事务的环境并且没有@transaction注解

2、spring配置文件



```
<aop:config>
<!-- 配置事务通知和切入点表达式 -->
<aop:advisor advice-ref="txAdvice" pointcut="execution(*
com.atguigu.spring.tx.xml.service.impl.*.*(..))"></aop:advisor>
</aop:config>
<!-- tx:advice标签：配置事务通知 -->
<!-- id属性：给事务通知标签设置唯一标识，便于引用 -->
<!-- transaction-manager属性：关联事务管理器 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
<tx:attributes>
<!-- tx:method标签：配置具体的事务方法 -->
<!-- name属性：指定方法名，可以使用星号代表多个字符 -->
<tx:method name="get*" read-only="true"/>
<tx:method name="query*" read-only="true"/>
<tx:method name="find*" read-only="true"/>
<!-- read-only属性：设置只读属性 -->
<!-- rollback-for属性：设置回滚的异常 -->
<!-- no-rollback-for属性：设置不回滚的异常 -->
<!-- isolation属性：设置事务的隔离级别 -->
<!-- timeout属性：设置事务的超时属性 -->
<!-- propagation属性：设置事务的传播行为 -->
<tx:method name="save*" read-only="false" rollbackfor="java.lang.Exception" propagation="REQUIRES_NEW"/>
<tx:method name="update*" read-only="false" rollbackfor="java.lang.Exception" propagation="REQUIRES_NEW"/>
<tx:method name="delete*" read-only="false" rollbackfor="java.lang.Exception" propagation="REQUIRES_NEW"/>
</tx:attributes>
</tx:advice>

```

```
  <!--扫描组件-->
    <context:component-scan base-package="com.atguigu.spring">
    </context:component-scan>
    <!-- 导入外部属性文件 -->
    <context:property-placeholder location="classpath:jdbc.properties" />
    <!-- 配置数据源 -->
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    <!-- 配置 JdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!-- 装配数据源 -->
        <property name="dataSource" ref="druidDataSource"/>
    </bean>

<!--    配置事务管理器相当于切面-->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="druidDataSource"></property>
    </bean>
<!--    开启事务的注解驱动相当于通知，注意这里的driven标签以tx结尾-->
<!--    将使用@Transaction注解所标识的方法或类中所有的方法使用事务进行管理-->
<!--    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>-->

<!--    配置事务通知-->
    <tx:advice id="tx" transaction-manager="transactionManager" >
        <tx:attributes>
            <tx:method name="buyBook" timeout="5"/>
        </tx:attributes>
    </tx:advice>
    <aop:config>
        <aop:advisor advice-ref="tx" pointcut="execution(* com.atguigu.spring.service.Impl.*.*(..))">				</aop:advisor>
    </aop:config>
```

3、注意：基于xml实现的声明式事务，必须引入aspectJ依赖、

```
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-aspects</artifactId>
<version>5.3.1</version>
</dependency>
```











































































































































































































































































































































































