# 实现机制

- jdk动态代理

  > JDK动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK动态代理的核心是`InvocationHandler`接口和`Proxy`类。

- cglib动态代理

  > 目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为`final`，那么它是无法使用CGLIB做动态代理的。
  >
  > 
  >
  > 作者：AskHarries
  > 链接：https://juejin.cn/post/6844903604118093831
  > 来源：稀土掘金
  > 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#  JDK和CGLIB动态代理的区别
- JDK代理使用的是反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理。
  CGLIB代理使用字节码处理框架ASM，对代理对象类的class文件加载进来，通过修改字节码生成子类。

- JDK创建代理对象效率较高，执行效率较低；
  CGLIB创建代理对象效率较低，执行效率高。

- JDK动态代理机制是委托机制，只能对实现接口的类生成代理，通过反射动态实现接口类；
  CGLIB则使用的继承机制，针对类实现代理，被代理类和代理类是继承关系，所以代理类是可以赋值给被代理类的，因为是继承机制，不能代理final修饰的类。

- JDK代理是不需要依赖第三方的库，只要JDK环境就可以进行代理，需要满足以下要求：
   1.实现InvocationHandler接口，重写invoke()
     2.使用Proxy.newProxyInstance()产生代理对象
     3.被代理的对象必须要实现接口

- CGLib 必须依赖于CGLib的类库,需要满足以下要求：
   1.实现MethodInterceptor接口，重写intercept()
     2.使用Enhancer对象.create()产生代理对象
  ————————————————
  版权声明：本文为CSDN博主「Been Doing」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
  原文链接：https://blog.csdn.net/weixin_45723046/article/details/124422910



# SpringBoot 2.x 为何默认使用 Cglib
​        因为JDK 动态代理是基于接口的，代理生成的对象只能赋值给接口变量。JDK动态代理使用Proxy.newProxyInstance()创建代理实现类，然而第二个参数就需要接口类型，如果没有接口类型就会报错。

Proxy.newProxyInstance(iCustomerInstance.getClass().getClassLoader(), iCustomerInstance.getClass().getInterfaces(), this);
而 CGLIB 就不存在这个问题。因为 CGLIB 是通过生成子类来实现的，代理对象无论是赋值给接口还是实现类，这两者都是代理对象的父类
————————————————
版权声明：本文为CSDN博主「tomorrow.hello」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/myli92/article/details/127586235