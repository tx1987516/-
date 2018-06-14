1. springboot核心自动装配

答：

* SpringBootApplication注解由配置、自动配置可用、扫描三个注解组合而成，核心注解是EnableAutoConfiguration，它import一个class----EnableAutoConfigurationImportSelector使用SpringFactoriesLoader.loadFactoryNames扫描META-INF/spring.factories,发现自动装配的核心类，使用ConfigurationProperties，EnableConfigurationProperties，ConditionalOnClass,ConditionalOnProperties,ConditionalOnMissBean，并在classpath下META-INF/spring.fatories中添加自动装配触发类

2. spring的生命周期

答：
* spring的生命周期涉及到BeanFacttoryPostProcessor(bean工厂后置处理器)、BeanPostProcessor(bean后置处理器)、InstantiationAwareBeanPostProcessor(实例化监听bean后置处理器)、BeanFactoryAware(bean工厂监听)、BeanNameAware(bean名字监听)、InitializingBean(初始化bean)、DisposableBean(销毁bean)接口对应的方法及bean的init-method和destroy-method
* bean工厂后置处理器构造方法-->beanFactoryPostProcessory的postProcessBeanFactory方法--—>beanPostProcessory构造方法-->实例化监听bean后置处理器构造方法-->
* 实例化监听bean后置处理器的postProcessBeforeInstantition方法-->bean构造器-->实例化监听bean后置处理器的postProcessAfterIntantiation方法-->实例化监听bean后置处理器的postProcessPropertyValues方法-->bean的set方法-->beanName监听的设置beanName方法-->bean工厂监听的设置bean工厂的方法-->bean后置处理器的实例化之前的后置处理-->初始化bean接口的afterPropertySet方法-->bean的初始化方法-->bean后置处理器的实例化后的的后置处理方法-->
* disposableBean的destroy方法-->bean的destorymehod方法

* 整体思路bean工厂后置处理器实例化---后调用后置处理方法---bean后置处理器实例化--实例化监听bean后置处理器实例化   

* bean实例化前后执行实例化监听bean后置处理器的实例化前后方法，再执行属性值后置处理方法---调用set注入属性--设置beanName---设置Bean工厂

* 初始化 调用afterPropertySet和bean的初始化方法前后执行bean后置处理器的初始化前后方法最后执行销毁动作