## Spring

### IOC

#### IOC介绍和概念引出

- IOC控制反转是Spring框架的核心内容，控制反转的意思是由Spring框架来控制对象的生命周期和对象间的关系。
  - 在原来的场景中，被注入对象 直接依赖 被依赖对象，现在被注入对象去找IOC容器，让容器给它要依赖的被依赖对象。
  - ![image-20220629214351396](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220629214351396.png)
- 被注入对象有两种常用方式去，告诉IOC容器自己需要哪些被依赖对象，即有两种注入方式，构造方法注入和setter方法注入。
  - 接口注入：
  - 构造方法注入：被注入对象在其构造方法中将被依赖对象作为参数列表声明
    - ![image-20220629215021226](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220629215021226.png)
  - setter方法注入：被注入对象为其声明的被依赖对象变量添加setter方法，就可以将注入被依赖对象。
    - ![image-20220629231456577](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220629231456577.png)
  - <img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220629231921697.png" alt="image-20220629231921697" style="zoom:80%;" />
- 由前面说的可知，Spring实现IOC功能的基础是IOC容器，IOC容器有BeanFactory和ApplicationContext。
  - IOC容器的职责：加载配置信息用来创建Bean对象、管理Bean对象的生命周期、管理Bean对象间的依赖关系。
  - 这一点也可以叫IOC容器提供的IOC服务有哪些。IOC容器不是只能提供IOC服务，也能提供其他服务。

#### BeanFactory和ApplicationContext的区别

- Application继承了BeanFactory，在BeanFactory的基础上提供了其他高级特性，比如事件发布等。所以介绍BeanFactory差不多就是在介绍ApplicationContext了。
  - ![image-20220630000721952](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220630000721952.png)

- 初始化策略不同：BeanFactory采用延迟初始化策略，只有要访问容器中某个对象时，容器才会初始化这个对象，然后进行依赖注入操作。容器启动初期速度快，所需资源不多；ApplicationContext在启动时就完成所有初始化，它本身要的资源就多一些。
- <img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701193211543.png" alt="image-20220701193211543" style="zoom: 67%;" />

#### 依赖关系管理方式

>  副标题：如何管理对象间依赖关系（即BeanFactory的对象注册与依赖绑定方式）

- 外部配置文件
  - ![image-20220630200153114](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220630200153114.png)
  - properties（yml？）配置文件的加载
  - XML配置文件的加载
    - <beans>里面包含多个<bean>，还有些其他的的每部标签
    - <bean>定义单个对象
      - id属性：身份证号。一个可以指代它的字符串？
      - name属性：可以指代对象的别名，格式更灵活
      - class属性：对象的类型，是哪一个类
    - Spring中出现同名的Bean怎么办？
    - <constructor-arg>构造方法注入的XML、<property>setter方法注入的XML
      - 里面有些东西，<value>, <ref>, 内部<bean>, <list>, <map>, <set>
      - ![image-20220701200656970](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701200656970.png)
    - bean的scope（范围）
      - singleton，这里就是一个Spring中的单例模式了，一直存活
      - prototype，多实例，自生自灭（GC？）
      - request、session、global session只用于web
    - FactoryBean工厂方法
      - 这又是一个设计模式了。注意不是BeanFactory
      - <img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701200206891.png" alt="image-20220701200206891" style="zoom: 67%;" />
- 注解
  - 有哪些注释使声明Bean的：Component、Controller、Service、Repository
  - @Autowired是主角，告知容器要为当前对象注入哪些依赖对象。
    - @Resource和@Autowired的区别
    - ![image-20220701202916430](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701202916430.png)
    - <img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701212035646.png" alt="image-20220701212035646" style="zoom:80%;" />
    - <img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701212255835.png" alt="image-20220701212255835" style="zoom:80%;" />
  - 配合@Component和再xml中开启component-scanning就可以注入了？？
- 基于Java Api的配置？？

#### 附加一：Bean的生命周期

- Bean的生命周期可以概括为：**4个阶段 + 2个切入点**
  - ![image-20220630221154687](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220630221154687.png)
  - ![image-20220630221115492](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220630221115492.png)
- 第一阶段：Bean的实例化与BeanWrapper
  - ![image-20220701082814449](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701082814449.png)
- 第一个切入点：各色的Aware接口
  - ![image-20220701183531755](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701183531755.png)
- 第二个切入点：BeanPostProcessor
  - BeanPostProcessor有两个方法
  - ![image-20220701184314188](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701184314188.png)
- 第二阶段：InitializingBean和init-method
  - 一个是spring写死的方法，一个是可以自定义的方法？？
  - ![image-20220701192039323](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701192039323.png)
  - ![image-20220701192100795](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701192100795.png)
- 第三阶段：使用Bean
- 第四阶段：DisposableBean与destroyed-method
  - ![image-20220701192601478](C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701192601478.png)
- <img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701194139093.png" alt="image-20220701194139093" style="zoom:67%;" />

#### 附加二：Spring(Bean)的循环依赖

<img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701205913177.png" alt="image-20220701205913177" style="zoom: 80%;" />

​								单例对象初始化步骤

<img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701205954097.png" alt="image-20220701205954097" style="zoom:80%;" />

<img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701210100117.png" alt="image-20220701210100117" style="zoom:80%;" />

<img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701210218345.png" alt="image-20220701210218345" style="zoom:80%;" />

#### 附加三：单例Bean是否有线程安全问题

> 多例咋就不问，就没有吗？

<img src="C:\Users\14296\AppData\Roaming\Typora\typora-user-images\image-20220701211118392.png" alt="image-20220701211118392" style="zoom:80%;" />