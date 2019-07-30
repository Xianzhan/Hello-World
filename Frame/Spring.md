<!-- TOC -->

- [Interview](#interview)
- [SpEL](#spel)
- [生命周期](#生命周期)
- [资源](#资源)

<!-- /TOC -->

# Interview

[29题弄懂Spring底层原理，这些题目你都会吗？](https://mp.weixin.qq.com/s/LDglUrsocEkH8NwBjDXkPA)<br>
[69道Spring面试题和答案](https://zhuanlan.zhihu.com/p/38131490)<br>

# SpEL

Spring Expression Language

[Spring的EL表达式](https://blog.csdn.net/keda8997110/article/details/52767087)<br>

# 生命周期

```shell
Bean 初始化
↓
注入属性
↓
调用 BeanNameAware.setBeanName 方法
↓
调用 BeanFactoryAware.setBeanFactory 方法
↓
调用 ApplicationContextAware.setApplicationContext() 方法
↓
BeanPostProcessor.beforInit 方法
↓
@PostConstruct
↓
InitializingBean.afterPropertiesSet
↓
Init-method 调用定制的初始化方法
↓
BeanPostProcessor.afterInit 方法
↓
Bean 准备就绪
↓
是否实现 DisposableBean 接口, 调用 destroy
↓
destroy-method 调用定制的销毁方法
```

1. 首先容器启动后，对 bean 进行初始化
2. 按照 bean 的定义，注入属性
3. 检测该对象是否实现了 xxxAware 接口，并将相关的 xxxAware 实例注入给 bean，如 BeanNameAware 等
4. 以上步骤，bean 对象已正确构造，通过实现 BeanPostProcessor 接口，可以再进行一些自定义方法处理。如: postProcessBeforeInitialzation。
5. BeanPostProcessor 的前置处理完成后，可以实现 postConstruct，afterPropertiesSet, init-method 等方法， 增加我们自定义的逻辑，
6. 通过实现 BeanPostProcessor 接口，进行 postProcessAfterInitialzation 后置处理
7. 接着Bean准备好被使用啦。
8. 容器关闭后，如果 Bean 实现了 DisposableBean 接口，则会回调该接口的 destroy() 方法
9. 通过给 destroy-method 指定函数，就可以在 bean 销毁前执行指定的逻辑

# 资源

[Spring 源码导读](https://mp.weixin.qq.com/s/-Ce5T6LIzFe-TLTxmjYuSQ)<br>
[Spring事务管理（一）快速入门](https://mp.weixin.qq.com/s/yw5qZg3X2U5b2jjU9PI5Qw)<br>
[Spring事务管理（二）分布式事务管理之JTA与链式事务](https://mp.weixin.qq.com/s/Z20VyyzdWysJC6U0lMEvoQ)<br>
