# LifeCycle
A better understanding on Bean LifeCycle.
![avatar](/img/BeanLifeCycle.jpg)
现在我们回顾一下一个完整的Bean生命周期会经历一下步骤：
<br>1、	现在开始初始化容器
<br>2、	这是BeanFactoryPostProcessor实现类构造器！！
<br>3、	BeanFactoryPostProcessor调用postProcessBeanFactory方法
```java
public interface BeanFactoryPostProcessor {
    void postProcessBeanFactory(ConfigurableListableBeanFactory var1) throws BeansException;
}
```
<br>postProcessBeanFactory()：在标准初始化后修改应用程序上下文的内部bean工厂。
<br>BeanDefinition：描述了一个bean实例，它具有属性值、构造函数参数值和由具体实现提供的进一步信息。
<br>4、	这是BeanPostProcessor实现类构造器！！
<br>5、	这是InstantiationAwareBeanPostProcessorAdapter实现类构造器！！
<br>6、	InstantiationAwareBeanPostProcessor调用postProcessBeforeInstantiation方法
<br>7、	InstantiationAwareBeanPostProcessor调用postProcessAfterInstantiation方法
<br>8、	InstantiationAwareBeanPostProcessor调用postProcessPropertyValues方法
```java
public interface InstantiationAwareBeanPostProcessor extends BeanPostProcessor {
    Object postProcessBeforeInstantiation(Class<?> var1, String var2) throws BeansException;

    boolean postProcessAfterInstantiation(Object var1, String var2) throws BeansException;

    PropertyValues postProcessPropertyValues(PropertyValues var1, PropertyDescriptor[] var2, Object var3, String var4) throws BeansException;
}
```
<br>（1）在目标bean实例化之前应用此Bean后置处理器。
<br>（2）在实例化bean之后，通过构造函数或工厂方法，但是在Spring属性填充（来自显式属性或自动连接）发生之前，执行操作。
<br>（3）在工厂将它们应用到给定的bean之前，对给定的属性值进行后处理，而不需要任何属性描述符。
<br>9、	BeanPostProcessor接口方法postProcessBeforeInitialization对属性进行更改！
在任何bean初始化回调之后（如InitializingBean的afterPropertiesSet或自定义的init-method），将该BeanPostProcessor应用于给定的新bean实例。
<br>10、	BeanPostProcessor接口方法postProcessAfterInitialization对属性进行更改！
<br>11、	InstantiationAwareBeanPostProcessor调用postProcessBeforeInstantiation方法
<br>12、	【构造器】调用Person的构造器实例化
<br>13、	InstantiationAwareBeanPostProcessor调用postProcessAfterInstantiation方法
<br>14、	InstantiationAwareBeanPostProcessor调用postProcessPropertyValues方法
<br>15、	【注入属性】注入属性address
<br>16、	【注入属性】注入属性name
<br>17、	【注入属性】注入属性phone
<br>18、	【BeanNameAware接口】调用BeanNameAware.setBeanName()
将配置文件中Bean对应的名称设置到Bean中。
<br>19、	【BeanFactoryAware接口】调用BeanFactoryAware.setBeanFactory()
将BeanFactory实例设置到Bean中。
<br>20、	BeanPostProcessor接口方法postProcessBeforeInitialization对属性进行更改！
<br>21、	【InitializingBean接口】调用InitializingBean.afterPropertiesSet()
<br>22、	【init-method】调用<bean>的init-method属性指定的初始化方法
<br>23、	BeanPostProcessor接口方法postProcessAfterInitialization对属性进行更改！
<br>24、	容器初始化成功
<br>25、	Person [address=广州, name=张三, phone=110]
<br>26、	现在开始关闭容器！
<br>27、	【DiposibleBean接口】调用DiposibleBean.destory()
<br>28、	【destroy-method】调用<bean>的destroy-method属性指定的初始化方法
<br>注：
<br>1、Bean后处理器：即当Spring容器实例化Bean实例之后进行的增强处理。
<br>2、容器后处理器：对容器本身进行处理，并总是在容器实例化其他任何Bean之前读取配置文件的元数据并可能修改这些数据。
