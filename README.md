<div align = "center" stype ="font-weight:bold;font-size:140px;">Spring5</div>

尚硅谷听课笔记

听课地址：https://www.bilibili.com/video/BV1Vf4y127N5?p=50&spm_id_from=pageDriver

完课时间：2021.5.25

------



[TOC]

------



# Spring框架概述

1. Spring是轻量级的开源的JavaEE框架

2. Spring可以解决企业应用开发的复杂性

3. Spring有两个核心部分：IOC和Aop

   （1）IOC：控制反转，把创建对象过程交给Spring进行管理

   （2）Aop：面向切面，不修改源代码进行功能增强

4. Spring特点

   （1）方便解耦，简化开发

   （2）Aop编程支持

   （3）方便程序测试

   （4）方便和其他框架整合

   （5）方便进行事务操作

   （6）降低API开发难度
   
   

# IOC（Inversion of Control，控制反转）



## 什么是IOC

1. 控制反转，把对象创建和对象之间的调用过程，交给Spring进行管理
2. 使用IOC目的：为了耦合度降低
3. 做入门案例就是IOC实现

## IOC底层原理

1. xml解析、工厂模式、反射 

## IOC过程

1. xml配置文件，配置创建的对象

~~~xml
   <bean id="dao" class="com.john.UserDao"></bean>
~~~

2. 有service类和dao类，创建工程类

~~~java
class UserFactory{
    public static UserDao getDao(){
        //1.xml解析
        String classValue = class属性值；//xml解析
        //2.通过反射创建对象
        Class clazz = Class.forName(classValue);
        return (UserDao)clazz.newInstance();
    }
}
~~~

## IOC(接口） 

1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
2. Spring提供IOC容器实现两种方式：（两个接口）

   - BeanFactory：

     - IOC容器基本实现，是Spring内部的使用接口，不提供开发人员使用

     - 加载配置文件时不会创建对象，在获取对象（使用）才去创建对象

   - ApplicationContext：

     - BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员使用
     - 加载配置文件时就会把在配置文件对象进行创建
3. ApplicationContext  接口有实现类

## IOC操作Bean管理

1. 什么是Bean管理

   - Bean管理指的是两个操作
     - Spring创建对象
     - Spring注入属性
2. Bean管理操作有两种方式
   - 基于xml配置文件方式实现
   - 基于注解方式实现
### 基于xml配置文件方式实现

1. 基于xml方式创建对象

~~~xml
<!--配置User对象创建-->
<bean id="user" class="com.john.spring5.User"></bean>
~~~
2. 基于xml方式注入属性
   DI:依赖注入，就是注入属性
   2.1第一种注入方式：使用set方法进行注入
   （1）创建类，定义属性和对应的set方法

```java
        public class book{
       //创建属性
            private String bname;
            private String bauthor;
            //创建属性对应的set方法
            public void setBname(String bname){
                this.bname = bname;
            }
            public void setBauthor(String bauthor){
                this.bauthor = bauthor; 
            }
        }
```

​			(2)在Spring配置文件配置对象创建，配置属性注入

~~~xml
        <bean id="book" class="com.john.spring5.Book">
            <!--使用property完成属性注入
              name：类里面属性名称
              value：向属性注入的值
              -->
             <property name="bname" value="围城"></property>
             <property name="bauthor" value="钱钟书"></property>
        </bean>
~~~

​			2.2第二种注入方式：使用有参数构造进行注入

​			（1）创建类，定义属性，创建属性对应有参数构造方法

~~~java
        public class Orders{
            private String oname;
            private String address;

            public Order(String oname,String address){
                this.oname = name;
                this.address = address;
            }
        }
~~~

​				(2）在Spring配置文件中进行配置

~~~xml
        <!-- 有参数构造注入属性-->
        <bean id="orders" class="com.john.spring5.Orders">
            <constructor-arg name="oname" value="Computer"></constructor-arg>
            <constructor-arg name="address" value="China"></constructor-arg>
        </bean>
~~~

3. p名称空间注入（了解）

   使用p名称空间注入，可以简化基于xml配置方式

   （1）添加p名称空间在配置文件中

   ![image-20210312185149409](D:\Typora\storage\img\p名称注入)

    （2）进行属性注入，在bean标签里面进行操作

   ~~~xml
   <bean id="book"  class=“com.john.spring5.Book" p:bname="我们仨" p:bauthor="杨绛"></bean>
   ~~~

4. xml注入其他类型属性

   （1）null值

   ~~~xml
   <property name="address">
   	<null/>
   </property>
   ~~~

   (2)属性值包含特殊符号

   ~~~xml
   <!--属性值包含特殊符号
       1.把<>进行转义&lt;&gt;
       2.把带特殊符号内容写到CDATA
   -->
   <property name="address">
   	<value><![CDATA[<<南京>>]]></value>
   </property>
   ~~~

5. 注入属性-外部bean

   （1）创建两个类service类和dao类

   （2）在service调用dao里面的方法

   （3）在Spring配置文件中进行配置

6. 注入属性-内部bean和级联赋值

7. xml注入集合属性

   （1）注入数组类型属性

    ~~~xml
   <property name="courses">
   	<arrary>
       	<value>java</value>
           <value>mysql</value>
       </arrary>
   </property>
    ~~~

   （2）注入List集合类型属性

   ~~~xml
   <property name="list">
   	<list>
       	<value>张三</value>
           <value>法外狂徒</value>
       </list>
   </property>
   ~~~

   （3）注入Map集合类型属性

   ~~~xml
   <property name="maps">
   	<map>
       	<entry key="java" value="java"></entry>
           <entry key="mysql" value="mysql"></entry>
       </map>
   </property>
   ~~~

   8. 在集合里面设置对象类型值

   ~~~xml
   <!--创建多个course对象-->
   <bean id="course1" class="com.john.spring5.collectiontype.Course">
   	<property name="cname" value="Spring5框架"></property>
   </bean>
   <bean id="course2" class="com.john.spring5.collectiontype.Course">
   	<property name="cname" value="MyBatis框架"></property>
   </bean>
      
   <!--注入list集合类型，值是对象-->
   <property name="courseList">
   	<list>
   		<ref bean="couse1"></ref>
   		<ref bean="couse2"></ref>
   	</list>
   </property>
   ~~~

   9. 把集合注入部分提取出来

      - 在spring配置文件中引入名称空间util

        ~~~xml
        <?xml version="1.0" encoding="UTF-8"?>
        <bean xmlns="http://www.springframework.org/schema/beans"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xmlns:p="http://www.springframework.org/schema/p"
              xmlns:util="http://www.springframework.org/schema/util"
              xsi:schemaLocation="http://www.springframework.org/schema/beans
                          http://www.springframework.org/schema/beans/spring-beans.xsd
                                  http://www.springframework.org/schema/util
                          http://www.springframework.org/schema/util/spring-util.xsd
        ~~~

      - 使用util标签完成list集合注入提取

        ~~~xml
        <!--提取list集合类型属性注入-->
        <util:list id="bookList">
        	<value>疯狂讲义</value>
            <value>视觉十八讲</value>
        </util:list>
        
        <!--提取list集合类型属性注入使用-->
        <bean id="book" class="com.john.spring5.collectiontype.Book">
        	<property name="list" ref="bookList"></property>
        </bean>
        ~~~

   10. Spring有两种类型bean，一种普通bean，另外一种工厂bean（FactoryBean）

       - 普通bean：在配置文件中定义bean类型就是返回类型

       - 工厂bean：在配置文件定义bean类型可以和返回类型不一样

         - 第一步：创建类，让这个类作为工厂bean，实现接口FactoryBean

         - 第二布：实现接口里面的方法，在实现的方法中定义返回的bean类型 

           MyBean.java

           ~~~java
           public class MyBean implements FactoryBean<Course>{
               @Override
               public Course getObject() throws Exception{
                   Course course = new Course;
                   course.setCname("abc");
                   return course;
               }
               
               @Override
               public Class<?> getObjectType(){
                   return null;
               }
               
               @Override
               public boolean isSingleton(){
                   return false;
               } 
           }
           ~~~

           bean3.xml

           ~~~xml
           <bean id="myBean" class="com.john.spring5.factorybean.MyBean"></bean>
           ~~~

           ~~~java
           @Test
           public void test3(){
               ApplicationContext context =
                   new ClassPathXmlApplicationContext("bean3.xml");
               Course course = context.getBean("myBean",Course.class);
               System.out.println(course);
           }
           ~~~

   11. bean作用域

      - 在Spring里面，设置创建bean实例是单实例还是多实例
        
      - 在Spring里面，默认情况下，bean是单实例对象 
        
                 <img src="C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210412201449483.png" alt="image-20210412201449483"  />
        
      -  在spring配置文件bean标签里面有属性（scope）用于设置单实例还是多实例
          scope属性值：	--默认值，singleton，表示是单实例。加载spring配置文件时候就会                         												创建单实例对象
                 ​							 -- prototype，表示是多实例对象。在调用getBean方法时候创建多实 												例对象
                 ![image-20210412204013633](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210412204013633.png)

   12. bean生命周期

         - 生命周期：从对象创建到对象销毁的过程

         - bean生命周期
            - 通过构造器创建bean实例（无参数构造）
            - 为bean的属性设置值和对其他bean引用（调用set方法 ）
            - 调用bean的初始值的方法
            - bean可以使用了（对象获取到了）
            - 当容器关闭时候，调用bean的销毁的方法（需要进行设置销毁的方法)

             ```java
             public class Orders{
                 public orders(){
                     System.out.rintln("第一步，执行无参数结构创造bean实例");
                 }
                 
                 private String oname;
                 public void setOname(String oname){
                     this.name = oname;
                      System.out.rintln("第二步，调用set方法设置属性值");
                 }
                 
                 //创建执行的初始化方法
                 public void initMethod(){
                     System.out.rintln("第三步，执行初始化的方法");
                 }
                 
                 //创建执行的销毁的方法
                 public void destroyMethod(){
                     System.out.rintln("第五步，执行销毁的方法");
                 }
             }
             ```
            
             ```java
             @Test
             public void testBean3(){
                 ClassPathXmlApplicationContext context = 
                     new  ClassPathXmlApplicationContext("bean4.xml");
                 Order orders = context.getBean("orders",Orders.class);
                 System.out.rintln("第四步，获取创建bean实例对象");
                 System.out.rintln(orders);
                 
                 //手动让bean实例销毁
                 context.close();
             }
             ```
            
            bean4.xml:
            
            ~~~xml
            <bean id="order" class="com.john.spring5.bean.Orders" init-method="initMethod"       destroy-methon="destroyMethod">
            	<property name="oname" value="手机"></property>
            </bean> 
            ~~~
            
            ​	![image-20210412214240462](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210412214240462.png)
       
   13. xml自动装配

       - 自动装配：根据指定装配规则（属性名称或属性类型），Spring自动将匹配的属性进行注入。

         bean标签属性autowrite，配置自动装配。

          	--byName根据属性名称注入，注入值bean的id值和类属性名称一样

         ​	 --byType根据属性类型注入

   14. 外部属性文件

       - 直接配置数据库信息

         - 配置德鲁伊连接池
         - 引入德鲁伊连接池jar包（druid-1.1.9.jar）

         ~~~xml
         <!--直接配置连接池-->
         <bean id="dataSource" class="com.alibab.druid.pool.DruidDataSource">
         	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
             <property name="url" value="jdbc:mysql://localhost:3306/userDb">                   </property>
             <property name="username" value="root"></property>
             <property name="password" value="root"></property>    
         </bean>
         ~~~

       - 引入外部属性文件配置数据库连接池

         ~~~properties
         peop.driverClass=com.mysql.jdbc.Driver
         prop.url=jdbc:mysql://localhost:3306/userDb
         prop.userName=root
         prop.passsword=root
         ~~~

         ~~~xml
         <!--引入外部属性文件-->
         <context:property-placaholder location="classpath:jdbc.properties"/>
         
         <!--配置连接池-->
         <bean id="dataSource" class="com.alibab.druid.pool.DruidDataSource">
         	<property name="driverClassName" value="${prop.driverClass}"></property>
             <property name="url" value="${prop.url}"></property>
             <property name="username" value="${userName}"></property>
             <property name="password" value="${password}"></property>    
         </bean>
         ~~~

### 基于注解方式

1. 什么是注解

   （1）注解是代码特殊标记，格式：@注解名称（属性名称=属性值，属性名称=属性值..)

   （2）使用注解，注解作用在类上面，方法上面，属性上面

   （3）使用注解目的：简化xml配置

2. Spring针对Bean管理中创建对象提供注解

   （1）@Component

   （2）@Service

   （3）@Controller

   （4）@Repository
   
3. 基于注解方式实现对象创建
   
   - 引入依赖（spring-aop-5.2.6.RELEASE.jar)
   
   - 开启组件扫描
   
     ~~~xml
     <!--开启组件扫描
     	1 如果扫描多个包，多个包使用逗号隔开
     	2 扫描包上层目录
     -->
     <context:component-scan base-package="com.john"></context:component-scan>
     ~~~
   
   - 创建类，在类上面添加创建对象注解
   
     ~~~java
     import org.springframework.stereotype.Component;
     //在注解里面value属性值可以省略不写
     //默认值是类名称，首字母小写
     @Component(value="userService")  //类似bean中的id
     public class UserService{
         public void add(){
             System.out.println("service add...")
         }
     }
     ~~~
   
     ![image-20210413161343498](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210413161343498.png)
   
     ![image-20210413161413842](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210413161413842.png)
   
4. 开启组件扫描细节配置

   ~~~xml
   <!--实例1
   	use-default-filters="false"表示现在不适用默认filter，自己配置filter
   	context:include-filter，设置扫描哪些内容
   -->
   <context:component-scan base-package="com.john" use-default-filters="false">
   	<context:include-filter type="annotation"     	                  									 expression="org.springframework.stereotype.Controller"/>
   </context:component-scan>
   ~~~

   ~~~xml
   <!--实例2
   	下面配置扫描包所有内容
   	context:exclude-filter，设置哪些内容不进行扫描
   -->
   <context:component-scan base-package="com.john">
   	<context:exclude-filter type="annotation"     	                  									 expression="org.springframework.stereotype.Controller"/>
   </context:component-scan>
   ~~~

5. 基于注解方式实现属性注入

   （1）@Autowired：根据属性类型进行自动装配

   第一步：把service和dao对象创建，在service和dao类添加创建对象注解

   第二布：在service注入到对象，在service类添加dao类型属性，在属性上面使用注解

   ~~~~java
   public class UserService{
       //定义dao类型属性
       //不需要添加set方法
       //添加注入属性注解
       @Autowired
       private UserDao userDao
           
       public void add(){
           System.out.println("service add...");
           userDao.add();
       }
   }
   ~~~~

   （2）@Qualifer：根据属性名称进行注入

   这个@Qualifier注入的使用，和上面@Autowired一起使用

   ~~~java
   //定义dao类型属性
   //不需要添加set方法
   //添加注入属性注解
   @Autowired //根据类型进行注入
   @Qualifier(value="userDaoImpl1")
   private UserDao userDao;
   ~~~

   （3）@Resource：可以根据类型注入，可以根据名称注入

   ~~~java
   //@Resource  //根据类型进行注入
   @Resource(name="userDaoImpl1")  //根据名称进行注入
   private UserDao userDao;
   ~~~

   （4）@Value：注入普通类型属性

   ~~~java
   @Value(value="abc")
   private String name;
   ~~~

6. 完全注解开发

   （1）创建配置类，代替xml配置文件

   ~~~java
   @Configuration  //作为配置类，代替xml配置文件
   @ComponentScan(basePackages={"com.john"})
   public class SpringConfig{}
   ~~~

   （2）编写测试类

   ~~~java
   @Test
   public void testService2(){
       ApplicationContext context
           		   =new AnnocationConfigApplicationContext(SpringConfig.class);
       UserService userService = context.getBean("userService",UserService.class);
       System.out.println(userService);
       userService.add();
   }
   ~~~

   



# AOP(Aspect Oriented Programming,面向切面编程)

## 概念

什么是AOP

​	面向切面编程（方面）。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

 ## 底层原理

1. AOP底层使用动态代理

   （1）有两种情况动态代理

   ​		第一种，有接口情况下，使用JDK动态代理

   ​		创建接口实现类代理对象，增强类的方法

   ​		![image-20210414103737428](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210414103737428.png)

   ​			第二种，没有接口情况下，使用CGLIB动态代理

   ​			创建子类的代理对象，增强类的方法

   ​				<img src="C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210414104516659.png" alt="image-20210414104516659"  />

## JDK动态代理

1. 使用JDK动态代理，使用Proxy类里面的方法创建代理对象。

   ![image-20210414142807897](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210414142807897.png)

2. 编写JDK动态代码代理

   （1）创建接口，定义方法
   
   ```java
   public interface UserDdao{
       public int add(int a,int b);
       public String update(String id);
   }
   ```
   
   （2）创建接口实现类，实现方法
   
   ~~~java
   public class UserDaoImpl implements UserDao{
       @Override
       public int add(int a,int b){
           System.out.println("add方法执行了...");
           return a+b;
       }
       
       @Override
       public String update(String id){
           System.out.println("update方法执行了...");
           return id;
       }
   }
   ~~~
   
   （3）  使用Proxy类创建接口代理对象
   
   ~~~java
   public class JDKProxy{
       public static void main(String[] args){
           //创建接口实现类代理对象
           Class[] interfaces = {UserDao.class};
           UserDaoImpl userDao = UserDaoImpl()
           UserDao dao = (UserDao)Proxy.newProxyInstance(JDKProxy.getClassLoader()
                                                         ,interfaces
                                                         ,new UserDaoProxy(userDao));
           int result = dao.add(1,2);
           System.out.println("result:"+result);
       }
   }
   
   //创建代理对象代码
   class UserDaoProxy implements InvocationHandler{
       //把创建的是谁的代理对象，把谁传递过来
       //有参数构造传递
       private Object obj;
       public UserDaoProxy(Object obj){
           this.obj = obj;
       }
          
       //增强逻辑
       @Override
       public Object invoke(Object proxy,Method method,Object[] args) 
           throws Throwable{
           //方法之前
           System.out.println("方法之前执行..."+method.getName()
                              +":传递的参数..."+Arrays.toString(args));
           
           //被增强的方法执行
           Object res = method.invoke(obj,args);
           
           //方法之后
           System.out.println("方法之后执行..."+obj);
           return res;
       }
       
   }
   ~~~



## 操作术语

1. 连接点

   类里面哪些方法可以被增强，这些方法称为连接点

2. 切入点

   实际被真正增强的方法，称为切入点

3. 通知（增强）

   （1）实际增强的逻辑部分称为通知（增强）

   （2）通知有多种类型
     - 前置通知
     - 后置通知
     - 环绕通知
     - 异常通知
     - 最终通知
   
4. 切面

   把通知应用到切入点过程



## 准备工作

1. Spring框架一般都是基于AspectJ实现AOP

   - AspectJ不是Spring组成部分，是独立AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作。

2. 基于AspectJ实现AOP操作

   - 基于xml配置文件实现
   - 基于注解方式实现（使用）

3. 在项目工程里面引入AOP相关依赖

   ![image-20210414200125306](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210414200125306.png)

4. 切入点表达式

   （1）切入点表达式作用：知道对哪个类里面的哪个方法进行增强

   （2）语法结构：

   		execution([权限修饰符][返回类型][类全路径][方法名称]([参数列表]))
   		举例子1：对com.john.dao.BookDao类里面的add进行增强
   				execution(* com.john.dao.BookDao.add(..))     //*后面加空格
   		举例子2：对com.john.dao.BookDao类里面的所有方法进行增强
   				execution(* com.john.dao.BookDao.*(..))
   		举例子1：对com.john.dao包里面所有类，类里所有方法进行增强
   				execution(* com.john.dao.*.*(..))



## AspectJ注解

1. 创建类，在类里面定义方法

   ~~~java
   public class User{
       public void add(){
           System.out.println("add...");
       }
   }
   ~~~

2. 创建增强类（编写增强逻辑）

   （1）在增强类里面，创建方法，让不同方法代表不同通知类型

   ~~~java
   //增强的类
   public class UserProxy{
       //前置通知
       public void before(){
           System.out.println("before...");
       }
   }
   ~~~

3. 进行通知的配置

   （1）在spring配置文件中，开启注解扫描		![image-20210414203522643](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210414203522643.png)

   （2）使用注解创建User和UserProxy对象

   （3）在增强类上面添加注解@Aspect

   （4）在spring配置文件中开启生成代理对象

   ~~~xml
   <!--开启Aspect生成代理对象-->
   <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
   ~~~

4. 配置不同类型的通知

   （1）在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置

   ~~~java
    //增强的类
   @Component
   @Aspect    //生成代理对象
   public class UserProxy{
       //前置通知
       //@Before注解表示作为前置通知
       @Before(value = "execution(* com.john.spring5.aopanno.User.add(..))")
       public void before(){
           System.out.println("before...");
       } 
       
       //后置通知（返回通知）
       @AfterReturning(value = "execution(* com.john.spring5.aopanno.User.add(..))")
       public void afterReturning(){
           System.out.println("afterReturning...");
       } 
       
       //最终通知
       @After(value = "execution(* com.john.spring5.aopanno.User.add(..))")
       public void before(){
           System.out.println("after...");
       } 
       
       //异常通知
       @AfterThrowing(value = "execution(* com.john.spring5.aopanno.User.add(..))")
       public void before(){
           System.out.println("afterThrowing...");
       } 
       
       //环绕通知
       @Around(value = "execution(* com.john.spring5.aopanno.User.add(..))")
       public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
           System.out.println("环绕之前...");
           
           //被增强的方法执行
           proceedingJoinPoint.proceed();
           
           System.out.println("环绕之后...");
       } 
   }
   ~~~
   
   测试：
   
   ~~~java
   import com.john.spring5.aopanno.User;
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class TestAop{
       
       @Test
       public void testAopAnno(){
           ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
           User user = context.getBean("user", User.class);
           user.add();
       }
   }
   ~~~
   
5. 公共切入点抽象

   ~~~java
   public class UserProxy{
   
       @Pointcut(value = "execution(* com.john.spring5.aopanno.User.add(..))")
       public void pointdemo(){
           System.out.println("before...");
       } 
       
       @Before(value = "pointdemo")
       public void before(){
           System.out.println("before...");
       } 
   ~~~

6. 有多个增强类多个同一个方法进行增强，设置增强类优先级

   （1）在增强类上面添加注解@Order（数字类型值），数字类型值越小，优先级越高

   ~~~java
   @Component
   @Aspect
   @Order(1)
   public class PersonProxy
   ~~~

7. 完全使用注解开发

   （1）创建配置类，不需要创建xml配置文件

   ~~~java
   @Configuration
   @ComponentScan(basePackages ={"com.john"})
   @EnableAspectJAutoProxy(proxyTargetClass = true)
   public class ConfigAop{}
   ~~~



## AspectJ配置文件

1. 创建两个类，增强类和被增强类，创建方法

   ~~~java
   public class Book{
       public void buy(){
           System.out.println("buy...");
       }
   }
   ~~~

   ~~~java
   public class BookProxy{   
       public void before(){
           System.out.println("before...");
       } 
   ~~~

2. 在spring配置文件中创建两个类对象

   ~~~xml
   <!--创建对象-->
   <bean id="book" class="com.john.spring5.aopxml.Book"></bean>
   <bean id="bookProxy" class="com.john.spring5.aopxml.BookProxy"></bean>

3. 在spring配置文件中配置切入点

   ~~~xml
   <!--配置aop增强-->
   <aop:config>
   	<!--切入点-->
       <aop:pointcut id="p" expression="execution(* com.john.spring5.aopxml.Book.buy(..))"/>
       
       <!--配置切面-->
       <aop:aspect ref="bookProxy">
       	<aop:before method="before" pointcut-ref="p"/>
       </aop:aspect>
   </aop:config>
   ~~~

# JdbcTemplate

## 概念和准备

1. 什么是JdbcTemplate

   （1）Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库操作

2. 准备工作

   （1）引入相关jar包

   ![image-20210515145815272](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210515145815272.png)

   （2）在spring配置文件配置数据库连接池
	
	~~~xml
	<!--数据库连接池-->
	<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="class">
		<property name="url" value="jdbc:mysql:///user_db"/>
	    <property name="username" value="root" />
	    <property name="password" value="root" />
	    <property name="driverClassName" value="com.mysql.jdbc.Driver" />
	</bean>
	~~~
	
	（3）配置JdbcTemplate对象，注入DataSource
	
	~~~xml
	<!--JdbcTemplate对象-->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!--注入dataSource-->
	    <property name="dataSource" ref="dataSource">
	    </property>
	</bean>
	~~~
	
	（4）创建service类，创建dao类，在dao注入JdbcTemplate对象
	
	- 配置文件
	
	  ~~~xml
	  <!--组件扫描-->
	  <context:component-scan base-package="com.john"></context:component-scan>
	  ~~~
	
	- Service
	
	  ~~~java
	  @Service
	  public class BookService{
	      //注入dao
	      @Autowired
	      private BookDao;
	  }
	  ~~~
	
	- Dao
	
	  ~~~java
	  @Repository
	  public class BookDaoImpl implements BookDao{
	      //注入JdbcTemplate
	      @Autowired
	      private JdbcTemplate jdbcTemplate; 
	  }
	  ~~~



## 操作数据库（添加）

1. 对应数据库

   ~~~java
   public class User{
       private String userId;
       private String username;
       private String ustatus;
       
       public void setUserId(String userId){
           this.userId = userId;
       }
       
       public void setUsername(String username){
           this.username = username;
       }
       
       public void setUstatus(String ustatus){
           this.ustatus = ustatus;
       }
       
       public String getUserId(){
           return userId;
       }
       
       public String getUsername(){
           return username;
       }
       
       public String getUstatus(){
           return ustatus;
       }
   }
   ~~~

2. 编写service和dao

   （1）在dao进行数据库添加操作

   （2）调用JdbcTemplate对象里面update方法实现添加操

   ~~~html
   update(String sql,Object... args)
   第一个参数：sql语句
   第二个参数：可变参数，设置sql语句值
   ~~~

   ~~~java
   @Repository
   public class BookDaoImpl implements BookDao{
       //注入JdbcTemplate
       @Autowired
       private JdbcTemplate jdbcTemplate;
       
       //添加的方法
       @Override
       public void add(Book book){
           //1.创建sql语句
           String sql ="insert into t_book value(?,?,?)";
           //2.调用方法实现
           Object[] args = {book.getUserId(),book.getUsername(),book.getUstatue()};
           int update = jdbcTemplate.update(sql,args);
           System.out.print(update);    
       }
   }
   ~~~

3. 测试类

   ~~~java
   public class TestBook{
       @Test
       public void testJdbcTemplate(){
           ApplicationContext context=
               new ClassPathXmlApplicationContext("bean.xml");
           BookService bookService = context.getBean("bookService",BookService.class);
           
           Book book = new Book();
           book.setUserId("1");
           book.setUsername("java");
           book.setUstatus("a");
           bookService.addBook(book);
       }
   }
   ~~~

## 修改和删除

1. BookService：

   ~~~java
   public class BookService{
       //注入dao
       @Autowired
       private BookDao bookDao;
       
       //添加的方法
       public void addBook(Book book){
           bookDao add(book);
       }
       
       //修改的方法
       public void updateBook(Book book){
           bookDao updateBook(book);
       }
       
        //删除的方法
       public void deleteBook(String id){
           bookDao delete(id);
       }
   }
   ~~~

2. BookDao

   ~~~java
   public interface BookDao{
       //添加的方法
       void add(Book book);
       
       //修改的方法
       void updateBook(Book book);
       
       //删除的方法
       void delete(String id);
   }
   ~~~

3. BookDaoImpl

   ~~~java
   @Repository
   public class BookDaoImpl implements BookDao{
       //注入JdbcTemplate
       @Autowired
       private JdbcTemplate jdbcTemplate;
       
       //添加的方法
       @Override
       public void add(Book book){
           //1.创建sql语句
           String sql ="insert into t_book value(?,?,?)";
           //2.调用方法实现
           Object[] args = {book.getUserId(),book.getUsername(),book.getUstatue()};
           int update = jdbcTemplate.update(sql,args);
           System.out.print(update);    
       }
       
        //修改的方法
       @Override
       public void uodateBook(Book book){
           String sql ="uodate t_book set username=?,ustatus=? where user_id";
           Object[] args = {book.getUsername(),book.getUstatue(),book.getUserId()};
           int update = jdbcTemplate.update(sql,args);
           System.out.print(update);    
       }
       
        //删除的方法
       @Override
       public void add(Book book){
           String sql ="delete from t_book where user_id=?";
           int update = jdbcTemplate.update(sql,id);
           System.out.print(update);    
       }
   }
   ~~~

4. TestBook

   ~~~java
   public class TestBook{
       @Test
       public void testJdbcTemplate(){
           ApplicationContext context=
               new ClassPathXmlApplicationContext("bean.xml");
           BookService bookService = context.getBean("bookService",BookService.class);
           
           //添加
           Book book = new Book();
           book.setUserId("1");
           book.setUsername("java");
           book.setUstatus("a");
           bookService.addBook(book);
           
           //修改
           Book book = new Book();
           book.setUserId("1");
           book.setUsername("javaweb");
           book.setUstatus("ab");
           bookService.addBook(book);
           
           //删除
   		bookService.delete("1"); 
       }
       
   }
   ~~~

## 查询返回某个值

1. 查询表里有多少条记录，返回是某个值

2. 使用JdbcTemplate查询返回某个值的代码

   ~~~html
   queryForObject(String sql,Class<T> requiredType)
   第一个参数：sql语句
   第二个参数：返回类型Class
   ~~~

   ~~~java
   //查询表记录数
   @Override
   public int selectCount(){
       String sql = "select count(*) from t_book";
       Integer count = jdbcTemplate queryForObject(sql,Integer.clas);
       return count;
   }
   ~~~

## 查询返回对象

1. 场景：查询图书详情

2. JdbcTemplate实现查询返回对象

   ~~~html
   queryForObject(String sql,RowMapper<T> rowMapper, Object...args)
   第一个参数：sql语句
   第二个参数：RowMapper，是接口，返回不同类型数据，使用这个接口里面实现类完成数据封装
   第三个参数：sql语句值
   ~~~

   ~~~java
   //查询返回对象
   @Override
   public Book findBookInfo(String id){
       String sql = "select * from t_book where user_id=?";
       //调用方法
       Book book = jdbcTemplate queryForObject(sql,new BeanPropertyRowMapper<Book>(Book.class),id);
       return book;
   }
   ~~~

## 查询返回集合

1. 场景：查询图书列表分页...

2. 调用JdbcTemplate方法实现查询返回集合

   ~~~html
   query(String sql,RowMapper<T> rowMapper, Object...args)
   第一个参数：sql语句
   第二个参数：RowMapper，是接口，返回不同类型数据，使用这个接口里面实现类完成数据封装
   第三个参数：sql语句值
   ~~~

   ~~~java
   //查询返回集合
   @Override
   public List<Book> findAllBook(){
       String sql = "select * from t_book";
       //调用方法
       Book bookList = jdbcTemplate query(sql,new BeanPropertyRowMapper<Book>(Book.class));
       return bookList;
   }
   ~~~

## 批量操作

1. 批量操作：操作表里多条记录

2. JdbcTemplate实现批量添加操作

   ~~~html
   batchUpdate(String sql,List<Obkect[]> batchArgs)
   第一个参数：sql语句
   第二个参数：List集合，添加多条记录数据
   ~~~

   ~~~java
   //批量添加
   @Override
   public void batchAdd(List<Object[]> batchArgs){
       String sql = "insert into t_book value(?,?,?)";
       int[] ints = jdbcTemplate batchUpdate(sql,batchArgs)
       System.out.println(Arrays.toString(ints));
       
   }
   ~~~



# 事物操作

## 事物概念

1. 什么事物

   （1）事务是数据库操作最基本单元，逻辑上一组操作，要么都成功，如果有一个失败所有操作都失败。

2. 事物四大特性（ACID)

   （1）原子性

   （2）一致性

   （3）隔离性

   （4）持久性

## 搭建事物操作环境

![image-20210516161223865](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210516161223865.png)

1. 创建数据库表，添加记录

   ![image-20210516161443366](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210516161443366.png)

2. 创建service，搭建dao，完成对象创建和注入关系

3. 在dao创建两个方法：多钱和少钱的方法，在service创建方法（转账的方法）

   service注入dao，在dao注入JdbcTempl，在Jdbctemplate注入DataSource

   bean.xml:

   ~~~xml
   <!--JdbcTemplate对象-->
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">   
       <!--注入dataSource-->    
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ~~~

   

   UserDao：

   ~~~java
   public interface UserDao{
       //多钱的方法
       public void addMoney();
       //少钱的方法
       public void reduceMoney();
   }
   ~~~

   UserDaoImpl:

   ~~~java
   @Repository
   public class UserDaoImpl implements UserDao{
       @Autowired
       private JdbcTemplate jdbcTemplate;
       
       //少钱
       @Override
       public void reduceMoney(){
           String sql ="update t_account set money=money-? where username=?";
           jdbcTemplate update(sql,"100","lucy");
       }
       
       //多钱
       @Override
       public void addMoney(){
           String sql ="update t_account set money=money+? where username=?";
           jdbcTemplate update(sql,"100","mary");
       }
       
   }
   ~~~

   UserService:

   ~~~java
   public class UserService{
       // 注入dao
       @Autowired
       private UserDao userDao;
       
       //转账的方法
       @Override
       public void accountMoney(){
           //Lucy少100
           userDao reduceMoney();
           
           //Mary多100
           userDao addMoney();
       }
   }
   ~~~


## Spring事务管理介绍

1. 事务添加到JavaEE三层结构里面Service层（业务逻辑层）

2. 在Spring进行事务管理操作

   （1）有两种方式：编程式事务管理和声明式事务管理（使用）

3. 声明式事务管理

   （1）基于注释方式

   （2）基于xml配置文件方式

4. 在Spring进行声明式事务管理，底层使用Aop原理

5. Spring事务管理API

   （1）提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类

   ![image-20210518223938633](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210518223938633.png)



## 注解声明式事务管理

1. 在Spring配置文件配置事务管理器

   bean.xml:

   ~~~xml
   <!--创建事务管理器-->
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   	<!--注入数据源-->
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ~~~


2. 在spring配置文件，开启事务注解

   （1）在spring配置文件引入名称空间tx

   ~~~xml
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                              http://www.springframework.org/schema/aop http://www.springframework.org/schema/beans/spring-aop.xsd
                              http://www.springframework.org/schema/tx http://www.springframework.org/schema/beans/spring-tx.xsd">
   ~~~

   (2) 开始事务的注解

   ~~~xml
   <!--开启事务注解-->
   <tx:annotation-driven transaction-manager="transactionManeger"></tx:annotation-driven>
   ~~~

3. 在service类上面（获取service类里面方法上面）添加事务注解

   （1）@Transactional,这个注解添加到类上面，也可以添加到方法上面

   （2）如果把这个注解添加类上面，这个类里面所有的方法都添加事务

   （3）如果把这个注解添加方法上面，为这个方法添加事务

      ~~~java
      @Service
      @Transactional
      public class UserService{}
      ~~~

## 声明式事务管理参数配置

1. 在service类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数

   ![image-20210519204550478](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210519204550478.png)

2. propagation：事务传播行为
   （1）多事务方法直接进行调用，这个过程中事务是如何进行管理的（事务方法：对数据库表数据进行变化的操作）
   
3. ioslation：事务隔离级别

   （1）事务有特性成为隔离性，多事务之间不会产生影响，不考虑隔离性产生很多问题

   （2）有三个读问题：脏读、不可重复读、虚（幻）读

   （3）通过设置事务隔离级别，解决读问题

   ![image-20210520105133088](C:\Users\朱钧\AppData\Roaming\Typora\typora-user-images\image-20210520105133088.png)

4. timeout：超时时间

5. readOnly：是否只读

6. rollbackFor：回滚

7. noRollbackFor：不回滚



## XML声明式事务管理

1. 在spring配置文件中进行配置

   第一步 配置事务管理器

   第二步 配置通知

   第三部 配置切入点和切面

   ~~~xml
   <!--JdbcTemplate对象-->
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">   
       <!--注入dataSource-->    
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   
   <!--1 创建事务管理器-->
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   	<!--注入数据源-->
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   
   <!--2 配置通知-->
   <tx:advice id="txadvice">
   	<!--配置事务参数-->
       <tx:attributes>
       	<!--指定哪种规则的-->
           <tx:method name="accountMonry" propagation="REQUIRED" />
           <!--<tx:method name="account*" />-->
       </tx:attributes>
   </tx:advice>
   
   <!--3 配置切入点和切面-->
   <aop:config>
       <!--配置切入点-->
   	<aop: pointcut id="pt" expression="execution(* com.john.spring5.service.UserService.*(..))" />
       <!--配置切面-->
       <aop:advisor advice-ref="txadvice" pointcut-ref="pt" />
   </aop:config>
   ~~~



## 完全注解声明式事务管理

1. 创建配置类，使用配置类替代xml配置文件

   ~~~java
   @Configuration //配置类
   @ComponentScan(basePackages = "com.john")
   @EnableTransactionManagement //开启事务
   public class TxConfig{
       //创建数据库连接池
       @Bean
       public DruidDataSource getDruidDataSource(){
           DruidDataSource dataSource = new DruidDataSource();
           dataSource.setDriverClassName("com.mysql.jdbc.Driver");
           dataSource.setUrl("jdbc:mysql:///user_db");
           dataSource.setUsername("root");
           dataSource.setPassword("root");
           return dataSource;
       }
       
       //创建JdbcTemplate对象
       @Bean
       public JdbcTemplate getJdbcTemplate(){
           //到ioc容器中根据类型找到dataSource
           JdbcTemplate jdbcTemplate =new JdbcTemplate();
           //注入DataSource
           jdbcTemplate.setDataSource(dataSource);
           return jdbcTemplate;
       }
       
       //创建事务管理器
       public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource){
           DataSourceTransactionManager transactionManager= new DataSourceTransactionManager();
           transactionManager.setDataSource(dataSource);
           return transactionManager；
       }
   }
   ~~~

   

