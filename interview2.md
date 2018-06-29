1. springboot核心自动装配

答：

* SpringBootApplication注解由配置、自动配置可用、扫描三个注解组合而成，核心注解是EnableAutoConfiguration，它import一个class----EnableAutoConfigurationImportSelector使用SpringFactoriesLoader.loadFactoryNames扫描META-INF/spring.factories,发现自动装配的核心类，使用ConfigurationProperties，EnableConfigurationProperties，ConditionalOnClass,ConditionalOnProperties,ConditionalOnMissBean，并在classpath下META-INF/spring.fatories中添加自动装配触发类

2. spring的生命周期

答：
* spring的生命周期涉及到BeanFacttoryPostProcessor(bean工厂后置处理器)、BeanPostProcessor(bean后置处理器)、InstantiationAwareBeanPostProcessor(实例化监听bean后置处理器)、BeanFactoryAware(bean工厂监听)、BeanNameAware(bean名字监听)、InitializingBean(初始化bean)、DisposableBean(销毁bean)接口对应的方法及bean的init-method和destroy-method
* bean工厂后置处理器构造方法-->beanFactoryPostProcessory的postProcessBeanFactory方法--—>beanPostProcessory构造方法-->实例化监听bean后置处理器构造方法-->
* 实例化监听bean后置处理器的postProcessBeforeInstantition方法-->bean构造器-->实例化监听bean后置处理器的postProcessAfterIntantiation方法-->实例化监听bean后置处理器的postProcessPropertyValues方法-->bean的set方法-->beanName监听的设置beanName方法-->bean工厂监听的设置bean工厂的方法-->bean后置处理器的初始化之前的后置处理-->初始化bean接口的afterPropertySet方法-->bean的初始化方法-->bean后置处理器的初始化后的的后置处理方法-->
* disposableBean的destroy方法-->bean的destorymehod方法

* 整体思路bean工厂后置处理器实例化---后调用后置处理方法---bean后置处理器实例化--实例化监听bean后置处理器实例化   

* bean实例化前后执行实例化监听bean后置处理器的实例化前后方法，再执行属性值后置处理方法---调用set注入属性--设置beanName---设置Bean工厂

* 初始化 调用afterPropertySet和bean的初始化方法前后执行bean后置处理器的初始化前后方法最后执行销毁动作

3。大型网站架构

答：
* 高性能
    * 指标 ---响应时间、吞吐量、并发数、性能计数器
    * 前端优化
        * 浏览器优化--减少http请求、浏览器缓存、启用压缩、CSS前js后、减少cookie传输
        * cdn加速 
        * 反向代理（也可以实现负载均衡）---传统代理在浏览器侧
    * 应用服务器优化
        * 分布式缓存---关键字、频繁修改、热点、不一致、可用性、预热、缓存穿透
        * 异步
        * 集群
        * 代码优化--多线、资源复用、数据结构、JVM优化
    * 存储优化
        * 机械硬盘VS固态硬盘
        * RAID VS HDFS
* 高可用
    * 指标---4个9 ，故障分--------关键字：数据和服务的冗余备份和失效转移
    * 应用高可用--负载均衡、session管理
    * 服务高可用--分级、超时、异步、服务降级（拒绝服务、关闭服务）、幂等设计（多次调用的一致性设计）
    * 数据高可用--CAP原理、数据一致等级、数据备份（冷备、异步热备、同步热备）、失效转移（失效确认（心跳、报告）、访问转移、数据恢复）
    * 发布-自动化测试（selenium）、预发布验证、自动发布、灰度发布（ab测试）
    * 运行监控---数据采集（日志。服务器性能）、监控管理（告警、失效转移，自动降级）
* 伸缩性
    * 应用服务器伸缩性--不同功能物理拆分、相同管理集群（http重定向负载均衡、DNS域名解析负载均衡、反向代理负载均衡、IP负载均衡、数据链路层负载均衡（LVS））
    * 缓存的伸缩性--模拟请求预热 Hash一致算法 150虚拟比例
    * 数据存储的伸缩性--- 避免事务，利用事务补偿机，分解数据逻辑访问避免join操作 数据库中间件，Hbase
* 扩展性 （开闭原则）
    * 分布式消息队列  EDA事件驱动架构
    * 分布式可复用服务
    * 生态圈
* 安全问题
    * 攻击 -XSS(消毒。HTTPOnly) sql注入（避免暴露数据库设计，预编译）CSRF（表单token，验证码，请求头referer check）web应用防火墙-ModSecurity。siteshell
    * 加密 -单向散列加盐，非对称安全传输，数字签名防抵赖，秘钥分片管理
    * 风控 -统计模型预测
